# Cardano como arquitectura base de Midnight

## ¿Qué problema resuelve?
Cardano es una plataforma blockchain de tercera generación diseñada con rigor académico: separación de capas, modelo de contabilidad formal y consenso con seguridad demostrable ([[ouroboros]]). Su relevancia aquí es doble: es la base tecnológica e institucional sobre la que se construye Midnight, la cadena de protección de datos con ZK.

## Idea intuitiva
Cardano se concibió como "blockchain por diseño científico": cada componente nace de un paper revisado por pares antes de implementarse. Arquitectónicamente separa la **capa de liquidación** (CSL, transferencia de valor con modelo eUTXO) de la **capa de cómputo** (CCL, smart contracts), permitiendo evolucionar cada una sin comprometer la otra.

## Definición formal
- **Modelo eUTXO**: las transacciones consumen y producen salidas no gastadas con scripts de validación deterministas; a diferencia del modelo de cuentas (Ethereum), el efecto de una transacción es localmente predecible, lo que facilita verificación y paralelismo.
- **Consenso**: [[ouroboros]] (PoS con VRF), épocas de 5 días, slots de 1 segundo, finalidad probabilística ([[finality]]).
- **Gobernanza**: tesorería on-chain y, desde la era Voltaire, gobernanza con representantes (CIP-1694).
- **Partner chains**: marco para cadenas laterales que aprovechan la seguridad y los SPOs (stake pool operators) de Cardano; Midnight es el primer caso emblemático.

## Complejidad / garantías / limitaciones
- **Garantías**: seguridad formalmente probada del consenso; determinismo de scripts eUTXO; ~9 años de operación sin caídas de red.
- **Limitaciones**: throughput base modesto (~tps de dos dígitos, mitigado por Hydra y mejoras); privacidad nula en la capa base — de ahí la necesidad de Midnight; curva de aprendizaje de Plutus/Haskell.

## Aplicaciones concretas
- Liquidación de activos y staking delegado.
- Identidad (Atala PRISM) y trazabilidad de cadenas de suministro.
- Infraestructura de seguridad para Midnight y futuras partner chains.

## Relación con Midnight / Compact (si aplica)
Midnight hereda el ecosistema de Cardano: su token (NIGHT), sus validadores iniciales (SPOs) y el puente de activos con Cardano. El lenguaje Compact (basado en TypeScript) compila a circuitos ZK cuyas pruebas se verifican on-chain en Midnight, mientras Cardano aporta la capa de confianza y liquidez. Para el contraste probar/verificar, ver [[prover-architecture]] y [[verifier-architecture]].

## Relación con mi proyecto (si aplica)
El peritaje ZK se apoya en esta pila: Cardano/Midnight garantizan que los compromisos de evidencia son inmutables y públicamente auditables, mientras Compact permite probar la identidad del perito y la posesión de evidencia sin revelar datos — la combinación exacta que exige un análisis Daubert riguroso (metodología revisada por pares + tasa de error conocida).

## Referencias
- IOG. *Cardano Documentation*. https://docs.cardano.org
- Chakravarty et al. *The Extended UTXO Model* (2020). https://iohk.io/en/research/library/papers/the-extended-utxo-model/
- Midnight. https://midnight.network y https://docs.midnight.network
