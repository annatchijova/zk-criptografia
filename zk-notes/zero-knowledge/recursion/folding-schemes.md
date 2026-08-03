# Folding schemes

## ¿Qué problema resuelve?
La [[recursive-proofs|recursión completa]] exige verificar un SNARK *dentro* del circuito en cada paso: caro (pairings, hash Fiat–Shamir). Los **folding schemes** (Nova, 2021) reemplazan esa verificación por algo más barato: *combinar* dos instancias de la relación en una sola, sin decidir todavía si eran válidas; la decisión se posterga al final. Es la base de Nova, SuperNova, HyperNova y Protogalaxy.

## Idea intuitiva
En vez de "revisar el expediente completo en cada paso", anexá el expediente nuevo al anterior: si ambos eran correctos, el expediente combinado también lo es, y si alguno era falso, el combinado queda corrupto y el auditor final lo detecta. Combinar cuesta una suma de compromisos y una multiplicación escalar — nada de pairings dentro del circuito.

## Definición formal
Nova trabaja con **relaxed R1CS**: instancias $(U, W)$ con $U = (x, u, E)$ y testigo $W = (w, r_E, r_W)$, donde la restricción de [[r1cs]] se relaja a
$$(A z)\circ(B z) = u\,(C z) + E, \qquad z = (W, x, u),$$
con compromisos $\bar{E} = \text{Com}(E; r_E)$, $\bar{W} = \text{Com}(W; r_W)$. Dadas $(U_1, W_1), (U_2, W_2)$, con un desafío aleatorio $r$ se pliega:
$$U = U_1 + r\,U_2, \qquad W = W_1 + r\,W_2,$$
y el término cruzado se absorbe redefiniendo $E$. Soundness: si ambas instancias satisfacen relaxed R1CS, la plegada también; y un prover que produce instancias plegadas válidas "conoce" testigos de las originales. En [[incrementally-verifiable-computation|IVC]], el circuito del paso solo ejecuta este folding en lugar de verificar un SNARK completo.

## Complejidad / garantías / limitaciones
- Circuito de recursión ~10⁴ restricciones (vs. millones con pairings): overhead por paso casi constante.
- Al final se prueban las instancias acumuladas con un SNARK sobre relaxed R1CS.
- Limitaciones: requiere curvas con ciclo (Pasta) para eficiencia; la verificación final necesita toda la instancia acumulada.

## Aplicaciones concretas
- Nova/SuperNova (IVC eficiente), HyperNova (folding + CCS), Protogalaxy (folding de muchas instancias, usado en zkEVMs).

## Relación con Midnight / Compact
Midnight usa Halo2/Plonk; los esquemas de folding tipo Protogalaxy son el camino natural para agregar pruebas de muchas transacciones privadas en una sola.

## Relación con mi proyecto
Cadena de evidencia que crece: cada nueva acta pericial se "pliega" al acumulador; el juez solo verifica el estado final, garantizando continuidad sin exponer datos de víctimas.

## Referencias
- Kothapalli, Setty, Tzialla, *Nova: Recursive Zero-Knowledge Arguments from Folding Schemes*, CRYPTO 2022.
- Kothapalli, Setty, *HyperNova: Recursive Arguments for Customizable Constraint Systems*, CRYPTO 2024.
- Eagen, *Protogalaxy: Efficient ProtoStar-style Folding of Multiple Instances* (2023).
