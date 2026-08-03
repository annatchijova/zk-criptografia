# QAP (Quadratic Arithmetic Program)

## ¿Qué problema resuelve?
[[r1cs]] expresa la corrección del circuito como muchas ecuaciones independientes; verificar cada una costaría tiempo lineal. El QAP comprime *todas* las restricciones en **una sola identidad polinomial** verificable en un punto aleatorio. Es el puente algebraico que hace posible Groth16: pruebas de 3 elementos de grupo (≈200 bytes) con verificación constante.

## Idea intuitiva
Interpolación polinomial como "compresión": en vez de $m$ ecuaciones sueltas, construí polinomios cuyas evaluaciones en $m$ puntos distinguibles son exactamente los coeficientes de cada fila del R1CS. Entonces "todas las filas se cumplen" equivale a "un polinomio $h(x)$ existe tal que $p(x) = h(x)\,t(x)$", donde $t$ es el polinomio que se anula en los $m$ puntos. Basta chequear divisibilidad polinomial en un solo punto secreto $\tau$.

## Definición formal
Dado un R1CS con matrices $A, B, C \in \mathbb{F}^{m \times n}$, se eligen puntos distintos $r_1, \dots, r_m$ y se interpolan polinomios $A_j(x), B_j(x), C_j(x)$ (grado $< m$) con $A_j(r_i) = A[i,j]$, etc. Para $z = (1, x, w)$, definí $P(x) = \big(\sum_j z_j A_j(x)\big)\big(\sum_j z_j B_j(x)\big) - \big(\sum_j z_j C_j(x)\big)$ y $t(x) = \prod_{i=1}^m (x - r_i)$. Entonces:
$$z \text{ satisface el R1CS } \iff t(x) \mid P(x),$$
es decir, existe $h(x)$ con $P = h \cdot t$.

## Complejidad / garantías / limitaciones
- Soundness vía Schwartz–Zippel: si la identidad vale en un $\tau$ aleatorio, con probabilidad abrumadora vale como polinomios.
- El prover calcula $h$ (FFT, $O(m \log m)$) y evalúa en $\tau$ "en el exponente" usando el CRS.
- Limitaciones: CRS específico por circuito (trusted setup de Groth16); el grado de $P$ es $\approx 2m$, lo que fija el costo del prover.

## Aplicaciones concretas
- Groth16 en Zcash Sapling y Tornado Cash.
- Librerías: libsnark, arkworks (`relations` → `QAP`), bellman.

## Relación con Midnight / Compact
Midnight usa un backend tipo Plonk/Halo2 (sin QAP con setup por circuito), pero entender QAP explica por qué Plonk reemplazó la interpolación fila-por-fila por un argumento de permutación universal.

## Relación con mi proyecto
Si la investigación exigiera Groth16 (prueba mínima on-chain), la lógica "perito + evidencia" pasaría por R1CS → QAP; el tamaño del circuito definiría el SRS y el gas de verificación.

## Referencias
- Gennaro, Gentry, Parno, Raykova, *Quadratic Span Programs and Succinct NIZKs without PCPs*, EUROCRYPT 2013.
- Groth, *On the Size of Pairing-Based Non-interactive Arguments*, EUROCRYPT 2016.
- arkworks: https://github.com/arkworks-rs/snark
