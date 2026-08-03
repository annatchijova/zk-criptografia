# Compromisos de Pedersen

## ¿Qué problema resuelve?
Un **commitment** es el equivalente digital de un sobre cerrado: publicás algo que te ata a un valor (no podés cambiarlo) pero no lo revela (nadie ve qué hay dentro). Los compromisos de Pedersen lo logran con curvas elípticas y, a diferencia de un hash clásico, son **homomórficos**: se pueden operar sin abrirlos.

## Idea intuitiva
Para comprometer un valor $v$, elegís un blinding aleatorio $r$ y publicás el punto $C = vG + rH$. Como nadie conoce el logaritmo discreto de $H$ respecto de $G$, $C$ parece un punto aleatorio (oculta $v$). Pero si después revelás $(v, r)$, cualquiera verifica la igualdad y no hay forma de que el mismo $C$ abra a otro valor. Además, $C_1 + C_2 = (v_1+v_2)G + (r_1+r_2)H$: los compromisos se suman como los valores.

## Definición formal
Sea $\mathbb{G}$ grupo cíclico de orden primo $q$, $G, H \in \mathbb{G}$ generadores con $\log_G H$ desconocido. El commitment a $v \in \mathbb{Z}_q$ es $\mathrm{Com}(v, r) = vG + rH$ con $r \xleftarrow{\$} \mathbb{Z}_q$.
- **Hiding perfecto**: $C$ es uniforme en $\mathbb{G}$ independientemente de $v$ (seguridad incondicional).
- **Binding computacional**: abrir $C$ a $(v', r') \neq (v, r)$ implica resolver DL, ya que $(v-v')G = (r'-r)H$.
- **Homomorfismo**: $\mathrm{Com}(v_1,r_1)+\mathrm{Com}(v_2,r_2)=\mathrm{Com}(v_1+v_2, r_1+r_2)$.

## Complejidad / garantías / limitaciones
- Costo: dos multiplicaciones escalar–punto; en circuito ZK, barato sobre curvas embebidas (ver [[pedersen-hash]]).
- Limitaciones: binding depende del DL; requiere generadores sin relación conocida (setup transparente si se derivan de forma verificable); un compromiso aislado no prueba rangos — se complementa con range proofs (Bulletproofs).

## Aplicaciones concretas
- Transacciones confidenciales (Monero, Mimblewimble, Confidential Assets): compromisos a montos cuya suma verifica conservación.
- Votación y subastas selladas.
- Building block de ZK proofs de rangos y de balance.

## Relación con Midnight / Compact (si aplica)
En Midnight, los compromisos a datos privados del ledger local usan esquemas de este estilo junto a hashes ZK-friendly ([[poseidon]]); Compact permite expresar la verificación de commitments dentro del circuito que genera la prueba.

## Relación con mi proyecto (si aplica)
Para el perito: se commitea su credencial y cada evidencia de víctima con Pedersen; gracias al homomorfismo se prueban agregados (cantidad de víctimas, suma de atributos) sin revelar nada, y el binding sostiene la cadena de custodia — coherente con la exigencia Daubert de fiabilidad demostrable.

## Referencias
- Pedersen, T., *Non-Interactive and Information-Theoretic Secure Verifiable Secret Sharing*, CRYPTO 1991.
- Maxwell, A., *Confidential Transactions* (2015). https://elementsproject.org/features/confidential-transactions
- Bulletproofs: Bünz et al., IEEE S&P 2018. https://eprint.iacr.org/2017/1066
