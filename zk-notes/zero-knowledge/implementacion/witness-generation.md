# Generación de witness

## ¿Qué problema resuelve?
Una prueba de conocimiento cero (ZK) no surge de la nada: alguien debe traducir los datos privados del mundo real (una firma, un hash, un listado de evidencias) al formato algebraico que el circuito entiende. Ese artefacto es el **witness**: la asignación concreta de valores a todos los cables del circuito que satisface sus restricciones.

## Idea intuitiva
Pensemos en un crucigrama: el enunciado público es la grilla con las definiciones (la *statement*), y el witness es la solución completa, palabra por palabra. Quien prueba conoce la solución; el circuito solo verifica que encaja. Sin una generación de witness correcta, el prover no puede producir ninguna prueba válida, por más que conozca el secreto "conceptualmente".

## Definición formal
Dado un circuito aritmético $C(x, w)$ sobre un campo $\mathbb{F}_q$, donde $x$ es la entrada pública y $w$ la privada, el witness es un vector $w$ y una asignación completa $a \in \mathbb{F}_q^m$ tal que $C(x, w) = 0$ (todas las restricciones se satisfacen). En sistemas tipo [[plonk]] o [[halo2]], generar el witness equivale a evaluar el circuito puerta a puerta ("witness trace"). En Midnight, el lenguaje Compact compila a circuitos y el witness se genera localmente en el cliente a partir del estado privado del usuario.

## Complejidad / garantías / limitaciones
- **Costo**: lineal en el tamaño del circuito $O(|C|)$ en tiempo y memoria; suele ser la fase más barata de la prueba, pero domina la latencia de pre-cómputo.
- **Garantía**: si el witness es correcto, el prover honesto siempre produce pruebas que verifican (completitud).
- **Limitación**: cualquier error de redondeo, encoding (p. ej., de bytes a elementos de campo) o endianness invalida silenciosamente el witness. La generación debe ocurrir **en el dispositivo del usuario** para preservar la privacidad: enviar los datos crudos a un servidor destruye la propiedad ZK.

## Aplicaciones concretas
- Probar que un hash de evidencia forense pertenece a una cadena de custodia firmada, sin mostrar la evidencia.
- Probar mayoría de edad desde un documento de identidad.
- Probar solvencia sin revelar saldos.

## Relación con Midnight / Compact (si aplica)
En Compact, el desarrollador define el circuito; el witness se construye en el *witness generation code* que corre en la wallet/nodo del usuario, combinando el ledger público con el estado privado local. Ver [[prover-architecture]].

## Relación con mi proyecto (si aplica)
Para el peritaje ZK: el witness codifica (1) la credencial del perito emitida por el colegio profesional y (2) los hashes compromiso de la evidencia de víctimas. La generación ocurre en el equipo del perito, de modo que ni la identidad ni la evidencia salen de su control — clave para la fiabilidad exigida por el estándar Daubert.

## Referencias
- Thaler, J. *Proofs, Arguments, and Zero-Knowledge* (cap. sobre aritmetización). https://people.cs.georgetown.edu/jthaler/ProofsArgsAndZK.html
- Documentación de Midnight / Compact. https://docs.midnight.network
