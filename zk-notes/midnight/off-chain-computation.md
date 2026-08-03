# Computación off-chain en Midnight

## ¿Qué problema resuelve?
Generar una prueba ZK es costoso (segundos de CPU) y requiere los datos privados. Ninguna de las dos cosas puede hacerse en nodos públicos: sería carísimo y destruiría la privacidad. La solución es ejecutar el circuito localmente y solo enviar la prueba.

## Idea intuitiva
Es como firmar un balance contable: el contador hace todos los cálculos en su oficina con los libros secretos (off-chain) y presenta al registro solo la certificación firmada (la prueba). El registro no recalcula todo: verifica la firma.

## Definición formal
El componente off-chain de una DApp Midnight incluye:
1. **Estado privado**: almacén local (o en nube del usuario) con datos que nunca se transmiten.
2. **Witnesses**: funciones TypeScript que el circuito invoca para leer ese estado privado durante la ejecución de prueba ([[tipos]]).
3. **Proof server**: proceso local (distribuido como imagen Docker) que ejecuta el circuito con la prover key y genera la prueba SNARK.
4. **Lógica arbitraria**: el componente local puede correr cualquier código TypeScript (UI, llamadas a APIs, preprocesamiento) fuera del circuito.

Flujo: la DApp llama al circuito → el runtime ejecuta witnesses → el proof server produce la prueba → se arma y firma la transacción → se envía ([[transaction-lifecycle]]).

## Complejidad / garantías / limitaciones
- Garantía: confidencialidad total de entradas; la red nunca ve los datos.
- Costo: generación de prueba en el cliente (segundos, según tamaño del circuito); verificación on-chain ~6 ms (BLS12-381).
- Limitación: disponibilidad del estado privado depende del usuario; no hay replicación ([[limitaciones]]).
- Seguridad crítica: un witness malicioso o bugueado puede filtrar secretos o producir pruebas inválidas (OpenZeppelin advierte auditar los witnesses propios).

## Aplicaciones concretas
- Scoring crediticio privado (ZKLoan), identidad (DID), peritajes.

## Relación con mi proyecto
La evidencia de víctimas jamás sale del entorno del perito: el witness lee los hashes locales, el proof server genera la prueba de posesión, y solo la prueba + commitment viajan a la red. Esto materializa el "probar sin revelar" del pitch y protege el secreto profesional.

## Referencias
- https://docs.midnight.network/ — guías "run the proof server"
- https://github.com/midnightntwrk/example-zkloan — procesamiento de datos privados
- https://github.com/OpenZeppelin/compact-contracts — advertencias sobre witnesses
