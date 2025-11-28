---
layout: post
title: "Fra Prototype til Service"
date: 2025-11-20
tags: [NLP, FastAPI, ML, SBERT, Python, OCR]
categories: [ML / AI]
---


Efter at have bygget den grundlæggende fil-upload og OCR-funktionalitet i mit forrige indlæg, er fokus i dag skiftet fra at få det til at virke til at få det til at virke pålideligt.
Dette indlæg dokumenterer rejsen fra en funktionel prototype til en robust, produktionsklar service.

En model er kun værdifuld, hvis den kan håndtere den rodede virkelighed.
Det betyder at forberede sig på ugyldigt input, overvåge systemets adfærd og sikre, at koden er let at vedligeholde og konfigurere.

---

Formålet

Målet var at hærde API'et og implementere de funktioner, der kendetegner professionel softwareudvikling. Konkret har jeg fokuseret på:

    Stabilitet: Forhindre nedbrud forårsaget af uventet input (f.eks. for store filer, forkerte filtyper).

    Intelligent Pre-processing: Sikre høj datakvalitet før data rammer modellen ved at rense tekst og validere sprog.

    Overvågning: Erstatte simple print()-kald med struktureret logging for bedre indsigt i systemets drift.

    Vedligeholdelse: Centralisere konfiguration for nemmere opsætning og justering.

    Tilgængelighed: Gøre den lokalt kørende service tilgængelig for eksterne test via ngrok.


---

Løsningen: En Produktionsklar Arkitektur

For at opnå dette har jeg introduceret flere forbedringer i api.py-koden.
1. Centraliseret Konfiguration

Hårdkodede værdier, som f.eks. stien til Tesseract, er en dårlig praksis. De er nu flyttet til en central config.py-fil.
Dette gør det nemt at justere parametre uden at skulle ændre i selve applikationslogikken.

```python
# config.py

# Sti til Tesseract-installationen
TESSERACT_PATH = r'C:\Program Files\Tesseract-OCR\tesseract.exe'

# Sikkerhedsindstillinger for uploads
MAX_FILE_SIZE = 10 * 1024 * 1024  # 10 MB
ALLOWED_MIME_TYPES = [
    "application/pdf",
    "application/vnd.openxmlformats-officedocument.wordprocessingml.document", # .docx
    "text/plain" # .txt
]
```

2. Struktureret Logging

Alle print()-kald er blevet erstattet af Pythons indbyggede logging-modul.
Det giver en langt mere professionel overvågning med tidsstempler, log-niveauer (INFO, WARNING, ERROR) og formateret output.

```python
# api.py
import logging

logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s'
)
```

# Eksempel på brug
logging.info(f"Modtog fil '{file.filename}' til analyse.")

Dette giver et rent og informativt output i konsollen og kan nemt udvides til at logge til en fil.


3. Intelligent Pre-processing Pipeline

For at sikre, at modellen altid modtager data af høj kvalitet, har jeg indført en pre-processing pipeline, der eksekveres for hver fil:

    Sprog-detektion: Ved hjælp af langdetect identificeres sproget i den udtrukne tekst. Hvis det ikke er dansk eller engelsk, afvises anmodningen med en fejl. Dette forhindrer modellen i at gætte på sprog, den ikke forstår.

    Tekst-rengøring: En simpel clean_text-funktion fjerner overflødige linjeskift og specialtegn, hvilket giver et mere konsistent input til modellen.

API-svaret er også blevet opdateret til at inkludere det detekterede sprog, hvilket giver værdifuld metadata tilbage til klienten.

```json
{
  "filename": "CV_Dansk.pdf",
  "content_type": "application/pdf",
  "detected_language": "da",
  "result": {
    "prediction": "relevant",
    "confidence": 0.891
  }
}
```

4. Håndtering af Edge Cases

For at beskytte API'et er der tilføjet validering direkte i /predict_file/-endpointet, som tjekker for:

    Filtype: Kun de MIME-typer, der er defineret i config.py, accepteres.

    Filstørrelse: Filer, der er større end MAX_FILE_SIZE, afvises med en 413 Payload Too Large fejl.

Dette gør servicen markant mere robust over for fejl og potentielt misbrug.

---

Test med ngrok: Fra Lokal til Global

En lokal server på 127.0.0.1 er god til udvikling, men svær at integrere med andre systemer.
Ved hjælp af ngrok kan man med én simpel kommando oprette en sikker tunnel fra en offentlig URL til sin lokale server.

```bash
# Kør denne kommando i en separat terminal
ngrok http 8000
```

Dette genererer en live-URL (f.eks. https://<random-id>.ngrok-free.app), som alle i verden kan bruge til at kalde min lokalt kørende API.
Dette er et uvurderligt værktøj til hurtig test og demonstration.

---

Læringspunkter

Dette arbejde har været et dyk ned i de praktiske aspekter af MLOps:

    Logging er essentielt for at forstå, hvad der sker i et produktionsmiljø.

    Konfiguration adskilt fra kode gør systemer skalerbare og vedligeholdelsesvenlige.

    Input-validering og pre-processing er lige så vigtigt som selve modellen for at sikre pålidelige resultater.

    Værktøjer som ngrok bygger bro mellem lokal udvikling og reel integration.

---

Videre udvikling

Nu hvor API'et er robust og logger sin aktivitet, er det næste logiske skridt at udnytte de data, det genererer. I næste indlæg vil jeg fokusere på:

    Opsamling af logs: Gemme metadata fra hvert API-kald i en struktureret form.

    Data-analyse af performance: Bruge det indsamlede data til at analysere, hvordan modellen klarer sig, og hvor den har svagheder.

    Data-drevet forbedring: Bruge analysen til at definere, hvordan modellen kan trænes til at blive endnu bedre.

---

Refleksion

Projektet har nu bevæget sig ud over den rene model-udvikling og ind i domænet for software engineering og MLOps.
Ved at tilføje logging, konfiguration og robust fejlhåndtering er API'et gået fra at være et eksperiment til at være en pålidelig service-komponent, klar til at blive integreret i et større system.
Det er denne proces, der transformerer en god model til et værdifuldt produkt.