# Patricia Trie (Merkle Patricia Trie)

## ¿Qué problema resuelve?
Se requiere un [[state-commitments|commitment]] de un mapa clave→valor muy grande y dinámico (cuentas, balances, storage) con actualizaciones y pruebas de membresía/no-membresía eficientes, donde las claves largas comparten prefijos.

## Idea intuitiva
Un trie es un árbol donde el camino desde la raíz está determinado por los caracteres (nibbles) de la clave. *Patricia* comprime los tramos sin ramificaciones en nodos de extensión, evitando cadenas de nodos con un solo hijo. Cada nodo se referencia por su hash, convirtiéndolo en una estructura autenticada: la raíz compromete todo el mapa, como en un [[merkle-trees|Merkle tree]].

## Definición formal
Claves codificadas como secuencias de nibbles (hex). Tres tipos de nodo: **rama** (16 hijos + valor), **extensión** (prefijo compartido comprimido + hijo) y **hoja** (sufijo + valor). La referencia a un nodo es $H(\text{RLP}(nodo))$; la raíz del trie es el commitment. Un proof para clave $k$ es la lista de nodos RLP del camino; si el camino termina antes de una hoja válida o en una hoja con distinto sufijo, constituye prueba de **no-pertenencia**. Profundidad efectiva $O(\log_{16} n)$ amortizada por compresión.

## Complejidad / garantías / limitaciones
- Lookup/update/proof: $O(\text{longitud de clave})$ ≈ $O(\log n)$ en la práctica.
- Garantía: raíz determinista e incremental; soporta pruebas de inclusión y exclusión.
- Limitación: implementación compleja (RLP, codificación hex-prefix); pruebas largas (~cientos de nodos KB) y costosas de verificar dentro de circuitos ZK, lo que motiva alternativas como [[sparse-merkle-trees]] o Verkle trees.

## Aplicaciones concretas
- Ethereum: state trie, transactions trie, receipts trie por bloque y storage trie por cuenta. [[light-clients|Light clients]] pueden pedir pruebas Merkle-Patricia de un balance.

## Relación con Midnight / Compact (si aplica)
Midnight no usa el trie de Ethereum (deriva de Cardano, [[utxo-eutxo-account]]), pero el concepto es el referente clásico de estado comprometido con pruebas; en Compact las pruebas se verifican dentro de un circuito, por lo que se prefieren estructuras con caminos de profundidad fija.

## Relación con mi proyecto (si aplica)
Ilustra cómo un estado judicial (padrón de peritos habilitados, registro de evidencias) podría comprometerse en una raíz pública contra la cual el perito prueba pertenencia en ZK sin revelar su entrada concreta.

## Referencias
- Ethereum Yellow Paper (Wood, G.), Apéndice D: Modified Merkle Patricia Trie.
- Morrison, D. (1968). *PATRICIA—Practical Algorithm To Retrieve Information Coded in Alphanumeric*, JACM.
- https://ethereum.org/en/developers/docs/data-structures-and-encoding/patricia-merkle-trie/
