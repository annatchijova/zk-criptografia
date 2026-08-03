# Hash de Pedersen

## ¿Qué problema resuelve?
Permite construir una función hash directamente a partir de un commitment de curva elíptica ([[pedersen-commitments]]), obteniendo un hash **algebraico y homomórfico** que es barato en circuitos ZK definidos sobre la misma curva (curvas "embedded"). Resuelve el mismo problema que [[poseidon]] — costo de circuito — pero con seguridad reducida al logaritmo discreto.

## Idea intuitiva
Tomamos el mensaje, lo partimos en escalares $m_1, \dots, m_n$ y calculamos el punto $H(m) = \sum_i m_i G_i$ con generadores $G_i$ fijos e independientes. Como es una combinación lineal de puntos, dentro del circuito solo hay multiplicaciones escalar–punto, que en curvas embebidas (p. ej. Jubjub sobre el campo escalar de BLS12-381 o BN254) se verifican con relativamente pocos constraints usando windowed scalar multiplication.

## Definición formal
Sea $\mathbb{G}$ un grupo de orden primo $q$ con generadores $G_1,\dots,G_n$ elegidos sin que se conozca relación alguna $\sum a_i G_i = 0$. El hash de Pedersen es $H: \mathbb{Z}_q^n \to \mathbb{G}$, $H(m_1,\dots,m_n) = \prod_i G_i^{m_i}$. Encontrar una colisión equivale a resolver el logaritmo discreto en $\mathbb{G}$ (binding bajo DL). Para usarlo como hash de salida fija se suele extraer la coordenada $x$ del punto.

## Complejidad / garantías / limitaciones
- Costo en circuito: del orden de **pocos cientos a ~2.000 constraints** según el número de ventanas y bits por ventana (más caro que [[poseidon]], mucho más barato que [[sha2]]).
- Seguridad: colisiones = DL en la curva (~128 bits en curvas estándar). **No** es resistente a preimagen como hash genérico en todos los usos; tampoco modela un random oracle tan limpiamente.
- Limitación clave: necesita generadores "nothing-up-my-sleeve" y que el campo del circuito coincida con el orden de la curva (embedded curves).

## Aplicaciones concretas
- Árboles de Merkle en Zcash Sapling (Jubjub) para notas y nullifiers.
- Compromisos acumulativos: $H(a)+H(b)$ se relaciona con $H(a\|b)$, útil en agregación.
- Sistemas de credenciales donde el hash debe ser también un commitment ([[pedersen-commitments]]).

## Relación con Midnight / Compact (si aplica)
Midnight favorece Poseidon para hashing general, pero la noción de hash/commitment de Pedersen aparece al diseñar compromisos de atributos dentro de Compact cuando se requiere homomorfismo (sumar atributos comprometidos sin abrirlos).

## Relación con mi proyecto (si aplica)
Para acreditar que el perito posee evidencia, un hash de Pedersen permite que los compromisos a cada pieza de evidencia se agreguen homomórficamente (p. ej. conteo de víctimas) sin revelar valores individuales; para el Merkle tree general, [[poseidon]] sigue siendo más barato.

## Referencias
- Pedersen, T., *Non-Interactive and Information-Theoretic Secure Verifiable Secret Sharing*, CRYPTO 1991.
- Hopwood et al., *Zcash Protocol Specification* (sección Pedersen Hash / Jubjub). https://zips.z.cash/protocol/protocol.pdf
- Zcash ZIP 0 / Sapling design notes.
