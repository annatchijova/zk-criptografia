# Compromisos KZG (Kate–Zaverucha–Goldberg)

## ¿Qué problema resuelve?
Permite comprometer un **polinomio completo** en un único elemento de grupo de tamaño constante, y luego probar evaluaciones puntuales ("$p(z)=y$") con una prueba también constante. Es la pieza que hace a Plonk y a los modernos SNARKs sucintos posibles (ver [[polynomial-commitments]]).

## Idea intuitiva
Imaginá que codificaste tus datos en un polinomio $p(X)$. Con KZG, el commitment es "el polinomio evaluado en un punto secreto $\tau$, en el exponente": $C = g^{p(\tau)}$. Nadie conoce $\tau$, pero gracias a los **pairings** (emparejamientos bilineales) cualquiera puede comprobar que una evaluación declarada es consistente con $C$, sin aprender nada del polinomio ni poder falsearla.

## Definición formal
Setup: $\mathrm{SRS} = \{g^{\tau^i}\}_{i=0}^{d}$ en un grupo $\mathbb{G}_1$ con pairing $e: \mathbb{G}_1 \times \mathbb{G}_2 \to \mathbb{G}_T$.
- **Commit**: $C = g^{p(\tau)} = \prod_i (g^{\tau^i})^{a_i}$ para $p(X)=\sum a_i X^i$.
- **Open** en $z$: $y = p(z)$; cociente $q(X) = \frac{p(X)-y}{X-z}$; prueba $\pi = g^{q(\tau)}$.
- **Verify**: $e(C / g^y, g) = e(\pi, g^{\tau}/g^z)$, usando $g^\tau, g^z \in \mathbb{G}_2$.

## Complejidad / garantías / limitaciones
- Commitment y prueba: **1–2 elementos de grupo** (~48–96 bytes); verificación: 2 pairings, tiempo constante.
- Garantías: binding bajo $d$-SDH / q-PKE en el modelo AGM; ocultamiento con una máscara (versión hiding).
- **Limitación central: trusted setup.** Quien genere $\tau$ y no lo destruya puede crear pruebas falsas. Se mitiga con ceremonias MPC multi-participante (p. ej. Perpetual Powers of Tau, con decenas de miles de contribuyentes); basta que **un solo** participante sea honesto.
- Requiere curvas con pairing (BN254, BLS12-381).

## Aplicaciones concretas
- **Plonk** y variantes (Plonkish arithmetization): el argumento de circuito se reduce a identidades polinomiales verificadas con KZG.
- Ethereum Danksharding / EIP-4844: blob commitments KZG para disponibilidad de datos.
- Vector commitments y esquemas de stateless clients.

## Relación con Midnight / Compact (si aplica)
Midnight usa un sistema de pruebas estilo Plonk (con compromisos polinomiales) sobre BLS12-381 / curvas compatibles; el SRS de KZG proviene de un setup MPC universal y reutilizable, de modo que los contratos Compact no requieren setup por circuito.

## Relación con mi proyecto (si aplica)
Cada prueba ZK que genera el perito en Midnight ya depende de KZG por debajo: tamaño de prueba constante y verificación barata on-chain — argumentos de fiabilidad y reproducibilidad útiles al defender el método ante un estándar tipo Daubert.

## Referencias
- Kate, Zaverucha, Goldberg, *Constant-Size Commitments to Polynomials and Their Applications*, ASIACRYPT 2010. https://www.iacr.org/archive/asiacrypt2010/6477178/6477178.pdf
- Gabizon, Williamson, Ciobotaru, *PLONK* (2019). https://eprint.iacr.org/2019/953
- EIP-4844 (KZG en Ethereum). https://eips.ethereum.org/EIPS/eip-4844
