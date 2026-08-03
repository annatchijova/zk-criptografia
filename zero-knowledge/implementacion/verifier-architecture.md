# Arquitectura de un verifier

## ¿Qué problema resuelve?
La promesa central de los SNARKs es la **asimetría**: probar es costoso, verificar es barato. Esto permite que una cadena de bloques (o un juez, o cualquier auditor) compruebe una afirmación compleja en milisegundos, sin re-ejecutar el cómputo ni conocer los datos privados. Sin esta asimetría, ZK no tendría valor práctico on-chain.

## Idea intuitiva
Verificar un Sudoku resuelto es trivialmente más fácil que resolverlo: basta comprobar filas, columnas y bloques. Los SNARKs llevan esta idea al extremo criptográfico: la prueba es un objeto de tamaño constante o logarítmico cuya comprobación exige solo unas pocas operaciones algebraicas, independientemente del tamaño del cómputo original.

## Definición formal
Para una relación $R(x, w)$ y una prueba $\pi = \text{Prove}(x, w)$, el verificador computa $\text{Verify}(vk, x, \pi) \in \{0,1\}$. En Groth16, la verificación son 3 *pairings* y unas pocas exponenciaciones: tiempo $O(|x|)$ y prueba de ~200 bytes (constante). En Plonk/Halo2, la prueba es de ~1 KB y la verificación es $O(|x| + \log n)$. En STARKs, prueba y verificación son polilogarítmicas ($O(\log^2 n)$) pero con constantes mayores.

## Complejidad / garantías / limitaciones
- **Tiempo de verificación**: milisegundos (Groth16: ~2–5 ms off-chain; ~200k–300k gas on-chain en Ethereum).
- **Garantías**: *soundness* — un prover que no conoce $w$ válido no puede convencer al verificador salvo con probabilidad despreciable; *zero-knowledge* — $\pi$ no revela nada de $w$.
- **Limitaciones**: (1) el verificador necesita la clave de verificación $vk$ correcta (vinculada al circuito y, en Groth16, al trusted setup); (2) verificar no valida la *semántica* del circuito — si el circuito está mal diseñado, prueba cosas falsas; (3) los costos on-chain, aunque bajos, no son nulos.

## Aplicaciones concretas
- Verificación on-chain de pruebas en rollups (zkSync, Polygon zkEVM).
- Credenciales verificables: cualquier tercero verifica sin infraestructura pesada.
- Auditoría judicial: el tribunal verifica la prueba del perito con software abierto y reproducible.

## Relación con Midnight / Compact (si aplica)
En Midnight, la verificación ocurre on-chain como parte de la validación de transacciones con datos protegidos: los nodos verifican la prueba ZK adjunta sin ver los datos. Esto hace viable el cumplimiento (compliance) con privacidad.

## Relación con mi proyecto (si aplica)
El tribunal, la contraparte y el peritaje de parte actúan como verificadores independientes: cualquiera puede re-verificar la prueba del perito, cumpliendo el criterio Daubert de *testabilidad* y *tasa de error conocida* — la verificación es determinística y su fiabilidad es matemática, no discrecional. Ver [[prover-architecture]] y [[finality]].

## Referencias
- Groth, J. *On the Size of Pairing-based Non-interactive Arguments*. https://eprint.iacr.org/2016/260
- Daubert v. Merrell Dow Pharmaceuticals, 509 U.S. 579 (1993).
- Midnight docs. https://docs.midnight.network
