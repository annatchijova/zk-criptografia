# HVZK: conocimiento cero contra verificador honesto vs malicioso

## ¿Qué problema resuelve?
Definir conocimiento cero solo contra un verificador que *sigue el protocolo* es mucho más fácil que contra uno que se desvía para extraer información. Distinguir ambos niveles evita falsas sensaciones de seguridad.

## Idea intuitiva
Un verificador honesto elige sus desafíos con aleatoriedad genuina y sigue las reglas; un verificador malicioso puede elegir desafíos adversariales precisamente para forzar filtraciones. HVZK es el "caso amable"; el ZK completo ([[zero-knowledge-property]]) exige un simulador para *todo* V*.

## Definición formal
(P, V) es **honest-verifier zero-knowledge (HVZK)** si existe un simulador S de tiempo polinomial tal que para todo (x, w) ∈ R:

{ ⟨P(x,w), V(x)⟩ } ≈ { S(x) },

es decir, solo se simula la vista del verificador **honesto** V. Es ZK "completo" cuando el simulador existe para todo V* polinomial (posiblemente con acceso de rebobinado a V*). Para Σ-protocolos, HVZK especial: S(x, e) produce transcripts (a, e, z) correctamente distribuidos para un desafío e dado.

## Complejidad / garantías / limitaciones
HVZK se logra gratis en Σ-protocolos (Schnorr, etc.). Transformaciones estándar elevan HVZK a ZK malicioso: compromisos de desafío, técnicas de Fischlin, o el **heurística de Fiat–Shamir** (el desafío sale de un hash, el verificador no lo elige), que convierte Σ-protocolos público-moneda en NIZK en el modelo de oráculo aleatorio. La conversión Fiat–Shamir pierde rigor (modelo idealizado) pero gana no interactividad —ver [[interactive-proofs-nizk]].

## Aplicaciones concretas
- Schnorr HVZK → firmas de Schnorr vía Fiat–Shamir.
- Bulletproofs y PLONK: HVZK + Fiat–Shamir = NIZK práctico.

## Relación con Midnight / Compact (si aplica)
Midnight opera en el mundo no interactivo: la protección contra "verificadores maliciosos" la provee la estructura Fiat–Shamir del sistema de prueba subyacente, no una interacción viva.

## Relación con mi proyecto (si aplica)
Como la verificación es pública (cualquiera puede auditar la prueba), HVZK no bastaría en un canal interactivo; el NIZK de Midnight neutraliza por diseño al verificador adversarial. Ver [[witness-statement]].

## Referencias
- Cramer, Damgård, Schoenmakers, "Proofs of Partial Knowledge and Simplified Design of Witness Hiding Protocols", CRYPTO 1994.
- Fiat, Shamir, "How to Prove Yourself", CRYPTO 1986.
- Damgård, "On Σ-protocols", notas de curso, Aarhus University.
