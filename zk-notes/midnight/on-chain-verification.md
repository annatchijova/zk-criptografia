# Verificación on-chain de pruebas Compact

## ¿Qué problema resuelve?
Si el cómputo ocurre off-chain ([[off-chain-computation]]), ¿cómo confía la red en que se ejecutó correctamente sin ver los datos? La verificación on-chain es el ancla de confianza: el nodo comprueba la prueba y solo entonces aplica los cambios de estado público.

## Idea intuitiva
El nodo es un escribano con una lupa criptográfica: no lee el expediente, pero verifica que el sello (la prueba) corresponde exactamente a la receta pública del trámite (la verifier key del circuito). Sello válido → se protocoliza el acta (se actualiza el ledger). Sello inválido → se rechaza sin costo de estado.

## Definición formal
Cuando llega una transacción que invoca un circuito exportado que toca el ledger:
1. El nodo recupera la **verifier key** asociada al circuito (generada en la compilación, [[circuitos-compact]]).
2. Extrae las **entradas públicas** de la prueba: los valores `disclose()`-ados y los efectos declarados sobre el ledger.
3. Ejecuta el **verificador SNARK** (Halo2 sobre BLS12-381): acepta o rechaza en tiempo constante (~6 ms por prueba).
4. Si acepta: aplica atómicamente las escrituras al estado público del contrato junto con los efectos UTXO de la tx (modelo híbrido).
5. Si rechaza: la transacción es inválida y no entra al bloque ([[mempool]]).

También se verifican las firmas de las partes, el pago en DUST ([[gas]]) y las pruebas de los componentes shielded (Zswap).

## Complejidad / garantías / limitaciones
- Garantía de soundness: es computacionalmente inviable producir una prueba aceptada para una ejecución inválida.
- Verificación barata y de tamaño de prueba constante (~5 KB), independiente de la complejidad del cómputo.
- Limitación: el nodo solo verifica lo que el circuito declara; una lógica de contrato mal escrita se prueba "correctamente" (garbage in, gospel out).

## Aplicaciones concretas
- Todo circuito provable: contadores, tokens, DIDs, registros.

## Relación con mi proyecto
El tribunal no necesita confiar en el perito ni en nuestra DApp: cualquier nodo (o verificador independiente con la verifier key) confirma que la prueba pericial es válida respecto del contrato publicado. Esto respalda el criterio Daubert de "método testeable y con tasa de error conocida". Ver [[deployment]].

## Referencias
- https://docs.midnight.network/ — arquitectura de verificación
- https://docs.midnight.network/compact/reference/compact-reference — ProvableCircuits
- https://midnight.network/ — dual-state ledger
