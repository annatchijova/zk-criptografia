# Completitud (completeness)

## ¿Qué problema resuelve?
Garantiza que el sistema de pruebas *no rechaza a los honestos*: si el enunciado es verdadero y el prover sigue el protocolo, el verificador acepta. Sin completitud, una prueba ZK sería inútil aunque fuera perfectamente privada.

## Idea intuitiva
Un perito que realmente posee las credenciales y la evidencia correctas siempre logra convencer al verificador. La propiedad distingue errores "del lado honesto" (no tolerados) de intentos de trampa (cubiertos por [[soundness]]).

## Definición formal
Sea R ⊆ {0,1}* × {0,1}* una [[np-relations|relación NP]] y (P, V) un sistema de prueba interactivo para el lenguaje L_R = {x : ∃w, (x,w) ∈ R}. Decimos que (P, V) tiene **completitud** con error ε_c(·) si para todo (x, w) ∈ R:

Pr[ ⟨P(x, w), V(x)⟩ = accept ] ≥ 1 − ε_c(|x|),

donde la probabilidad se toma sobre las monedas aleatorias de P y V. Si ε_c = 0 hablamos de **completitud perfecta**; si ε_c es despreciable (negligible), de completitud estadística.

## Complejidad / garantías / limitaciones
La mayoría de los protocolos clásicos (Schnorr, Σ-protocolos, PLONK) tienen completitud perfecta: la verificación algebraica pasa siempre cuando el witness es válido. Un error de completitud no nulo suele provenir de rechazo probabilístico (p. ej. en pruebas de rango con cotas ajustadas) y debe acotarse explícitamente en el análisis.

## Aplicaciones concretas
- Login sin contraseña: un usuario legítimo nunca queda bloqueado.
- En SNARKs: un circuito bien formado con witness válido siempre genera una prueba que verifica.

## Relación con Midnight / Compact (si aplica)
En Midnight, una transacción Compact con witness correcto genera una prueba que el verificador on-chain acepta siempre (completitud perfecta heredada del sistema de prueba subyacente).

## Relación con mi proyecto (si aplica)
Un perito auténtico, con credencial válida y hashes de evidencia correctos, siempre pasa la verificación: la completitud es la contracara operativa de la admisibilidad Daubert (el método no excluye al testigo calificado).

## Referencias
- Goldwasser, Micali, Rackoff, "The Knowledge Complexity of Interactive Proof Systems", SIAM J. Comput. 18(1), 1989.
- Goldreich, "Foundations of Cryptography, Vol. 1", Cap. 4, Cambridge University Press, 2001.
- Thaler, "Proofs, Arguments, and Zero-Knowledge", Foundations and Trends in Privacy and Security, 2022.
