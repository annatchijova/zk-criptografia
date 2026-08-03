# Scalable, transparent, and post-quantum secure computational integrity (Ben-Sasson, Bentov, Horesh, Riabzev, 2018)

## ¿Qué problema abierto atacaba?
Antes de 2018, probar integridad computacional a gran escala exigía elegir entre: trusted setup (SNARKs), pruebas gigantes, o vulnerabilidad a computación cuántica. No existía un sistema que fuera simultáneamente escalable, transparente y post-cuántico, apto para uso real.

## Idea central (analogía simple)
Imagina auditar una fábrica entera sin entrar: en vez de revisar cada máquina, tomas las fotos de cada paso del proceso, las "estiras" matemáticamente (interpolación polinomial) y solo muestreas unos pocos puntos al azar. Si cualquier paso fue falsificado, el error se propaga por toda la película y el muestreo lo detecta con alta probabilidad.

## Innovación técnica concreta
Presenta el primer zk-STARK completo: convierte la ejecución en una traza algebraica (AIR, Algebraic Intermediate Representation), la codifica con Reed–Solomon y usa FRI (Fast Reed–Solomon IOP of Proximity) para probar cercanía al código con complejidad cuasi-logarítmica. Solo requiere funciones hash: transparencia total (sin setup) y seguridad post-cuántica. Tiempo de verificación polilogarítmico en el tamaño de la computación.

## Limitaciones (reconocidas o descubiertas después)
Las pruebas son grandes (decenas a cientos de KB), mucho más que un Groth16 (~200 bytes), lo que encarece la verificación on-chain. El prover consume bastante memoria y tiempo en la práctica; la eficiencia real requirió años de ingeniería (Cairo, optimizaciones FRI, recursión).

## Impacto histórico (qué sistemas reales lo usan hoy)
Es la base tecnológica de StarkWare/StarkNet/StarkEx, usados en producción por dYdX (v3), Immutable X, Sorare. La familia STARK/FRI influye en Polygon zkEVM, RISC Zero y el ecosistema de zkVMs. Para un proyecto como Midnight (privacidad + pruebas ZK), STARKs ejemplifican el camino transparente y post-cuántico frente a SNARKs con setup.

## Referencias
- Ben-Sasson, Bentov, Horesh, Riabzev. "Scalable, transparent, and post-quantum secure computational integrity". IACR ePrint 2018/046. https://eprint.iacr.org/2018/046
- Ben-Sasson et al., "Fast Reed-Solomon Interactive Oracle Proofs of Proximity" (FRI), ICALP 2018, https://eprint.iacr.org/2017/1130
