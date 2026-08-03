# Light Clients (Clientes Ligeros)

## ¿Qué problema resuelve?
Verificar la cadena de bloques completa exige descargar y procesar todo. Un cliente ligero (móvil, navegador, contrato de otro chain, o un tribunal auditor) necesita **confiar en afirmaciones concretas** —"esta transacción se incluyó", "este balance existe"— con una fracción mínima de datos y cómputo.

## Idea intuitiva
En lugar de leer todo el libro contable, el cliente sigue solo las cabeceras (la "portada" de cada bloque) y pide un proof [[merkle-trees|Merkle]]/[[patricia-trie|Patricia]] contra la raíz de estado en la cabecera. En PoS, un *sync committee* firma las cabeceras, reduciendo la confianza a verificar firmas de un comité rotativo.

## Definición formal
Un light client mantiene: (a) una cabecera de confianza inicial (checkpoint), (b) actualizaciones de cabeceras validadas por consenso (PoW: mayor trabajo acumulado; PoS: firmas de $>2/3$ o del sync committee, como en el protocolo Altair de Ethereum). Para verificar un hecho $x$, el servidor full node devuelve $(x, \pi)$ y el cliente comprueba $Verify(\text{raíz en cabecera}, x, \pi)$ usando el [[state-commitments|state commitment]]. La seguridad es la misma que la del consenso, salvo el supuesto de conectividad honesta durante la sincronización (weak subjectivity en PoS).

## Complejidad / garantías / limitaciones
- Datos: cabeceras (~KB por época) + pruebas $O(\log n)$.
- Limitación clásica: un light client no puede detectar retención de datos ([[data-availability]]); DAS lo corrige (Celestia).
- FlyClient: con [[merkle-mountain-range|MMRs]] basta muestrear $O(\log n)$ cabeceras para PoW.

## Aplicaciones concretas
- SPV de Bitcoin (whitepaper §8), Helios (light client PoS de Ethereum), IBC (light clients on-chain entre cadenas Cosmos), bridges.

## Relación con Midnight / Compact (si aplica)
La analogía es directa con ZK: una [[validity-proofs|validity proof]] es el "cliente ligero extremo" — no verifica una muestra sino la validez completa del cómputo con coste constante. Midnight permite que cualquier parte (un juez) sea un verificador ultraligero de hechos privados.

## Relación con mi proyecto (si aplica)
El tribunal actúa como light client del expediente: no recibe la evidencia, solo las raíces ancladas y las pruebas del perito. La verificación es barata, reproducible y no exige custodia de datos sensibles — principio de minimización de datos con garantía criptográfica, alineado con los factores Daubert de testabilidad y revisión por pares.

## Referencias
- Nakamoto (2008), Bitcoin whitepaper §8.
- Bünz et al. (2020). *FlyClient: Super-Light Clients for Cryptocurrencies*, IEEE S&P.
- Ethereum Consensus Specs: Altair Light Client (github.com/ethereum/consensus-specs).
