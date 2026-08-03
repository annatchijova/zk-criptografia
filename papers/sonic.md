# Sonic: Zero-Knowledge SNARKs from Linear-Size Universal and Updateable Structured Reference Strings (Maller, Bowe, Kohlweiss, Meiklejohn, 2019)

## ¿Qué problema abierto atacaba?
Groth16 y Pinocchio exigían una ceremonia de confianza **por cada circuito**: cambiar una línea del programa invalidaba los parámetros. Para aplicaciones reales (smart contracts, credenciales que evolucionan) esto era inviable. El problema abierto era conseguir un *structured reference string* (SRS) **universal** (sirve para cualquier circuito) y **actualizable** (cualquiera puede reforzar la confianza sin depender de los participantes originales).

## Idea central (analogía simple)
Pensá en una caja fuerte comunitaria: Groth16 obligaba a fundir una llave nueva por cada documento; Sonic propone una llave maestra que cualquier ciudadano puede "re-cifrar" encima. Mientras **al menos uno** de los que participaron en alguna actualización haya sido honesto, la llave es segura.

## Innovación técnica concreta
- SRS **universal y de tamaño lineal**, con actualizaciones continuas: el supuesto de confianza pasa de "todos menos uno en la ceremonia inicial" a "uno honesto en cualquier momento".
- Construcción basada en *polynomial commitments* y una técnica de "grand product" sobre permutaciones, precursora directa de PLONK.
- Modo de "helper" que permite agregar muchas pruebas con verificación batch barata.

## Limitaciones (reconocidas o descubiertas después)
- Costo de prueba mayor que Groth16 (decenas de exponenciaciones por compuerta).
- Verificación individual más cara que Groth16; su ventaja aparece en batch.
- Pronto superado en ergonomía por PLONK y Marlin, que simplificaron el sistema de restricciones.

## Impacto histórico (qué sistemas reales lo usan hoy)
Sonic legitimó los SRS universales y las ceremonias perpetuamente actualizables: la ceremonia "Powers of Tau" de Zcash y las de Aztec siguen ese modelo. Su enfoque de polynomial commitments es el ancestro directo de PLONK, que hoy corre en Aztec, Scroll y, con variantes Halo2/KZG, en el ecosistema alrededor de **Midnight**.

## Referencias
- Maller, Bowe, Kohlweiss, Meiklejohn. "Sonic: Zero-Knowledge SNARKs from Linear-Size Universal and Updateable Structured Reference Strings", ACM CCS 2019. ePrint: https://eprint.iacr.org/2019/099 ; DOI: 10.1145/3319535.3339817
