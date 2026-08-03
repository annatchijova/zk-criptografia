# Polynomial IOP

## ¿Qué problema resuelve?
[[qap]] comprimió el circuito pero quedó atado a Groth16 (setup por circuito, estructura rígida). Los **Polynomial IOPs** son el paradigma más general: un protocolo interactivo donde el prover envía *oráculos polinomiales* y el verifier hace consultas puntuales. Plonk, Marlin y los STARKs son Polynomial IOPs compilados con compromisos polinomiales: la arquitectura dominante hoy.

## Idea intuitiva
En vez de mandar valores, el prover "promete" polinomios: deja cada polinomio en un sobre cerrado (commitment). El verifier no abre los sobres; pide evaluarlos en puntos que él elige y combina esos valores con desafíos aleatorios para chequear identidades. Si el prover miente, para engañar tendría que adivinar el punto del desafío — probabilidad despreciable por Schwartz–Zippel.

## Definición formal
Un Polynomial IOP para una relación $R$ es un protocolo interactivo donde en cada ronda el prover envía un oráculo $[p_i]$ (un polinomio de grado acotado sobre un dominio multiplicativo $H \subset \mathbb{F}$) y el verifier responde con desafíos aleatorios; al final el verifier hace **consultas de evaluación** $p_i(z_j)$ y acepta según una función de las respuestas. Las identidades de circuito (restricciones de puerta, copia/permutación) se reducen a que el cociente $q(x) = f(x)/Z_H(x)$ sea polinomial, i.e., $f$ se anula en todo $H$. Se compila a argumento no interactivo con un polynomial commitment scheme (KZG, FRI, Inner Product) + Fiat–Shamir.

## Complejidad / garantías / limitaciones
- Separación limpia: la eficiencia depende del commitment (KZG: pruebas constantes con trusted setup universal; FRI: transparente, pruebas $O(\log^2 n)$).
- Soundness estadística/controlada por el campo y los desafíos; flexible: soporta lookups (plookup) y custom gates.
- Limitación: requiere FFTs sobre dominios grandes; costo del prover $O(n \log n)$.

## Aplicaciones concretas
- Plonk/Marlin (Aztec, zkSync), STARKs (StarkNet/StarkEx), Halo2.

## Relación con Midnight / Compact
El backend de Midnight (estilo Plonk/Halo2 sobre BLS12-381) es exactamente un Polynomial IOP compilado: Compact genera las restricciones y el proving system las convierte en oráculos polinomiales con compromisos KZG/Inner-Product.

## Relación con mi proyecto
La prueba de custodia pericial puede dividirse en oráculos separados (identidad del perito, hash de evidencia, registro de acreditación) y chequearse con una sola identidad de cociente — modularidad que simplifica auditar el circuito ante un estándar Daubert.

## Referencias
- Gabizon, Williamson, Ciobotaru, *PLONK: Permutations over Lagrange-bases for Oecumenical Noninteractive Arguments of Knowledge* (2019).
- Ben-Sasson et al., *DEEP-FRI* / *Scalable, Transparent, and Post-Quantum Secure Computational Integrity* (STARK, 2018).
- Bünz, Fisch, Szepieniec, *Transparent SNARKs from DARK Compilers* (2020).
