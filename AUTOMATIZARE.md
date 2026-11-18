# Logica ESPHome: comportament observat și limite

## Originea informației

A fost citit fișierul `gorwatt-cu-stari-si-automatizar.yaml` în ESPHome Device Builder. Există și o altă configurație în interfață; această documentație nu pretinde verificarea binarului instalat sau a identității sale cu fișierul citit.

Fișierul integral nu este publicat: include configurări de acces și alte componente care nu sunt necesare articolului. Fragmentele de mai jos documentează logica relevantă și **nu constituie un firmware complet de instalat**.

## Inventarul logic

| Identificator | Comportament citit |
|---|---|
| `do1` | Ieșire GPIO prin expander PCA9554, nume DO1 Retea, inversată |
| `do2` | Ieșire pentru sarcină, inversată |
| `do3` | Ieșire pentru nul invertor, inversată |
| `di1` | Schimbarea de stare comută DO1 după inițializare; filtrare 50 ms |
| `di2` | La apăsare pornește DO2; la eliberare oprește DO2; filtrare 10 ms |

## Interblocarea DO1–DO3

Extras din proprietățile DO1:

```yaml
id: do1
restore_mode: RESTORE_DEFAULT_OFF
interlock: [do3]
interlock_wait_time: 1s
on_turn_off:
  - delay: 2s
  - if:
      condition:
        switch.is_off: do1
      then:
        - switch.turn_on: do3
```

Extras din proprietățile DO3:

```yaml
id: do3
restore_mode: ALWAYS_OFF
interlock: [do1]
interlock_wait_time: 2s
```

Aceste fragmente omit intenționat secțiunile de hardware și conectivitate. Nu se lipesc ca atare într-o configurație funcțională.

### Cum se interpretează

1. La cererea de pornire DO1, interblocarea gestionează oprirea ieșirii incompatibile DO3.
2. La oprirea DO1, secvența întârziată așteaptă 2 s și verifică dacă DO1 este încă OFF înainte de a cere DO3 ON.
3. DO3 are și propria interblocare cu DO1 și așteptarea configurată de 2 s.
4. Valorile din configurație nu trebuie însumate sau prezentate ca un timp garantat de transfer al circuitelor de putere; comportamentul complet trebuie înregistrat în teste.

**Nu există în această condiție o măsurare de absență tensiune sau o confirmare de contact auxiliar.** O comandă OFF nu dovedește izolarea fizică. Comportamentul la comenzi repetate în timpul întârzierilor trebuie testat separat.

## Repornire și restaurare

În `on_boot`, la prioritatea -100, configurația așteaptă 3 s. Dacă DO1 este OFF, cere DO3 ON, apoi activează indicatorul intern `di1_ready`, care permite comutarea prin DI1.

DO1 folosește `RESTORE_DEFAULT_OFF`: aceasta nu trebuie descrisă ca garanția „întotdeauna OFF la orice repornire”, deoarece este un mod de restaurare a stării cu valoare implicită OFF. DO3 folosește `ALWAYS_OFF`, urmat de logica din `on_boot`. Pentru DO2 nu este declarat explicit `restore_mode` în fragmentul citit; nu deducem o stare sigură la resetare.

Verificarea fizică a pornirii, resetării și căderii alimentării controlerului rămâne necesară. Home Assistant poate lipsi, însă alimentarea controlerului, modul API și restul configurației pot influența disponibilitatea sistemului; autonomia locală nu echivalează cu o garanție de continuitate.

## Interblocaje distincte

| Pereche | Dovadă disponibilă | Limită |
|---|---|---|
| DO1/DO3 | Interblocare în fișierul citit | Software; nu este confirmarea contactelor C1/C2 |
| C3/C4 | Interblocaj mecanic și electric declarat de realizator | Schema AAR și testele nu sunt furnizate |

Starea „rețea prezentă” nu este dedusă din simplul DO1 ON; starea „baterie” nu este dedusă doar din DO1 OFF. Legarea acestor stări de măsurători și reacții fizice este un subiect de validare, nu o funcție pe care acest articol pretinde că a demonstrat-o.

## Ce nu publicăm ca soluție verificată

Nu publicăm o schemă de borne, o punte N–PE universal aplicabilă, o secvență de avarie pretins certificată sau un firmware complet pentru replicare. Nu s-a modificat instalația pentru redactarea documentației.

Referință: [ESPHome GPIO Switch](https://esphome.io/components/switch/gpio/). Autorul platformei avertizează explicit asupra limitelor interblocării exclusiv software.
