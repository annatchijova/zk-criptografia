# Merkle Trees y Merkle Proofs

## ¿Qué problema resuelve?
Necesitamos comprometer un conjunto grande de datos en un único valor corto (la raíz) y luego poder demostrar que un dato concreto pertenece al conjunto sin revelar ni transmitir el conjunto completo. Es la base de la verificación ligera en blockchains y de la [[chain-of-custody]] digital.

## Idea intuitiva
Imagina un torneo de emparejamientos: cada hoja es el hash de un dato; cada nodo interno es el hash de la concatenación de sus dos hijos. La raíz resume todo el árbol. Para demostrar que una hoja está, basta mostrar los hashes "hermanos" del camino hacia la raíz: cualquiera puede recalcular y comparar.

## Definición formal
Dado un conjunto ordenado de hojas $d_1,\dots,d_n$ y una función hash resistente a colisiones $H$, se define $L_i = H(d_i)$ y cada nodo padre $P = H(P_{izq}\|P_{der})$. La raíz $R$ es el commitment. Un proof de membresía (authentication path) para $d_i$ es la secuencia de hermanos del camino $L_i \to R$, de longitud $\lceil \log_2 n \rceil$. Verificación: recomputar la raíz y comparar con $R$.

## Complejidad / garantías / limitaciones
- Proof y verificación: $O(\log n)$ hashes. Construcción: $O(n)$.
- Garantía: bajo resistencia a colisiones de $H$, un proof falso implica una colisión.
- Limitación: un árbol clásico NO prueba no-pertenencia (ver [[sparse-merkle-trees]]).

## Aplicaciones concretas
- Verificación SPV de transacciones en Bitcoin.
- Compromiso de lotes de evidencia en un registro [[merkle-mountain-range]].
- Base de muchos [[state-commitments]].

## Relación con Midnight / Compact (si aplica)
Midnight usa árboles de Merkle para comprometer estados privados y leaves cifradas; en Compact se verifica un proof dentro del circuito para probar pertenencia sin revelar el elemento (patrón "membership proof en ZK").

## Relación con mi proyecto (si aplica)
La evidencia de víctimas se hashea como hoja; el perito prueba en ZK que conoce una hoja incluida en la raíz publicada por la fiscalía, sin revelar qué víctima ni el contenido — apoya el factor Daubert de fiabilidad técnica verificable.

## Referencias
- Merkle, R. (1988). *A Digital Signature Based on Conventional Encryption Functions*, CRYPTO '87.
- Nakamoto, S. (2008). *Bitcoin: A Peer-to-Peer Electronic Cash System*, §8 (SPV).
- Documentación Midnight: https://docs.midnight.network
