# Halo: Recursive Proof Composition without a Trusted Setup (Bowe, Grigg, Hopwood, 2019)

## ¿Qué problema abierto atacaba?
Componer pruebas de forma recursiva (una prueba que verifica otra prueba) prometía blockchains succinctas y escalabilidad infinita, pero exigía ciclos de curvas pairing caros y/o trusted setup. Además, los esquemas sin setup (tipo Bulletproofs) tenían verificación lineal, imposible de anidar. El problema abierto: **recursión eficiente sin ceremonia de confianza**.

## Idea central (analogía simple)
Imaginá una cadena de notarios donde cada uno certifica la firma del anterior. El problema es que el último notario debe revisar toda la cadena. Halo logra que el costo de revisar toda la historia se "acumule" en una única deuda pendiente que se paga una sola vez al final: la verificación de toda la recursión se colapsa en un chequeo constante.

## Innovación técnica concreta
- **Accumulation scheme**: en cada paso recursivo no se verifica el argumento interno completo (inner product argument, estilo Bulletproofs); su parte costosa se *aplaza* y acumula, amortizándola al final.
- Primer sistema práctico de **recursión sin trusted setup**, usando curvas "ordinarias" (Pallas/Vesta, la pasta curves) en vez de ciclos pairing-friendly costosos.
- Habilita pruebas recursivas de tamaño prácticamente constante.

## Limitaciones (reconocidas o descubiertas después)
- Basado en discrete-log: tamaños de prueba mayores que pairing-SNARKs (escalan logarítmicamente).
- Verificación más cara que Groth16/PLONK-KZG.
- La aritmetización original era tipo R1CS; faltaban compuertas custom, resuelto luego en Halo2.

## Impacto histórico (qué sistemas reales lo usan hoy)
Halo dio origen a **Halo2** (Zcash Orchard, live en mainnet), a las pasta curves usadas por Mina y al paradigma de *accumulation* que culminó en Nova/folding schemes. Es el antepasado directo de los sistemas que permiten a **Midnight** generar pruebas locales sin trusted setup, ideal para un perito que prueba su identidad sin exponer datos.

## Referencias
- Bowe, Grigg, Hopwood. "Halo: Recursive Proof Composition without a Trusted Setup", 2019. ePrint: https://eprint.iacr.org/2019/1021
