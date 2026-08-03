# Bulletproofs: Short Proofs for Confidential Transactions and More (Bünz, Bootle, Boneh, Poelstra, Wuille, Maxwell, 2017/2018)

## ¿Qué problema abierto atacaba?
Las transacciones confidenciales (ocultar montos con Pedersen commitments) necesitaban *range proofs* para demostrar que un monto está en un rango válido sin revelarlo. Los range proofs existentes eran enormes (lineales en bits por output) o requerían trusted setup, hinchando la blockchain.

## Idea central (analogía simple)
Es como demostrar que un número está entre 0 y 2ⁿ reescribiendo la pregunta una y otra vez a la mitad del tamaño, como partir una barra de chocolate por la mitad repetidamente: al final queda una porción diminuta que el verificador puede chequear directamente, y toda la cadena de "mitades" garantiza el número original.

## Innovación técnica concreta
Introduce un *inner-product argument* recursivo: probar que dos vectores comprometidos tienen cierto producto interno cuesta log₂(n) elementos de grupo. Sobre eso construye range proofs de tamaño logarítmico (≈672 bytes para 64 bits, y agregables: m outputs cuestan poco más que uno), sin trusted setup, solo con el supuesto de logaritmo discreto. Generaliza a pruebas de circuitos aritméticos arbitrarios.

## Limitaciones (reconocidas o descubiertas después)
La verificación es lineal en el tamaño del circuito (O(n) multiexponentiaciones), más lenta que SNARKs. No hay recursión barata ni verificación sublineal; las pruebas no son sucintas para circuitos grandes. La seguridad depende de ECDLP: no es post-cuántica.

## Impacto histórico (qué sistemas reales lo usan hoy)
Adoptado en producción por Monero (desde 2018, redujo ~80% el tamaño de transacciones), Grin/MimbleWimble, Mina (componentes), Zcash (herramientas relacionadas) y librerías como dalek-bulletproofs. Su inner-product argument inspiró Halo/Halo2 (recursión sin trusted setup), relevante para Midnight/Cardano. En un hackathon: ideal para probar que datos sensibles (montos, edades, conteos de evidencia) caen en rangos válidos sin revelarlos.

## Referencias
- Bünz, Bootle, Boneh, Poelstra, Wuille, Maxwell. "Bulletproofs: Short Proofs for Confidential Transactions and More". IEEE S&P 2018. https://eprint.iacr.org/2017/1066
- DOI: 10.1109/SP.2018.00020
