# Computación Multiparte Segura (MPC)

## ¿Qué problema resuelve?

MPC permite que $N$ partes que **no confían entre sí** computen conjuntamente una función sobre sus entradas privadas, revelando únicamente la salida. Resuelve el dilema de la colaboración sin confianza: nadie ve los datos de los demás, pero todos obtienen el resultado correcto. Es la base teórica de las ceremonias de [[trusted-setup-ceremonies|trusted setup distribuidas]].

## Idea intuitiva

Cada participante "reparte" su entrada en fragmentos (*secret sharing*) que por separado no revelan nada; el cómputo se realiza sobre los fragmentos mediante operaciones locales y mensajes entre pares; al final se reconstruye solo la salida. Mientras un umbral mínimo de participantes sea honesto, la privacidad se mantiene.

## Definición formal

Un protocolo MPC para $f(x_1, \dots, x_N)$ debe cumplir:

- **Corrección**: la salida es $f$ evaluada sobre las entradas efectivamente comprometidas.
- **Privacidad**: la vista de cualquier coalición corrupta es simulable a partir de sus entradas y la salida (paradigma real/ideal).
- **Robustez / fairness** (opcional): los honestos obtienen la salida aunque otros aborten.

Técnicas: secret sharing aditivo/Shamir, circuitos enmascarados (Yao, GMW, BGW), multiplicación con triples de Beaver (SPDZ), enmascaramiento homomórfico. Modelos: honesto-mayoría vs. deshonesto-mayoría; seguridad semi-honesta vs. maliciosa.

## Complejidad / garantías / limitaciones

- Sobrecosto típico de 10×–1000× sobre cómputo en claro; la comunicación suele ser el cuello de botella.
- Garantías condicionadas al umbral de honestidad (p. ej. $t < N/2$ corruptos en BGW malicioso).
- Un caso especial crucial: **MPC con una sola parte honesta basta para la seguridad del setup** — si al menos un participante destruye su fragmento secreto, los parámetros resultantes son seguros (propiedad usada por Zcash).

## Aplicaciones concretas

Subastas selladas (caso danés de remolacha, 2008), estadísticas salariales de Boston, firmas umbral (MPC-ECDSA), machine learning privado, y generación distribuida de parámetros criptográficos.

## Relación con Midnight / Compact (si aplica)

Los sistemas de pruebas que usa Midnight descienden de Groth16/Plonk y pueden requerir parámetros de referencia generados por MPC; además, MPC off-chain puede precomputar testigos que luego se prueban on-chain con Compact.

## Relación con mi proyecto (si aplica)

Varios peritos pueden computar conjuntamente un estadístico sobre evidencia de víctimas (p. ej. consistencia de hashes) sin que ninguno vea la evidencia completa de los demás, y luego probar el resultado en ZK.

## Referencias

- Yao, "Protocols for Secure Computations", FOCS 1982.
- Goldreich, Micali, Wigderson, "How to Play any Mental Game", STOC 1987.
- Ben-Or, Goldwasser, Wigderson, "Completeness Theorems for Non-Cryptographic Fault-Tolerant Distributed Computation", STOC 1988.
- Bogetoft et al., "Secure Multiparty Computation Goes Live", FC 2009.
