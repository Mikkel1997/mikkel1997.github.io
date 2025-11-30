---
layout: post
title: "Opbygning af data"
date: 2025-11-23
tags: [MLOps, Logging, SQLite, FastAPI, Data Collection, Python]
categories: [ML / AI]
---

I mit forrige indlæg transformerede jeg en prototype til en robust, produktionsklar API-service. Men en service, der kører som en "black box", har begrænset værdi.
For at kunne forbedre min model og forstå, hvordan den performer i praksis, er jeg nødt til at besvare spørgsmål som:

    Hvor ofte er OCR nødvendigt?

    Hvilke filtyper er mest almindelige?

    Hvad er modellens gennemsnitlige confidence?

    Er der situationer, hvor modellen er systematisk usikker?

For at besvare dette, skal jeg have data.
I dagens indlæg bygger jeg derfor fundamentet for dataindsamling ved at implementere en struktureret logging-mekanisme direkte i API'en.

---

## Formålet

Målet er at omdanne mit API fra at være en passiv service til at være en aktiv dataindsamler. Hver gang en bruger uploader et CV, skal systemet automatisk og diskret logge værdifulde metadata om kaldet.

Den data, jeg ønsker at indsamle, inkluderer:

    Timestamp: Hvornår blev kaldet lavet?

    Fil-metadata: Navn, type og størrelse.

    Proces-metadata: Blev OCR brugt? Hvad var den totale behandlingstid?

    Model-output: Hvad var forudsigelsen og confidence-scoren?

Dette vil skabe et struktureret datasæt, som jeg i næste fase kan bruge til dybdegående analyse – det første skridt i en datadrevet forbedringscyklus.

---

## Løsningen: En Letvægts Database-pipeline

Simple print()-kald eller en flad logfil er ikke robuste nok. De er svære at forespørge i og kan skabe problemer ved samtidige requests. Derfor har jeg valgt en mere professionel løsning:

    Database: SQLite, en serverless, fil-baseret database, der er indbygget i Python. Den er perfekt til letvægts-logging uden behov for en ekstern databaseserver.

    Arkitektur: Al database-logik er isoleret i et separat database.py-modul for at holde koden ren og overskuelig (separation of concerns).

    Performance: For at sikre at logningen ikke forsinker svaret til brugeren, bruger jeg FastAPI's BackgroundTasks. Dette tillader API'en at sende sit svar tilbage med det samme, mens database-skrivningen sker i baggrunden.

Arkitektur

Ved opstart af API'en initialiseres databasen automatisk via FastAPI's lifespan-manager, som opretter en cv_scanner.db-fil og en logs-tabel.

# database.py - Opretter tabellen
cursor.execute("""
    CREATE TABLE IF NOT EXISTS logs (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        timestamp TEXT NOT NULL,
        filename TEXT,
        ocr_used BOOLEAN,
        prediction TEXT,
        confidence REAL,
        processing_time_ms REAL
        -- ... og andre kolonner
    )
""")

Selve logningen i api.py sker asynkront. BackgroundTasks injiceres i endpointet, og add_task bruges til at kalde log_request-funktionen, efter svaret er returneret.

# api.py - Udsnit af endpoint
from fastapi import BackgroundTasks

@app.post("/predict_file/")
async def predict_from_file(background_tasks: BackgroundTasks, file: UploadFile = File(...)):
    # ... (hele analyse-pipelinen kører her)
    
    # Forbered data til logging
    log_data = {
        "filename": file.filename,
        "ocr_used": ocr_used,
        "prediction": result['prediction'],
        # ... andre data
    }

    # Tilføj logningen som en baggrundsopgave
    background_tasks.add_task(database.log_request, log_data)
    
    # Returner det hurtige svar til brugeren
    return { "filename": file.filename, "result": result }


---

## Resultatet

Efter at have kørt API'en og uploadet en række test-dokumenter, er en cv_scanner.db-fil blevet oprettet.
Ved at forbinde til denne fil via PyCharms indbyggede database-værktøj, kan jeg nu se de indsamlede data i et pænt, struktureret tabelformat:
(Eksempeldata)
id	timestamp	filename	ocr_used	prediction	confidence	processing_time_ms
1	2025-11-16T10:30:05	CV_digital.docx	0 (false)	relevant	0.921	450.75
2	2025-11-16T10:32:15	Scannet_CV.pdf	1 (true)	relevant	0.785	3210.50
3	2025-11-16T10:35:40	ansøgning.txt	0 (false)	ikke relevant	0.998	215.20

Hver række er et værdifuldt datapunkt, der fortæller en historie om et specifikt API-kald.

---

## Læringspunkter

    Logging er dataindsamling: Professionel logging handler ikke kun om fejlfinding, men om at skabe et fundament for forretningsindsigt og model-overvågning.

    Vælg det rigtige værktøj: SQLite er et fremragende eksempel på en simpel, men kraftfuld løsning, der passer perfekt til projektets skala.

    Performance er afgørende: Brugen af BackgroundTasks sikrer en god brugeroplevelse, da kernefunktionaliteten (forudsigelsen) ikke bremses af sekundære opgaver (logning).

---

## Refleksion

Dette indlæg markerer et vigtigt skift i projektet. Fokus er flyttet fra ren ML Engineering (at bygge og deploye en service) til at forberede Data Science (at lære af den deployede service).
Ved at instrumentere API'en med logging, har jeg bygget en bro mellem de to discipliner og skabt en feedback-loop, der er essentiel for at bygge intelligente systemer, der bliver bedre over tid.