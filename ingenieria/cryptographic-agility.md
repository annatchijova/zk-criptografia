# Agilidad criptográfica (Cryptographic Agility)

## ¿Qué problema resuelve?
Los esquemas criptográficos caducan: avances cuánticos (Shor), criptoanálisis nuevo o bugs de implementación pueden romper la curva, el hash o el sistema de pruebas elegido hoy. Un sistema rígido queda inutilizable; uno ágil migra sin perder la evidencia ya emitida.

## Idea intuitiva
Diseñar el sistema como un enchufe modular: el algoritmo es una pieza reemplazable, no cemento estructural.

## Definición formal
Propiedad arquitectónica por la cual los parámetros criptográficos (suite de curva, función hash, sistema de pruebas ZK) se identifican mediante **identificadores de versión** en mensajes y pruebas, y el protocolo admite negociación/migración de *suites*. Incluye:
- *Versioning* de commitments y pruebas (cada registro declara con qué suite se generó).
- Plan de **transición post-cuántica**: hashes como SHA-256/Poseidon resisten mejor (Grover solo da raíz cuadrada), pero firmas ECDSA y SNARKs basados en emparejamientos no; candidatos: firmas hash-based (SPHINCS+, XMSS) o esquemas de retículos (Dilithium/ML-DSA, FIPS 204).
- *Hybrid mode*: durante la migración se exigen dos firmas (clásica + PQ).

## Complejidad / garantías / limitaciones
Costo: complejidad de implementación y riesgo de downgrade attacks — un atacante fuerza el uso de la suite vieja y débil. Mitigación: política de *suite mínima aceptada* y fecha de caducidad por versión.

## Aplicaciones concretas
Las credenciales de perito emitidas hoy deben poder revocarse y re-emitirse con otra suite sin alterar el resto del protocolo. Véase [[security-assumptions]].

## Relación con Midnight / Compact
Abstraer las llamadas criptográficas detrás de interfaces; si Midnight migra de sistema de pruebas, el contrato Compact debería requerir cambios acotados.

## Relación con mi proyecto
Bajo Daubert, demostrar un plan de migración prueba diligencia y mantenibilidad del método, no solo que "funciona hoy".

## Referencias
- NIST FIPS 204 (ML-DSA), 2024: https://csrc.nist.gov
- Housley, "Guidelines for Cryptographic Algorithm Agility", RFC 7696.
- Grover, "A Fast Quantum Mechanical Algorithm for Database Search", STOC 1996.
