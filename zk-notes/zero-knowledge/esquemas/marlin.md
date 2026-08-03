# Marlin

## ¿Qué problema resuelve?
Como [[plonk]], ofrece un SNARK con **setup universal y actualizable**: una sola SRS sirve para todos los circuitos. Se diferencia en el camino algebraico: en lugar de permutaciones de cableado, reduce R1CS a chequeos sobre **sumas de polinomios** mediante un "holographic IOP", obteniendo constantes de prover competitivas.

## Idea intuitiva
"Holografía" significa que el verificador no lee la descripción del circuito completa: recibe solo un *encoding* (commitment) de las matrices R1CS A, B, C y hace consultas puntuales. El prover demuestra, con sumchecks sobre dominios multiplicativos, que existe un testigo w tal que A·w ∘ B·w = C·w, y que las matrices sparse están bien codificadas. La sparsidad es clave: el costo depende del número de entradas no nulas, no del tamaño total de la matriz.

## Definición formal
Preprocessing SNARK para R1CS con dos capas: (1) un *algebraic holographic proof* (AHP) que convierte R1CS en ecuaciones polinómicas vía un protocolo tipo sumcheck no interactivo, y (2) un polynomial commitment (KZG o variantes) que materializa la holografía. Setup universal: la SRS es independiente del circuito; el *indexer* deriva por circuito una proving/verification key a partir del encoding de sus matrices.

## Complejidad / garantías / limitaciones
- Prueba: ~800 bytes–1 KB (constante); verificación: O(1) con pocos pairings, levemente más cara que Groth16.
- Prover: O(n log n); el costo de indexing es una sola vez por circuito.
- Garantías: setup universal/actualizable, knowledge soundness bajo supuestos algebraicos (AGM).
- Limitaciones: requiere trusted setup (aunque universal); en la práctica fue eclipsado por Plonk/Halo2, cuyos custom gates y lookups producen circuitos más chicos para programas reales.

## Aplicaciones concretas
Aleo (conceptualmente, en su línea de SNARKs universales), Marlin de Arkworks como biblioteca académica de referencia, y como fundamento teórico de esquemas posteriores (Fractal, con IOPs de Reed–Solomon transparentes).

## Relación con Midnight / Compact (si aplica)
Midnight eligió la aritmetización PLONKish ([[halo2]]) y no R1CS holográfico; Marlin es útil como contraste conceptual para entender por qué la flexibilidad de gates ganó en producción.

## Relación con mi proyecto (si aplica)
Si se prototipara el circuito pericial directamente en R1CS (circom-style) pero se quisiera evitar un setup por circuito, Marlin es la vía natural; la lección práctica es que los lookups de Plonk suelen importar más que la universalidad.

## Referencias
- Chiesa, Hu, Maller, Mishra, Vesely, Ward (2019). *Marlin: Preprocessing zkSNARKs with Universal and Updatable SRS*. EUROCRYPT 2020; IACR ePrint 2019/1047.
- Chiesa, Ojha, Spooner (2019). *Fractal: Post-quantum and Transparent Recursive Proofs from Holography*. EUROCRYPT 2020.
