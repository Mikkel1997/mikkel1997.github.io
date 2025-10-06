---
layout: post
title: "ML i praksis: Bygning af en CV-klassifikationsmodel"
date: 2025-10-06
tags: [NLP, ML, Python, Projekt]
categories: [ML / AI]
---

I dette indlæg vil jeg præsentere et praktisk projekt, hvor jeg har udviklet en machine learning-model i Python, der kan klassificere jobansøgninger (CV’er) som relevante eller ikke relevante for et specifikt jobopslag — i dette tilfælde et advokatfirma.

Projektet er bygget fra bunden i PyCharm og demonstrerer hele workflowet fra dataforberedelse til modeltræning, opdatering og brug i praksis.

## Formål

Formålet var at automatisere en proces, som i mange virksomheder udføres manuelt:
at gennemgå ansøgninger og finde dem, der matcher et jobopslag.

Ved hjælp af Natural Language Processing (NLP) og klassisk Machine Learning kan systemet hurtigt identificere om en ansøgning er relevant ud fra tekstens indhold — fx om den nævner “juridisk erfaring”, “advokat”, “kontraktret” osv.

## Teknologier

Projektet er bygget i Python og anvender følgende biblioteker:

pandas – håndtering af data
numpy – numeriske operationer
scikit-learn – modeltræning, tekstvektorisering og evaluering
nltk – sproglig forbehandling (stopord, tokenisering)
pickle – gemning og genindlæsning af model og vektorizer

## Arkitektur

Projektet består af tre Python-filer med hver sin funktion:

**train.py**

Træner den første version af modellen.
Den genererer et syntetisk datasæt med 100 eksempler på juridiske og ikke-juridiske ansøgninger.
Derefter bruges en CountVectorizer med danske stopord til at omdanne tekst til numeriske vektorer, hvorefter modellen (en Multinomial Naive Bayes) trænes.

Til sidst gemmes både:

model.pkl → den trænede model

vectorizer.pkl → teksttransformeren

Dette sikrer, at fremtidige scripts kan genbruge nøjagtig samme ordforståelse uden at starte forfra.

**update.py**

Muliggør inkrementel træning, så modellen bliver klogere over tid.
Ved at loade den eksisterende model og nye eksempler, udføres partial_fit() på de nye data, så systemet kan opdateres uden at miste tidligere læring.

Dette er især vigtigt i en virkelig kontekst, hvor nye typer CV’er løbende tilføjes.

**predict.py**

Loader både model og vectorizer for at klassificere nye ansøgninger.
Eksempelvis kan følgende sætninger analyseres:

“Jeg har arbejdet som advokat i 5 år med fokus på familieret” → relevant

“Jeg er uddannet sygeplejerske og har arbejdet på hospital” → ikke relevant

## Læringspunkter

Dette projekt er et godt eksempel på praktisk brug af NLP i en virksomhedskontekst.
Jeg lærte blandt andet:

Hvordan tekstdata omdannes til numeriske vektorer (TF-IDF og CountVectorizer)

Hvordan man gemmer og genindlæser modeller med pickle

Hvordan man implementerer inkrementel læring, så en model kan opdateres over tid

Hvordan man evaluerer en model med precision, recall og f1-score

## Videre udvikling

Næste skridt vil være at udvide projektet med semantisk forståelse via embeddings — fx med BERT eller Sentence Transformers — så modellen ikke kun genkender ord, men også betydningen bag dem.

Dette vil gøre det muligt for modellen at forstå, at ordene “jurist” og “advokat” betyder omtrent det samme, selvom de ikke optræder i samme form i træningsdata.

## Reflektion

Projektet kombinerer teori med praksis:
fra simple TF-IDF repræsentationer til idéen om løbende modelopdatering, som er central i moderne ML-systemer.

Selvom modellen bruger en klassisk tilgang, viser den tydeligt hvordan Machine Learning kan implementeres som et effektivt værktøj i rekrutteringsprocesser og tekstfiltrering.