---
layout: post
title: "Fil- og Billedeopload"
date: 2025-11-13
tags: [NLP, FastAPI, ML, SBERT, Python, OCR]
categories: [ML / AI]
---

I dette indlæg bygger jeg videre på mit eksisterende CV-klassifikations-API og fokuserer på en af de mest praktiske udfordringer i et rigtigt rekrutteringsflow:  
**håndtering af CV’er i alle former** – PDF, DOCX, TXT samt scannede dokumenter og billeder.

I modsætning til rene tekstinput kræver filtyper ekstra logik, fejlbehandling og fallback-strategier. Derfor har jeg udvidet API’et til at kunne håndtere både digital tekst og OCR-baseret billedgenkendelse.

---

## Formålet

Målet med arbejdet var at gøre API’et i stand til at:

- Modtage filer i flere formater  
- Udtrække tekst automatisk (digitalt eller via OCR)  
- Sikre stabil upload i FastAPI’s Swagger-miljø  
- Gøre processen robust nok til at håndtere rigtige CV’er fra brugere  

Dette betyder, at API’et nu er i stand til at klassificere CV’er, selv når de består af **scannede PDF’er**, **Word-dokumenter** eller **billeder**, uden at klienten behøver at tænke over filformatet.

---

## Udfordringer

Under udviklingen opstod der flere praktiske problemer:

- PDF’er kan være digitale eller blot billeder  
- DOCX-konvertering kræver skriveadgang og midlertidige filer  
- FastAPI kan bryde Swagger-upload UI hvis 3.-parts libraries laver fejl  
- OCR kræver installation af Tesseract lokalt  
- Nogle filer returnerer ingen tekst og kræver fallback  

---

## Min løsning

Jeg designede en **robust, modulær pipeline**, der automatisk vælger den korrekte metode afhængigt af filen:

### **1. Filtype-detektion**
API’et identificerer filtypen ud fra extension og vælger passende strategi.

### **2. DOCX → PDF konvertering**
For at undgå at vedligeholde to tekstudtræks-metoder konverteres DOCX automatisk:

- DOCX gemmes midlertidigt  
- Konverteres til PDF via `docx2pdf`  
- Viderebehandles som en helt almindelig PDF  

### **3. PDF-udtræk + OCR fallback**
PDF’er behandles med PyMuPDF:

### **4 Billede- og tekstudtræk:**

Hvis der ikke findes digital tekst → bruges OCR:

PDF-siden konverteres til billede

Tesseract (dan+eng) læser teksten

Teksten samles og sendes videre til ML-modellen

Dette gør scannede CV’er analyserbare.

Jeg testede med forskellige PDF’er og billeder for at sikre, at både digitale og scannede dokumenter blev håndteret korrekt.

### **5 Stabilitet via TemporaryDirectory**

For at undgå filhåndteringsfejl i FastAPI’s Swagger UI brugte jeg `TemporaryDirectory` til at sikre, at midlertidige filer altid slettes korrekt efter brug.

```python
with tempfile.TemporaryDirectory() as temp_dir:
```
Dette sikrer:

Ingen permanente filer

Ingen konflikter mellem samtidige brugere

Swagger UI fungerer stabilt

---

## Resultat

Efter implementeringen kan systemet nu:

Håndtere PDF, DOCX, TXT og billeder
Automatisk falde tilbage til OCR hvis digital tekst mangler
Sikre stabil upload i Swagger
Levere ensartet tekst videre til modellen
Virke sammen med eksterne klienter via REST

Dette betyder, at API’et nu er anvendeligt i realistiske scenarier, hvor brugere uploader CV’er i alle mulige formater.

---

## Læringspunkter

Dette arbejde har givet værdifuld erfaring med:

Filhåndtering i FastAPI

Midlertidige filer & tempfile

OCR med Tesseract

PDF-parsing via PyMuPDF

Robust fallback-logik når tekst mangler

Stabil schema-håndtering til Swagger UI