# Arquitectura de Midnight: on-chain vs off-chain

## ¿Qué problema resuelve?
Una blockchain pública replica todo en todos los nodos: si subís datos privados, ya no son privados. Midnight resuelve la tensión entre verificabilidad pública y confidencialidad dividiendo qué se ejecuta y almacena en cada lado.

## Idea intuitiva
La cadena es el acta notarial pública: registra que un hecho ocurrió y que fue verificado, pero no el contenido del expediente. El expediente (datos privados) vive en la computadora del usuario; la cadena solo guarda el sello criptográfico (commitment) y la prueba de que todo se hizo según las reglas.

## Definición formal
Midnight es una blockchain de protección de datos (partner chain de Cardano, basada en Substrate) con tres componentes por contrato (según la referencia de Compact):
1. **Componente replicado on-chain**: el estado público del ledger (`ledger` en Compact), visible y consensuado.
2. **Circuito ZK**: prueba confidencialmente que las transiciones del estado público resultaron de una ejecución válida ([[circuitos-compact]]).
3. **Componente local off-chain**: código arbitrario en TypeScript que maneja el estado privado y provee *witnesses* ([[off-chain-computation]]).

El ledger es dual: UTXO para valor (NIGHT, DUST, tokens shielded vía Zswap) + estado de contratos basado en cuentas, actualizados atómicamente.

## Complejidad / garantías / limitaciones
- Garantía: los datos privados nunca salen del dispositivo del usuario; la red solo ve pruebas y compromisos.
- Garantía: verificación on-chain en ~6 ms por prueba (BLS12-381).
- Limitación: el estado privado no está replicado: si el usuario lo pierde, no hay recuperación on-chain.
- Consenso: AURA (producción) + GRANDPA (finalidad), bloques ~6 s.

## Aplicaciones concretas
- Identidad verificable sin publicar datos personales.
- Finanzas reguladas con auditoría selectiva.
- Registros probatorios privados.

## Relación con mi proyecto
El perito mantiene off-chain su credencial y los hashes de evidencia de víctimas; on-chain solo se publica la prueba de "soy un perito habilitado y poseo evidencia íntegra", cumpliendo Daubert sin exponer a las víctimas. Ver [[privacy-model]].

## Referencias
- https://docs.midnight.network/ — "How Midnight works"
- https://docs.midnight.network/compact/reference/compact-reference — estructura tripartita del contrato
- https://midnight.network/ — dual-state ledger
