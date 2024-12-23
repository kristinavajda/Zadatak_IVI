
# Zadatak IVI

## Pregled
Ovde su detaljno objašnjeni koraci uključei u obradu 10 anonimizovanih mamografa. Kod je dizajniran za učitavanje, validaciju, predobradu i augmentaciju medicinskih slika sačuvanih u DICOM formatu. Pored toga, izvršena je predikcija klase za svaki mamogram pomoću pretreniranih modela.

## Koraci u obradi

### Učitavanje potrebnih biblioteka i mamograma
Prvi korak je učitavanje svih potrebnih bibilioteka. Zatim sledi učitavanje DICOM slika Prolazi se kroz folder u kojem se nalazi 10 anonimizovanih mamograma i učitavaju se. Ako slika ne može da se učita ispisuje se poruka o grešci. Zatim se proverava se da li su učitane slike validne (da li su prazne, nepravilnih dimenzija i da li intenzitet piksela spada u očekivani opseg) za dalju obradu da bi se sprečile greške tokom preprocesinga.

### Preprocesing

Vrednosti piksela se normalizuju na opseg od 8 bita (0-255), zatim se uklanja šum median blur filterom (koji manje deformiše ivice) i onda se primenjuje CLAHE za poboljšanje kontrasta. Ovo se koristi jer medicinske slike često imaju nizak kontrast, a CLAHE naglašava razlike u intenzitetu piksela, čime se poboljšava vizualizacija i performanse modela. Promenjena je dimenzija slika se na 512x512.

### Histogrami

Vizualizacija histograma pomaže u proveri uticaja predobrade na distribuciju intenziteta piksela.

### Priprema skupa podataka

Predobrađene slike se konvertuju u NumPy niz. Dodaje se dodatna dimenzija kanala.

### Klasifikacija

Za klasifikaciju mamograma korišećni su pretrenirani modeli VGG16 koji su fine-tune -ovani na INBreast bazi podataka. Prvi model klasifikuje slike na suspektne i nesuspektne, a drugi model klasifikuje na Bi-rads kategorije. Dobijene predikcije sačuvane su u csv fajlu. 

## Korišćene tehnologije i biblioteke

- Python
- Google Colab za izvršavanje koda
- Biblioteke:
  - pydicom: za rad sa DICOM formatom.
  - cv2 (OpenCV): za preprocesiranje slika.
  - numpy: za numeričke operacije.
  - matplotlib: za vizualizaciju histograma.
  - tensorflow: za učitavanje pretreniranih modela.
  - pandas: za manipulaciju i čuvanje rezultata u tabelarnom formatu.

## Uputstvo za pokretanje rešenja

1. Instalacija potrebnih biblioteka:
   Pokrenite sledeću komandu za instalaciju:
   ```bash
   pip install pydicom opencv-python-headless numpy matplotlib tensorflow pandas
   ```

2. Priprema podataka:
   - Postavite DICOM slike i pretrenirane modele u direktorijum `/content/drive/MyDrive/zadatak` ili promeniti putanju u kodu.

3. Izvršavanje koda:
   - Pokrenite skriptu u Python okruženju ili Google Colab-u.

4. Rezultati:
   - Predikcije će biti sačuvane u CSV datotekama:
     - `predikcije.csv`: Sadrži informacije o suspektnim i nesuspektnim slikama.
     - `predikcije_birads.csv`: Sadrži Bi-rads kategorije za svaku sliku.


