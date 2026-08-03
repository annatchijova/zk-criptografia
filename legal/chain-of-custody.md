# Cadena de custodia: de la bolsa de evidencia al commitment on-chain

## ¿Qué problema resuelve?
La cadena de custodia documenta quién tuvo la evidencia, cuándo, dónde y qué hizo con ella, para que el tribunal pueda confiar en que el objeto presentado es el mismo que se recolectó y no fue alterado ni sustituido.

## Idea intuitiva
Es un registro continuo e ininterrumpido: cada transferencia de custodia se anota (recolector, fecha, sello, destinatario). Un "eslabón roto" no invalida automáticamente la evidencia, pero reduce su peso y puede llevar a su exclusión si genera duda razonable sobre su integridad o autenticidad (Fed. R. Evid. 901; véase [[federal-rules-evidence]]).

## Definición formal
Requisitos tradicionales: (1) identificación única del ítem (etiqueta/sello); (2) registro cronológico de cada posesor y transferencia; (3) condiciones de almacenamiento que prevengan contaminación o alteración; (4) documentación de cualquier análisis que consuma o modifique la muestra; (5) disponibilidad para inspección de la contraparte. En lo digital, se añade: hash en el momento de adquisición, imágenes forenses bit a bit y registros de acceso ([[digital-evidence]], [[hashes-como-evidencia]]).

## Alcance / garantías / limitaciones
- Garantiza integridad y autenticidad procesales, no veracidad del contenido.
- Los registros en papel son alterables y dependen de la honestidad de cada custodio.
- Un gap de custodia puede explicarse; el estándar judicial es "probabilidad razonable" de no alteración.

## Aplicaciones concretas
Drogas en un juicio penal, dispositivos incautados, videos de cámaras, mensajes extraídos de teléfonos.

## Relación con Midnight / Compact (si aplica)
Un **commitment criptográfico** del hash de la evidencia, anclado on-chain con timestamp (véase [[timestamping-rfc3161]]), crea un punto de custodia inmutable y públicamente verificable sin revelar el contenido. Los eventos de transferencia pueden organizarse en una estructura de [[merkle-trees]] donde cada hoja es un evento de custodia (adquisición, transferencia, análisis) y la raíz comprometida on-chain prueba el historial completo. En Midnight, pruebas ZK en Compact permiten demostrar que un custodio autorizado registró el evento sin exponer su identidad ni la evidencia.

## Relación con mi proyecto (si aplica)
El perito registra el commitment de la evidencia de víctimas on-chain al recolectarla; mi circuito prueba después que posee la preimagen y el historial, sin revelar datos sensibles.

## Referencias
- Fed. R. Evid. 901(a).
- SWGDE, *Best Practices for Digital Evidence Collection* (swgde.org).
- NIST SP 800-86, *Guide to Integrating Forensic Techniques into Incident Response* (2006).
