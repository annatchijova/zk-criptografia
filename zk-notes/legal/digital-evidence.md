# Evidencia digital: particularidades frente a la evidencia física

## ¿Qué problema resuelve?
La evidencia digital (archivos, logs, mensajes, metadatos) rompe los supuestos del forense clásico: no hay un "original" tangible, se altera con solo tocarla y se copia sin pérdida. El derecho debe adaptar autenticación, original y custodia a estas propiedades.

## Idea intuitiva
Una pistola no cambia por guardarla en una bolsa; un archivo sí cambia al abrirlo (timbres de acceso, metadatos). Además, una copia bit a bit es indistinguible del original, lo que convierte el hash —no el soporte— en la verdadera "huella" de la evidencia ([[hashes-como-evidencia]]).

## Definición formal
Propiedades distintivas:
1. **Mutabilidad**: el contenido y los metadatos se modifican con operaciones triviales; exige adquisición forense (write-blockers, imágenes bit a bit).
2. **Duplicabilidad perfecta**: copias idénticas sin degradación; la Rule 1001(d) acepta impresiones/salidas legibles como originales y la Rule 1003 admite duplicados salvo duda genuina ([[federal-rules-evidence]]).
3. **Volatilidad**: RAM y conexiones desaparecen al apagar; orden de volatilidad (RFC 3227, *Guidelines for Evidence Collection and Archiving*).
4. **Metadatos y contexto**: la evidencia incluye datos sobre datos (timestamps de sistema, EXIF), cuya fiabilidad depende del reloj y del sistema que los generó.
5. **Escala y dependencia de sistema**: se autentica el proceso que genera el registro (Rule 901(b)(9), 902(13)).

## Alcance / garantías / limitaciones
- Garantía: integridad demostrable vía hash si se adquiere correctamente.
- Limitación: un hash prueba que el archivo no cambió desde la adquisición, no que el contenido sea verdadero ni quién lo creó.
- Cadena de custodia digital exige registrar cada copia y acceso ([[chain-of-custody]]).

## Aplicaciones concretas
Imágenes forenses de discos (E01), extracciones de móviles (UFDR), logs de servidores, correos con cabeceras.

## Relación con Midnight / Compact (si aplica)
Commitments on-chain fijan el estado de la evidencia digital sin publicarla; las pruebas ZK de Compact permiten demostrar propiedades (posesión, autoría por perito acreditado) preservando confidencialidad de víctimas.

## Relación con mi proyecto (si aplica)
Mi evidencia de víctimas es digital y sensible: el proyecto explota exactamente la duplicabilidad (comprometo el hash) y la mutabilidad (anclo el estado con [[timestamping-rfc3161]] on-chain) sin exponer el contenido.

## Referencias
- RFC 3227, *Guidelines for Evidence Collection and Archiving* (2002).
- NIST SP 800-86 (2006).
- Fed. R. Evid. 1001(d), 1003, 901(b)(9).
