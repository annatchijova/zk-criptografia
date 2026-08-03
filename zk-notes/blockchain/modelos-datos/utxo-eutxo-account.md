# UTXO vs eUTXO vs Modelo de Cuentas

## ¿Qué problema resuelve?
Toda blockchain debe decidir cómo representar el estado ("quién posee qué") y cómo las transacciones lo modifican. La elección afecta paralelismo, privacidad, expresividad de contratos y facilidad de pruebas ([[state-commitments]]).

## Idea intuitiva
- **Cuentas** (Ethereum): como un banco; cada cuenta tiene un balance y un estado mutable, las transacciones lo alteran.
- **UTXO** (Bitcoin): como billetes; cada transacción consume "billetes" previos completos y crea nuevos. No hay balances globales.
- **eUTXO** (Cardano, y base de Midnight): UTXO extendido donde cada salida puede llevar un *datum* arbitrario y un script que valida su gasto, permitiendo contratos expresivos con ejecución local y determinista.

## Definición formal
- UTXO: estado = conjunto de salidas no gastadas $(txid, i, valor, scriptPubKey)$. Una transacción válida consume entradas referenciando salidas existentes, crea salidas, y se exige $\sum entradas \ge \sum salidas$.
- eUTXO: salida = $(valor, datum, validator)$. El validador es una función pura $validator(datum, redeemer, contextoTx) \to bool$, evaluada off-chain al construir la transacción y verificada por los nodos; el contexto incluye la transacción entera, lo que da determinismo.
- Cuentas: función global $estado: address \to (balance, nonce, storage)$; transición secuencial por nonce, con ejecución dentro de la VM global.

## Complejidad / garantías / limitaciones
- UTXO/eUTXO: validación paralelizable y local; determinismo de tarifas; complicado para estado compartido (contención del UTXO). Cuentas: simple para lógica con estado global, pero ejecución secuencial y tarifas impredecibles.
- eUTXO facilita privacidad y ZK: la lógica vive en la transacción, no en un estado global.

## Aplicaciones concretas
- Bitcoin (UTXO), Cardano (eUTXO, Plutus), Ethereum ([[patricia-trie|trie de cuentas]]).

## Relación con Midnight / Compact (si aplica)
Midnight hereda el modelo eUTXO de Cardano y lo combina con ZK: los contratos Compact tienen estado privado (local, del usuario) y estado público on-chain; las transacciones llevan pruebas ([[validity-proofs]]) de que la transición privada fue correcta sin revelar datos.

## Relación con mi proyecto (si aplica)
La credencial del perito y los hashes de evidencia de víctimas se modelan como UTXOs con datos privados: el circuito prueba posesión y validez (firma de la fiscalía, no revocación vía [[sparse-merkle-trees]]) sin publicar la identidad, y el gasto del UTXO genera un nullifier que evita reutilización.

## Referencias
- Chakravarty et al. (2020). *The Extended UTXO Model*, FC/WTSC.
- Nakamoto, S. (2008). *Bitcoin Whitepaper*.
- https://docs.midnight.network (modelo de estado de Midnight).
