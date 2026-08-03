# Análisis de complejidad del circuito

## ¿Qué problema resuelve?
El tiempo de ejecución (wall-clock) depende del hardware y no transfiere bien entre máquinas. El análisis de [[complejidad]] del circuito —cuántos constraints y de qué tipo— da una medida estructural, hardware-independiente, del costo de probar y verificar.

## Idea intuitiva
Contar constraints es como contar las operaciones de una receta en lugar de cronometrar a un cocinero concreto: cualquier cocinero tardará proporcional a la receta, no al reloj de un día concreto.

## Definición formal
Un circuito aritmético sobre un campo F se expresa como sistema de constraints (R1CS, PLONKish, AIR). Las magnitudes clave:
- **n = número de constraints** (o filas de la tabla de ejecución).
- **Costo del prover**: típicamente O(n log n) en SNARKs universales (dominado por FFTs/MSMs), O(n) lineal en algunos esquemas.
- **Costo del verificador**: O(1) u O(log n) en SNARKs; O(log² n) o O(√n) en STARKs/Bulletproofs respectivamente (véase [[trade-offs]]).
- **Tamaño de prueba**: constante (Groth16: 3 elementos de grupo), O(log n) o polilogarítmico según esquema.
- **Costo por gadget**: p. ej., SHA-256 ≈ 20–30k constraints; Poseidon ≈ 200–300 por hash; verificación de firma EdDSA ≈ 3–5k; range proof de k bits ≈ k constraints.

## Complejidad / garantías / limitaciones
El conteo estático predice bien el comportamiento asintótico, pero constantes ocultas (curva elegida, recursión, lookups) importan en la práctica; por eso complementa, no reemplaza, al [[benchmarking]]. En sistemas con lookups (PLONKish/Halo2), hay que contar también columnas y grados de polinomios.

## Aplicaciones concretas
- Presupuesto de constraints antes de escribir código: "identidad pericial ≤ 10k constraints".
- Decisión de hash in-circuit: Poseidon vs SHA-256 según costo por bit comprometido.
- Detección de regresiones: el CI falla si el conteo de constraints crece >5 % sin justificación.

## Relación con Midnight / Compact (si aplica)
Compact compila a circuitos cuyo tamaño impacta directamente el costo del prover local y los recursos de verificación on-chain; el toolchain expone métricas del circuito compilado que deben citarse en el análisis.

## Relación con mi proyecto (si aplica)
El peritaje exige que la generación de prueba corra en hardware modesto (laptop del perito). Acotar n ≤ ~50k constraints garantiza proving en segundos, lo que se declara formalmente en el informe junto al [[benchmarking]] empírico.

## Referencias
- Groth, "On the Size of Pairing-based Non-interactive Arguments", EUROCRYPT 2016.
- Documentación Halo2 (costo de gadgets): https://zcash.github.io/halo2/
- Zac Williamson, "the mathematics of PLONK": https://hackmd.io/@aztec-network/plonk-arithmetization
