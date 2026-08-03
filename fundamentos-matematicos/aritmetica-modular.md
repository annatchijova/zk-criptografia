# Aritmética modular: el "reloj" donde vive la criptografía ZK

## ¿Qué problema resuelve?
Las computadoras no manejan números de tamaño arbitrario de forma natural en protocolos algebraicos, y los esquemas criptográficos necesitan operaciones **acotadas, cerradas e invertibles**. La aritmética modular confina todo cálculo a un conjunto finito de residuos, lo que la hace el lenguaje nativo de [[campos-finitos]] y de los circuitos ZK.

## Idea intuitiva
Es la aritmética del reloj: si son las 10 y pasan 5 horas, son las 3, porque calculamos "módulo 12". Generalizando, trabajar módulo un primo $p$ significa que tras cada operación nos quedamos solo con el resto de dividir entre $p$. Todo queda dentro de $\{0, 1, \dots, p-1\}$.

## Definición formal
Decimos $a \equiv b \pmod n$ si $n \mid (a-b)$. La relación es de equivalencia y las clases forman el anillo $\mathbb{Z}/n\mathbb{Z}$, donde suma y producto están bien definidos sobre representantes. Cuando $n = p$ es primo, todo elemento no nulo tiene inverso multiplicativo (por la identidad de Bézout, ver [[teoria-numeros]]), y $\mathbb{Z}/p\mathbb{Z}$ es un **campo**, denotado $\mathbb{F}_p$. La exponenciación $g^x \bmod p$ se calcula en $O(\log x)$ multiplicaciones por cuadrados repetidos; invertirla (logaritmo discreto, ver [[grupos]] y [[curvas-elipticas]]) es el problema duro que da seguridad.

## Complejidad / garantías / limitaciones
- Suma, producto e inverso: tiempo polinomial en $\log p$.
- La "envoltura" (wrap-around) módulo $p$ es trampa común en circuitos: una suma de enteros que desborda el campo se reduce silenciosamente, y hay que acotar rangos explícitamente (*range checks*).
- El primo debe ser grande (~256 bits) para que el logaritmo discreto sea inviable.

## Aplicaciones concretas
Firmas ECDSA/Schnorr, compromisos de Pedersen, restricciones R1CS/Plonkish: todas son ecuaciones en $\mathbb{F}_p$.

## Relación con Midnight / Compact (si aplica)
Compact compila a restricciones sobre el campo escalar de la curva de Midnight; cada variable testigo es un elemento de $\mathbb{F}_p$ y cada igualdad se entiende módulo $p$.

## Relación con mi proyecto (si aplica)
La afirmación "conozco la firma del colegio de peritos sobre este hash de evidencia" se expresa como ecuaciones modulares; el verificador solo ve que el circuito se satisface, no los valores.

## Referencias
- Shoup, *A Computational Introduction to Number Theory and Algebra*, caps. 2 y 7.
- Menezes, van Oorschot & Vanstone, *Handbook of Applied Cryptography*, cap. 2 (libre en cacr.uwaterloo.ca/hac).
- Katz & Lindell, *Introduction to Modern Cryptography*, 3ª ed., §8.1.
