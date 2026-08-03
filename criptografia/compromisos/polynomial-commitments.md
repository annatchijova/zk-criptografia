# Compromisos polinomiales: el concepto general

## ¿Qué problema resuelve?
Un **polynomial commitment scheme (PCS)** permite comprometer un polinomio $p(X)$ y luego probar evaluaciones $p(z)=y$ de forma sucinta. Es la abstracción que unifica a casi todos los SNARKs modernos: primero se "polinomiza" la computación (arithmetización), luego el PCS garantiza que el prover no puede cambiar el polinomio a mitad de camino.

## Idea intuitiva
Un polinomio de grado $d$ queda determinado por $d+1$ puntos. Comprometer el polinomio es como sellarlo en un sobre: el prover queda atado a un único $p(X)$, y solo puede responder consultas de evaluación **consistentes con ese polinomio**. La prueba de evaluación debe ser mucho más chica que el polinomio y fácil de verificar.

## Definición formal
Un PCS es una tupla de algoritmos $(\mathrm{Setup}, \mathrm{Commit}, \mathrm{Open}, \mathrm{Verify})$:
- $\mathrm{Setup}(1^\lambda, d) \to pp$
- $\mathrm{Commit}(pp, p) \to C$
- $\mathrm{Open}(pp, p, z) \to (y, \pi)$ con $y = p(z)$
- $\mathrm{Verify}(pp, C, z, y, \pi) \to \{\mathrm{accept}, \mathrm{reject}\}$

Propiedades: **binding** (no se puede abrir $C$ a dos polinomios/evaluaciones distintas), opcionalmente **hiding** (C no revela $p$), y **sucintez** ($|C|, |\pi|$ sublineales en $d$). Con una Fiat–Shamir o un protocolo de evaluación interactivo, un PCS + una IOP polinomial = un SNARK.

## Complejidad / garantías / limitaciones
Tres familias dominantes, con trade-offs distintos:

| Esquema | Base | Setup | Tamaño prueba | Verificación |
|---|---|---|---|---|
| [[kzg-commitments]] (KZG) | Pairings, DL | Trusted (SRS) | ~1 elemento (constante) | 2 pairings, constante |
| FRI | Solo hashes ([[poseidon]], [[sha2]]) | Transparente | $O(\log^2 d)$, ~KBs | $O(\log d)$ |
| Pedersen vectorial ([[pedersen-commitments]], Bulletproofs) | DL, sin pairings | Transparente | $O(\log d)$ | $O(d)$ lineal |

- KZG: pruebas mínimas, pero requiere confianza en la ceremonia.
- FRI (STARKs): transparente y plausiblemente post-cuántico, pero pruebas más grandes.
- Pedersen/IPA: transparente, pruebas logarítmicas, verificación cara.

## Aplicaciones concretas
- Plonk/Halo2 → KZG o IPA; STARKs (StarkNet, Polygon Miden) → FRI; Bulletproofs → Pedersen vectorial.
- Data availability (EIP-4844), verifiable computation, databases con integridad.

## Relación con Midnight / Compact (si aplica)
El backend de pruebas de Midnight (estilo Plonk sobre curvas con pairing) corresponde a la columna KZG; entender el PCS ayuda a dimensionar costos de proving y verificación en Compact.

## Relación con mi proyecto (si aplica)
Al documentar la cadena de custodia de evidencias, el PCS heredado de Midnight da pruebas sucintas y verificables por terceros — sustento técnico de la fiabilidad exigida por Daubert (método testeable, revisable, con tasas de error conocidas).

## Referencias
- Kate, Zaverucha, Goldberg, ASIACRYPT 2010. https://www.iacr.org/archive/asiacrypt2010/6477178/6477178.pdf
- Ben-Sasson et al., *FRI* (ePrint 2017/1000). https://eprint.iacr.org/2017/1000
- Bünz et al., *Bulletproofs*, IEEE S&P 2018. https://eprint.iacr.org/2017/1066
- Gabizon et al., *PLONK* (2019). https://eprint.iacr.org/2019/953
