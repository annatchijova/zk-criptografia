# Poseidon: A New Hash Function for Zero-Knowledge Proof Systems (Grassi, Khovratovich, Rechberger, Roy, Schofnegger, 2019)

## ¿Qué problema abierto atacaba?
Dentro de un circuito ZK, los hashes clásicos (SHA-256, Keccak) son carísimos: miles de constraints por hash porque sus operaciones binarias (XOR, rotaciones) no se traducen bien a aritmética de campos finitos. Hashing es ubiqué en ZK (Merkle trees, commitments, nullifiers) y dominaba el costo de las pruebas.

## Idea central (analogía simple)
Es como diseñar una cerradura cuyo idioma nativo es el mismo que el del cerrajero: en vez de traducir un hash binario al "idioma" de los campos finitos, Poseidon habla directamente en sumas, multiplicaciones y potencias sobre el campo, así el circuito lo expresa con pocas restricciones.

## Innovación técnica concreta
Define una permutación esponja sobre campos Fp con la construcción HADES: rondas completas de S-boxes (x^α, típicamente x^5) al inicio y final para seguridad estadística, y rondas parciales en el medio (S-box en un solo elemento de estado) para eficiencia, con una capa lineal MDS. El paper incluye análisis de seguridad contra ataques algebraicos (Gröbner, interpolación) y tablas de parámetros por campo y nivel de seguridad. Resultado: hasta ~8x menos constraints que Pedersen hash y órdenes de magnitud menos que SHA en circuito.

## Limitaciones (reconocidas o descubiertas después)
Criptoanálisis posterior (ataques algebraicos mejorados) forzó revisar parámetros y aumentar rondas en algunas instancias; es una primitiva joven comparada con SHA. Fuera del circuito es más lento que hashes tradicionales en CPU. Surgieron variantes (Poseidon2, Rescue-Prime, Reinforced Concrete) para corregir debilidades y mejorar rendimiento.

## Impacto histórico (qué sistemas reales lo usan hoy)
Estándar de facto en el ecosistema ZK: Filecoin, Loopring, zkSync, Polygon Hermez/iden3, Mina, Semaphore (identidad anónima) y librerías circom/halo2. En Midnight es clave para nullifiers y árboles de Merkle de estado privado. En la investigación: hash de compromisos de evidencia (fotos, hashes de archivos de la víctima) dentro del circuito con bajo costo.

## Referencias
- Grassi, Khovratovich, Rechberger, Roy, Schofnegger. "Poseidon: A New Hash Function for Zero-Knowledge Proof Systems". USENIX Security 2021. https://eprint.iacr.org/2019/458
- Grassi et al., "Poseidon2", https://eprint.iacr.org/2023/323
