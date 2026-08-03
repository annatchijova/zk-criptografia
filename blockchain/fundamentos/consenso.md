# Panorama de mecanismos de consenso — NOTA CANÓNICA

## ¿Qué problema resuelve?
El consenso es el corazón de toda blockchain: determina cómo nodos que no confían entre sí acuerdan el siguiente estado del ledger, tolerando participantes bizantinos (maliciosos o fallidos). Es la nota canónica del tema: las notas específicas ([[ouroboros]], PoW en Bitcoin) enlazan aquí.

## Idea intuitiva
Hay tres grandes filosofías para decidir "quién propone el próximo bloque":
1. **PoW (Proof of Work)**: quien resuelve primero un acertijo computacional propone. El poder de decisión es proporcional al hashrate.
2. **PoS (Proof of Stake)**: se sortea al proponente con probabilidad proporcional a su participación económica (stake). El poder es proporcional al capital comprometido.
3. **BFT (Byzantine Fault Tolerance)**: votación explícita entre un conjunto conocido de validadores; acuerdo por rondas de mensajes firmados.

## Definición formal
Un protocolo de consenso debe garantizar **seguridad** (safety: no dos nodos honestos confirman historias incompatibles) y **vivacidad** (liveness: la cadena sigue creciendo). En redes parcialmente síncronas, los protocolos BFT clásicos (PBFT, Tendermint, HotStuff) toleran $f < n/3$ bizantinos y dan [[finality]] determinística. Las cadenas tipo Nakamoto (PoW, [[ouroboros]]) dan finalidad probabilística: la probabilidad de reversión decae exponencialmente con las confirmaciones.

## Complejidad / garantías / limitaciones
| Familia | Tolerancia | Finalidad | Costo |
|---|---|---|---|
| PoW (Bitcoin) | <50% hashrate | Probabilística | Energía masiva |
| PoS (Ouroboros, Ethereum) | <50% stake (asumiendo honestidad mayoritaria) | Probabilística o por checkpoints | Capital + riesgo de slashing |
| BFT (Tendermint, HotStuff) | <33% validadores | Determinística | Escalabilidad limitada a ~cientos de validadores |

Limitaciones transversales: ataques de largo alcance en PoS (mitigados con checkpoints), centralización de pools, y dependencia de supuestos de sincronía de red.

## Aplicaciones concretas
- Bitcoin (PoW) como reserva de valor.
- [[cardano]] con [[ouroboros]] (PoS), base de Midnight.
- Cadenas de consorcio judiciales/regulatorias con BFT (Hyperledger Fabric usa variantes BFT).

## Relación con Midnight / Compact (si aplica)
Midnight hereda la seguridad del consenso PoS del ecosistema Cardano; las pruebas ZK generadas en Compact se anclan en un ledger cuya irreversibilidad define cuándo un compromiso de evidencia es judicialmente "inmutable".

## Relación con mi proyecto (si aplica)
La elección de consenso fija el estándar de inmutabilidad que citará el peritaje: bajo Daubert, la tasa de error del método incluye la probabilidad de reorganización de la cadena — cuantificable gracias a [[finality]].

## Referencias
- Lamport et al. *The Byzantine Generals Problem* (1982).
- Castro & Liskov. *Practical Byzantine Fault Tolerance* (1999).
- Kiayias et al. *Ouroboros: A Provably Secure Proof-of-Stake Protocol* (2017). https://eprint.iacr.org/2016/889
