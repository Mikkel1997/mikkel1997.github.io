---
layout: post
title: "Analyse af API Performance"
date: 2025-11-26
tags: [Data Science, Pandas, Matplotlib, Seaborn]
categories: [Data Science]
---

I de forrige indlæg har jeg bygget og hærdet en fuldt funktionel CV-analyse API.
Servicen kører stabilt og indsamler nu data for hvert eneste kald i en SQLite-database.
Men at indsamle data er kun halvdelen af arbejdet.
Den virkelige værdi opstår, når vi begynder at analysere den data for at forstå og forbedre det system, vi har bygget.

I dagens indlæg tager jeg Data Scientist-hatten på.
Jeg vil dykke ned i de indsamlede logs for at bevæge mig fra antagelser til evidensbaseret viden om, hvordan min model og mit API performer i praksis.

---

## Formålet

Målet er at udføre en eksplorativ data-analyse (EDA) for at besvare en række forretningskritiske spørgsmål:

    Effektivitet: Hvor ofte er den dyre OCR-proces reelt nødvendig?

    Performance: Hvad er den reelle performance-omkostning ved at skulle køre OCR?

    Modellens Selvtillid: Hvordan fordeler modellens confidence-scores sig? Er den generelt sikker eller usikker i sine forudsigelser?

    Identifikation af Svagheder: Hvilke typer af CV'er er modellen mest i tvivl om? Kan vi finde "gråzone"-dokumenterne?

Svarene på disse spørgsmål vil give en klar retning for, hvordan jeg mest effektivt kan forbedre systemet.

---

## Løsningen: Analyse i en Jupyter Notebook

Til denne opgave er en Jupyter Notebook det perfekte værktøj. Den lader mig kombinere kode, visualiseringer og kommentarer i et interaktivt format. Ved hjælp af standard Data Science-biblioteker som Pandas, Matplotlib og Seaborn, kan jeg indlæse data direkte fra cv_scanner.db-filen og begynde at udforske.

Processen er simpel:

    Opret forbindelse til SQLite-databasen med SQLAlchemy.

    Indlæs logs-tabellen i en Pandas DataFrame.

    Rens og forbered dataen (f.eks. konvertere timestamps).

    Visualiser data for at afdække mønstre.

# analysis.ipynb
import pandas as pd
from sqlalchemy import create_engine

# Indlæs data fra databasen til en DataFrame
engine = create_engine("sqlite:///cv_scanner.db")
df = pd.read_sql_query("SELECT * FROM logs", engine)

# Vis de første rækker
df.head()

---

## Resultater: Indsigter fra Dataen

Efter at have kørt en række tests mod API'et, gav analysen af de indsamlede logs følgende indsigter:
Indsigt 1: OCR er en Nødvendig, men Sjælden Proces

Pie-chartet nedenfor viser, at i mit test-datasæt var det kun nødvendigt at aktivere OCR i en lille brøkdel af tilfældene. Størstedelen af dokumenterne indeholdt digital tekst. Dette er en vigtig indsigt: min hurtige, digitale pipeline håndterer hovedparten af arbejdet, mens OCR-fallbacken fungerer som en afgørende, men sjældnere, "sikkerhedsline".


![Brug af OCR vs Digital Tekst](images/OCR)

Indsigt 2: Der er en Markant Performance-omkostning ved OCR

Boxplottet bekræfter, hvad man kunne forvente: OCR er en tidskrævende proces. I gennemsnit er behandlingstiden markant højere for kald, der kræver OCR. Dataen viser, at digitale dokumenter analyseres på under et halvt sekund, mens OCR-processen kan tage adskillige sekunder. Dette understreger værdien af den to-trins-pipeline, der kun aktiverer OCR ved behov.

![Behandlingstid med og uden OCR](images/Behandlingstid)


Indsigt 3: Modellens Selvtillid er Generelt Høj, men med en interessant "Gråzone"

Histogrammet over confidence-scores viser en positiv tendens: de fleste forudsigelser ligger i den høje ende (tæt på 1.0), hvilket indikerer, at modellen ofte er meget sikker. Der er dog en interessant "dal" i midten – et område mellem ca. 0.5 og 0.7, hvor modellen er i tvivl.

![Confidence Score Distribution](images/Confidence)

Dette er det mest værdifulde fund i hele analysen. Disse "gråzone"-forudsigelser er guld værd, for de viser os præcis, hvor modellen har brug for at blive bedre.

---

## Kernen i Analysen: Isolering af Svagheder

Ved at filtrere min DataFrame kan jeg isolere præcis de filer, hvor modellen var mest usikker:

# Find alle forudsigelser med en confidence mellem 0.5 og 0.7
uncertain_df = df[(df['confidence'] >= 0.5) & (df['confidence'] <= 0.7)]

Resultatet er en konkret liste over dokumenter, som jeg manuelt kan inspicere. En hurtig gennemgang afslører et mønster: modellen kæmper ofte med CV'er fra brancher, der ligger tæt op ad det juridiske (f.eks. revision, HR, offentlig administration), men som ikke er direkte relevante.

---

## Læringspunkter

    Fra Logs til Viden: Rå log-data er støj, men med de rette værktøjer kan det omdannes til klar, handlingsorienteret indsigt.

    Visualisering Fortæller Historien: Et enkelt plot kan ofte kommunikere et komplekst mønster mere effektivt end en tabel med tal.

    Find Fejlene med Data: Den vigtigste funktion af data-analyse i et MLOps-loop er ikke at bekræfte, hvad der virker, men at afdække, hvad der ikke virker.

---

## Refleksion

Dette indlæg fuldender den første cirkel i projektets MLOps-livscyklus: Byg -> Indsaml -> Analysér.
Ved at bruge data, som systemet selv har genereret, til at evaluere dets performance, har jeg skabt en feedback-mekanisme.
Dette er kernen i at bygge intelligente systemer, der ikke er statiske, men som kan udvikle sig og blive bedre over tid baseret på reel brugerinteraktion.