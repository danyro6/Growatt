# Ghid pas cu pas pentru întreținerea proiectului

Repository: **https://github.com/danyro6/Growatt**

## 1. Citește pagina principală

Deschide repository-ul și derulează la README. Fotografia și diagrama sunt fișiere ale proiectului, nu imagini dependente de un serviciu temporar.

## 2. Modifică un text

1. Conectează-te pe GitHub cu propriul cont.
2. Deschide fișierul dorit, de exemplu `ARTICOL.md`.
3. Apasă pictograma creion / **Edit this file**.
4. Modifică textul și folosește **Preview** pentru verificare.
5. Apasă **Commit changes**, scrie o descriere clară și confirmă salvarea în ramura proiectului.

## 3. Adaugă o fotografie

1. Revino în pagina principală a repository-ului.
2. Alege **Add file → Upload files**.
3. Selectează fotografia fără date private și așteaptă încărcarea.
4. Scrie un mesaj de commit și salvează.
5. În README adaugă `![Descrierea fotografiei](nume-fisier.png)`.

Nu publica tabloul cu etichete de acces, parole sau informații personale care nu trebuie făcute publice. Verifică metadatele fișierelor noi înaintea publicării.

## 4. Păstrează drepturile și atribuirile

Fișierele `LICENSE`, `NOTICE.md` și `SURSE.md` explică domeniul drepturilor rezervate. Nu înlocui mențiunea cu o licență MIT/GPL dacă nu dorești să acorzi acele permisiuni. Pentru materiale preluate, păstrează atribuirea și respectă licența lor.

Public nu înseamnă că poți împiedica vizualizarea sau fork-ul permis de GitHub. Mențiunea de copyright nu este brevet și nu rezervă ideea tehnică.

## 5. Adaugă dovezi reale

Actualizează `VALIDARE.md` numai după ce există măsurători și rapoarte reale. Adaugă schema verificată și manualul exact prin link către producător, fără a copia manuale protejate. Nu transforma „declarat” în „verificat” doar pentru că un releu apare ON în interfață.

## 6. Versiuni

Păstrează schimbările în commit-uri cu mesaje clare. Pentru o ediție importantă poți folosi pagina **Releases → Draft a new release**, de exemplu tag-ul `v1.0.0`, apoi titlul și lista modificărilor. Nu este necesară o versiune nouă la fiecare corectură de ortografie.

## 7. Nu încărca secrete

Nu publica `secrets.yaml`, parole Wi-Fi, chei API, tokenuri Home Assistant, fișiere `.storage`, copii de siguranță sau firmware-uri care le includ. Fișierul `.gitignore` ajută pentru lucrul local cu Git; **nu verifică și nu blochează încărcarea manuală din browser**.

## Descriere sugerată pentru pagina About

`Studiu de caz Growatt UPS: WaveShare, ESPHome, Home Assistant, comenzi separate, tratarea nulului și bypass AAR. Ghiulaiu Florin Daniel.`

Referință pentru interfața de încărcare: [GitHub Docs](https://docs.github.com/en/repositories/working-with-files/managing-files/adding-a-file-to-a-repository).
