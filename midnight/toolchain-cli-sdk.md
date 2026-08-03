# Toolchain de Midnight: compilador, CLI y SDK

## ¿Qué problema resuelve?
Construir una DApp en Midnight requiere coordinar tres mundos: el contrato Compact, la generación de pruebas y la aplicación cliente. Cada herramienta del toolchain cubre una capa; confundirlas genera expectativas erróneas en el desarrollo.

## Idea intuitiva
Es un taller con tres máquinas: el **compilador** traduce la receta (Compact) a planos criptográficos; el **CLI** es la caja de herramientas que instala y actualiza las máquinas; el **SDK** (midnight-js) es el tablero de control con el que la aplicación opera todo.

## Definición formal
1. **Compact developer tools / CLI** (`compact`):
   - `compact update` — instala/actualiza versiones del compilador y el lenguaje.
   - `compact compile <archivo.compact> <dir_salida>` — genera claves prover/verifier y el módulo TypeScript en `managed/` ([[circuitos-compact]]).
   - `compact --version` — verificación de instalación.
2. **Compilador Compact/Minokawa** (0.26, lenguaje 0.18): chequeo de tipos y de disclosure, generación de circuitos ZK y del API TS (`Contract`, `Witnesses`, `Circuits`, `Ledger`).
3. **midnight-js (SDK TypeScript, open source, LF Decentralized Trust)**: librerías para conectar la DApp con nodos, indexer y proof server; construir, firmar y enviar transacciones; gestionar estado privado.
4. **Componentes de infraestructura**: proof server (Docker, [[off-chain-computation]]), indexer (consulta de datos on-chain), nodo local o testnet, wallet Lace (extensión de navegador).
5. **Extensión VSCode**: resaltado, chequeo en vivo, snippets.

## Complejidad / garantías / limitaciones
- Requisitos: Node.js 22+, Yarn/pnpm, Docker para el proof server.
- Las versiones importan: `pragma language_version` fija compatibilidad entre contrato, compilador y runtime.
- Toolchain joven: APIs cambian entre releases ([[limitaciones]]).

## Aplicaciones concretas
- Flujo oficial: compilar contrato → implementar witnesses → correr proof server → deploy e interacción vía SDK ([[deployment]]).
- Plantillas: OpenZeppelin midnight-apps, ejemplos oficiales (bboard, counter, ZKLoan).

## Relación con mi proyecto
Presupuesto de ingeniería realista para la investigación: el CLI cubre compilación, el SDK la integración con la wallet del perito; la lógica probatoria vive íntegramente en el contrato Compact y los witnesses TypeScript.

## Referencias
- https://docs.midnight.network/blog/compact-developer-tools
- https://github.com/midnightntwrk/midnight-js
- https://github.com/OpenZeppelin/midnight-apps — development flow
