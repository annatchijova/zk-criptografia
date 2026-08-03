# On the Size of Pairing-based Non-interactive Arguments (Groth, 2016)

## ¿Qué problema abierto atacaba?
Los SNARKs anteriores (Pinocchio, GGPR13) eran compactos pero no óptimos: pruebas de 6-8 elementos de grupo y verificación con varias pairings. Para blockchains y credenciales móviles, cada byte y cada milisegundo de verificación on-chain cuenta. La pregunta abierta era cuál era el tamaño mínimo alcanzable de una prueba no interactiva de conocimiento para circuitos aritméticos.

## Idea central (analogía simple)
Pensá en un sello notarial: en vez de adjuntar el expediente entero, el documento lleva un único sello minúsculo que cualquiera coteja en segundos contra un registro público. Groth16 reduce ese "sello" al mínimo físico posible: apenas tres elementos de grupo.

## Innovación técnica concreta
- Pruebas de **3 elementos de grupo** (~128 bytes en BN254), cercanas al óptimo demostrable.
- Verificación con una única ecuación de pairing (pocas pairings en la práctica), muy barata on-chain.
- Construcción sobre R1CS en el generic group model, con costo de prueba competitivo.

## Limitaciones (reconocidas o descubiertas después)
- Requiere **trusted setup por circuito**: cada programa nuevo exige una ceremonia; si la "toxic waste" no se destruye, se pueden falsificar pruebas.
- No es universal ni actualizable: motivó Sonic, PLONK y Marlin.
- Dependencia de curvas pairing-friendly que pueden quedar obsoletas (debilidades conocidas en BN254).

## Impacto histórico (qué sistemas reales lo usan hoy)
Es el SNARK más desplegado en producción: **Zcash Sapling**, Tornado Cash, Filecoin, Semaphore/Worldcoin y la librería gnark. Para un sistema pericial en Midnight, Groth16 es la referencia de eficiencia de verificación, aunque hoy se prefieren esquemas universales.

## Referencias
- Jens Groth. "On the Size of Pairing-based Non-interactive Arguments", EUROCRYPT 2016. ePrint: https://eprint.iacr.org/2016/260 ; DOI: 10.1007/978-3-662-49896-5_11
