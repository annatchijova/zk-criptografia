# Rollups

## ¿Qué problema resuelve?
Las cadenas de capa 1 (L1) tienen throughput limitado y comisiones altas porque cada nodo re-ejecuta cada transacción. Los rollups ejecutan transacciones **fuera de la cadena** pero anclan datos y resultados en la L1, heredando su seguridad sin heredar su cuello de botella.

## Idea intuitiva
Una "sucursal" que procesa miles de operaciones y luego entrega a la casa central (L1) un resumen compacto: el nuevo [[state-commitments|state commitment]] (raíz post-estado) y los datos comprimidos de las transacciones. La L1 no re-ejecuta todo; solo verifica una prueba o deja abierta una ventana para impugnar.

## Definición formal
Un rollup es una máquina de estados cuya transición $S \xrightarrow{batch} S'$ se ejecuta off-chain por un secuenciador, que publica en L1: (a) raíz $R = Commit(S')$, (b) datos del batch en calldata/blobs ([[data-availability]]). La validez de la transición se garantiza por uno de dos mecanismos:
- **Validity proofs**: una prueba ZK/[[validity-proofs]] verificada on-chain en cada batch (ZK-rollup).
- **Fraud proofs**: la raíz se acepta salvo que alguien la impugne dentro de una ventana de disputa ([[optimistic-rollups]]).

## Complejidad / garantías / limitaciones
- Mejora típica: 10–100x en throughput; coste por transacción dominado por la DA en L1.
- Garantía clave: seguridad heredada de L1 (censura y retiros forzados posibles usando los datos on-chain).
- Limitaciones: secuenciador centralizado (riesgo de censura/MEV), latencia de retiro (7 días en optimistic), coste de generación de pruebas (ZK).

## Aplicaciones concretas
- ZK: zkSync, StarkNet, Polygon zkEVM, Scroll.
- Optimistic: Arbitrum, Optimism, Base.

## Relación con Midnight / Compact (si aplica)
Midnight no es un rollup, pero comparte la filosofía: computación privada off-chain con una [[validity-proofs|prueba de validez]] verificada on-chain, generada desde contratos Compact.

## Relación con mi proyecto (si aplica)
El patrón "ejecutar fuera, probar dentro" es el mismo que usa el perito: procesa la evidencia sensible off-chain y solo publica una prueba verificable, sin exponer datos de víctimas — escalabilidad y privacidad con el mismo principio criptográfico.

## Referencias
- Buterin, V. (2021). *An Incomplete Guide to Rollups* (vitalik.eth.limo).
- Documentación zkSync / Arbitrum (sitios oficiales).
- Ben-Sasson et al., *STARK papers* (2018).
