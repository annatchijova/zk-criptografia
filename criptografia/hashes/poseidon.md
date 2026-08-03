# Poseidon: hash ZK-friendly

## ¿Qué problema resuelve?
Los hashes clásicos como [[sha2]] son prohibitivamente caros dentro de circuitos ZK. Poseidon resuelve el trade-off: ofrecer seguridad comparable minimizando el número de constraints, de modo que árboles de Merkle y compromisos internos sean económicos de probar.

## Idea intuitiva
En un circuito ZK, la multiplicación en el campo $\mathbb{F}_p$ es la operación barata; los bits son caros. Poseidon invierte el diseño clásico: en lugar de XORs y rotaciones, usa una esponja sobre elementos del campo, con S-boxes $x \mapsto x^\alpha$ (típicamente $\alpha = 5$) y capas lineales MDS. Casi todo el costo del circuito son pocas exponenciaciones pequeñas.

## Definición formal
Poseidon es una construcción tipo **esponja** sobre $\mathbb{F}_p$ con estado de $t$ elementos, $R = R_F + R_P$ rondas: rondas completas (S-box a todo el estado) al inicio y al final, y rondas parciales (S-box a un solo elemento) en el medio. Cada ronda: suma de constantes (AddRoundConstants), S-box $x^\alpha$, y multiplicación por una matriz MDS. Seguridad objetivo: ~128 bits contra ataques algebraicos (Gröbner, interpolación, CICO), con márgenes conservadores.

## Complejidad / garantías / limitaciones
- Costo típico: ~**200–300 constraints** por permutación (estado $t=3$), unas **100 veces menos** que SHA-256 en circuito.
- Garantías: análisis público desde 2019 (USENIX Security 2021); sin ataques prácticos con los parámetros recomendados.
- Limitaciones: más joven que SHA-2; la seguridad depende de la elección de $p$, $\alpha$ y la matriz MDS; ataques algebraicos redujeron margen en variantes mal parametrizadas.

## Aplicaciones concretas
- Árboles de Merkle ZK (membership de credenciales).
- Compromisos y nullifiers en protocolos de privacidad (Tornado Cash, Semaphore, Zcash).
- Firmas hash-based ZK-friendly (asociado a [[pedersen-hash]] como alternativa).

## Relación con Midnight / Compact (si aplica)
Midnight usa Poseidon como hash principal en Compact y en su ledger (estado privado, Merkle trees del estado), porque el prover corre en el dispositivo del usuario y el costo de constraints domina la experiencia. Está disponible en la librería estándar de Compact.

## Relación con mi proyecto (si aplica)
Para el árbol de Merkle de evidencias de víctimas (probar posesión sin revelar el contenido) y para el compromiso de identidad del perito, Poseidon minimiza el tiempo de prueba en el cliente, clave en una investigación con demo en navegador.

## Referencias
- Grassi, Khovratovich, Rechberger, Roy, Schofnegger, *Poseidon: A New Hash Function for Zero-Knowledge Proof Systems*, USENIX Security 2021. https://eprint.iacr.org/2019/458
- Poseidon Initiative / paper de parámetros: https://www.poseidon-hash.info/
- Midnight docs (Compact standard library): https://docs.midnight.network/
