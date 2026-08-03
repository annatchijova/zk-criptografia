# Gas y costos de transacción

## ¿Qué problema resuelve?
Todo cómputo on-chain consume recursos de miles de nodos. Sin un precio por unidad de trabajo, la red sería vulnerable a spam y a bucles infinitos. El gas (o su equivalente) convierte el costo computacional en un costo económico medible y cobrable.

## Idea intuitiva
Es el medidor de luz de la red: cada operación consume unidades, y al final del mes (del bloque) se paga la factura. Verificar una prueba ZK es como un electrodoméstico específico con consumo conocido: caro de fabricar (probar off-chain), barato de enchufar (verificar on-chain).

## Definición formal
En Ethereum, cada opcode tiene un costo en gas (EIP-150 y posteriores); el remitente fija `gasLimit` y `gasPrice`, y paga `gasUsado × precio`. Verificar un SNARK on-chain cuesta típicamente 200k–500k gas (pairings sobre curvas elípticas). Midnight usa un modelo dual distinto: NIGHT es el token de capital y gobernanza; DUST es un recurso shielded, intransferible y renovable que se genera automáticamente al mantener NIGHT y se consume al ejecutar transacciones. No se "compra" gas: se gasta DUST regenerable.

## Complejidad / garantías / limitaciones
- Ethereum: costos variables según congestión; verificar pruebas es el postor dominante del costo de apps ZK.
- Midnight: costos predecibles ("batería recargable"); DUST decae si no se usa y no puede transferirse.
- Limitación: el costo crece con el tamaño del circuito y la cantidad de datos públicos ([[limitaciones]]).

## Aplicaciones concretas
- Presupuestar el costo operativo de una DApp con pruebas frecuentes.
- Self-funding DApps: el desarrollador mantiene NIGHT y delega DUST para que los usuarios operen "gratis".

## Relación con Midnight / Compact
Una transacción que invoca un circuito Compact exportado paga DUST proporcional al trabajo de verificación y a las escrituras en el ledger público ([[on-chain-verification]], [[arquitectura]]). El modelo separa el activo financiero del recurso operativo, pensado para cumplimiento regulatorio.

## Relación con mi proyecto
Nuestro peritaje ZK puede autofinanciarse: el estudio pericial mantiene NIGHT, genera DUST y cubre las pruebas de los peritos sin costo por uso. La previsibilidad de costos es un argumento fuerte en el pitch frente a alternativas en Ethereum con gas volátil.

## Referencias
- https://midnight.network/night — modelo NIGHT/DUST
- https://docs.midnight.network/ — tokenomics y resource model
- Ethereum Yellow Paper, Apéndice G (fee schedule)
