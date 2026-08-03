# Manejo de claves en Midnight

## ¿Qué problema resuelve?
En un sistema de privacidad programable conviven dos necesidades opuestas: firmar y probar en secreto, y permitir auditoría cuando corresponde. El esquema de claves define quién puede gastar, quién puede ver y quién puede probar.

## Idea intuitiva
Hay tres "llaves" distintas: la que abre la billetera (gastar), la que genera las pruebas en tu computadora (probar), y una "llave de solo lectura" que podés entregar a un auditor para que mire sin tocar (viewing key).

## Definición formal
1. **Claves de billetera**: derivadas de una seed HD; controlan UTXOs NIGHT y saldos shielded (esquema tipo Zcash Sapling en Zswap: claves de gasto y claves de visualización entrante).
2. **Viewing keys**: autorizan acceso de solo lectura a transacciones shielded — primitiva de cumplimiento para reguladores y auditores.
3. **Claves del sistema de pruebas**: por cada circuito provable, el compilador genera *prover key* (off-chain, la usa el proof server del usuario) y *verifier key* (on-chain, la usan los nodos). No son secretos del usuario: son artefactos del contrato ([[circuitos-compact]]).
4. **DUST**: recurso shielded ligado a las tenencias de NIGHT; no requiere claves separadas pero consume el estado shielded.

## Complejidad / garantías / limitaciones
- Garantía: separación gasto/visión permite auditoría sin riesgo de robo.
- Limitación: la custodia de la seed es responsabilidad del usuario; perder el estado privado (claves + datos locales) es irrecuperable on-chain.
- Las verifier keys son públicas y deterministas a partir del contrato compilado: cualquiera puede verificar.

## Aplicaciones concretas
- Auditorías regulatorias con viewing key.
- Wallets (Lace) que gestionan seed, estado shielded y firma en el navegador.

## Relación con Midnight / Compact
En la DApp, la wallet firma la transacción; el proof server local usa la prover key del contrato y los witnesses con datos privados ([[off-chain-computation]]); el nodo usa la verifier key ([[on-chain-verification]]).

## Relación con mi proyecto
El perito conserva su clave privada y su credencial en su dispositivo (custodia personal, coherente con secreto profesional). El colegio de peritos o el tribunal recibiría una viewing key acotada para verificar sin acceder al contenido de la evidencia de víctimas. Ver [[privacy-model]].

## Referencias
- https://docs.midnight.network/ — wallet y guías de desarrollo
- https://midnight.network/night — auditability y compliance
- Zcash Sapling spec (modelo de claves de referencia para Zswap)
