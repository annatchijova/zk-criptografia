# Teoría de números: prerrequisitos mínimos para criptografía ZK

## ¿Qué problema resuelve?
Toda la criptografía moderna —y en particular los sistemas de pruebas de conocimiento cero— descansa sobre un puñado de resultados de teoría de números. Esta nota recoge **solo** los que se usan de verdad en [[curvas-elipticas]], [[campos-finitos]] y [[aritmetica-modular]], evitando el resto del temario clásico.

## Idea intuitiva
Los números enteros tienen una estructura "rígida" (factorización única, primos que no se pueden descomponer) que permite construir operaciones fáciles en una dirección y difíciles de revertir. Esa asimetría es el combustible de la criptografía.

## Definición formal
Resultados usados:

- **División euclídea**: para $a, b \in \mathbb{Z}$, $b \neq 0$, existen únicos $q, r$ con $a = bq + r$, $0 \le r < |b|$. Base de la [[aritmetica-modular]].
- **Máximo común divisor y algoritmo de Euclides**: calcula $\gcd(a,b)$ en $O(\log \min(a,b))$ pasos. Su versión extendida da $x, y$ con $ax + by = \gcd(a,b)$ (identidad de Bézout), lo que permite calcular **inversos modulares**: $a^{-1} \bmod p$ existe si y solo si $\gcd(a,p)=1$.
- **Teorema fundamental de la aritmética**: todo entero $>1$ factoriza de forma única como producto de primos.
- **Pequeño teorema de Fermat**: si $p$ es primo y $p \nmid a$, entonces $a^{p-1} \equiv 1 \pmod p$. Implica que el grupo multiplicativo módulo $p$ tiene orden $p-1$ (ver [[grupos]]).
- **Teorema chino del resto (CRT)**: si $n = pq$ con $\gcd(p,q)=1$, entonces $\mathbb{Z}/n\mathbb{Z} \cong \mathbb{Z}/p\mathbb{Z} \times \mathbb{Z}/q\mathbb{Z}$.

## Complejidad / garantías / limitaciones
Euclides y la exponenciación modular (cuadrados repetidos) son polinomiales; la factorización y el logaritmo discreto se creen super-polinomiales clásicamente. Esa brecha empírica, no demostrada, es un **supuesto**, no un teorema.

## Aplicaciones concretas
Generación de parámetros de curvas, cálculo de inversos en circuitos ZK, construcción de [[campos-finitos]] primos $\mathbb{F}_p$.

## Relación con Midnight / Compact (si aplica)
Los circuitos de Compact operan sobre el campo escalar de la curva subyacente; cada restricción aritmética se interpreta con estos resultados.

## Relación con mi proyecto (si aplica)
Probar "el perito posee una credencial válida" sin revelarla se reduce a relaciones modulares verificables en un campo finito.

## Referencias
- Hardy & Wright, *An Introduction to the Theory of Numbers*, 6ª ed., Oxford.
- Katz & Lindell, *Introduction to Modern Cryptography*, cap. 8 (preliminares matemáticos).
- Shoup, *A Computational Introduction to Number Theory and Algebra* (libre en shoup.net).
