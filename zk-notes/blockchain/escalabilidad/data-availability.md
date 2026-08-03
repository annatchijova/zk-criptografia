# Data Availability (Disponibilidad de Datos)

## ¿Qué problema resuelve?
En [[rollups]] y cadenas modulares, verificar que una transición es válida ([[validity-proofs]]) no basta: si el operador publica la raíz pero **oculta los datos** del batch, nadie puede reconstruir el estado, detectar fraude ni retirar sus fondos. La DA es un problema independiente de la validez de la transición.

## Idea intuitiva
Un notario puede certificar que un documento es auténtico y aun así guardárselo en un cajón. La DA pregunta: "¿puedo descargar los datos si los necesito?" La solución moderna es no descargar todo: con *data availability sampling* (DAS), un cliente ligero pide pequeñas muestras aleatorias; si el bloque fue erasure-coded, que todas las muestras respondan implica con alta probabilidad que el bloque completo es recuperable.

## Definición formal
Sea un bloque de datos $D$ codificado con códigos de borrado (Reed–Solomon) extendido a $2\times$ tamaño, comprometido mediante [[merkle-trees|Merkle roots]] o KZG commitments. Un sampler toma $k$ posiciones uniformemente al azar; si un atacante retiene más del 50% del bloque, la probabilidad de que todas las muestras pasen es $\le 2^{-k}$ (más las pruebas de inclusión contra el commitment). Propiedades: **disponibilidad** (con suficientes samplers honestos, $D$ es recuperable) y vínculo al commitment (los datos muestreados son los comprometidos). Esquemas: calldata on-chain, blobs EIP-4844, validium (comité de DA, supuesto de confianza), capas DA dedicadas.

## Complejidad / garantías / limitaciones
- Coste dominante de un rollup: publicar datos en L1.
- Garantía estadística con $k$ muestras; seguridad depende de suficientes samplers.
- Limitación: si la DA está off-chain (validium), se reintroduce confianza en un comité.

## Aplicaciones concretas
- Ethereum Dencun (EIP-4844, blobs + KZG), Celestia (DAS nativo), EigenDA, Avail.

## Relación con Midnight / Compact (si aplica)
Midnight resuelve una DA selectiva: los datos privados nunca van on-chain; solo commitments y pruebas. Los datos necesarios para reconstruir estado público sí deben estar disponibles; los privados quedan bajo custodia del usuario (daños de "user-side DA").

## Relación con mi proyecto (si aplica)
Distinción judicial clave: la prueba ZK y las raíces van on-chain (verificables por el tribunal), pero la evidencia de víctimas permanece off-chain bajo [[chain-of-custody]] del perito. El diseño debe decidir qué metadatos mínimos requieren DA pública (raíces, timestamps en un [[merkle-mountain-range|MMR]]) para auditabilidad sin exposición.

## Referencias
- Al-Bassam, Sonnino, Buterin (2019). *Fraud and Data Availability Proofs*, FC.
- EIP-4844: *Shard Blob Transactions* (eips.ethereum.org).
- Celestia whitepaper (2019/2023).
