# Sistema de tipos de Compact para datos privados

## ¿Qué problema resuelve?
En un circuito ZK no existe "cualquier dato": todo valor debe tener tamaño y representación fijos para convertirse en restricciones aritméticas. El sistema de tipos de Compact obliga a declarar exactamente qué forma tienen los datos privados, haciendo los programas compilables a circuitos y seguros frente a fugas.

## Idea intuitiva
Los tipos son los moldes del circuito: un `Bytes<32>` es un molde de exactamente 32 bytes. El dato privado entra por el molde off-chain; el circuito solo puede manipular lo que el molde permite. Además, el compilador etiqueta qué valores "nacen" privados (argumentos de circuitos exportados, retornos de witnesses) y rastrea todo lo derivado de ellos: nada de eso puede tocar el ledger sin `disclose()`.

## Definición formal
Tipos principales (referencia oficial):
- `Field` — elemento del campo finito del sistema de pruebas (nativo del circuito, operaciones aritméticas baratas).
- `Uint<0..n>` — enteros acotados con rango comprobado en circuito.
- `Bytes<n>` — secuencias de bytes de longitud fija; soportan indexación, slices, `map`/`fold` (desde Minokawa 0.18).
- `Boolean`, `Enum`, tuplas, `Vector<n, T>` de tamaño fijo.
- `struct { ... }` — registros, con destructuring en parámetros (`circuit f({x, y}: S)`).
- `Opaque<"string">` — dato opaco cuya estructura solo conoce el lado TypeScript.
- ADTs genéricos del ledger: `Counter`, `Set<T>`, `Map<K,V>`, `MerkleTree<n>` en `CompactStandardLibrary`.

Regla de divulgación: todo valor derivado de dato privado requiere `disclose()` antes de almacenarse en el ledger, retornarse de un circuito exportado o condicionar efectos públicos ([[compact-language]]).

## Complejidad / garantías / limitaciones
- Tipado estático y fuerte; rangos verificados en compilación y en circuito.
- Todo tipo tiene tamaño fijo: no hay strings ni listas dinámicas (limitación de los circuitos).
- `Opaque` delega la serialización al runner: flexible pero menos chequeable.

## Aplicaciones concretas
- Modelar una credencial como `struct { matricula: Bytes<32>, vencimiento: Uint<64> }` privada, y publicar solo su hash.

## Relación con mi proyecto
La evidencia de víctimas se tipa como `Bytes<32>` (hash) y la identidad del perito como struct privado vía witness; el ledger solo guarda `Bytes<32>` commitments. El tipo `MerkleTree` permite probar pertenencia de una evidencia a un lote sin revelar las demás.

## Referencias
- https://docs.midnight.network/compact/reference/compact-reference — tipos y disclose
- https://docs.midnight.network/relnotes/compact/minokawa-0-18-26-0
