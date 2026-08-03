# Random Oracle Model: el supuesto conveniente que no siempre se cumple

## ¿Qué problema resuelve?
Muchas pruebas de seguridad criptográfica (Fiat–Shamir para hacer no interactivas las pruebas ZK, firmas, compromisos) necesitan una función hash que se comporte como **azar puro**. Ningún hash real lo es. El Random Oracle Model (ROM) es el idealización que permite demostrar seguridad de forma manejable.

## Idea intuitiva
Imagina que el hash es un oráculo omnisciente: ante cada consulta nueva responde con un valor uniformemente aleatorio, y ante consultas repetidas responde igual (consistencia). Nadie —ni el adversario— puede predecir una salida sin preguntar. Con ese oráculo, programar respuestas dentro de la prueba de seguridad (el "truco de programación") es legítimo y permite simular transcripciones.

## Definición formal
En el ROM (Bellare–Rogaway, 1993), todos los participantes comparten acceso a una función $\mathcal{H}: \{0,1\}^* \to \{0,1\}^n$ elegida uniformemente al azar de entre todas las funciones posibles. El reductor de seguridad además puede:
- **Programar** $\mathcal{H}$ en puntos aún no consultados.
- **Observar** las consultas del adversario (útil para extraer testigos, p. ej., en la prueba de conocimiento de Schnorr con rebobinado).

La **transformada Fiat–Shamir** reemplaza el desafío del verificador por $\mathcal{H}(\text{transcript})$, convirtiendo un protocolo interactivo de divulgación pública (*public-coin*) en una firma o argumento no interactivo.

## Complejidad / garantías / limitaciones
- **No realista en general**: Canetti, Goldreich y Halevi (1998) construyeron esquemas demostrablemente seguros en el ROM e **inseguros con cualquier** función hash concreta instanciada.
- En la práctica, SHA-2/SHA-3/Poseidon se comportan "suficientemente bien" para los esquemas desplegados, pero la garantía es heurística.
- Alternativas: modelo estándar (más riguroso, menos eficiente) y el supuesto *algebraic group model* + ROM usado en análisis de SNARKs modernos.
- En SNARKs, Fiat–Shamir aplicado a argumentos con cierta estructura tiene análisis más fino (programabilidad y *rewinding* cuántico aún debatidos).

## Aplicaciones concretas
Firmas Schnorr, Fiat–Shamir en Bulletproofs y PLONK, loterías VRF, KDFs.

## Relación con Midnight / Compact (si aplica)
Las pruebas no interactivas de Midnight se obtienen vía Fiat–Shamir sobre el protocolo del SNARK; los hashes dentro de circuito (Poseidon/SHA) se modelan como oráculos aleatorios al argumentar seguridad.

## Relación con mi proyecto (si aplica)
El compromiso de evidencia (hash del lote de víctimas) y la no-interactividad de la prueba del perito dependen del ROM: conviene documentar este supuesto explícitamente al defender la fiabilidad del método ante el estándar Daubert.

## Referencias
- Bellare & Rogaway, "Random Oracles are Practical: A Paradigm for Designing Efficient Protocols", CCS 1993.
- Fiat & Shamir, "How to Prove Yourself", CRYPTO '86.
- Canetti, Goldreich & Halevi, "The Random Oracle Methodology, Revisited", STOC 1998.
- Pointcheval & Stern, "Security Arguments for Digital Signatures and Blind Signatures", *J. Cryptology* 13(3), 2000.
