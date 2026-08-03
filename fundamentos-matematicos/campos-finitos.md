# Campos finitos: el suelo donde se construyen los circuitos ZK

## ¿Qué problema resuelve?
Los sistemas de pruebas SNARK transforman cualquier cómputo en **identidades polinómicas** que deben verificarse con exactitud y eficiencia. Eso exige una estructura algebraica donde sumar, restar, multiplicar y dividir (salvo por cero) siempre funcione y los resultados sean de tamaño fijo: un campo finito.

## Idea intuitiva
Un campo finito es como la recta numérica "enrollada": con los residuos módulo un primo $p$ (ver [[aritmetica-modular]]) se pueden hacer las cuatro operaciones sin salir jamás del conjunto $\{0,\dots,p-1\}$, y la división existe porque cada no nulo tiene inverso (Bézout, [[teoria-numeros]]).

## Definición formal
Un **campo** $(F, +, \cdot)$ es un anillo conmutativo con unidad donde todo $a \neq 0$ tiene inverso multiplicativo. Un **campo de Galois** $GF(q)$ o $\mathbb{F}_q$ es un campo con $q$ elementos; existe si y solo si $q = p^n$ con $p$ primo, y es único salvo isomorfismo. Casos:
- **Campos primos** $\mathbb{F}_p$: los residuos módulo $p$.
- **Campos de extensión** $\mathbb{F}_{p^n}$: polinomios módulo un irreducible de grado $n$ sobre $\mathbb{F}_p$.

Propiedades clave para ZK: el grupo multiplicativo $\mathbb{F}_q^*$ es **cíclico** de orden $q-1$ (existen raíces primitivas de la unidad, usadas en FFT/NTT), y un polinomio de grado $d$ tiene a lo sumo $d$ raíces — base del lema de Schwartz–Zippel: evaluar identidades en un punto aleatorio detecta diferencias con probabilidad $\le d/q$.

## Complejidad / garantías / limitaciones
Las operaciones cuestan $O(\log^2 p)$ (o mejor con NTT, $O(d \log d)$ para polinomios de grado $d$). Limitación práctica: solo operaciones nativas del campo son baratas en circuito; comparaciones y divisiones de enteros requieren gadgets extra.

## Aplicaciones concretas
R1CS y Plonkish expresan el cómputo como restricciones en $\mathbb{F}_p$; los compromisos polinómicos (KZG, FRI) viven sobre extensiones de estos campos (ver [[emparejamientos-bilineales]]).

## Relación con Midnight / Compact (si aplica)
Los circuitos privados de Compact se aritmetizan sobre el campo escalar de la curva de Midnight (BLS12-381/Jubjub en el ecosistema Zcash-Pallas según la versión); el tamaño del campo fija la precisión y los límites de los testigos.

## Relación con mi proyecto (si aplica)
Los hashes de evidencia de víctimas y las credenciales del perito se codifican como elementos de $\mathbb{F}_p$; la prueba ZK demuestra relaciones polinómicas sobre ellos sin revelarlos.

## Referencias
- Lidl & Niederreiter, *Finite Fields*, Cambridge University Press.
- Shoup, *A Computational Introduction to Number Theory and Algebra*, caps. 16–20.
- Schwartz, "Fast probabilistic algorithms for verification of polynomial identities", *JACM* 27(4), 1980.
