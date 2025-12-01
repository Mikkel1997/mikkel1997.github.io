---
layout: post
title: "Analyse af API Performance"
date: 2025-11-26
tags: [Data Science, NLP, NER, SpaCy]
categories: [Data Science]
---

I mit forrige indlæg brugte jeg data-analyse til at dykke ned i mit API's performance.
Den vigtigste konklusion var, at min SBERT-model, selvom den generelt er præcis, kæmper med en "gråzone" af CV'er – typisk fra kandidater i brancher, der tangerer det juridiske, men som ikke er jurister.
Modellen ser nøgleord som "kontrakt" og "GDPR" og bliver i tvivl, selvom kandidatens primære rolle er f.eks. projektleder.

Dette er en klassisk begrænsning ved simple klassifikationsmodeller.
For at løse det, må vi gå et spadestik dybere. Vi skal ikke kun se på, hvilke ord der er i teksten, men også på hvad de betyder i deres kontekst.
I dagens indlæg udforsker jeg, hvordan Named Entity Recognition (NER) kan være nøglen til at opnå denne dybe forståelse.

## Formålet

Målet med dette eksperiment er at validere en hypotese: Kan vi ved at udtrække specifikke entiteter som personnavne, organisationer og stillingsbetegnelser skabe en mere kontekstbevidst og præcis klassifikationsmodel?

I stedet for at bygge en helt ny model fra bunden, vil jeg lave et hurtigt proof-of-concept for at se, hvilken type information en præ-trænet NER-model kan udtrække fra et af mine "gråzone"-CV'er.

## Løsningen: Et Eksperiment med spaCy

Jeg bruger spaCy, et state-of-the-art Python-bibliotek til avanceret NLP. Med en præ-trænet, flersproget model kan jeg hurtigt analysere en tekst og få identificeret navngivne entiteter.

Eksperimentet foregår i min analysis.ipynb-notebook som en direkte forlængelse af den forrige analyse.
Eksempel: Et "Gråzone" CV

Jeg tager udgangspunkt i en fiktiv tekst, der ligner de CV'er, min model fandt udfordrende:

    
# Et CV fra en projektleder med juridisk erfaring
graazone_cv_tekst = """
Ole Nordmann
Projektleder med 10 års erfaring i IT-branchen hos firmaet Tech Solutions A/S. 
Jeg har specialiseret mig i at lede agile udviklingsteams og sikre leverancer til tiden. 
I mit seneste projekt i København var jeg ansvarlig for at udarbejde en række servicekontrakter 
og sikre, at vores produkt var i overensstemmelse med GDPR. Jeg har derfor tæt kendskab til juridisk terminologi.
Min kollega var Mette Nielsen.

Resultatet: Kontekst kommer til live

Ved at køre denne tekst igennem spaCy's NER-model, får jeg et utroligt rigt output. Ikke bare en liste, men en visuel repræsentation af den fundne kontekst:

Modellen identificerer korrekt:

   - PERSON: Ole Nordmann, Mette Nielsen

   - ORG (Organisation): Tech Solutions A/S

   - LOC (Lokation): København

   - MISC (Diverse): GDPR (ofte kategoriseret som diverse af generelle modeller)


## Indsigt: Fra Ord til Betydning

Dette lille eksperiment er en øjenåbner. Hvor min SBERT-model kun ser en "suppe" af ord, ser NER-modellen struktur og betydning.
Den forstår, at "Tech Solutions A/S" er et firma, og at "Ole Nordmann" er en person – ikke bare tilfældige ord.
Denne kontekst er afgørende. Selvom CV'et indeholder ord som "kontrakter" og "juridisk", afslører NER-analysen, at ingen af de identificerede roller eller organisationer er juridiske af natur.
Denne indsigt er nøglen til at rette op på modellens usikkerhed.

## En Datadrevet Køreplan for Fremtiden

Min analyse i forrige indlæg fortalte mig, hvor problemet var. Dette NER-eksperiment fortæller mig, hvordan jeg kan løse det. Den klare køreplan for næste version af modellen er:

   - Feature Engineering: Integrer NER i pre-processing pipelinen. For hvert CV, udtræk alle navngivne entiteter.

   - Opret nye features: Lav nye input-features til modellen, f.eks. en contains_legal_org (True/False) eller primary_role_is_legal (True/False).

   - Gen-træn modellen: Træn den oprindelige klassifikationsmodel igen, men nu med disse nye, kontekst-rige features. Dette vil give modellen den ekstra information, den mangler, for at kunne skelne en projektleder fra en advokat.

 ## Refleksion

Dette afsluttende indlæg fuldender MLOps-cirklen. Vi startede med at bygge en service, indsamlede data om dens performance, analyserede dataen for at finde svagheder, og har nu brugt avancerede NLP-teknikker til at lægge en konkret, datadrevet plan for at udbedre disse svagheder.

Projektet viser, at det at bygge en succesfuld ML-løsning ikke slutter, når den første model er trænet. Det er en kontinuerlig cyklus af deployment, overvågning, analyse og forbedring.
  