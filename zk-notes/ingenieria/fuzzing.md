# Fuzzing

## ¿Qué problema resuelve?
El [[fuzzing]] busca fallos robustecimiento un sistema con entradas masivas, aleatorias o mutadas: panics en el generador de witnesses, inconsistencias entre el circuito y su especificación, inputs que saturan recursos o producen pruebas inválidas. Donde el [[property-testing]] verifica invariantes semánticos, el fuzzing ataca la robustez.

## Idea intuitiva
Es arrojar miles de formas raras de completar un formulario —campos vacíos, valores enormes, negativos, repetidos— para ver si la oficina se rompe o acepta algo indebido.

## Definición formal
Un fuzzer mantiene un corpus de entradas, las muta (bit-flips, splicing, valores límite guiados por cobertura) y ejecuta el sistema bajo prueba observando oráculos de fallo: crashes, aserciones, divergencias entre implementaciones (differential fuzzing) o violaciones de invariantes. En circuitos ZK hay dos objetivos distintos:
1. **Fuzzing del witness generator** (código nativo, p. ej. Rust/TypeScript): AFL/libFuzzer clásicos aplican directo.
2. **Fuzzing del circuito**: generar witnesses aleatorios o adversarios y comprobar que la satisfacción de constraints coincide con la especificación de alto nivel (diferencial: simulador vs. circuito compilado).

## Complejidad / garantías / limitaciones
El costo por ejecución sobre el circuito compilado es O(constraints) (véase [[complejidad]]), mucho menor que generar pruebas completas, por lo que se logran miles de iteraciones/segundo fuzzing satisfacción de constraints y reservando el prover para casos sospechosos. Limitación central: el fuzzer no entiende la aritmética del campo; valores "raros" para humanos (p−1, 2^k) hay que inyectarlos con mutadores específicos de dominio. No ofrece garantías de ausencia de fallos; para eso está la [[formal-verification]].

## Aplicaciones concretas
- Mutar commitments y openings buscando aperturas inválidas aceptadas.
- Fuzzing diferencial entre el modelo matemático (Python) y el circuito compilado.
- Inputs patológicos en la decodificación de evidencia (tamaños máximos, padding, encoding).

## Relación con Midnight / Compact (si aplica)
El toolchain de Midnight permite ejecutar el circuito localmente con witnesses arbitrarios sin tocar la red; ahí se inserta el harness de fuzzing. El generador de pruebas en TypeScript se fuzza con herramientas estándar (Jazzer.js, cargo-fuzz según capa).

## Relación con mi proyecto (si aplica)
Fuzzear el pipeline pericial completo: desde el parsing de la evidencia de víctimas hasta la generación del witness, porque cualquier crash o divergencia descubierto en la contrapericia destruiría la credibilidad ante el tribunal.

## Referencias
- AFL++: https://aflplus.plus/
- libFuzzer (LLVM): https://llvm.org/docs/LibFuzzer.html
- ZK Bug Tracker (0xPARC), motivación de dominio: https://0xparc.org/blog/zk-bug-tracker
