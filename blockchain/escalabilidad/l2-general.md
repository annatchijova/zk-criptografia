# Capas 2 (L2): panorama general

## ¿Qué problema resuelve?
La L1 prioriza seguridad y descentralización a costa de capacidad. Las soluciones L2 mueven ejecución (y a veces datos) fuera de la cadena principal manteniendo un anclaje de seguridad en ella, buscando el triángulo escalabilidad–seguridad–descentralización.

## Idea intuitiva
Distintas formas de "no hacer todo en la cadena": abrir un canal bilateral, correr una cadena hija con anclajes periódicos, o empaquetar transacciones en [[rollups]]. Se diferencian en **qué garantía** ofrece la L1 cuando la L2 falla o miente.

## Definición formal
Familias principales:
1. **State channels**: participantes firmas transiciones off-chain; la L1 solo ve apertura, disputas y cierre (Lightning, Raiden). Seguridad: cualquier parte puede forzar el último estado firmado.
2. **Sidechains**: cadena independiente con su propio consenso y un puente (Polygon PoS). La L1 NO garantiza su seguridad.
3. **Plasma**: cadena hija con raíces [[merkle-trees|Merkle]] ancladas; salidas masivas ante fallos; limitada por el problema de [[data-availability]] y exits complejos.
4. **Rollups**: datos en L1 + [[validity-proofs]] o fraud proofs ([[optimistic-rollups]]); seguridad heredada completa.
5. **Validium**: como ZK-rollup pero con datos off-chain (comité de DA): más barato, menor garantía.

## Complejidad / garantías / limitaciones
- Trade-off central: datos on-chain (seguridad plena, coste alto) vs off-chain (barato, supuestos extra).
- Channels: finalidad instantánea pero liquidez bloqueada y liveness requerida. Sidechains: flexibles pero con confianza en su validador. Rollups: el estándar actual de Ethereum.

## Aplicaciones concretas
- Lightning Network (pagos Bitcoin); Arbitrum/Optimism (optimistic); StarkNet/zkSync (ZK); Immutable X (validium).

## Relación con Midnight / Compact (si aplica)
Midnight es una sidechain/partner chain de Cardano con pruebas ZK nativas: se ubica conceptualmente entre sidechain y rollup, usando [[state-commitments]] y pruebas para privacidad más que para throughput.

## Relación con mi proyecto (si aplica)
El proyecto sigue el patrón L2 mentalmente: el dato sensible vive off-chain (con el perito), la L1/sidechain solo guarda raíces y pruebas — minimiza exposición de datos de víctimas y mantiene verificabilidad judicial.

## Referencias
- Poon & Dryja (2016). *The Bitcoin Lightning Network*.
- Poon & Buterin (2017). *Plasma: Scalable Autonomous Smart Contracts*.
- Buterin, V. (2021). *An Incomplete Guide to Rollups*.
