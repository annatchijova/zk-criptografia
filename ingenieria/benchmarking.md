# Benchmarking de pruebas ZK

## ¿Qué problema resuelve?
Una afirmación de eficiencia sin números reproducibles no sirve ni ante un tribunal ni en un hackathon. El [[benchmarking]] define qué medir, cómo medirlo y cómo reportarlo para que un tercero lo crea (véase [[reproducibilidad]]).

## Idea intuitiva
Como el consumo de un auto: no alcanza decir "gasta poco"; hay que declarar ciclo de prueba, condiciones y cifras concretas, y que otro pueda repetir la medición.

## Definición formal
Métricas mínimas para un sistema ZK:
- **Tiempo de prueba (proving time)**: mediana y percentiles (p50/p95) sobre ≥100 ejecuciones, en hardware declarado (CPU, RAM, núcleos).
- **Tiempo de verificación**: igual metodología; suele ser O(1) u O(log n) en SNARKs.
- **Tamaño de la prueba**: bytes exactos del proof serializado.
- **Tamaño de claves/parámetros**: proving key, verifying key.
- **Costo on-chain**: gas o fee de la transacción de verificación en la red destino.
- **Memoria pico del prover**: RSS máximo durante la generación.
- **Setup**: tiempo de la ceremonia si aplica (véase [[trade-offs]]).
Reporte creíble: hardware, versión del software (commit hash), dataset, número de repeticiones, desviación estándar, y scripts públicos para repetir.

## Complejidad / garantías / limitaciones
Las cifras de wall-clock dependen fuertemente del hardware y deben acompañarse del análisis estático de [[complejidad]] (número de constraints), que es hardware-independiente. Cuidado con sesgos: medir con caches fríos y calientes por separado, desactivar turbo/parallelismo variable o declararlo. Una sola medición no es evidencia.

## Aplicaciones concretas
- Tabla: "prueba de identidad pericial: prove 1.8 s (p50), verify 4 ms, proof 192 B, gas 210 k".
- Comparativa antes/después de optimizar un gadget (p. ej. reemplazar SHA-256 por Poseidon dentro del circuito).
- Presupuesto de costo por peritaje para el tribunal.

## Relación con Midnight / Compact (si aplica)
En Midnight la verificación on-chain consume recursos de la red y tiene fee asociado; el benchmark debe incluir el costo de la transacción en testnet, además de los tiempos locales del prover del SDK.

## Relación con mi proyecto (si aplica)
El informe pericial incluye un anexo de benchmark reproducible: un juez o contraperito ejecuta el script y obtiene las mismas cifras, sustentando que el sistema es operable en la práctica y no un prototipo frágil.

## Referencias
- Criterion.rs (benchmarking estadístico en Rust): https://github.com/bheisler/criterion.rs
- Prácticas de medición: "Producing Wrong Data Without Doing Anything Obviously Wrong", Mytkowicz et al., ASPLOS 2009.
- Documentación de fees de Midnight: https://docs.midnight.network/
