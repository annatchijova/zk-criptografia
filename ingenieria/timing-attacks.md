# Ataques de temporización (Timing Attacks)

## ¿Qué problema resuelve?
Si el tiempo que tarda una operación depende del valor secreto, medir ese tiempo revela información sobre el secreto, bit a bit, sin romper ninguna matemática.

## Idea intuitiva
Como adivinar una contraseña midiendo cuánto tarda el sistema en decir "incorrecto": si falla en el primer carácter responde más rápido que si falla en el décimo.

## Definición formal
Caso particular de [[side-channels]] donde el canal observable es la latencia. Fuentes típicas:
- Comparaciones con *early exit* (`==` byte a byte) sobre secretos, MACs o nullifiers.
- Ramas condicionales dependientes del secreto en aritmética de curva (doble-y-suma sin protección: multiplicación escalar insegura).
- Accesos a memoria indexados por el secreto (tablas, caché).

Mitigaciones estándar:
- Comparación en **tiempo constante** (`constant_time_eq`, XOR acumulado).
- Multiplicación escalar de Montgomery ladder o implementaciones *constant-time* auditadas.
- Evitar ramas y tablas dependientes del secreto.

## Complejidad / garantías / limitaciones
**En circuitos ZK la situación es favorable**: un circuito aritmético se ejecuta en un número fijo de restricciones — no hay ramas dinámicas — así que el *witness generation* es esencialmente de tiempo fijo respecto a la estructura del circuito. El riesgo real queda en: (a) la preparación de inputs fuera del circuito (parsing, hashing previo, firma), (b) la comparación del nullifier o del digest de evidencia en el contrato/aplicación, (c) librerías de curva subyacentes. Para la investigación: priorizar comparaciones de tiempo constante en el glue code y usar librerías auditadas, no escribir aritmética propia.

## Aplicaciones concretas
Comparar el commitment de evidencia recibido con el registrado usando comparación constante; nunca `if hash == esperado { early return }`.

## Relación con Midnight / Compact
Compact compila a circuitos de tamaño fijo; la superficie de timing se concentra en el código host y en el cliente del prover.

## Relación con mi proyecto
Documentar este análisis muestra al tribunal que la "tasa de error" por fuga temporal fue considerada y acotada (Daubert).

## Referencias
- Kocher, "Timing Attacks…", CRYPTO 1996.
- "SoK: Computer-Aided Cryptography", IEEE S&P 2021 (herramientas constant-time).
