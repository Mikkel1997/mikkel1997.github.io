---
layout: post
title: "Fra Model til API – SBERT som Webservice"
date: 2025-10-20
tags: [NLP, FastAPI, ML, SBERT, Python, REST]
categories: [ML / AI]
---

I dette indlæg bygger jeg videre på mit tidligere **SBERT-baserede CV-klassifikationsprojekt**, hvor jeg i dag har taget næste skridt:  
at omdanne modellen til en **API-service**, som kan modtage input fra andre systemer og returnere en klassifikation i realtid.

Det betyder, at vores model nu ikke kun kører lokalt i et script — den fungerer som en **intelligent backend-komponent**, som en webapplikation eller et HR-system kan kalde direkte.

---

## Formålet

Målet med dagens arbejde var at gøre modellen **tilgængelig via HTTP**.  
I praksis betyder det, at et frontend-system (for eksempel en webformular hvor en bruger uploader et CV) kan sende teksten til vores API og få et svar tilbage med:

- Klassifikationen ("relevant" / "ikke relevant")  
- Confidence-scoren (modellens sikkerhed på forudsigelsen)

Dette er et vigtigt skridt mod en **produktionsklar løsning**, hvor maskinlæring integreres i et eksisterende rekrutteringsflow.

---

## Teknologier

API’en er bygget i **FastAPI**, et moderne Python-framework til RESTful webservices.  
Kombineret med vores eksisterende model opstår et effektivt setup:

- **FastAPI** – oprettelse af REST-endpoints  
- **SentenceTransformers** – til at encode nye CV-tekster  
- **Scikit-learn** – modelafvikling og confidence score  
- **Joblib** – til at indlæse gemte modeller  
- **Swagger UI** – automatisk genereret testinterface  

---

## Arkitektur

Den nye komponent består af én fil:  
`api/main.py` – som indeholder selve webserveren.

Ved opstart loades modellen og encoderen automatisk:

```python
MODEL_PATH = "sbert_version/model_sbert.pkl"
ENCODER_PATH = "sbert_version/encoder_sbert.pkl"
model = joblib.load(MODEL_PATH)
encoder = joblib.load(ENCODER_PATH)
```

```json
{
  "text": "Jeg har arbejdet som advokat med kontraktret og familieret."
}
```
Svaret returneres direkte som JSON:

```json
{
  "input_text": "Jeg har arbejdet som advokat med kontraktret og familieret.",
  "prediction": "relevant",
  "confidence": 0.92
}
```
## Test i Swagger

En af de store fordele ved **FastAPI** er, at den automatisk genererer en interaktiv **Swagger-grænseflade**. Efter at have startet serveren med `uvicorn api.main:app --reload`, kan man gå til [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs) hvor API’et kan testes direkte via browseren. Her kan man indsætte tekst, trykke **"Execute"** og med det samme se modellens respons — både *prediction* og *confidence-score*.

Dette gør udvikling og debugging markant hurtigere, da man ikke behøver lave et separat frontend-interface for at teste funktionaliteten.

---

## Læringspunkter

Dagens arbejde har givet indsigt i, hvordan man:

- Pakker en **ML-model** ind i et **web-API**, klar til integration  
- Håndterer indlæsning af modeller og encodere effektivt ved serverstart  
- Tilføjer beregning af **confidence** for at vurdere modellens sikkerhed  
- Tester API’er nemt med **Swagger UI**  

Det er et vigtigt skridt i retning af en **reel produktionspipeline**, hvor modellen kan anvendes af eksterne systemer.

---

## Videre udvikling

Næste skridt bliver at:

- Udvide API’et med **upload af PDF/DOCX-filer**, hvor teksten automatisk udtrækkes  
- Logge forudsigelser og bruge dem til **modelmonitorering**  
- Udvikle et **Data Science-dashboard** til at følge modellens præstation over tid  
- Gøre API’en tilgængelig online via en cloud-platform som **Render** eller **Railway**  

---

## Refleksion

Ved at omdanne SBERT-modellen til en **API** har projektet bevæget sig fra ren modeludvikling til **systemintegration**. Det er et vigtigt skridt mod praktisk anvendelse af NLP-modeller i virkelige arbejdsgange — hvor AI ikke blot er et eksperiment, men en del af et produktionsmiljø.

I næste indlæg begynder jeg arbejdet med **dokumentbehandling** – hvor API’en skal kunne modtage og analysere CV’er direkte fra **PDF- og Word-filer**.
