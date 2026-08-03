# zk-notes

Knowledge base en español para un proyecto de hackathon sobre **Midnight (ZK)**: probar, sin revelar datos privados, que un perito es quien dice ser y que posee evidencia de víctimas, cumpliendo el estándar **Daubert**.

## Estructura

| Carpeta | Notas | Contenido |
|---|---|---|
| `fundamentos-matematicos/` | 7 | Teoría de números, campos finitos, grupos, curvas elípticas, pairings, ROM |
| `criptografia/hashes/` | 3 | SHA-2, Poseidon, Pedersen hash |
| `criptografia/compromisos/` | 3 | Pedersen, KZG, polynomial commitments |
| `criptografia/firmas/` | 5 | EdDSA, Schnorr, BLS, agregación, VRF |
| `criptografia/protocolos/` | 4 | Fiat-Shamir, MPC, trusted setup, nullifiers |
| `zero-knowledge/fundamentos/` | 9 | Historia, completitud, soundness, ZK, simulación, witness, NP, NIZK |
| `zero-knowledge/sistemas-restricciones/` | 6 | Constraint systems, circuitos, R1CS, QAP, polynomial IOP |
| `zero-knowledge/esquemas/` | 11 | SNARKs, STARKs, Bulletproofs, Groth16, Plonk, Halo/Halo2, Marlin, Spartan, Nova, lookups |
| `zero-knowledge/recursion/` | 3 | Pruebas recursivas, folding, IVC |
| `zero-knowledge/implementacion/` | 3 | Witness generation, prover, verifier |
| `blockchain/fundamentos/` | 5 | Consenso, Ouroboros, Cardano, finalidad |
| `blockchain/modelos-datos/` | 6 | Merkle trees, SMT, MMR, Patricia, UTXO/eUTXO, state commitments |
| `blockchain/escalabilidad/` | 6 | Rollups, L2, validity/fraud proofs, DA, light clients |
| `blockchain/operacion/` | 3 | Mempool, gas, ciclo de vida de transacción |
| `midnight/` | 11 + enlaces | Arquitectura, Compact, tipos, circuitos, privacidad, claves, toolchain, deployment, limitaciones |
| `ingenieria/` | 17 | Threat modeling, supuestos, TCB, agilidad criptográfica, side channels, replay, determinismo, reproducibilidad, verificación formal, testing, fuzzing, benchmarking, complejidad, trade-offs |
| `papers/` | 17 | Una nota por paper fundacional (GMR85 → Midnight whitepaper) |
| `legal/` | 15 | Daubert, Frye, FRE 702, cadena de custodia, evidencia digital, RFC 3161, ISO 27037, NIST, ENFSI, SWGDE, tasa de error |
| `hackathon-notes/` | plantilla | Decisiones tomadas durante el evento |

## Convenciones

- **Estructura fija por nota**: problema → intuición → definición formal → complejidad/garantías/limitaciones → aplicaciones → relación con Midnight/Compact → relación con el proyecto → referencias.
- **Notas canónicas**: temas transversales (Merkle trees, witness, finalidad, trusted setup, nullifiers) viven en una sola carpeta; las demás carpetas los referencian con `[[wikilinks]]` o archivos-enlace cortos.
- Todo el contenido está escrito con palabras propias; cada nota cierra con referencias verificables (papers, RFCs, normas, documentación oficial).
