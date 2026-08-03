# Lookup tables y custom gates

## ¿Qué problema resuelve?
Los circuitos ZK "piensan" en aritmética de campo: sumas y multiplicaciones son baratas, pero operaciones **no aritméticas** —rangos, XOR, AND, shifts, funciones no lineales como hashes— explotan en restricciones si se expresan bit a bit. Los lookups y custom gates bajan ese costo de órdenes de magnitud.

## Idea intuitiva
Un **custom gate** extiende la ecuación de puerta de [[plonk]] con términos adicionales (grado mayor, selectores extra), permitiendo que una sola fila del circuito exprese operaciones complejas. Un **lookup argument** va más lejos: el prover afirma "este valor está en la tabla T" (una tabla pública precomputada, p. ej., todas las tuplas (a, b, a XOR b) de 8 bits) y demuestra la pertenencia con un argumento polinomial, en vez de re-computar la operación.

## Definición formal
- **plookup** (Gabizon–Williamson, 2020): reduce la pertenencia de una tupla f en una tabla t a una identidad entre polinomios sobre un subgrupo, usando la observación de que el conjunto unido y ordenado {f} ∪ {t} tiene una estructura de "ordenada por tabla" chequeable con dos productos de grado uno (argumento de grand product).
- **Custom gates**: en la aritmetización PLONKish ([[halo2]]), selectores y columnas fijas definen restricciones polinómicas arbitrarias por fila: q(x)·P(advice₁..k, fixed₁..m) = 0.
Extensiones modernas: logUp (Scroll), cq, Lasso (lookups gigantes vía multilineales, estilo [[spartan]]).

## Complejidad / garantías / limitaciones
- Costo del lookup: O(n) en el prover con constantes bajas; un XOR de 8 bits pasa de ~8–16 restricciones R1CS a 1 lookup.
- Range checks de 16–32 bits: de cientos de restricciones a 1–2 lookups.
- Limitaciones: la tabla consume columnas fijas y el argumento añade polinomios de permutación; tablas muy grandes encarecen el setup del indexer.

## Aplicaciones concretas
zkEVMs (opcodes EVM vía lookups), hashes SHA-256/Keccak dentro de circuitos, comparaciones y rangos en credenciales verificables.

## Relación con Midnight / Compact (si aplica)
El motor PLONKish de Midnight soporta custom gates y lookups; las primitivas de Compact (hash, verificación de firmas, rangos) se implementan así. Sin lookups, un contrato Compact realista sería imposible de probar en tiempo razonable.

## Relación con mi proyecto (si aplica)
El circuito pericial necesita: range proof de fechas de cadena de custodia, XOR/rotaciones del hash de la evidencia, y comparaciones de matrícula. Con lookups el circuito queda en decenas de miles de restricciones; sin ellos, en millones — la diferencia entre demo viable e inviable en la investigación.

## Referencias
- Gabizon & Williamson (2020). *plookup: A simplified polynomial protocol for lookup tables*. IACR ePrint 2020/315.
- Gabizon, Williamson, Ciobotaru (2019). *PLONK*. IACR ePrint 2019/953.
- Haböck (2022). *Logarithmic derivatives (logUp)*. IACR ePrint 2022/1530.
