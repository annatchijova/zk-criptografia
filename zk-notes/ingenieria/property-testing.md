# Testing por propiedades

## ¿Qué problema resuelve?
Los tests de casos puntuales ("con este input, esta salida") solo cubren los escenarios que el desarrollador imaginó. En circuitos ZK, los bugs críticos (constraints sub-restringidos, overflow en el campo) suelen vivir en inputs que nadie imaginó. El testing por propiedades explora el espacio de entradas buscando contraejemplos a invariantes declarados.

## Idea intuitiva
En vez de afirmar "f(3)=9", se afirma "para todo x, f(x) ≥ 0" y el framework genera miles de x aleatorios —incluidos casos límite— intentando violar la propiedad. Si la viola, encoge (shrinking) el contraejemplo al caso mínimo.

## Definición formal
Dada una propiedad P como predicado sobre inputs, un property-based tester genera x₁..xₙ de una distribución sobre el dominio y comprueba ∀i: P(xᵢ). En circuitos, las propiedades típicas son:
- **Completitud**: witness honesto siempre satisface los constraints.
- **Soundness del circuito**: ningún witness inválido (según la especificación) satisface el circuito.
- **Determinismo del witness generator**: mismos datos privados → mismo witness.
Frameworks: QuickCheck (Haskell), Hypothesis (Python), proptest (Rust).

## Complejidad / garantías / limitaciones
Conviene testear propiedades sobre la **satisfacción de constraints** (barato, O(constraints), véase [[complejidad]]) y reservar la generación de pruebas completas para smoke tests. No garantiza ausencia de bugs —eso es terreno de la [[formal-verification]]— pero su costo-beneficio es excelente. Complementario con [[fuzzing]], enfocado en robustez más que en propiedades semánticas.

## Aplicaciones concretas
- Propiedad: "si el perito no posee la clave, el circuito rechaza para toda variante de witness".
- Propiedad: "el compromiso de la evidencia es binding: ningún par (evidencia, nonce) distinto abre el mismo commitment".
- Propiedad: "el nullifier es estable ante reordenamientos de los datos de entrada".

## Relación con Midnight / Compact (si aplica)
Los contratos Compact pueden testearse off-chain evaluando el circuito generado con witnesses arbitrarios antes de desplegar; las propiedades sobre los outputs públicos (ledger updates) se expresan directamente en el lenguaje de test del SDK.

## Relación con mi proyecto (si aplica)
Las dos propiedades centrales del peritaje —"solo el perito legítimo genera prueba válida" y "toda prueba válida implica posesión de la evidencia comprometida"— se formulan como property tests ejecutados en CI, sustento directo del criterio Daubert de tasa de error conocida.

## Referencias
- Claessen & Hughes, "QuickCheck: A Lightweight Tool for Random Testing of Haskell Programs", ICFP 2000.
- Hypothesis: https://hypothesis.readthedocs.io/
- proptest (Rust): https://docs.rs/proptest
