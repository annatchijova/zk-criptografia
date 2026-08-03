# Hashes criptográficos como prueba de integridad en juicio

## ¿Qué problema resuelve?
Para admitir evidencia digital, el proponente debe demostrar que no fue alterada desde su adquisición. El hash criptográfico es el mecanismo técnico aceptado para esa demostración.

## Idea intuitiva
Un hash (SHA-256) es una huella digital del archivo: cualquier cambio de un solo bit produce un hash distinto. Comparar el hash calculado en la adquisición con el calculado en el juicio prueba identidad bit a bit sin inspeccionar todo el contenido ([[digital-evidence]]).

## Definición formal
Propiedades relevantes: determinismo, resistencia a preimagen y a colisiones. Jurídicamente, el hash opera como medio de **autenticación** bajo Fed. R. Evid. 901(b)(9) (proceso que produce resultados precisos) y soporta certificaciones bajo Rule 902(13)–(14) ([[federal-rules-evidence]]). Guías forenses lo prescriben: NIST SP 800-86 recomienda verificar imágenes con hashes; SWGDE exige documentar hash de adquisición y verificación; ISO/IEC 27037:2012 (identificación, colección y preservación de evidencia digital) lo estandariza internacionalmente.

## Alcance / garantías / limitaciones
- **Garantía**: integridad (detección de cualquier alteración posterior al hash).
- **Limitaciones**: no prueba autoría, veracidad ni fecha de creación del contenido; la resistencia a colisiones es un supuesto computacional (MD5 y SHA-1 están rotos: uso judicial desaconsejado; SHA-256 es el estándar vigente).
- El hash debe documentarse en el momento de la adquisición y dentro de la [[chain-of-custody]].

## Aplicaciones concretas
Verificación de imágenes forenses E01, integridad de videos de body-cams, correos y bases de datos incautadas. Los tribunales de EE. UU. aceptan rutinariamente hashes para autenticar archivos, y el bloque de génesis/transacciones ancladas en blockchain se ha usado para fijar hashes con timestamp ([[timestamping-rfc3161]]).

## Relación con Midnight / Compact (si aplica)
En Midnight, el hash de la evidencia se compromete on-chain (commitment) de forma inmutable y con timestamp, sin publicar el archivo. Un circuito Compact prueba en ZK que el perito posee la preimagen del commitment, cumpliendo el rol probatorio del hash más el de posesión, sin revelar datos de víctimas.

## Relación con mi proyecto (si aplica)
El núcleo probatorio de la investigación: hash SHA-256 de la evidencia → commitment on-chain en Midnight → prueba ZK de conocimiento de la preimagen por el perito acreditado.

## Referencias
- NIST SP 800-86, §3.1 (2006).
- ISO/IEC 27037:2012.
- SWGDE, *Best Practices for Digital Evidence Collection*.
- Fed. R. Evid. 901(b)(9), 902(13)–(14).
- NIST FIPS 180-4 (SHA-2).
