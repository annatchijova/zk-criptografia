# Ciclo de vida de una transacción

## ¿Qué problema resuelve?
Entender el ciclo completo de una transacción permite saber en qué momento un hecho queda criptográficamente fijado y es oponible a terceros. Sin claridad sobre finalidad, no se puede afirmar "la evidencia quedó registrada en el instante T".

## Idea intuitiva
Como un trámite notarial: (1) redactás y firmás el documento, (2) lo presentás en ventanilla ([[mempool]]), (3) el escribano lo revisa y lo incluye en el protocolo (bloque), (4) el protocolo se sella y ya no puede alterarse (finalidad).

## Definición formal
Etapas genéricas:
1. **Construcción**: el cliente arma la tx (entradas, salidas, datos). En Midnight, además se genera off-chain la prueba ZK del circuito ([[off-chain-computation]]).
2. **Firma y envío**: la tx firmada se propaga por gossip P2P.
3. **Validación en mempool**: cada nodo verifica firma, fondos/tarifa ([[gas]]/DUST), ausencia de doble gasto y, en Midnight, la prueba ZK ([[on-chain-verification]]).
4. **Inclusión en bloque**: un productor la incluye. Midnight usa consenso tipo Substrate: AURA para producción round-robin y GRANDPA para finalidad asincrónica, con ~6 s por bloque.
5. **Finalidad**: GRANDPA finaliza bloques cuando una supermayoría de validadores los vota; desde ahí la tx es irreversible.

## Complejidad / garantías / limitaciones
- Finalidad probabilística (Bitcoin) vs. determinista (GRANDPA, Ethereum PoS): Midnight hereda finalidad determinista.
- Entre envío y finalidad puede haber segundos a minutos; la tx es visible pero aún reversible.
- Una prueba inválida rechaza la tx antes de inclusión: no queda registro on-chain del intento.

## Aplicaciones concretas
- Diseñar UX que distinga "enviada", "en bloque" y "finalizada".
- Auditoría: el hash de bloque finalizado ancla temporalmente cualquier compromiso.

## Relación con Midnight / Compact
El modelo híbrido UTXO + cuenta ejecuta en un paso atómico la transferencia de valor y la actualización del estado público del contrato Compact, una vez verificada la prueba.

## Relación con mi proyecto
La cadena de custodia de la evidencia de víctimas se ancla en la finalidad: el compromiso (commitment) de la evidencia queda timestamped e inmutable, mientras el contenido permanece privado. En el pitch, mostramos el recorrido: prueba generada → tx → finalidad GRANDPA → certificado verificable por el tribunal.

## Referencias
- https://docs.midnight.network/ — arquitectura y consenso
- https://wiki.polkadot.network/docs/learn-consensus — AURA/GRANDPA
- https://midnight.network/blog/state-of-the-network-september-2025
