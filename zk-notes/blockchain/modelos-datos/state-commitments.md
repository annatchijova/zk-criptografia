# State Commitments

## ¿Qué problema resuelve?
Un nodo ligero, un contrato de capa 1 o un juez auditor no pueden re-ejecutar ni almacenar todo el estado de una cadena. Se necesita un valor corto y vinculante que comprometa todo el estado, contra el cual se puedan verificar afirmaciones puntuales ("esta cuenta tiene X", "esta hoja está incluida") con pruebas pequeñas.

## Idea intuitiva
Es el mismo truco de un [[merkle-trees|Merkle tree]] llevado al estado completo: una raíz hash $R$ resume millones de claves y valores. Quien publica $R$ queda comprometido: cualquier prueba que se verifique contra $R$ es tan confiable como el estado entero, pero cuesta $O(\log n)$ comprobarla.

## Definición formal
Un esquema de commitment autenticado de estado consta de: $Commit(estado) \to R$, $Prove(estado, k) \to (v, \pi)$, $Verify(R, k, v, \pi) \to \{0,1\}$, y a menudo $Update(R, k, v_{nuevo}) \to R'$. Debe ser **vinculante** (no existen dos estados con la misma raíz salvo colisión de $H$) y admitir pruebas sucintas. Implementaciones: árboles Merkle, [[sparse-merkle-trees]], [[patricia-trie]], vector commitments (Verkle) y accumulators.

## Complejidad / garantías / limitaciones
- Pruebas: $O(\log n)$ típicas; constantes en accumulators RSA.
- Garantías: integridad y autenticidad bajo dureza criptográfica; habilitan [[light-clients]] y puentes.
- Limitaciones: el commitment no da **disponibilidad** del estado (ver [[data-availability]]); actualizar la raíz en cada bloque tiene coste; pruebas grandes dificultan la verificación dentro de circuitos ZK.

## Aplicaciones concretas
- Raíz de estado en cada bloque de Ethereum ([[patricia-trie]]).
- Raíces de estado de [[rollups]] publicadas en L1.
- Archivos de evidencia con raíz pública anclada (timestamping).

## Relación con Midnight / Compact (si aplica)
Midnight publica commitments del estado (público) y de estructuras privadas; un circuito Compact prueba conocimiento de una preimagen/hoja incluida bajo la raíz on-chain, logrando "verificar sin revelar".

## Relación con mi proyecto (si aplica)
La fiscalía ancla una raíz $R$ del lote de evidencias de víctimas; el perito demuestra en ZK que posee una evidencia incluida en $R$ (membresía) y que su credencial está en la raíz del padrón de peritos — fundamento técnico de autenticidad bajo el estándar Daubert (método comprobable y con tasa de error conocida).

## Referencias
- Merkle, R. (1988), CRYPTO '87.
- Buterin, V. *Verkle Trees* (ethereum.org, 2021).
- Boneh, Bünz, Fisch (2019). *Batching Techniques for Accumulators*, CRYPTO.
