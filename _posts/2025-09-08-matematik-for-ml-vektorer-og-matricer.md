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
