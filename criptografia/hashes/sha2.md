# SHA-2 dentro de circuitos ZK

## ¿Qué problema resuelve?
SHA-2 (SHA-256, SHA-512, etc.) es la familia estándar de funciones hash criptográficas. Resuelve integridad, compromiso de datos y verificación de preimágenes. El problema aparece cuando queremos **probar en un circuito ZK que conocemos una preimagen de un hash SHA-256**: funciona, pero es muy caro en constraints.

## Idea intuitiva
SHA-2 fue diseñado para correr rápido en CPUs: usa operaciones de 32/64 bits como XOR, AND, rotaciones y sumas módulo $2^{32}$. Un circuito ZK, en cambio, opera sobre un campo finito $\mathbb{F}_p$ y cada operación no aritmética (XOR, rotación, carry) debe "simularse" con muchas multiplicaciones del campo. Lo que en hardware es gratis, en el circuito se multiplica en costo.

## Definición formal
SHA-256 es una función hash iterativa tipo Merkle–Damgård con función de compresión basada en una red de Feistel generalizada (estructura Davies–Meyer sobre un estado de 8 palabras de 32 bits, 64 rondas). Mapea mensajes arbitrarios a $\{0,1\}^{256}$ con resistencia a colisiones de ~128 bits. Dentro de un circuito R1CS/plonkish, cada ronda requiere decomponer palabras en bits: XOR, AND, NOT y rotaciones consumen decenas de constraints por bit.

## Complejidad / garantías / limitaciones
- Un solo hash SHA-256 cuesta aproximadamente **25.000–30.000 constraints** en R1CS (vs. ~300 para [[poseidon]]).
- Garantías de seguridad: muy estudiadas desde 2001, sin ataques prácticos.
- Limitación: en pruebas recursivas o con muchas hojas de Merkle, el costo se vuelve prohibitivo para provers en el navegador o dispositivos móviles.

## Aplicaciones concretas
- Compatibilidad con sistemas existentes (certificados TLS, firmas, hashes ya publicados on-chain).
- ZK-email, pruebas de posesión de documentos hasheados con estándares previos.

## Relación con Midnight / Compact (si aplica)
Midnight expone SHA-256 en su librería estándar de Compact para interoperabilidad, pero recomienda hashes ZK-friendly como [[poseidon]] para estructuras internas (árboles de Merkle del estado privado) por costo de proving.

## Relación con mi proyecto (si aplica)
Para probar que el perito posee evidencia de víctimas sin revelarla, conviene commitear la evidencia con [[pedersen-commitments]] o Poseidon en vez de SHA-256, reduciendo el tiempo de prueba; solo usar SHA-2 si la evidencia llega ya hasheada desde un sistema externo.

## Referencias
- NIST FIPS 180-4, *Secure Hash Standard* (2015).
- Grassi et al., *Poseidon: A New Hash Function for Zero-Knowledge Proof Systems* (USENIX Security 2021).
- Documentación de Midnight: https://docs.midnight.network/
