---
layout: post
title: "Fra TF-IDF til SBERT"
date: 2025-10-27
tags: [NLP, ML, Python, SBERT, Projekt]
categories: [ML / AI]
---

I dette indlæg præsenterer jeg den nyeste version af mit CV-klassifikationsprojekt — en videreudvikling af den oprindelige TF-IDF-model, som nu anvender **Sentence-BERT (SBERT)** til at forstå meningen bag teksten.

Formålet er det samme: at automatisere vurderingen af jobansøgninger (CV’er) og identificere, hvilke der er relevante for et givent jobopslag — i dette tilfælde et advokatfirma.  
Men hvor den gamle model blot “talte ord”, kan denne nye version **forstå betydning og kontekst**.

---

## Formål

I mange virksomheder gennemgås ansøgninger manuelt for at finde relevante profiler.  
Ved at kombinere **Natural Language Processing (NLP)** og moderne **Transformer-baserede modeller**, kan denne proces automatiseres på en langt mere præcis måde.

Hvor TF-IDF kun kigger på ord som “advokat” eller “kontraktret”, kan SBERT genkende at “juridisk rådgiver” og “jurist” betyder noget lignende – selv uden at ordene matcher direkte.

---

## Teknologier

Projektet er bygget i Python og anvender følgende biblioteker og teknologier:

- **pandas** – håndtering af CSV-data (CV-tekster og labels)  
- **sentence-transformers** – generering af semantiske sætningsembeddings  
- **scikit-learn** – klassifikation (Logistic Regression) og evaluering  
- **joblib** – gemning og genindlæsning af model og encoder  
- **NumPy** – numeriske operationer  

---

## Arkitektur

Projektet består af tre Python-filer i mappen `sbert_version/`, hver med sin funktion:

### **train_sbert.py**
Læser CV-data fra en CSV-fil (`data/training_data.csv`) med to kolonner:  
`tekst` (selve CV’et) og `label` (enten “relevant” eller “ikke relevant”).

SBERT (Sentence-BERT) encoder hver tekst til en vektor, som derefter bruges til at træne en **Logistic Regression**-model.  
Til sidst gemmes:

- `model_sbert.pkl` → den trænede klassifikationsmodel  
- `encoder_sbert.pkl` → den anvendte SBERT-encoder  

Dette gør det muligt at genbruge samme embeddings-struktur i fremtidige forudsigelser.

---

### **update_sbert.py**
Muliggør inkrementel træning.  
Her loades den eksisterende model og nye CV-data fra CSV, hvorefter der trænes videre på de nye eksempler — uden at starte forfra.  
Dette afspejler en realistisk brugssituation, hvor modellen løbende forbedres i takt med nye ansøgninger.

---

### **predict_sbert.py**
Loader model og encoder og anvender dem til at analysere nye ansøgninger.  
Eksempel:

“Jeg har arbejdet som advokat i 5 år med fokus på familieret” → relevant
“Jeg er softwareudvikler med speciale i Python” → ikke relevant
“Jeg er jurist med erfaring i kontraktret” → relevant


Den semantiske forståelse gør, at modellen nu korrekt klassificerer sætninger baseret på **mening frem for ordvalg**.

---

## Læringspunkter

Dette projekt illustrerer overgangen fra klassisk Machine Learning til moderne NLP.  
Jeg har blandt andet lært:

- Hvordan transformer-modeller som **SBERT** kan bruges til tekstklassifikation  
- Hvordan man håndterer embeddings som input i traditionelle ML-modeller  
- Hvordan man træner og gemmer modeller og encodere separat  
- Hvordan semantisk forståelse forbedrer modelpræcision i reelle tekstanvendelser  

---

## Videre udvikling

Næste skridt vil være at:

- Finetune SBERT på et større, mere domænespecifikt datasæt  
- Implementere en webapplikation, hvor man kan uploade et CV og få en relevansscore   
- Integrere modellen med et REST API eller database  

---

## Reflektion

Denne version markerer et vigtigt skridt fra **statistisk tekstmatching** til **semantisk sprogforståelse**.  
Ved at udnytte Sentence-BERT kan systemet analysere CV’er med langt større præcision og kontekstforståelse — et klart skridt mod anvendelig **AI i rekruttering**.

Projektet demonstrerer, hvordan moderne NLP kan skabe reel værdi i HR- og ansøgningsprocesser ved at spare tid, øge præcision og fjerne gentagende manuelt arbejde.
