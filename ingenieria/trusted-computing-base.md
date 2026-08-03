# Trusted Computing Base (TCB)

## ¿Qué problema resuelve?
No todo el sistema puede ser verificado matemáticamente. Es indispensable distinguir qué componentes hay que **confiar** ciegamente de los que cualquiera puede **verificar**, para minimizar y auditar lo primero.

## Idea intuitiva
El TCB es el conjunto mínimo de piezas que, si fallan o mienten, rompen toda la seguridad. Todo lo demás debería poder comprobarse sin confianza.

## Definición formal
En un sistema ZK tipo Midnight, el TCB típico incluye:
1. **Implementación del verificador** y las primitivas (curva, hash) — se audita, pero en tiempo de ejecución se confía.
2. **El circuito compilado** (Compact → circuito aritmético): si el compilador introduce una restricción faltante, la prueba no significa lo que creemos. *Under-constrained circuits* son la fuente dominante de bugs ZK reales.
3. **Setup de parámetros** (si aplica) — véase [[security-assumptions]].
4. **Generación de claves del perito** y el hardware/software donde reside la clave privada.
5. **Oráculo de datos de entrada**: la prueba solo afirma algo sobre los *witnesses* que le dieron; "basura entra, prueba válida sale".

En cambio, la **verificación on-chain**, las salidas públicas y la validez matemática de la prueba son verificables por terceros.

## Complejidad / garantías / limitaciones
Objetivo: TCB mínimo y auditable. Limitación: la corrección del circuito no se demuestra con tests solos; se requieren revisiones, herramientas de verificación formal o linters de circuitos (ej. Ecne, Circomspect para Circom; equivalentes emergentes para Compact).

## Aplicaciones concretas
En el flujo perito/evidencia: el registro de peritos habilitados es parte del TCB (quién decide qué claves son "peritos válidos").

## Relación con Midnight / Compact
El contrato Compact y su compilador forman parte del TCB; el ledger público y la verificación, no.

## Relación con mi proyecto
Declarar el TCB explícitamente fortalece la defensa Daubert: muestra que sabemos exactamente dónde está el riesgo residual.

## Referencias
- Saltzer & Schroeder, "The Protection of Information in Computer Systems", Proc. IEEE 1975 (TCB).
- "SoK: What Don't We Know? Understanding Security Vulnerabilities in SNARKs" (USENIX Security 2024).
