# Incrementally Verifiable Computation (IVC)

## ¿Qué problema resuelve?
Una computación que crece por pasos —una cadena de custodia, una blockchain— no puede re-probarse desde cero ante cada eslabón nuevo. IVC lo resuelve: tras cada paso hay una prueba sucinta de que *toda* la ejecución fue correcta, y actualizarla cuesta lo mismo que un solo paso, sin importar el largo acumulado.

## Idea intuitiva
Un sello de cera que se renueva: cada vez que el expediente crece, el custodio produce un nuevo sello que garantiza "todo lo anterior estaba sellado y este paso nuevo es válido". Cualquiera puede verificar el sello vigente en segundos, sin abrir el expediente. IVC es la formalización de esa idea; la [[recursive-proofs|recursión]] y los [[folding-schemes|folding schemes]] son sus dos mecanismos de construcción principales.

## Definición formal
Un esquema IVC para una función de transición $F$ consiste en $(\text{Prove}, \text{Verify})$ tales que, dado un estado público $z_{i-1}$ con prueba $\Pi_{i-1}$ y un testigo $\omega_i$, $\text{Prove}$ emite $z_i = F(z_{i-1}, \omega_i)$ y una prueba $\Pi_i$ con:
- **Incrementalidad**: $|\text{Prove}_i| \approx \text{costo de un paso de } F$ (más un overhead constante), independiente de $i$.
- **Sucintez**: $|\Pi_i|$ y el tiempo de $\text{Verify}$ son $O(1)$ o polilogarítmicos en $i$.
- **Soundness de conocimiento**: un prover que produce $\Pi_n$ válida "conoce" una secuencia $(z_0, \omega_1, \dots, \omega_n)$ consistente desde el génesis.
Construcciones: Valiant (2008) vía recursión de SNARKs; Nova (2022) vía folding de instancias relaxed-[[r1cs]].

## Complejidad / garantías / limitaciones
- Overhead por paso constante pero no trivial (circuito del verificador o del folding, $10^4$–$10^6$ restricciones).
- Limitaciones: requiere ciclos de curvas o folding para eficiencia; el estado $z_i$ debe codificar lo mínimo; el folding exige auditoría final del acumulado.
- Garantía: la corrección se preserva por inducción en $n$.

## Aplicaciones concretas
- Blockchains sucintas (Mina), verificación de cómputo de larga duración, VDFs verificables, rollups con estado incremental.

## Relación con Midnight / Compact
Un contrato Compact puede llevar el estado público $z_i$ on-chain (hash raíz de la cadena) mientras las pruebas IVC se generan off-chain, aprovechando el backend Plonk/Halo2 de Midnight.

## Relación con mi proyecto
**Caso central**: cadena de evidencia de víctimas que crece en el tiempo. Cada acta pericial actualiza la prueba; el tribunal verifica una sola prueba vigente que avala autenticidad y continuidad — sin revelar identidades ni contenidos, trazabilidad auditable compatible con Daubert.

## Referencias
- Valiant, *Incrementally Verifiable Computation or Proofs of Knowledge Imply Time/Space Efficiency*, TCC 2008.
- Kothapalli, Setty, Tzialla, *Nova*, CRYPTO 2022.
- Bitansky, Canetti, Chiesa, Tromer, *Recursive Composition and Bootstrapping for SNARKs and Proof-Carrying Data*, STOC 2013.
