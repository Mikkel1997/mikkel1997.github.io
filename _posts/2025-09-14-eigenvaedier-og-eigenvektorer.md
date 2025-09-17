---
layout: post
title: "Matematik for Machine Learning: Eigenværdier og Eigenvektorer"
date: 2025-09-14
tags: [Teori, Proces]
categories: [Teori]
---

I dette bonus indlæg vil jeg dykke ned i eigenværdier og egenvektorer.

## Hvad er Eigenværdier og Egenvektorer?
- Ordet eigen kommer fra tysk og betyder "karakteristisk".
- Eigenværdier og eigenvektorer er grundlæggende begreber inden for lineær algebra, som har stor betydning i mange områder af matematik og datalogi, herunder maskinlæring og dataanalyse.

**Visualisering af Eigenværdier og Egenvektorer:**
- Lineære transformationer kan visualiseres som operationer, skalering, rotation eller refleksion af vektorer i et rum.
- For at forstå effekten kan man se på hvordan en figur (fx en firkant) ændrer sig under transformationen.
- Nogle vektorer ændrer kun deres længde (skaleres) og ikke deres retning under transformationen. Disse vektorer kaldes eigenvektorer.
- Eigenværdien er den faktor, hvormed eigenvektoren skaleres. fx hvis en eigenvektor bliver fordoblet i længde, er dens eigenværdi 2.

**Særlige tilfælde:**
- Uniform skalering: Hvis vi skalerer lige meget i alle retninger, er alle vektorer eigenvektorer med samme eigenværdi.
- Rotation 180 grader: Alminde rotationer har ingen eigenvektorer, men ved 180 graders rotation er alle vektorer eigenvektorer med eigenværdi -1.
- Shear + Uniform skalering: I 2D kan shear transformationer have en eller to eigenvektorer afhængigt af transformationens art.

**3D Transformationer:**
- 3D-visualisering: Skalering og Shear fungerer på samme måde som i 2D.
- Rotation i 3D: Rotationer giver en ny fortolkning, hvor der altid er mindst én eigenvektor (rotationsaksen) med en eigenværdi på 1.

## Beregning af Eigenværdier og Eigenvektorer
- Grundformel: For en matrix A og en vektor v, hvis Av = λv, så er λ en eigenværdi og v er en eigenvektor. Vi kan omskrive dette til (A - λI)v = 0, hvor I er identitetsmatricen.
- Vi er ikke interesseret i den trivielle løsning v = 0, så vi skal finde betingelserne for at der findes en ikke-triviel løsning, det vil sige (A - λI) = 0.
- Ligningen ender med at vi skal løse det karakteristiske polynomium: determinanten af (A - λI) = 0. Eller: det(A - λI) = 0.
- Eksempel med vertikal skalering:
	- Matrix A = [[1, 0], [0, 2]]]]
	- Eigenværdier: λ1 = 1, λ2 = 2
	- Eigenvektorer: For λ1 = 1, v1 = [1, 0]; for λ2 = 2, v2 = [0, 1]

- Eksempel med rotation 90 grader:
	- Matrix A = [[0, -1], [1, 0]]
	- Determinanten giver komplekse eigenværdier: λ1 = i, λ2 = -i
	- Dette indikerer at der ikke er reelle eigenvektorer for denne transformation.

- For store matricer (fx 100x100) er det ineffektivt at beregne eigenværdier og eigenvektorer manuelt. I stedet bruger vi computere til at udføre numeriske metoder.
- Det vigtige er at forstå konceptet og anvendelsen af eigenværdier og eigenvektorer i forskellige sammenhænge, men ikke at kunne beregne dem manuelt for store matricer.

**Diagonalisation og eigenbasis:**
- Når en transformation T anvendes på en vektor, kan vi udtrykke vektoren som en lineær kombination af eigenvektorerne.
- Problemet opstår når transformationen sker mange gange, fx hvert sekund i 2 uger, altså 1.2 million trin, hvilket gør det tungt at beregne.
- Vi bruger eigenvektorer til at danne en basis (eigenbasis), hvor transformationen kan repræsenteres som en diagonal matrix, hvilket gør det meget lettere at beregne gentagne anvendelser af transformationen.
- Hvis vi har en diagonal matrix D med eigenværdierne på diagonalen, kan vi nemt beregne D^n ved blot at hæve hver eigenværdi til n-te potens.
- Dette gør det muligt at beregne T^n effektivt ved at bruge eigenbasis og diagonalisation.
- Formlem ser sådan her ud: T = CDC ^-1, hvor C er matrixen med eigenvektorer som kolonner, D er diagonal matrixen med eigenværdier på diagonalen, og C^-1 er den inverse af C.
- For at finde T^n, kan vi bruge: T^n = CD^nC^-1.
- Idéen her er ved at skifte til en basis af eigenvektorer, bliver en lineær transformation, en simpel skalering (diagonal matrix), hvilket gør det meget lettere at arbejde med.

**Eksempel på diagonalisation:**
- Transformation T med matrix A = [[1, 0], [1, 2]]
	- Geometrisk: kombination af en lodret skalering (faktor 2) og en horisontal shear.
- Eigenværdier og eigenvektorer:
	- λ1 = 1, v1 = [1, 0] (x-aksen)
	- λ2 = 2, v2 = [1, 1] (diagonal linje y=x)
- Direkte multiplikation
	- T*(-1,1)^T = (0,2)^T
	- T*(0,2)^T = (2,4)^T
- Beregning af T^2
	- T^2 = [[1, 0], [3, 4]], Påvirking af (-1,1)^T = (2,4)^T giver samme resultat som direkte multiplikation.
- Diagonalisation:
	- C = [[1, 1], [0, 1]], D = [[1, 0], [0, 2]], C^-1 = [[1, -1], [0, 1]]
	- T^2 = CD^2C^-1 = [[1, 0], [3, 4]], hvilket stemmer overens med tidligere beregning.
- Dette viser hvordan diagonalisation gør det muligt at beregne gentagne transformationer effektivt.
- Forståelsen er vigtig selvom i praktisk lader vi computeren gøre arbejdet.

**PageRank**
- Udviklet af Larry Page, medstifter af Google, for at rangere websider baseret på deres vigtighed.
- Modellen repræsenterer internetsider som noder i en graf, hvor links mellem siderne er kanter.
- Vi har en Link Matrix L, hvor hver søjle repræsenterer en side, og værdierne angiver sandsynligheden for at klikke videre til en bestemt side.
- Fx for side A: LA = [0, 1/3, 1/3, 1/3] (siden linker til B, C, D med lige stor sandsynlighed).
- Denne Matrix er en stokastisk matrix, hvor hver søjle summerer til 1.
- Rangvektoren r repræsenterer vigtigheden af hver side, og vi ønsker at finde en stabil tilstand, hvor r = Lr.
- Opdateringsformel: r(i+1) = Lr(i), hvor vi gentager opdateringen indtil konvergens. Vi starter med en jævn fordeling, fx r(0) = [1/4, 1/4, 1/4, 1/4] for 4 sider, og itererer til r konvergerer.
- Når r holder op med at ændre sig, har vi fundet eigenvektoren til L med eigenværdi λ = 1, som repræsenterer den endelige rangering af siderne.


	