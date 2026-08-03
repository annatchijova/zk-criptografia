# Grupos: la estructura mínima que da seguridad a una curva elíptica

## ¿Qué problema resuelve?
La criptografía necesita una operación asimétrica: fácil de aplicar muchas veces (multiplicar un generador por un escalar) pero imposible de "deshacer". La noción de **grupo** formaliza dónde vive esa operación, y el **orden** del grupo y de sus subgrupos determina exactamente cuánta seguridad hay disponible.

## Idea intuitiva
Un grupo es un conjunto con una operación que se comporta bien: siempre puedes combinar elementos, deshacer la operación (inverso) y hay un elemento neutro. La "multiplicación de un punto por $k$" en una curva es sumar $k$ veces; el secreto es $k$, y recuperarlo del resultado es el logaritmo discreto.

## Definición formal
Un **grupo** $(G, \cdot)$ cumple: cerradura, asociatividad, existencia de neutro $e$ y de inversos. Si además conmuta, es **abeliano** (todos los grupos criptográficos relevantes lo son).

- **Orden del grupo**: $|G|$, número de elementos.
- **Orden de un elemento** $g$: el menor $n>0$ con $g^n = e$.
- **Teorema de Lagrange**: el orden de todo **subgrupo** divide a $|G|$; en particular, el orden de cada elemento divide a $|G|$.
- **Grupo cíclico**: generado por un solo elemento, $G = \langle g \rangle$.

La propiedad que hace segura una [[curvas-elipticas|curva elíptica]]: elegimos un subgrupo cíclico de **orden primo grande** $\ell$ donde el **problema del logaritmo discreto** (ECDLP) — dado $P$ y $Q = kP$, hallar $k$ — no tiene algoritmo conocido mejor que los genéricos.

## Complejidad / garantías / limitaciones
En un grupo genérico de orden primo $\ell$, el mejor ataque (Pollard-rho) cuesta $O(\sqrt{\ell})$ operaciones; con $\ell \approx 2^{256}$ se obtienen ~128 bits de seguridad. Limitaciones: subgrupos de orden pequeño habilitan ataques de confinamiento (*small-subgroup attacks*), de ahí el requisito de **cofactor** pequeño y validación de puntos; y Shor rompería el ECDLP con computación cuántica a escala.

## Aplicaciones concretas
Firmas Schnorr/ECDSA, Diffie–Hellman, compromisos de Pedersen $C = vG + rH$ usados en esquemas de divulgación selectiva.

## Relación con Midnight / Compact (si aplica)
Las firmas y compromisos dentro de los circuitos de Compact usan curvas embebidas cuyo grupo de puntos tiene un subgrupo primo grande; las pruebas ZK verifican relaciones $kG$ sin exponer $k$.

## Relación con mi proyecto (si aplica)
La credencial del perito es una firma sobre su clave pública; el circuito prueba conocimiento del escalar/firma válidos sin mostrarlos, anclando la identidad sin revelar datos.

## Referencias
- Shoup, *A Computational Introduction to Number Theory and Algebra*, cap. 6.
- Pollard, "Monte Carlo methods for index computation (mod p)", *Math. Comp.* 32, 1978.
- Shor, "Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer", *SIAM J. Comput.* 26(5), 1997.
