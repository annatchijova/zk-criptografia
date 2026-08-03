# Merkle Mountain Range (MMR)

## ¿Qué problema resuelve?
Un [[merkle-trees|árbol de Merkle]] clásico asume un conjunto fijo de hojas: añadir datos obliga a reconstruir todo. Para un **log que crece indefinidamente** (append-only), como un registro continuo de evidencias o bloques, se necesita un commitment que admita inserciones incrementales baratas y pruebas de pertenencia eficientes.

## Idea intuitiva
Mantén una lista de "montañas": árboles Merkle perfectos de tamaños potencias de dos, que son los "picos". Al añadir una hoja, se fusionan montañas del mismo tamaño (como sumar en binario). El commitment es la raíz combinada de los picos (bagging). Solo se guardan $O(\log n)$ raíces.

## Definición formal
Sea $n$ el número de hojas. La representación binaria de $n$ determina los picos: por cada bit 1 en la posición $k$ hay una montaña de tamaño $2^k$. La raíz es $R = H(P_1\|P_2\|\dots\|P_m)$ (o acumulado por bagging). Una prueba de inclusión consiste en el authentication path dentro de su montaña más los demás picos ($O(\log n)$). Se definen también pruebas de consistencia entre raíces en tiempos $t_1 < t_2$ (la raíz vieja es prefijo de la nueva).

## Complejidad / garantías / limitaciones
- Append: $O(1)$ amortizado. Proof: $O(\log n)$.
- Garantía: historial append-only verificable (transparency log); la seguridad depende de $H$.
- Limitación: no hay pruebas de no-pertenencia naturales (usar [[sparse-merkle-trees]]).

## Aplicaciones concretas
- Certificate Transparency / logs de transparencia (variantes).
- MMR en Grin/Beam (Mimblewimble) para comprometer el set [[utxo-eutxo-account|UTXO]].
- FlyClient y superlight clients ([[light-clients]]): muestrear pocos bloques con pruebas MMR.

## Relación con Midnight / Compact (si aplica)
Los historiales de commitments de estado o de transacciones cifradas pueden comprometerse en un MMR para que un verificador externo audite crecimiento append-only sin revelar contenido.

## Relación con mi proyecto (si aplica)
El registro de cadena de custodia ([[chain-of-custody]]) del expediente del perito es un MMR: cada ingreso/transferencia de evidencia es una hoja; se prueba que un evento ocurrió antes de cierta raíz sin exponer el resto del historial, y la prueba de consistencia demuestra que nadie reescribió el pasado.

## Referencias
- Todd, P. *Merkle Mountain Ranges* (OpenTimestamps docs).
- Bünz, B. et al. (2020). *FlyClient: Super-Light Clients for Cryptocurrencies*, IEEE S&P.
- Crosby & Wallach (2009). *Efficient Data Structures for Tamper-Evident Logging*, USENIX Security.
