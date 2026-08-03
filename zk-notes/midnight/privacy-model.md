# Modelo de privacidad de Midnight

## ¿Qué problema resuelve?
Las blockchains públicas exponen todo; las monedas de privacidad total (Monero) chocan con la regulación. Midnight propone "privacidad racional": proteger datos por defecto y permitir divulgación selectiva cuando la ley o el negocio lo exigen.

## Idea intuitiva
Como un expediente judicial bajo secreto: el público ve que existe el expediente, su número y los sellos del juzgado (commitments y pruebas), pero el contenido solo se abre a quien tenga autorización (viewing keys) y solo la parte necesaria (disclose selectivo).

## Definición formal
- **Estado público**: campos `ledger` del contrato, visibles y consensuados on-chain.
- **Estado privado (shielded)**: vive off-chain en el dispositivo del usuario; nunca se transmite a los nodos. Incluye saldos shielded (Zswap, inspirado en Zcash Sapling) y datos de aplicación.
- **Divulgación selectiva**: solo lo envuelto en `disclose()` en el código Compact cruza al mundo público; el compilador lo exige ([[compact-language]]).
- **Primitivas**: SNARKs (Halo2 → BLS12-381), commitments Pedersen, protocolo Kachina (seguridad UC) para probar transiciones de estado sin revelar datos, cuentas de un solo uso (OTA).

### Comparación
- **Zcash**: privacidad de valor (quién paga cuánto), sin contratos inteligentes generales.
- **Aztec**: privacidad programable sobre Ethereum (L2), enfoque DeFi.
- **Aleo**: L1 con lenguaje Leo, modelo similar de registros privados, pero sin separación token/recurso tipo NIGHT/DUST.
- **Midnight**: privacidad de datos de aplicación + diseño orientado a cumplimiento (DUST intransferible, divulgación explícita, partner chain de Cardano).

## Complejidad / garantías / limitaciones
- Garantía: minimización de datos alineada con GDPR/HIPAA; lo privado no sale del usuario.
- Limitación: no anonimato de red (IP/metadatos P2P fuera de alcance); el estado privado exige backups del usuario.

## Aplicaciones concretas
- KYC reutilizable, salud, votación, peritajes judiciales confidenciales.

## Relación con mi proyecto
Encaja con Daubert y con el deber de secreto profesional: el perito prueba identidad y posesión de evidencia; identidades de víctimas y contenido quedan shielded. La corte puede recibir divulgación selectiva posterior si lo ordena. Ver [[arquitectura]].

## Referencias
- https://midnight.network/ — programmable privacy / selective disclosure
- https://midnight.network/night — compliance-friendly resource model
- Kachina: *Kachina — Foundations of Private Smart Contracts* (Kerber et al., IOG, 2021)
