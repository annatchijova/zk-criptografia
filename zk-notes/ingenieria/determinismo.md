# Determinismo del sistema completo

## ¿Qué problema resuelve?
Para que una prueba ZK sea admisible bajo el estándar Daubert, el método debe ser **reproducible**: otro perito, con los mismos datos, debe obtener el mismo resultado. Si cualquier etapa es no determinista de forma no controlada, la evidencia deja de ser verificable independientemente.

## Idea intuitiva
Un experimento científico que da resultados distintos cada vez no se puede publicar; un pipeline forense que no se puede re-ejecutar no se puede auditar.

## Definición formal
Determinismo: misma entrada → misma salida, en todas las capas:
1. **Ingesta de evidencia**: hash del archivo con algoritmo y codificación fijados (canonicalización: orden de claves JSON, finales de línea, codificación UTF-8). Bytes distintos → hash distinto → prueba distinta.
2. **Circuito**: los circuitos aritméticos son deterministas por construcción — misma asignación de *witness*, mismas restricciones.
3. **Prover**: la prueba puede incluir aleatoriedad interna (*blinding*), lo cual es legítimo, pero su **validez** debe ser determinista y el resultado público (nullifier, commitments) debe ser reproducible — véase [[replay-attacks]].
4. **Verificador**: función booleana pura; mismo resultado on-chain siempre.
5. **Entorno**: versiones fijadas del compilador Compact, del circuito y de los parámetros (*pinning*, hashes de los binarios); relojes y aleatoriedad del SO documentados.

## Complejidad / garantías / limitaciones
No exige que las pruebas sean idénticas bit a bit (la aleatoriedad del prover lo impide), sino que el **predicado probado** y los outputs públicos sean estables y re-derivables. Requiere gestión de configuración rigurosa: SBOM, builds reproducibles.

## Aplicaciones concretas
Cadena de custodia digital: cada artefacto (evidencia, circuito, parámetros, prueba) con su hash registrado permite a un tercero re-ejecutar la verificación completa años después.

## Relación con Midnight / Compact
Fijar la versión del compilador Compact y registrar el hash del circuito desplegado junto con el contrato.

## Relación con mi proyecto
Es el requisito metodológico central de Daubert (testabilidad y tasa de error): sin determinismo extremo a extremo, todo lo demás — [[trusted-computing-base]], [[threat-modeling]] — no es defendible en tribunales.

## Referencias
- Daubert v. Merrell Dow Pharmaceuticals, 509 U.S. 579 (1993).
- NIST SP 800-86, "Guide to Integrating Forensic Techniques into Incident Response".
- Reproducible Builds: https://reproducible-builds.org
