# Introducción a blockchain

## ¿Qué problema resuelve?
¿Cómo logran miles de participantes que no se conocen ni confían entre sí ponerse de acuerdo sobre un registro compartido (quién posee qué, qué ocurrió primero) sin una autoridad central que lo dicte? Es el problema del **consenso distribuido**, agravado por actores maliciosos (el clásico "problema de los generales bizantinos"). Las blockchains lo resuelven combinando criptografía, incentivos económicos y un protocolo de consenso.

## Idea intuitiva
Una blockchain es un libro contable público, replicado en miles de computadoras, donde las páginas (bloques) se encadenan con sellos criptográficos: alterar una página pasada rompe todos los sellos posteriores. Todos pueden verificar la copia; nadie puede reescribir el pasado sin convencer a la mayoría. La confianza se traslada de instituciones a matemáticas e incentivos.

## Definición formal
Una blockchain es una estructura de datos en cadena $B_0 \to B_1 \to \dots \to B_n$ donde cada bloque $B_i$ contiene el hash criptográfico de $B_{i-1}$, un conjunto de transacciones y metadatos del consenso. El protocolo define: (1) quién propone el siguiente bloque ([[consenso]]: PoW, PoS, BFT), (2) cómo se validan transacciones (máquina de estados determinista), y (3) cuándo un bloque se considera irreversible ([[finality]]).

## Complejidad / garantías / limitaciones
- **Garantías**: integridad (inmutabilidad bajo umbrales de adversario, típicamente <1/3 o <1/2 del poder), disponibilidad, transparencia auditables.
- **Limitaciones**: escalabilidad (trilema descentralización–seguridad–rendimiento), latencia de confirmación, costo energético (PoW), y privacidad nula por defecto — todo es público salvo diseños con ZK como [[cardano|Midnight sobre Cardano]].
- **Costos**: cada nodo completo almacena y re-ejecuta todo; el throughput global está acotado por el nodo más lento honesto.

## Aplicaciones concretas
- Monedas digitales y liquidación de pagos sin bancos centrales.
- Registros de existencia y cadena de custodia: anclar hashes de evidencia con timestamp inmutable.
- Identidad y credenciales verificables.
- Computación verificable (smart contracts, rollups ZK).

## Relación con Midnight / Compact (si aplica)
Midnight es una blockchain de protección de datos construida como cadena socia del ecosistema [[cardano]]: hereda su seguridad y usa pruebas ZK (lenguaje Compact) para que las aplicaciones prueben hechos sin publicar los datos subyacentes. Ver [[ouroboros]].

## Relación con mi proyecto (si aplica)
La blockchain aporta lo que el expediente judicial necesita: un registro inmutable con timestamp de los compromisos (hashes) de la evidencia de víctimas y de la credencial del perito. La capa ZK de Midnight permite probar posesión e identidad sin exponer datos sensibles, alineado con los criterios de fiabilidad del estándar Daubert.

## Referencias
- Nakamoto, S. *Bitcoin: A Peer-to-Peer Electronic Cash System* (2008). https://bitcoin.org/bitcoin.pdf
- Narayanan et al. *Bitcoin and Cryptocurrency Technologies*. Princeton UP.
- Lamport, Shostak, Pease. *The Byzantine Generals Problem* (1982).
