# Midnight Network: Whitepaper y documentación técnica (Input Output Global / Shielded Technologies, 2023-2024)

## ¿Qué problema abierto atacaba?
Las blockchains públicas exponen todos los datos (mala privacidad) y las cadenas de privacidad pura dificultan el cumplimiento regulatorio. El reto: una plataforma de *data protection* que permita a las aplicaciones revelar solo lo necesario — "selective disclosure" — usando pruebas de conocimiento cero, sin sacrificar programabilidad ni auditabilidad.

## Idea central (analogía simple)
Es un expediente judicial sellado con ventanillas: el juez (la cadena) puede verificar por la ventanilla que el perito está acreditado y que la evidencia existe y es auténtica, sin abrir el expediente completo ni exponer a la víctima. La metáfora central de Midnight: datos privados off-chain, pruebas públicas on-chain.

## Innovación técnica concreta
Midnight es una *partner chain* de Cardano (seguridad heredada de Ouroboros) orientada a protección de datos. Componentes clave: **Compact**, lenguaje de contratos basado en TypeScript que compila a circuitos ZK (modelo zk-SNARK, familiar a desarrolladores); separación entre **estado privado** (local, nunca sale del dispositivo) y **estado público** (ledger); transacciones con ZK proofs que demuestran transiciones válidas sin revelar entradas; token NIGHT/DUST para recursos; integración con Kachina (fundamento académico de contratos inteligentes que preservan privacidad, de Kerber, Kiayias, Kohlweiss). La metodología: "rational privacy" — equilibrio configurable entre protección y divulgación.

## Limitaciones (reconocidas o descubiertas después)
Red joven (devnet/testnet 2023-2024, mainnet en curso): herramientas y documentación cambian rápido. Los SNARKs usados heredan cuestiones de setup/circuitos; el costo de pruebas grandes y la gestión del estado privado local (backup, sincronización) son retos de UX. Casos de uso regulatorios aún sin jurisprudencia.

## Impacto histórico (qué sistemas reales lo usan hoy)
Pionero en "ZK regulatorio": identidad verificable, credenciales, cumplimiento con privacidad. Para el hackathon es la plataforma objetivo: un contrato Compact puede verificar la acreditación del perito (credencial ZK) y la posesión de evidencia hasheada (Pedersen/Poseidon) generando un registro auditable que apoya los criterios Daubert de fiabilidad y cadena de custodia sin exponer a las víctimas.

## Referencias
- Midnight Network, documentación oficial: https://docs.midnight.network
- Kerber, Kiayias, Kohlweiss, "Kachina — Foundations of Private Smart Contracts", https://eprint.iacr.org/2020/543
- Input Output, "Midnight: data protection blockchain" (announcement 2023), https://midnight.network
