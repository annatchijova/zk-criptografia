# VRF (Verifiable Random Function)

## ¿Qué problema resuelve?

Un VRF genera un valor pseudoaleatorio **determinístico pero impredecible** a partir de una clave secreta y una entrada, junto con una prueba pública de corrección. Resuelve el dilema: necesito aleatoriedad verificable (no manipulable por su generador) pero sin revelar la clave que la produjo. En nuestro contexto: obtener un **identificador pseudoaleatorio no vinculable** de un perito.

## Idea intuitiva

Es como una firma digital cuya "firma" parece aleatoria y es única por entrada, pero que además puede publicarse en dos versiones: la salida completa (que actúa como número aleatorio verificable) y una prueba que cualquiera verifica con la clave pública. Si el perito usa como entrada un dominio distinto por caso ("caso-2025-017"), el identificador resultante no es vinculable entre casos.

## Definición formal

Un VRF es una tupla de algoritmos $(\mathsf{KeyGen}, \mathsf{Eval}, \mathsf{Verify})$:

- $\mathsf{Eval}_{sk}(x) \to (y, \pi)$, donde $y$ es pseudoaleatorio y $\pi$ una prueba.
- $\mathsf{Verify}_{pk}(x, y, \pi) \to \{0,1\}$.

Propiedades: **unicidad** (a lo sumo un $y$ válido por $(pk, x)$), **pseudoaleatoriedad** ($y$ indistinguible de uniforme sin $sk$), **verificabilidad** pública.

Construcciones: ECVRF (RFC 9381, basada en curvas elípticas y pruebas tipo [[schnorr|Schnorr]] con [[fiat-shamir]]), VRF RSA, y construcciones con [[bls|BLS]] donde la propia firma es la salida.

## Complejidad / garantías / limitaciones

- ECVRF: evaluación y verificación en 2–3 exponenciaciones; pruebas de ~80–100 bytes.
- Garantías bajo supuestos estándar (DDH / oráculo aleatorio).
- Limitación clave: el identificador es vinculable **si se reusa la misma entrada y la misma clave** — la no-vinculabilidad exige separación de dominios (distintas entradas por contexto). La clave pública usada para verificar sí puede vincular.

## Aplicaciones concretas

Selección de líderes en Algorand y Cardano (Ouroboros Praos), DNSSEC (NSEC5), sorteos verificables, rate-limiting privado (Privacy Pass), identificadores rotatorios.

## Relación con Midnight / Compact (si aplica)

Cardano ya usa VRF en consenso; en Midnight un circuito Compact puede evaluar o verificar un ECVRF in-circuit para emitir un identificador de perito estable dentro de un caso pero distinto entre casos.

## Relación con mi proyecto (si aplica)

El perito deriva $y = \mathsf{VRF}_{sk}(\text{ID-caso} \| \text{rol})$: pseudónimo verificable, impredecible para terceros, no vinculable entre casos, y complementario a los [[nullifiers-nonces|nullifiers]] para evitar doble uso.

## Referencias

- Micali, Rabin, Vadhan, "Verifiable Random Functions", FOCS 1999.
- RFC 9381, "Verifiable Random Functions (VRFs)", IETF, 2023.
- Goldberg et al., "Verifiable Random Functions (VRFs)" (Internet-Draft CFRG, ECVRF).
