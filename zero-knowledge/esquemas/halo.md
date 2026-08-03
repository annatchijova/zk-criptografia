# Halo

## ¿Qué problema resuelve?
Elimina por completo el trusted setup de los SNARKs sucintos. Hasta Halo (2019), las pruebas de tamaño constante requerían una CRS tóxica ([[groth16]], [[plonk]]-KZG); Halo logra verificación sucinta y **recursión de pruebas** usando solo el supuesto de logaritmo discreto, con transparencia total.

## Idea intuitiva
La idea central es reemplazar el polynomial commitment KZG (que exige setup) por un **inner product argument** estilo [[bulletproofs]], que es transparente pero deja una verificación lineal costosa. El truco: esa verificación cara es en sí misma un circuito, así que se **comprime recursivamente** — una prueba verifica a la anterior y aplaza el trabajo pendiente en una "acumulación" que solo se paga una vez al final de la cadena.

## Definición formal
Sistema de argumento recursivo sobre un ciclo de curvas elípticas amigables para recursión (originalmente Tweedledee/Tweedledum; luego el ciclo Pasta: Pallas/Vesta, curvas de 255 bits sin pairing donde el campo base de una es el campo escalar de la otra). La técnica de *nested amortization* permite que el prover acumule instancias de producto interno abiertas y las cierre con una sola prueba agregada.

## Complejidad / garantías / limitaciones
- Sin trusted setup: parámetros públicos derivables de forma transparente.
- Prueba: O(log n), más grande que KZG pero razonable; verificación amortizada constante en cadenas recursivas.
- Prover: más caro que esquemas con pairing (multiexponenciaciones por ronda de recursión).
- Limitación: sin pairings no hay verificación de un solo pairing; la verificación "desnuda" es O(n), solo se vuelve barata vía recursión/amortización.

## Aplicaciones concretas
Base conceptual de [[halo2]] (Zcash Orchard), del sistema de recursión de Mina (Pickles, sobre kimchi) y de diseños de verificación incremental previos a [[nova]].

## Relación con Midnight / Compact (si aplica)
El stack ZK de Midnight desciende directamente de esta línea: curvas sin pairing con recursión y esquemas tipo Halo2/Plonk. La transparencia evita ceremonies y facilita la auditoría regulatoria del sistema de pruebas.

## Relación con mi proyecto (si aplica)
La recursión permite componer pruebas: una prueba de "cadena de custodia íntegra" que agrega N transferencias de la evidencia sin re-verificar cada una on-chain — patrón útil cuando la evidencia pasa por varias manos periciales.

## Referencias
- Bowe, Grigg, Hopwood (2019). *Halo: Recursive Proof Composition without a Trusted Setup*. IACR ePrint 2019/1021.
- Hopwood et al. *The Pasta Curves (Pallas/Vesta)*, Electric Coin Co. (2020).
