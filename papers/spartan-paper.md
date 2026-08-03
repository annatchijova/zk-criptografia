# Spartan: Efficient and general-purpose zkSNARKs without trusted setup (Setty, 2019)

## ¿Qué problema abierto atacaba?
Los zkSNARKs más eficientes (Groth16, PLONK-KZG) requerían setup confiable; los transparentes (STARKs, Bulletproofs) pagaban con pruebas grandes o probadores lentos. La pregunta abierta: ¿se puede tener un SNARK **general, transparente (sin trusted setup) y con probador casi tan rápido** como los basados en pairings, apoyándose solo en supuestos estándar?

## Idea central (analogía simple)
Verificar un circuito enorme es como auditar un libro contable con millones de asientos. Spartan convierte el libro entero en **un único número aleatorio** (un punto de un polinomio multivariado) y audita solo ese punto: el protocolo sum-check permite verificar sumas gigantescas chequeando unas pocas evaluaciones al azar, como un auditor que muestrea en vez de revisar todo.

## Innovación técnica concreta
- zkSNARK **transparente** para R1CS combinando: (1) un *sum-check protocol* sobre polinomios multivariados que codifican el R1CS, y (2) un compromiso polinomial basado en **inner product arguments** (solo discrete log, sin pairings ni setup).
- Probadores con tiempo casi lineal; extracción de conocimiento demostrada (argument of knowledge real).
- Código abierto en Rust (microsoft/Spartan), usable sobre curvas estándar (curve25519).

## Limitaciones (reconocidas o descubiertas después)
- Pruebas de tamaño O(√n) o log²(n) según variante: más grandes que Groth16/PLONK.
- Verificación más costosa que los pairing-SNARKs (aunque polylog).
- El paper abrió líneas que lo mejoraron: Spartan con compromisos más eficientes, y su uso como compresor final en Nova (el autor lo combinó en trabajos posteriores).

## Impacto histórico (qué sistemas reales lo usan hoy)
Spartan demostró que "transparente" no implica "lento" y popularizó el dúo sum-check + polynomial commitment que domina los diseños modernos (HyperPlonk, Lasso/Jolt). Es el SNARK que **Nova** usa para comprimir su instancia plegada. En el contexto de evidencia pericial bajo estándar Daubert, la transparencia del setup es un argumento jurídico fuerte: no hay ceremonia ni "toxic waste" que cuestionar.

## Referencias
- Srinath Setty. "Spartan: Efficient and general-purpose zkSNARKs without trusted setup", CRYPTO 2020. ePrint: https://eprint.iacr.org/2019/550 ; DOI: 10.1007/978-3-030-56877-1_24
