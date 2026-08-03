# Bulletproofs

## ¿Qué problema resuelve?
Permite probar afirmaciones sobre valores comprometidos —en especial **range proofs**: "este compromiso de Pedersen esconde un valor en [0, 2ⁿ)"— sin trusted setup y sin revelar el valor. Es la herramienta estándar para montos confidenciales.

## Idea intuitiva
El rango se descompone bit a bit: el prover demuestra que cada bit es 0 o 1 y que su combinación con pesos binarios reconstruye el valor comprometido. En vez de enviar n compromisos, usa un **inner product argument** recursivo: en cada ronda el vector se pliega a la mitad combinando izquierda y derecha con un desafío del verificador, reduciendo la prueba a tamaño logarítmico.

## Definición formal
Argumento de conocimiento honest-verifier zero-knowledge, hecho no interactivo vía Fiat–Shamir. Se apoya en:
- Compromisos de Pedersen (perfectamente ocultos, computacionalmente vinculantes).
- Un argumento de producto interno: dado un compromiso a **a**,**b** y c = ⟨a,b⟩, probar la igualdad con comunicación O(log n).
Seguridad basada únicamente en el supuesto del logaritmo discreto; sin pairings ni CRS.

## Complejidad / garantías / limitaciones
- Tamaño de prueba: 2·log₂(n) + constante elementos de grupo (~672 bytes para n=64); rangos agregados crecen como O(log(m·n)) para m salidas.
- Verificación: **O(n)** — lineal, no sucinta. Este es el trade-off clave: a diferencia de un SNARK, el verificador paga proporcional al circuito.
- Sin trusted setup: solo parámetros públicos transparentes.
- Limitación: no práctico para circuitos generales grandes; brilla en rangos y circuitos aritméticos moderados.

## Aplicaciones concretas
Montos confidenciales en Monero (desde 2018), MimbleWimble/Grin, pruebas de solvencia de exchanges (probar que reservas ≥ pasivos sin revelar cifras), rangos de edad o saldo en credenciales.

## Relación con Midnight / Compact (si aplica)
Para rangos puntuales dentro de un circuito mayor, Midnight lo resuelve con [[lookup-tables-custom-gates]] sobre Plonk; Bulletproofs sería la alternativa standalone sin setup si el alcance se limita a range proofs.

## Relación con mi proyecto (si aplica)
Un range proof sirve para probar "la evidencia fue recolectada dentro del período autorizado por la orden judicial" o "el perito tiene N años de matrícula vigente" sin revelar fechas ni datos exactos de las víctimas.

## Referencias
- Bünz, Bootle, Boneh, Poelstra, Wuille, Maxwell (2018). *Bulletproofs: Short Proofs for Confidential Transactions and More*. IEEE S&P 2018.
- Bootle et al. (2016). *Efficient Zero-Knowledge Arguments for Arithmetic Circuits in the Discrete Log Setting*. EUROCRYPT.
