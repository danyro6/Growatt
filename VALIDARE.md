# Plan de validare și trasabilitate

**Acesta este un plan de verificare, nu un raport de teste efectuate.** Nu sunt disponibile rezultate numerice pentru a completa coloana de dovezi. Verificările la tensiune de rețea revin unei persoane competente, cu aparate și proceduri potrivite.

## Matricea dovezilor

| Afirmație | Origine | Statut |
|---|---|---|
| DO1 și DO3 sunt interblocate software | Fișier ESPHome citit | Confirmat la nivel de configurație |
| C2 închide N–PE în regim autonom | Explicația realizatorului | Declarat; schema de borne lipsește |
| C3/C4 au interblocaj mecanic/electric | Explicația realizatorului și ansamblul fotografiat | Declarat; neverificat funcțional |
| Diferențialele funcționează în toate regimurile | Fără raport disponibil | Nedemonstrat |
| Există confirmare de poziție pentru C1/C2 | Fără identificare de semnal disponibilă | Nedemonstrat |
| Fișierul citit este exact firmware-ul instalat | Fără verificare de build | Nedemonstrat |

## Informații necesare înaintea unei scheme de execuție

- Modelul complet și versiunea invertorului; instrucțiunile sale privind nulul și legătura N–PE.
- Schema unifilară și de comandă, cu traseele L, N, PE și toate sursele.
- Regimul de împământare al instalației; poziția și tipul protecțiilor diferențiale.
- Modelele contactorilor, tensiunile bobinelor, polii utilizați și contactele auxiliare.
- Schema AAR, semnalele de avarie, reacțiile de poziție și condițiile de revenire din bypass.
- Comportamentul la lipsa alimentării controlerului, la resetare și la pierderea comunicației.

## Registru de verificări

| ID | Verificare de documentat | Dovadă așteptată | Rezultat |
|---|---|---|---|
| V01 | Continuitatea PE și legăturile de protecție | Raport de măsurare | Neefectuat în acest studiu |
| V02 | Configurația N/PE în fiecare regim | Schemă validată și măsurători | Neefectuat |
| V03 | Declanșarea RCD pe alimentare AC, baterie și bypass | Rezultate cu aparat de test, conform schemei | Neefectuat |
| V04 | Timpii reali C1/C2 și absența suprapunerii nepermise | Înregistrare a reacțiilor și tensiunilor | Neefectuat |
| V05 | Transferul C3/C4 și interblocajul | Procedură și raport AAR | Neefectuat |
| V06 | Resetare și pierderea alimentării WaveShare | Jurnal de stări și reacții fizice | Neefectuat |
| V07 | Comenzi repetate în timpul întârzierilor | Test controlat al secvenței | Neefectuat |
| V08 | Discrepanța comandă/contact sau contact lipit | Analiză de defect și reacție proiectată | Neefectuat |
| V09 | Pierderea Home Assistant/API/rețelei locale | Jurnal și comportament observat | Neefectuat |

Butonul TEST al unei diferențiale nu substituie raportul privind protecția în toate regimurile instalației. Nu se provoacă defecte reale sau scurtcircuite pentru demonstrații fotografice.

## Limite ale fotografiei

Fotografia permite orientarea în tablou. Nu confirmă strângerea bornelor, secțiunile tuturor conductoarelor, curenții nominali, izolația, selectivitatea sau traseele complete. Fotografia nu trebuie utilizată pentru a reproduce cablajul.

## Cum se completează documentația

Pentru fiecare test se notează data, persoana care l-a realizat, aparatul utilizat, configurația exactă, rezultatul și fișierul justificativ. Datele care nu există rămân marcate „nedemonstrat”. O versiune ulterioară poate adăuga rapoarte reale, fără a rescrie retrospectiv statutul acestei ediții.
