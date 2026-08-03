# Validity Proofs (ZK-Rollups)

## ¿Qué problema resuelve?
En un rollup la L1 debe convencerse de que la transición de estado fue correcta sin re-ejecutarla. Los optimistic asumen validez y esperan impugnaciones ([[optimistic-rollups]]); los validity proofs **demuestran matemáticamente** la validez de cada batch, eliminando la ventana de disputa.

## Idea intuitiva
El operador no dice "créeme o impúgame", sino "aquí tienes un certificado criptográfico de que ejecuté correctamente las 10.000 transacciones". Verificar el certificado cuesta milésimas de lo que costaría re-ejecutar — es una prueba de conocimiento sucinta (ZK-SNARK/STARK).

## Definición formal
Por cada batch con transición $S \xrightarrow{T} S'$ el prover genera $\pi$ tal que $Verify(vk, (R, R', \text{hash}(T)), \pi) = 1$ si y solo si existe una ejecución válida de $T$ llevando el estado de raíz $R$ ([[state-commitments]]) a $R'$. Propiedades: **completitud** (ejecución honesta → prueba aceptada), **solidez** (transición inválida → ninguna prueba pasa, salvo probabilidad negligible) y opcionalmente **conocimiento-cero** (la prueba no revela datos privados). El contrato verificador en L1 actualiza la raíz solo si $\pi$ verifica. Los datos del batch se publican para [[data-availability]].

## Complejidad / garantías / limitaciones
- Probar: costoso (segundos–minutos, hardware potente); verificar: $O(1)$ o $O(\log)$, ~200–500k gas.
- Finalidad: inmediata tras verificación on-chain (retiros en minutos, no 7 días).
- Limitaciones: complejidad de circuitos, compatibilidad EVM (zkEVM tipos 1–4), riesgo de bugs en el circuito/verificador.

## Aplicaciones concretas
- StarkNet (STARKs), zkSync Era, Scroll, Polygon zkEVM, Loopring.

## Relación con Midnight / Compact (si aplica)
Midnight usa exactamente este mecanismo como núcleo de privacidad: el contrato Compact se compila a un circuito; la wallet genera localmente una validity proof (ZK-SNARK) de que la transición —p. ej. "soy un perito acreditado y poseo evidencia válida"— es correcta, sin revelar los testigos privados.

## Relación con mi proyecto (si aplica)
La acreditación del perito y la posesión de evidencia de víctimas se demuestran con una validity proof sobre las raíces del padrón y del lote de evidencias ([[merkle-trees]], [[sparse-merkle-trees]]): el tribunal verifica sin acceder a datos sensibles, cumpliendo el requisito Daubert de método empíricamente comprobable.

## Referencias
- Ben-Sasson et al. (2018). *Scalable, transparent, and post-quantum secure computational integrity* (STARK), IACR ePrint 2018/046.
- Groth (2016), *On the Size of Pairing-based Non-interactive Arguments* (SNARKs).
- https://docs.midnight.network
