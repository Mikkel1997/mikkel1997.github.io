---
layout: post
title: "Matematik for Machine Learning: Vektorer og Matricer"
date: 2025-09-08
tags: [Teori, Proces]
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

**Inverse Matricer**


       