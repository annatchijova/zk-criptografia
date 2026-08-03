# Mempool

## ¿Qué problema resuelve?
La mempool (memory pool) es la sala de espera donde las transacciones firmadas aguardan ser incluidas en un bloque. Sin ella no habría ordenamiento ni propagación previa a la confirmación, pero su existencia introduce latencia variable y exposición de metadatos antes de la finalidad.

## Idea intuitiva
Enviar una transacción es como entregar un sobre en una ventanilla: queda en una bandeja visible hasta que un empleado (productor de bloques) la toma. Mientras espera, cualquiera puede ver que el sobre existe, su tamaño y su remitente, aunque no necesariamente su contenido.

## Definición formal
Cada nodo mantiene un conjunto local de transacciones válidas pero no confirmadas, propagadas por gossip en la red P2P. Una transacción entra si: (1) la firma es válida, (2) paga la tarifa requerida ([[gas]] o, en Midnight, DUST), (3) no gasta entradas ya gastadas (doble gasto), y (4) en Midnight, la prueba ZK adjunta verifica correctamente. Los productores de bloques seleccionan transacciones, típicamente priorizando por tarifa.

## Complejidad / garantías / limitaciones
- La mempool es local a cada nodo: no hay una mempool global consistente.
- No hay garantía de inclusión ni de tiempo: congestión = espera.
- Limitación clave para ZK: la verificación de la prueba ocurre antes o al entrar al bloque; una prueba costosa de verificar retrasa la propagación.
- Riesgo de front-running/observación: los metadatos de una tx pendiente son visibles.

## Aplicaciones concretas
- Estimar tiempos de confirmación de una DApp.
- Diseñar reintentos y sustitución de transacciones (fee bumping).
- Monitorear congestión antes de enviar pruebas grandes.

## Relación con Midnight / Compact
En Midnight, una transacción con circuito Compact lleva una prueba ZK generada off-chain ([[off-chain-computation]]). La prueba se verifica al validarse la tx ([[on-chain-verification]]), lo que añade tiempo de procesamiento antes de la inclusión. El tamaño de la prueba (~5 KB con BLS12-381) afecta el costo en DUST y el tiempo en mempool.

## Relación con mi proyecto
El perito envía una tx con la prueba de posesión de evidencia sin revelarla. La latencia de mempool es irrelevante para la validez probatoria (Daubert): importa la finalidad ([[transaction-lifecycle]]), no la espera. Documentar timestamps de envío vs. confirmación refuerza la cadena de custodia.

## Referencias
- https://docs.midnight.network/ (sección "How Midnight works")
- Antonopoulos & Wood, *Mastering Ethereum*, cap. 6 (mempool y propagación)
- https://midnight.network/night (modelo DUST para tarifas)
