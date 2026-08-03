# Witness y statement: los dos lados de una prueba ZK

> **Nota canónica**: esta nota es la referencia central desde `midnight/` y desde el proyecto del perito digital.

## ¿Qué problema resuelve?
Toda prueba ZK exige separar con precisión qué es **público** (el statement, lo que se afirma) y qué es **secreto** (el witness, lo que permite demostrarlo). Confundirlos rompe la privacidad o la validez.

## Idea intuitiva
El statement es la afirmación que cualquiera puede leer ("existe una credencial válida y evidencia íntegra"); el witness es el material secreto que hace verdadera esa afirmación (la credencial, los hashes internos, las claves). La prueba ZK convence de que el statement es cierto *usando* el witness sin exponerlo.

## Definición formal
Dada una [[np-relations|relación NP]] R ⊆ {0,1}* × {0,1}*:
- Un **statement** (o instancia) es x ∈ {0,1}*.
- Un **witness** para x es w tal que (x, w) ∈ R.
- El lenguaje asociado: L_R = { x : ∃w con (x,w) ∈ R }.

Un sistema de prueba ZK para R permite al prover, en posesión de (x, w) ∈ R, convencer al verificador de que x ∈ L_R cumpliendo [[completeness]], [[soundness]] (típicamente como *proof of knowledge*) y [[zero-knowledge-property]]: el transcript no revela nada sobre w más allá de su existencia.

## Complejidad / garantías / limitaciones
La verificación de R(x, w) debe ser polinomial en |x|. La elección de qué va en x y qué en w es una decisión de **diseño de circuito**: meter datos públicos en el witness "congela" innecesariamente la flexibilidad; exponer datos sensibles en el statement destruye la privacidad.

## Aplicaciones concretas
- Zcash: statement = compromisos y nullifiers públicos; witness = claves y notas.
- Credenciales: statement = "credencial válida emitida por X"; witness = la credencial.

## Relación con Midnight / Compact (si aplica)
En Compact, las entradas públicas del circuito son el statement; el estado privado (ledger privado, inputs `witness`) es el witness. El compilador genera las restricciones R y el prover local produce la prueba.

## Relación con mi proyecto (si aplica)
Mi diseño: **statement** = (raíz del registro de peritos habilitados, compromiso con el lote de evidencia, timestamp); **witness** = (identidad del perito, su credencial, pruebas de Merkle, hashes de la evidencia de víctimas). Así, ante un estándar Daubert, se prueba autenticidad e integridad sin revelar a las víctimas. Ver [[np-relations]].

## Referencias
- Goldreich, "Foundations of Cryptography, Vol. 1", Caps. 2 y 4, 2001.
- Thaler, "Proofs, Arguments, and Zero-Knowledge", §1–2, 2022.
- Documentación de Midnight: https://docs.midnight.network/
