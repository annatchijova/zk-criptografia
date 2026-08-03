# Verificación formal

## ¿Qué problema resuelve?
Un circuito ZK con bugs puede probar enunciados falsos (constraints sub-restringidos) o rechazar testigos válidos (sobre-restringidos). La [[formal-verification|verificación formal]] busca garantías matemáticas de que el circuito implementa exactamente la especificación, algo que el [[property-testing]] y el [[fuzzing]] solo aproximan.

## Idea intuitiva
Probar un programa es como revisar todas las rutas posibles de un mapa en vez de caminar solo algunas: en lugar de tests concretos, se demuestra un teorema ("para todo input, la salida cumple la propiedad").

## Definición formal
Sea un circuito C como sistema de constraints R1CS/PLONKish y una especificación S como relación entre statement x y witness w. Se distinguen:
- **Correctness (soundness del circuito)**: si C(x, w) satisface, entonces S(x, w) vale.
- **Completitud**: si S(x, w) vale, existe w' tal que C acepta.
La verificación formal demuestra estas implicaciones en un asistente de pruebas (Lean, Coq) o mediante métodos automáticos (SMT, model checking) sobre el circuito compilado.

## Complejidad / garantías / limitaciones
Verificar formalmente un circuito completo es costoso: los circuitos reales tienen 10⁴–10⁷ constraints (véase [[complejidad]]) y los SMT solvers escalan mal con aritmética de campos finitos. La estrategia viable es la **verificación parcial**: demostrar formalmente los gadgets críticos (comparadores de rango, verificación de firma, árbol de Merkle, hash de compromiso) y dejar el resto cubierto por [[property-testing]] y [[fuzzing]]. Herramientas emergentes (p. ej. formalización de gadgets Halo2, Ecne, Picus) permiten detectar automáticamente circuits sub-restringidos.

## Aplicaciones concretas
- Demostrar que el gadget de "edad ≥ 18" no acepta valores negativos (wrapping mod p).
- Verificar que el nullifier es único por (identidad, contexto).
- Auditoría formal del gadget de verificación de firma del perito.

## Relación con Midnight / Compact (si aplica)
Compact compila a un backend de circuitos; sus primitivas de disclosure selectivo y sus librerías estándar son candidatas naturales a verificación formal reutilizable, mientras que la lógica propia del contrato se verifica parcialmente.

## Relación con mi proyecto (si aplica)
Prioridad: verificar formalmente (1) el chequeo de posesión de la clave del perito y (2) el compromiso sobre la evidencia de víctimas, que son los puntos donde un bug invalida el peritaje completo.

## Referencias
- Ecne / análisis de circuitos sub-restringidos: https://0xparc.org/blog/zk-bug-tracker
- ZK Bug Tracker (0xPARC): catálogo de bugs reales en circuitos.
- Lean 4: https://leanprover.github.io/
