---
layout: post
title: "Matematik for Machine Learning: Vektorer og Matricer"
date: 2025-09-08
tags: [Teori, Proces]
categories: [Teori]
---

Dette er det første af 4 teori opslag, dette har fokus på vektorer og matricer.

## Vektorer
- En vektor er en matematisk størrelse, der har både en størrelse (magnitude) og en retning. Vektorer kan repræsenteres som pile i et koordinatsystem, hvor længden af pilen angiver størrelsen, og vinklen angiver retningen.
- Grundlæggende operationer med vektorer inkluderer addition, subtraktion og skalar multiplikation.

**Vektoroperationer:**
- Addition: To vektorer kan lægges sammen ved at lægge deres tilsvarende komponenter sammen. Hvis vi har to vektorer A = (a1, a2) og B = (b1, b2), så er deres sum C = A + B = (a1 + b1, a2 + b2).
- Subtraktion: Subtraktion af vektorer udføres ved at trække deres tilsvarende komponenter fra hinanden. For vektorerne A og B er forskellen D = A - B = (a1 - b1, a2 - b2).
- Skalar multiplikation: En vektor kan multipliceres med en skalar (et enkelt tal) ved at gange hver komponent af vektoren med skalarens værdi. For en vektor A og en skalar k er resultatet E = k * A = (k * a1, k * a2).
- Dot produkt: Dot produktet af to vektorer A og B er en skalar værdi, der beregnes som A · B = a1*b1 + a2*b2. Det bruges ofte til at bestemme vinklen mellem to vektorer eller til at projicere en vektor på en anden.
- Kryds produkt: Kryds produktet af to tredimensionelle vektorer A og B resulterer i en ny vektor, der er vinkelret på både A og B. Det beregnes som A × B = (a2*b3 - a3*b2, a3*b1 - a1*b3, a1*b2 - a2*b1).

**Dotproduktets egenskaber:**
- Kommutativitet: A · B = B · A
- Distributivitet: A · (B + C) = A · B + A · C
- Associativitet med skalarer: (kA) · B = k(A · B)

**Vektorens længde:**
- Vektorens længde kan beregnes ved hjælp af Pythagoras' sætning. For en vektor A = (a1, a2) er længden ||A|| = √(a1² + a2²).

**Cosinus reglen:**
- Cosinus reglen bruges til at finde vinklen mellem to vektorer. For v
- Cosinus regnlen er A · B = ||A|| * ||B|| * cos(θ), hvor θ er vinklen mellem vektorerne A og B.
- Hvis vektorere A og B er ortogonale (vinkelrette), er deres dot produkt nul: A · B = 0.
- Cosinus reglen kan også bruges til at finde vinklen mellem to vektorer ved hjælp af deres dot produkt og længder. Formlen er: cos(θ) = (A · B) / (||A|| * ||B||).

**Projektion af en vektor på en anden:**
- Vektor projektionen af A på B er givet ved: vector_proj_B(A) = (A · B / ||B||²) * B. Denne formel giver en ny vektor, der repræsenterer projektionen af A på B.
- Skalar projektionen af A på B er givet ved: scalar_proj_B(A) = A · (B / ||B||). Denne formel giver længden af projektionen af A på B. Hvor meget af A der peger i retning af B.

**Basisvektorer:**
- Basisvektorer er en sæt af vektorer, der kan bruges til at repræsentere alle andre vektorer i et givent rum. I et 2D-rum er de mest almindelige basisvektorer i standardbasis e1 = (1, 0) og e2 = (0, 1). I et 3D-rum er de e1 = (1, 0, 0), e2 = (0, 1, 0) og e3 = (0, 0, 1).
- Hvis basisvektorerne er ortogonale (vinkelrette) og har en længde på 1, kaldes de ortonormale basisvektorer. Ortonormale basisvektorer gør det nemmere at beregne koordinaterne for andre vektorer i rummet.
- Hvis basisvektorerne ikke er ortogonale, skal vi bruge matricer til at konvertere mellem forskellige basisvektorer.
- Lineær uafhængighed: En sæt af vektorer er lineært uafhængige, hvis ingen af dem kan skrives som en lineær kombination af de andre. Dette betyder, at ingen vektor i sættet kan udtrykkes som en sum af de andre vektorer multipliceret med skalarer.

**Datasæt på linje:**
- Punkter i et datasæt, ligger næsten altid på en ret linje, vi kan beskrive dem bedre i et nyt koordinatsystem, hvor den ene akse går langs linjen og den anden er vinkelret på linjen. Dette kaldes Principal Component Analysis (PCA).
- Støj er afstande fra punkterne til linjen, og vi kan minimere denne støj ved at finde den bedste linje, der passer til punkterne. Dette gøres ved at minimere summen af kvadraterne af afstandene fra punkterne til linjen.
- Mere spredning fra linjen betyder mere støj, støj dimension er vigtig, fordi den fortæller os hvor god vores linjetilpasning er.
- Vi kan bruge dot produktet til at finde den bedste linje, der passer til punkterne ved at maksimere variansen langs linjen. Det gør vi ved at definere en vektor, der repræsenterer linjens retning, og derefter finde den vektor, der maksimerer variansen af punkterne langs denne retning.

## Matricer
- En matrix er en rektangulær tabel af tal, symboler eller udtryk, arrangeret i rækker og kolonner. Matricer bruges til at repræsentere og manipulere lineære transformationer og systemer af lineære ligninger.
- Et ekempel ville være at tage denne ligning og skrive den som en matrix:
  - 2a + 3b = 8
  - 10a + 1b = 13			
Dette kan skrives som:
[2  3] [a] = [8]
[10 1] [b]   [13]
Hvor den første matrix er en 2x2 matrix, den anden er en 2x1 matrix (vektor) og den tredje er også en 2x1 matrix (vektor).

**Transformation:**
- Når vi ganger en matrix med en vektor, transformerer vi vektoren ved at anvende den lineære transformation, som matricen repræsenterer. For eksempel, hvis vi ganger matrixen med basisvektoren e1 = (1, 0), får vi (2,10), og hvis vi ganger den med e2 = (0, 1), får vi (3, 1). Dette betyder, at matrixen transformerer basisvektorerne til nye positioner i rummet.

**Matrixoperationer:**
- For en matrix A, en vektor x og en skalar k, gælder følgende operationer:
  - Addition: A + B (hvor A og B har samme dimensioner)
  - Subtraktion: A - B (hvor A og B har samme dimensioner)
  - Skalar multiplikation: k * A
  - Matrix multiplikation: A * B (hvor antallet af kolonner i A er lig med antallet af rækker i B)
Eksempel på matrix multiplikation:
Matixen:[2  3]  med vektoren (3,2) giver: (2*3 + 3*2) = 12
        [10 1]                           (10*3 + 1*2) = 32)

**Særlige matricetyper:**
- Identitetsmatrix: En kvadratisk matrix, hvor alle diagonale elementer er 1, og alle andre elementer er 0. Den fungerer som en neutral element i matrixmultiplikation.
- Nulmatrix: En matrix, hvor alle elementer er 0. Den fungerer som en neutral element i matrixaddition.
- Diagonal matrix (skalering): En kvadratisk matrix, hvor alle ikke-diagonale elementer er 0. Diagonal matricer er nemme at arbejde med, da deres multiplikation og inversion er enklere end for generelle matricer.
- Spejlingsmatrix: En matrix, der reflekterer punkter over en bestemt akse eller plan. For eksempel, spejling over x-aksen i 2D kan repræsenteres ved matricen:
[-1 0]
[0  1]
- Rotationsmatrix: En matrix, der roterer punkter omkring et bestemt punkt eller akse. I 2D kan en rotation med vinkel θ repræsenteres ved matricen: Generel rotation med vinkel θ:
- [cos(θ)  sin(θ)]
- [-sin(θ) cos(θ)]
- Shear (forskydning) matrix: En matrix, der skubber punkter i en bestemt retning, hvilket ændrer formen af objekter)

**Kombination af transformationer:**
- Hvis man anvender en transformation A1 på en vektor x, og derfter en anden transformation A2 så bliver resultatet: x -> A1*x -> A2*(A1*x) = (A2*A1)*x. Dette betyder, at vi kan kombinere flere transformationer ved at gange deres respektive matricer sammen i den rækkefølge, de anvendes.
- Rotation og spejling: Hvis vi først roterer en vektor med en rotationsmatrix R og derefter spejler den med en spejlingsmatrix S, kan vi kombinere disse transformationer ved at gange matricerne sammen: S * R.

**Matrix egenskaber:**
- Som vi kan se på eksemplet med rotation og spejling, er matrix multiplikation ikke kommutativ, hvilket betyder at A * B ikke nødvendigvis er lig med B * A.
- Associativitet: (A * B) * C = A * (B * C)

**Matrixinversen**
- Vi kan isolere vektoren x i ligningen Ax = b ved at gange begge sider med matrixinversen A^(-1), hvilket giver: x = A^(-1) * b. Dette fungerer kun, hvis A er en kvadratisk matrix og har en invers (dvs. dens determinant er ikke nul).
- For at finde matrixinversen kan vi bruge forskellige metoder, såsom Gauss-Jordan elimination eller ved hjælp af adjungering og determinant. For en 2x2 matrix A = [a b; c d], er inversen givet ved: A^(-1) = (1/det(A)) * [d -b; -c a], hvor det(A) = ad - bc.
- Når man bruger elimination, transformerer man i praktisk matricen til identitetsmatricen ved hjælp af rækkeoperationer. De samme rækkeoperationer anvendes på en identitetsmatrix for at finde inversen.
- Hvis en matrix ikke er kvadratisk eller har en determinant på nul, har den ikke en invers. I sådanne tilfælde

**Determinanten og linæer uafhængighed**
- Determinanten af en kvadratisk matrix A, betegnet som det(A), er en skalar værdi, der giver information om matrixens egenskaber. For en 2x2 matrix A = [a b; c d], er determinanten givet ved: det(A) = ad - bc.
- det(a) = 0 betyder at rækkerne eller kolonnerne i matricen er lineært afhængige, hvilket betyder at en række eller kolonne kan skrives som en lineær kombination af de andre. Dette indebærer, at matrixen ikke har en invers, og at systemet af ligninger Ax = b enten har ingen løsning eller uendeligt mange løsninger.
- det(a) ≠ 0 betyder at rækkerne eller kolonnerne i matricen er lineært uafhængige, hvilket betyder at ingen række eller kolonne kan skrives som en lineær kombination af de andre. Dette indebærer, at matrixen har en invers, og at systemet af ligninger Ax = b har en unik løsning for enhver vektor b.
- Dette betyder at man skal sikre at determinanten ikke er nul for at kunne finde en unik løsning til et system af lineære ligninger. Ellers kan du miste information om løsningen.

**Einstein summations konvention**
- Einstein summations konvention er en notation, der bruges til at forenkle udtryk, der involverer summation over indekser i tensorer og matricer. Ifølge denne konvention antages det, at når en indeks optræder to gange i et udtryk (en gang som en øvre indeks og en gang som en nedre indeks), skal der implicit summeres over alle mulige værdier af denne indeks.
- For eksempel, hvis vi har en matrix A med elementer A_ij og en matrix B med elementer B_jk, kan vi skrive matrixmultiplikationen C_ik = Σ_j A_ij * B_jk som C_ik = A_ij * B_jk uden at skrive summationstegnet, fordi j optræder to gange i udtrykket.
- Denne konvention gør det muligt at skrive komplekse matematiske udtryk på en mere kompakt og læsbar måde, hvilket er særligt nyttigt i fysik og ingeniørvidenskab, hvor tensorer ofte bruges til at beskrive fysiske fænomener.
- Denne konvention gør det nemmere i kode, hvor du kun skal køre 3 loops over i, j og k i stedet for at skrive en masse summationer.
- Det er vigtigt at bemærke, at Einstein summations konvention kun gælder for indekser, der optræder to gange i et udtryk. Indekser, der optræder kun én gang, skal ikke summeres over.
- Vi kan regne med matricer der ikke har samme dimensioner, så længe de er kompatible for multiplikation. For eksempel kan en 2x3 matrix ganges med en 3x4 matrix, hvilket resulterer i en 2x4 matrix.

**Dotprodukt med einstein summations konvention**
- Dot produktet af to vektorer A og B kan skrives som A · B = A_i * B_i ved hjælp af Einstein summations konvention, hvor i summeres over alle komponenter af vektorerne.
- For eksempel, hvis A = (a1, a2, a3) og B = (b1, b2, b3), så er dot produktet A · B = a1*b1 + a2*b2 + a3*b3, hvilket kan skrives som A_i * B_i ved at antage summation over i fra 1 til 3.
- Denne notation gør det nemmere at arbejde med dot produkter i højere dimensioner og i mere komplekse matematiske udtryk, især når

**Matricer der skifter basis**
- Når vi har en vektor repræsenteret i en bestemt basis, kan vi ændre basis ved at bruge en transformationsmatrix. Hvis vi har en vektor x repræsenteret i basis B1 og ønsker at repræsentere den i basis B2, kan vi bruge en matrix P, der transformerer koordinaterne fra B1 til B2.
- Hvis P er transformationsmatrixen, og x_B1 er vektoren i basis B1, så kan vi finde vektoren i basis B2 ved at gange P med x_B1: x_B2 = P * x_B1.
- For at finde transformationsmatrixen P, kan vi bruge de nye basisvektorer som kolonner i matricen. Hvis B1 har basisvektorer e1 og e2, og B2 har basisvektorer f1 og f2, så kan vi skrive P som:
- P = [f1 | f2] * [e1 | e2]^(-1). Hvor [e1 | e2]^(-1) er inversen af matrixen dannet af basisvektorerne i B1.
- Når vi har transformationsmatrixen P, kan vi bruge den til at konvertere vektorer mellem de to baser. Dette er især nyttigt i machine learning, hvor data ofte skal repræsenteres i forskellige koordinatsystemer for at lette beregninger og analyser.
- Det er vigtigt at bemærke, at transformationsmatrixen P skal være invertibel for at kunne skifte mellem baser uden tab af information. Hvis P ikke er invertibel, kan vi ikke entydigt konvertere vektorer mellem de to baser.

**Ortogornale matricer**
- En ortogonal matrix er en kvadratisk matrix A, der opfylder betingelsen A^T * A = I, hvor Q^T er transponeret af A, og I er identitetsmatricen. Dette betyder, at rækkerne (eller kolonnerne) i en ortogonal matrix er ortogonale (vinkelrette) og har en længde på 1 (normeret).
- Ortogonale matricer: Bevarer længder og vinkler: Når en vektor x transformeres ved en ortogonal matrix Q, bevares dens længde og vinkler i forhold til andre vektorer. Dette gør ortogonale matricer særligt nyttige i geometriske transformationer som rotationer og spejlinger.

**Gram-Schmidt processen**
- Gram-Schmidt processen er en metode til at omdanne et sæt af lineært uafhængige vektorer til et sæt af ortogonale (eller ortonormale) vektorer.
- For eksempel, hvis vi har to vektorer v1 og v2, kan vi bruge Gram-Schmidt processen til at finde en ortogonal basis:
  - Start med den første vektor: u1 = v1
  - For den anden vektor, fjern komponenten i retning af u1: u2 = v2 - (proj_u1(v2)), hvor proj_u1(v2) er projektionen af v2 på u1. 
- Normaliser vektorerne (hvis nødvendigt) for at få en ortonormal basis:
  - e1 = u1 / ||u1||
  - e2 = u2 / ||u2||


       