# Coordonarea alimentării și a legăturii neutru–protecție într-un sistem UPS cu invertor Growatt

**Ghiulaiu Florin Daniel**  
Studiu de caz tehnic · 18 noiembrie 2026

![Arhitectura funcțională](arhitectura.svg)

## Rezumat

Lucrarea prezintă arhitectura unui sistem de comandă pentru un invertor Growatt utilizat ca sursă neîntreruptibilă cu stocare. Implementarea folosește un controler WaveShare ESP32-S3-POE-ETH-8DI-8DO, platforma ESPHome și integrarea Home Assistant. Trei ieșiri comandă alimentarea AC, conectarea consumatorilor și contactorul destinat legăturii dintre conductorul neutru și conductorul de protecție în regim autonom. O pereche de contactori cu interblocaj mecanic și electric, conform configurației declarate a instalației, asigură funcția de transfer către bypass. Analiza configurației evidențiază separarea funcțiilor și temporizarea comenzilor, precum și diferența dintre starea logică a unei ieșiri și starea fizică a circuitului de putere. Studiul are caracter descriptiv; eficacitatea protecțiilor și performanțele transferului nu au fost evaluate prin măsurători în cadrul lucrării.

**Cuvinte-cheie:** UPS; Growatt; ESPHome; Home Assistant; neutru; N–PE; protecție diferențială; interblocare; AAR.

## 1. Introducere

Utilizarea unui invertor cu baterie pentru alimentarea de rezervă a unei locuințe impune coordonarea alimentării AC, a conectării consumatorilor și a regimului conductorului neutru. Continuitatea alimentării și protecția la defect reprezintă obiective distincte: prezența tensiunii la ieșire nu demonstrează, singură, condițiile de funcționare a protecțiilor.

Obiectivul lucrării este descrierea unei implementări locale cu funcții separate de comandă și supraveghere. În instalația analizată, intrarea AC a Growatt poate fi alimentată din sistemul DEYE; absența acestei alimentări nu este considerată automat echivalentă cu absența rețelei publice.

## 2. Fundament teoretic

Protecția diferențială detectează curentul rezidual rezultat din dezechilibrul curenților care traversează conductoarele active monitorizate. Într-o sursă flotantă, primul defect față de pământ poate produce un curent insuficient pentru declanșarea anticipată într-o instalație cu sursă referențiată.

În schemele care prevăd această măsură, legătura N–PE la sursă contribuie la stabilirea traseului curentului de defect prin conductorul de protecție. Poziția legăturii față de dispozitivele diferențiale și coordonarea sa cu sursele alternative sunt esențiale. Continuitatea PE și alegerea protecțiilor rămân necesare independent de comanda acestei legături. Referința [1] prezintă principiul pentru invertoare și invertoare/încărcătoare; aplicabilitatea la modelul Growatt utilizat necesită verificarea documentației sale specifice.

Analiza privește instalația de locuință descrisă și nu exclude schemele izolate proiectate corespunzător, precum sistemele IT.

## 3. Materiale și metodă

Metoda constă în analiza configurației ESPHome, identificarea entităților Home Assistant și corelarea acestora cu fotografia și descrierea funcțională a tabloului. Fișierul analizat este `gorwatt-cu-stari-si-automatizar.yaml`; identitatea sa cu firmware-ul instalat nu a fost verificată printr-un identificator de build.

<p align="center"><img src="tablou-growatt.png" width="560" alt="Tabloul sistemului Growatt UPS" /></p>

**Figura 1.** Tabloul de comandă și comutație al instalației.

| Reper | Componentă | Funcție |
|---|---|---|
| C1 | Contactor Schrack, jos în stânga | Alimentarea AC a invertorului Growatt |
| C2 | Contactor Schneider, central | Comanda legăturii N–PE în regim autonom |
| C3 | Primul contactor mare din dreapta | Conectarea ieșirii Growatt către consumatori |
| C4 | Contactorul mare din extrema dreaptă | Alimentarea prin bypass în ansamblul AAR |

Reperele C1–C4 sunt utilizate în lucrare pentru identificarea funcțională. Perechea C3–C4 este descrisă ca având interblocaj mecanic și electric. Schema completă a AAR nu a făcut parte din materialul analizat.

## 4. Arhitectura de comandă

Cele trei ieșiri ESPHome sunt asociate unor funcții distincte:

| Ieșire | Denumire în configurație | Element comandat |
|---|---|---|
| DO1 | `DO1 Retea` | C1 – alimentare AC |
| DO2 | `DO2 Sarcina` | C3 – conectarea consumatorilor |
| DO3 | `DO3 Nul Invertor` | C2 – legătură N–PE |

Această separare permite coordonarea alimentării, a sarcinii și a referinței neutru–protecție. Funcția de bypass introduce al patrulea contactor, C4, distinct de cele trei elemente asociate direct ieșirilor DO1–DO3.

DO1 și DO3 formează un grup de interblocare software. Pentru DO1 este configurat `interlock_wait_time: 1s`, iar pentru DO3, `interlock_wait_time: 2s`. La oprirea DO1, o secvență așteaptă 2 s și verifică dacă DO1 este încă oprit înainte de a solicita activarea DO3.

La inițializare, programul așteaptă 3 s, solicită DO3 activ dacă DO1 este oprit și permite ulterior acționarea DI1. DI1 comută DO1 la schimbarea stării, iar DI2 comandă pornirea și oprirea DO2.

## 5. Integrarea Home Assistant

Comunicarea cu Home Assistant se realizează prin integrarea nativă ESPHome [3]. Controlerul execută secvența locală, iar Home Assistant oferă acces la entitățile de comandă și la informațiile disponibile.

| Entitate | Mărime reprezentată |
|---|---|
| `switch.waveshare_di8_do8_do1_retea` | Comanda alimentării AC |
| `switch.waveshare_di8_do8_do2_sarcina` | Comanda conectării consumatorilor |
| `switch.waveshare_di8_do8_do3_nul_invertor` | Comanda legăturii N–PE |

Aceste entități reprezintă stări logice de comandă. Confirmarea poziției contactorilor necesită reacții fizice distincte. Indisponibilitatea comunicației constituie lipsă de informație și nu este echivalentă cu starea oprit.

## 6. Rezultatele analizei

Analiza fișierului confirmă existența separării DO1–DO3, a interblocării software și a temporizărilor configurate. Activarea DO3 este condiționată de starea logică DO1, fără o verificare explicită a absenței tensiunii AC sau a poziției C1 în secvența examinată.

În consecință, configurația demonstrează coordonarea comenzilor, dar nu demonstrează izolarea fizică a sursei. Timpii configurați nu reprezintă măsurători ale deschiderii contactelor sau timpi certificați de transfer UPS. Funcționarea AAR și interblocajul C3–C4 sunt descrise pe baza informațiilor realizatorului, fără test funcțional inclus în studiu.

## 7. Discuție și limite

Interblocarea software facilitează exclusivitatea logică a comenzilor, însă nu înlocuiește măsurile hardware. Documentația ESPHome evidențiază limitele acestei funcții în prezența erorilor software și în timpul resetării [2]. Interblocajul C3–C4 nu constituie dovada unui interblocaj fizic între C1 și C2.

Validarea electrică necesită schema completă L/N/PE, regimul de împământare, caracteristicile invertorului și protecțiilor, precum și verificări în regim AC, autonom și bypass. Nu sunt disponibile în cadrul lucrării rezultate de continuitate PE, teste instrumentate ale diferențialelor, măsurători ale buclei de defect sau înregistrări ale transferului. Prin urmare, nu se formulează o concluzie de conformitate sau certificare a instalației.

## 8. Concluzii

Implementarea descrisă separă comanda alimentării AC, conectarea consumatorilor și tratarea nulului într-o arhitectură locală integrată în Home Assistant. Contribuția studiului constă în descrierea structurată a unei instalații personale și în analiza relației dintre comenzile logice și funcțiile fizice intenționate. Confirmarea eficacității protecțiilor și a comportamentului în avarie rămâne condiționată de verificarea electrică și de documentarea experimentală.

## Referințe

1. Victron Energy. *Wiring Unlimited*, capitolul 7: [Ground, earth and electrical safety](https://www.victronenergy.com/media/pg/The_Wiring_Unlimited_book/en/ground%2C-earth-and-electrical-safety.html).
2. ESPHome. [GPIO Switch – Interlocking](https://esphome.io/components/switch/gpio/).
3. Home Assistant. [ESPHome integration](https://www.home-assistant.io/integrations/esphome/).

---

© 2026 Ghiulaiu Florin Daniel. Toate drepturile rezervate pentru contribuțiile originale asupra cărora autorul deține drepturi. Drepturile terților și termenii GitHub rămân aplicabili. [Condiții de utilizare](LICENSE).
