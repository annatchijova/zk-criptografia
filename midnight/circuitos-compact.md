# Circuitos en Compact: definición y compilación paso a paso

## ¿Qué problema resuelve?
Un circuito es la unidad ejecutable que convierte lógica de negocio en una prueba ZK verificable on-chain. Sin entender cómo se define y compila, no se puede construir ni auditar nada en Midnight.

## Idea intuitiva
Escribir un circuito es como escribir la receta que un notario puede verificar sin ver los ingredientes: el cocinero (el prover, off-chain) la ejecuta con sus ingredientes secretos y entrega un sello (la prueba); cualquiera con la receta pública (verifier key) comprueba el sello.

## Definición formal
Un `circuit` en Compact es una función con tipo de retorno explícito. Es **puro** si no lee/escribe ledger ni llama witnesses; es **impuro** en caso contrario. Solo los circuitos exportados que tocan el ledger público reciben claves de verificación (`ProvableCircuits`). Los datos privados entran como argumentos de circuitos exportados o como retornos de `witness`.

### Paso a paso
1. Instalar devtools: `curl ... compact` y `compact update` (ver [[toolchain-cli-sdk]]).
2. Escribir `contrato.compact` con `pragma language_version`, `ledger`, `witness` y `circuit`.
3. Compilar: `compact compile src/contrato.compact src/managed` — genera:
   - claves prover/verifier por circuito provable,
   - módulo TypeScript con `Contract<PS, W>`, `Circuits`, `Witnesses`, `Ledger`, `pureCircuits`.
4. Implementar los witnesses en TypeScript (`witnesses.ts`) — funciones que devuelven el estado privado.
5. Instanciar: `new Contract(misWitnesses)` y llamar `contract.circuits.miCircuito(args)`.
6. Generar la prueba con el proof server local y enviar la transacción ([[off-chain-computation]], [[on-chain-verification]]).

## Complejidad / garantías / limitaciones
- Compilación segura: errores estáticos por disclosure no declarada y por tipos.
- El tamaño del circuito crece con la lógica (hashes, comparaciones, árboles); impacta tiempo de prueba y costo ([[limitaciones]]).
- Sin recursión ni bucles de cota desconocida.

## Aplicaciones concretas
- Contador (tutorial oficial), bulletin board (bboard), ZKLoan (scoring privado), DID con commitments.

## Relación con mi proyecto
El circuito central: `probarPosesion(sal)` llama a witnesses `credencialPerito()` y `evidenciaHashes()`, verifica firma/matrícula y pertenencia Merkle, y publica `disclose(commitment)`. La compilación produce la verifier key que el tribunal usaría para validar la prueba pericial.

## Referencias
- https://docs.midnight.network/tutorials/counter/smart-contract
- https://docs.midnight.network/compact/reference/compact-reference — pure/impure, exports TS
- https://github.com/midnightntwrk/example-zkloan
