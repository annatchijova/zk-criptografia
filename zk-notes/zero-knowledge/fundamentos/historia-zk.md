# Historia de las pruebas de conocimiento cero: de GMR85 a los SNARKs modernos

## ¿Qué problema resuelve?
Entender de dónde vienen las garantías que hoy usamos: por qué una prueba puede convencer sin revelar nada, y cómo pasamos de construcciones teóricas interactivas a sistemas prácticos como los que usa Midnight.

## Idea intuitiva
La idea de "demostrar sin revelar" nació como un concepto de teoría de la complejidad y tardó ~30 años en volverse ingeniería práctica: primero interactividad, luego no interactividad, luego sucintidad, y finalmente eficiencia suficiente para correr en una blockchain.

## Definición formal
Hitos fundamentales:
- **GMR85**: Goldwasser, Micali y Rackoff introducen las pruebas interactivas de conocimiento cero y la noción de "conocimiento cero" vía simulación (ver [[simulation]]).
- **GMW86**: Goldreich, Micali y Wigderson muestran que *todo* lenguaje NP admite una prueba ZK (bajo compromisos), usando 3-coloreo de grafos.
- **Blum86 / BFM88**: pruebas no interactivas de conocimiento cero (NIZK) en el modelo de cadena de referencia común (CRS).
- **Kilian92 / Micali94**: argumentos sucintos interactivos y luego SNARKs vía PCP y el paradigma de Fiat–Shamir.
- **Groth10, Groth16**: SNARKs con pruebas de tamaño constante y verificación en milisegundos.
- **PLONK (2019), Halo2 (2020)**: CRS universal/actualizable y recursión sin setup confiado.

## Complejidad / garantías / limitaciones
Las primeras construcciones eran inabordables computacionalmente; los SNARKs modernos reducen el costo del prover a casi lineal en el tamaño del circuito, a costa de supuestos criptográficos fuertes (pairings, modelos idealizados) y, en algunos casos, setup confiado.

## Aplicaciones concretas
Blockchains de privacidad (Zcash, Midnight), rollups ZK, credenciales anónimas, auditoría sin revelación.

## Relación con Midnight / Compact (si aplica)
Midnight se apoya en la línea PLONK/Halo2: NIZKs sucintos con circuitos escritos en Compact que se compilan a restricciones aritméticas.

## Relación con mi proyecto (si aplica)
Mi circuito ("perito íntegro + evidencia auténtica sin revelar identidades") es un descendiente directo de GMW: una relación NP probada en ZK. Ver [[witness-statement]].

## Referencias
- Goldwasser, Micali, Rackoff, "The Knowledge Complexity of Interactive Proof Systems", STOC 1985 / SIAM J. Comput. 1989.
- Goldreich, Micali, Wigderson, "Proofs that Yield Nothing but their Validity", FOCS 1986 / JACM 1991.
- Blum, Feldman, Micali, "Non-Interactive Zero-Knowledge and Its Applications", STOC 1988.
- Groth, "On the Size of Pairing-based Non-interactive Arguments", EUROCRYPT 2016.
- Gabizon, Williamson, Ciobotaru, "PLONK", ePrint 2019/953.
