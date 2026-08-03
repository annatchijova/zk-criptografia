# Constraint system

## ¿Qué problema resuelve?
Las computaciones reales (verificar una firma, hashear) no son polinomios ni curvas elípticas. Para probarlas en ZK hay que traducirlas a un lenguaje matemático común: un **constraint system**, un conjunto de ecuaciones cuya satisfacibilidad equivale a "la computación fue correcta". Es la interfaz entre "programa" y "sistema de pruebas".

## Idea intuitiva
Pensá una planilla de cálculo: hay celdas (variables) y reglas que dicen "esta celda debe ser el producto de estas dos". Si alguien te da una asignación de valores a todas las celdas y todas las reglas se cumplen, tenés la garantía de que la computación se hizo correctamente. El prover muestra que *conoce* una asignación satisfactoria (que incluye el testigo privado); el verifier no la ve, solo se convence de que existe.

## Definición formal
Un constraint system sobre un campo finito $\mathbb{F}$ es un par $(C, x)$ donde $C$ es una colección de restricciones algebraicas sobre un vector de variables $w$ (instancia pública + testigo), y una asignación $w$ satisface el sistema si todas las restricciones se anulan en $w$. Define un lenguaje NP: $x \in L \iff \exists w : C(x, w) = 0$. Instancias concretas: [[r1cs]], [[qap]], sistemas de Plonk, AIR de STARKs.

## Complejidad / garantías / limitaciones
- La eficiencia de la prueba depende del **número de restricciones** $n$: el tiempo del prover suele ser $O(n \log n)$ u $O(n)$ operaciones de campo.
- Limitación: solo expresan bien aritmética nativa del campo; operaciones booleanas o de rango cuestan muchas restricciones (ver [[boolean-circuits]]).
- Garantía: la satisfacibilidad es NP-completa, así que *cualquier* declaración verificable eficientemente se puede codificar.

## Aplicaciones concretas
- Zcash/Tornado: probar validez de una nota sin revelarla.
- Rollups: codificar la transición de estado como restricciones.
- Firma del hash de evidencia pericial dentro de un circuito.

## Relación con Midnight / Compact
Midnight compila programas **Compact** a circuitos (restricciones) sobre los que se generan pruebas (campo escalar BLS12-381). Diseñar bien el constraint system define el costo de cada transacción privada.

## Relación con mi proyecto
"Perito autenticado" y "custodia de hashes de evidencia" se expresan como restricciones: conocimiento de la clave secreta del perito + preimágenes de hashes, sin revelarlas.

## Referencias
- Ben-Sasson et al., *SNARKs for C: Verifying Program Executions Succinctly and in Zero Knowledge* (2013).
- Thaler, *Proofs, Arguments, and Zero-Knowledge*, capítulos sobre aritmetización.
- Documentación de Midnight: https://docs.midnight.network/
