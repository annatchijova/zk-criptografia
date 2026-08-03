# Optimistic Rollups

## ¿Qué problema resuelve?
Escalar la ejecución sin pagar el coste de generar pruebas ZK por cada batch ([[validity-proofs]]), manteniendo seguridad heredada de la L1 mediante un mecanismo de impugnación económica en lugar de demostración criptográfica inmediata.

## Idea intuitiva
El secuenciador publica la nueva raíz de estado "de buena fe" y deposita una fianza. Durante una **ventana de disputa** (típicamente 7 días), cualquier observador puede reclamar que la transición es inválida; entonces un fraud proof resuelve la disputa on-chain. Si nadie impugna, la raíz queda finalizada. Basta **un** verificador honesto vigilando para que el sistema sea seguro.

## Definición formal
El operador publica $(R_{pre}, R_{post}, batch)$ y bond $B$. Un challenger abre disputa afirmando que la transición $R_{pre} \xrightarrow{batch} R_{post}$ es falsa. Mediante un **juego de bisección interactivo**, las partes reducen el desacuerdo a un único paso de ejecución, que se re-ejecuta on-chain contra el [[state-commitments|commitment]] (con pruebas [[merkle-trees|Merkle]]/[[patricia-trie|Patricia]] del estado). El perdedor pierde su fianza (slashing). La seguridad descansa en: datos disponibles en L1 ([[data-availability]]), al menos un honesto, y censura limitada durante la ventana (supuesto de liveness).

## Complejidad / garantías / limitaciones
- Coste on-chain bajo (solo en disputas); sin overhead de proving.
- Finalidad real: 7 días para retiros nativos (los puentes de liquidez lo mitigan).
- Limitaciones: supuesto de 1-de-N honestos y liveness; ataques de censura contra challengers; complejidad de los juegos interactivos.

## Aplicaciones concretas
- Arbitrum (disputa interactiva multi-ronda), Optimism/Base (hacia fault proofs con Cannon), Metis.

## Relación con Midnight / Compact (si aplica)
Midnight elige el camino opuesto: pruebas de validez ZK generadas por el propio usuario (Compact). La comparación es útil: optimistic es "inocente hasta que se pruebe lo contrario"; ZK es "válido solo si se demuestra", más adecuado para privacidad, donde una disputa pública podría exponer datos.

## Relación con mi proyecto (si aplica)
Para evidencia judicial, una ventana de disputa de 7 días y la posibilidad de revelar estado en un fraud proof son inaceptables: el proyecto adopta validity proofs ([[validity-proofs]]) para que la verificación sea inmediata y no requiera exponer datos de víctimas.

## Referencias
- Kalodner et al. (2018). *Arbitrum: Scalable, private smart contracts*, USENIX Security.
- Documentación Optimism (fault proofs / Cannon).
- Buterin, V. (2021). *An Incomplete Guide to Rollups*.
