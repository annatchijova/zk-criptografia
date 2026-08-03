# Deploy de un contrato Compact a testnet

## ¿Qué problema resuelve?
Un contrato compilado localmente no existe para la red hasta que se despliega. El deploy fija on-chain la dirección del contrato y sus claves de verificación, habilitando que cualquiera interactúe y verifique pruebas contra él.

## Idea intuitiva
Es como protocolizar una escritura: ya la redactaste (el `.compact`) y la escribana la certificó (compilación); el deploy la inscribe en el registro público y a partir de ahí produce efectos frente a terceros.

## Definición formal
Pasos (flujo documentado, ej. tutorial counter y plantillas OpenZeppelin):
1. **Prerequisitos**: Node 22+, devtools Compact (`compact update`), Docker con el **proof server** corriendo, wallet Lace con fondos de **testnet** (tDUST del faucet).
2. **Compilar**: `compact compile src/miContrato.compact src/managed` — produce claves y API TS ([[circuitos-compact]]).
3. **Implementar witnesses** en TypeScript y construir el paquete (`npm run build`).
4. **Configurar red**: endpoints del nodo testnet, indexer y proof server en `.env`.
5. **Desplegar**: vía midnight-js ([[toolchain-cli-sdk]]) se crea el estado inicial (constructor del contrato), se genera la prueba de la transacción de deploy y se envía firmada.
6. **Confirmar**: esperar inclusión y finalidad ([[transaction-lifecycle]]); registrar la `contractAddress`.
7. **Interactuar**: llamar circuitos exportados; cada llamada genera su prueba y actualiza el ledger público ([[on-chain-verification]]).

## Complejidad / garantías / limitaciones
- El deploy también es una transacción con prueba: requiere el proof server activo.
- Las verifier keys quedan asociadas al contrato: cualquier tercero puede verificar pruebas futuras de forma independiente.
- Limitación: testnet sufre resets y cambios de versión; los contratos desplegados pueden volverse incompatibles ([[limitaciones]]).

## Aplicaciones concretas
- Publicar el contrato del hackathon y demo en vivo contra testnet con Lace.

## Relación con mi proyecto
La dirección del contrato desplegado es el "sello institucional": el peritaje ZK se ancla a un contrato público y auditable, cuya verifier key el tribunal puede usar sin depender de nosotros — un punto fuerte para el estándar Daubert de revisión por pares.

## Referencias
- https://docs.midnight.network/tutorials/counter/smart-contract
- https://github.com/OpenZeppelin/midnight-apps
- https://docs.midnight.network/how-to/bun-runtime-midnight
