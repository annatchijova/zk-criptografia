# Sparse Merkle Trees

## ¿Qué problema resuelve?
Un [[merkle-trees|Merkle tree]] clásico solo prueba pertenencia. En muchos sistemas (revocación, registros de identidad, estados) se necesita también probar que una clave **no** está en el conjunto, y actualizar el árbol eficientemente con claves arbitrarias.

## Idea intuitiva
Piensa en un árbol Merkle completo de profundidad fija 256, donde cada posible hash de clave es una hoja. Casi todas están vacías, así que no se materializan: cada subárbol vacío se representa por un hash por defecto precomputable. Probar que una hoja está vacía equivale a probar no-pertenencia.

## Definición formal
Espacio de claves $\{0,1\}^{256}$ indexado por $H(clave)$. Cada nivel tiene un valor por defecto: $D_0 = H(\text{vacío})$, $D_{k+1} = H(D_k\|D_k)$. Una prueba para clave $k$ incluye el camino de hermanos de profundidad 256; si la hoja vale $D_0$, es un proof de **no-membresía**; si vale $H(v)$, de membresía. La raíz $R$ es un [[state-commitments|commitment]] del mapa clave→valor completo.

## Complejidad / garantías / limitaciones
- Proof y verificación: $O(256)$ hashes, constante respecto a $n$.
- Actualización de una hoja: $O(256)$.
- Garantía: seguridad heredada de la resistencia a colisiones; un único formato de prueba sirve para membresía y no-membresía.
- Limitación: pruebas más largas que en un árbol denso; coste dentro de circuitos ZK no trivial.

## Aplicaciones concretas
- State tree de [[validity-proofs|ZK-rollups]] (p. ej. zkSync).
- Listas de revocación de credenciales (acreditación y exclusión probables).
- IBC light clients (SMTs de Cosmos).

## Relación con Midnight / Compact (si aplica)
Midnight representa estados con estructuras tipo árbol disperso: probar que un nullifier aún no fue gastado es exactamente un proof de no-membresía, clave para prevenir doble gasto en un modelo privado ([[utxo-eutxo-account]]).

## Relación con mi proyecto (si aplica)
Permite al perito probar que su credencial NO está revocada (no-membresía en la lista de revocación) y que la evidencia aún no fue registrada bajo otra identidad — sin revelar la credencial misma.

## Referencias
- Laurie, B. & Kasper, E. *Revocation Transparency* (Google, 2013).
- Dahlberg, R., Pulls, T., Peeters, R. (2016). *Efficient Sparse Merkle Trees*, NordSec.
- https://docs.celestia.org (SMTs en Namespaced Merkle Trees).
