# Spartan

## ¿Qué problema resuelve?
Ofrece un SNARK para R1CS **sin trusted setup y sin pairings**, apoyándose en el protocolo sumcheck en lugar de en compromisos polinómicos estructurados. Resuelve el problema de la transparencia sin pagar el costo de pruebas gigantes de los [[zkstark]].

## Idea intuitiva
El testigo w y las matrices R1CS se interpretan como extensiones multilineales (polinomios en log n variables). La afirmación "A·w ∘ B·w = C·w" se verifica mediante **sumcheck**: una secuencia de rondas donde el prover envía polinomios de bajo grado y el verificador chequea sumas sobre el hipercubo booleano, reduciendo todo a evaluar los polinomios en un punto aleatorio. Esa evaluación final se cierra con un commitment multilineal (por ejemplo, basado en producto interno o en Hyrax).

## Definición formal
SNARK transparente con tres componentes: (1) una instancia de sumcheck para la suma combinada de las tres restricciones R1CS, (2) un segundo sumcheck para evaluar las matrices sparse A, B, C en el punto desafío (codificación con polinomios de "memory checking"), y (3) un polynomial commitment transparente para el testigo. Compilado no interactivo con Fiat–Shamir en el random oracle model.

## Complejidad / garantías / limitaciones
- Prover: **O(n)** con constantes excelentes — uno de los provers más rápidos de la literatura (Spark maneja matrices sparse en tiempo lineal).
- Tamaño de prueba: O(log² n) en la variante sucinta (~10–100 KB), u O(√n) en la variante NIZK original: **trade-off de tamaño** frente a [[groth16]] (pruebas 100× más grandes).
- Verificación: poli-logarítmica; sin setup, seguridad en discrete log o incluso variantes post-cuánticas con commitments hash-based.
- Limitación: no apto para verificación on-chain barata por tamaño de prueba.

## Aplicaciones concretas
Fundamento de los folding schemes modernos: [[nova]] adopta la maquinaria sumcheck/multilineal de Spartan. También base de Lasso/Jolt (lookup arguments a escala) y de pruebas de ML off-chain.

## Relación con Midnight / Compact (si aplica)
No es el stack de Midnight (Plonkish/Halo2), pero Spartan ilustra el eje de diseño opuesto: transparencia total y prover velocísimo a costa de pruebas grandes — relevante si las pruebas periciales se verifican off-chain entre partes y no on-chain.

## Relación con mi proyecto (si aplica)
Para un hackathon, Spartan vía librerías (microsoft/Spartan, Nova-Scotia) permite probar ejecuciones grandes de análisis forense (p. ej., "corrí este hasheo sobre 1 GB de evidencia") con prover rápido y sin ceremonia, verificando off-chain ante el tribunal.

## Referencias
- Setty (2019). *Spartan: Efficient and general-purpose zkSNARKs without trusted setup*. CRYPTO 2020; IACR ePrint 2019/550.
- Setty, Thaler, Wahby (2023). *Customizable constraint systems for succinct arguments* (ccS), extensión de Spartan.
