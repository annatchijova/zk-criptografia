# Emparejamientos bilineales: la lupa que hace sucintos a los SNARKs

## ¿Qué problema resuelve?
En una [[curvas-elipticas|curva elíptica]] se puede comprobar si $Q = kP$ conociendo $k$, pero no se pueden "multiplicar" dos puntos-secretos entre sí ni comparar productos ocultos. Los SNARKs necesitan verificar **igualdades entre productos de valores comprometidos** con unas pocas operaciones: eso es lo que aporta un pairing.

## Idea intuitiva
Un pairing es una máquina que toma dos puntos de curva y devuelve un elemento de un [[campos-finitos|campo finito]] multiplicativo, respetando los exponentes: $e(aP, bQ) = e(P,Q)^{ab}$. Gracias a eso, verificar "¿$ab = c$?" con los valores ocultos en los exponentes se reduce a comparar dos pairings — una única ecuación, sin conocer $a$, $b$ ni $c$.

## Definición formal
Sean $G_1, G_2$ subgrupos de orden primo $\ell$ de una curva pairing-friendly y $G_T$ subgrupo de $\mathbb{F}_{p^k}^*$. Un **emparejamiento bilineal** es $e: G_1 \times G_2 \to G_T$ con:
1. **Bilinealidad**: $e(P_1+P_2, Q) = e(P_1,Q)\,e(P_2,Q)$ y simétrico en el segundo argumento; equivale a $e(aP, bQ) = e(P,Q)^{ab}$.
2. **No degeneración**: existe $(P,Q)$ con $e(P,Q) \neq 1$.
3. **Computabilidad eficiente** (algoritmo de Miller, polinomial).

Las construcciones reales son el **pairing de Weil** y el de **Tate** (y variantes optimizadas: ate, óptimo ate). El **embedding degree** $k$ debe ser pequeño para eficiencia pero lo bastante grande para que el DLP en $\mathbb{F}_{p^k}^*$ siga siendo duro — de ahí curvas como BLS12-381.

## Complejidad / garantías / limitaciones
Un pairing cuesta varias veces una multiplicación escalar; la verificación Groth16 son 3 pairings, prueba de tamaño constante (~200 bytes). Limitaciones: la bilinealidad también facilita ciertos ataques (MOV transfiere el ECDLP a $\mathbb{F}_{p^k}^*$); requiere curvas especiales; y el setup de confianza de Groth16 depende de estos grupos.

## Aplicaciones concretas
Verificación sucinta de SNARKs (Groth16, compromisos KZG), firmas BLS agregables, cifrado basado en identidad (Boneh–Franklin).

## Relación con Midnight / Compact (si aplica)
Midnight usa un SNARK estilo PLONK/Halo2 con compromisos polinómicos: los pairings (o acumuladores IPA en Halo2) son el mecanismo que permite al verificador on-chain comprobar la prueba en milisegundos sin ver los testigos.

## Relación con mi proyecto (si aplica)
La prueba de "perito acreditado + evidencia íntegra" se verifica on-chain con coste fijo gracias a pairings: el juez o contrato verifica validez sin acceder a datos de víctimas — clave para la reproducibilidad exigida por Daubert sin divulgación indebida.

## Referencias
- Boneh & Franklin, "Identity-Based Encryption from the Weil Pairing", *SIAM J. Comput.* 32(3), 2003 (CRYPTO 2001).
- Groth, "On the Size of Pairing-Based Non-interactive Arguments", EUROCRYPT 2016.
- Barreto & Naehrig, "Pairing-Friendly Elliptic Curves of Prime Order", SAC 2005.
- Miller, "The Weil Pairing, and Its Efficient Calculation", *J. Cryptology* 17(4), 2004.
