# Tasa de error en sistemas deterministas — clave para Daubert

## ¿Qué problema resuelve?
El tercer factor de *Daubert* (509 U.S. 579, 1993) exige reportar la "tasa de error conocida o potencial" de la técnica. ¿Pero cómo se reporta la tasa de error de un sistema que, por construcción, nunca falla igual dos veces ni falla distinto —por ejemplo, la verificación de una prueba ZK?

## Idea intuitiva
Una herramienta forense clásica puede acertar 97 de 100 veces: su tasa de error es estadística. Un verificador criptográfico es **determinista**: dado el mismo input, siempre produce el mismo output. Su "tasa de error" no se mide en ejecuciones sino en supuestos: ¿puede fallar el circuito, la implementación o el manejo humano?

## Definición formal
Para un sistema determinista conviene descomponer el error total en:
1. **Error del método matemático**: para un esquema ZK-SNARK (Groth16, PLONK), el *soundness error* es despreciable (< 2⁻¹²⁸ por diseño). Es cuantificable y publicable en el paper del protocolo.
2. **Error de implementación**: bugs en el circuito o el verificador. Se estima por auditorías, pruebas de cobertura y comparación con implementaciones de referencia —análogo al programa CFTT del [[nist-digital-evidence|NIST]] y al *black-box testing* (NISTIR 8387).
3. **Error de procedimiento humano**: identidad equivocada, evidencia mal adquirida (ver [[iso-27037]]). Se mitiga con controles documentados, no con estadística.
4. **Error de supuestos**: si la curva elíptica o el hash fueran vulnerados, el sistema falla en bloque (error catastrófico, no aleatorio).

Reporte recomendado: tabla con cada componente, su mecanismo de fallo y su cota (por ejemplo, "soundness ≤ 2⁻¹²⁸; implementación auditada por X en versión Y; procedimiento conforme ISO/IEC 27037").

## Alcance / garantías / limitaciones
La ventaja Daubert de un sistema determinista es que el error del método es demostrable, no meramente estimado. La limitación: el tribunal puede no aceptar cotas matemáticas sin explicación pedagógica; conviene acompañar con validación empírica de la implementación.

## Aplicaciones concretas
- Sección "Known Error Rate" del informe pericial que incluye pruebas ZK.
- Comparación con herramientas forenses tradicionales que sí reportan tasas empíricas (p. ej., herramientas de carving validadas por CFTT).

## Relación con Midnight / Compact (si aplica)
Los circuitos [[compact]] en [[midnight]] heredan el soundness del backend ZK; la auditoría del circuito y del compilador constituye la mitigación documentada del componente de implementación.

## Relación con mi proyecto (si aplica)
Mi informe pericial reporta: soundness del esquema, versión auditada del circuito de identidad del perito, y protocolo de adquisición. Así el factor Daubert 3 queda cubierto de forma más fuerte que con herramientas estadísticas.

## Referencias
- *Daubert v. Merrell Dow Pharmaceuticals*, 509 U.S. 579 (1993)
- NISTIR 8387, *Forensic Science Error Management*
- SWGDE, *Error Mitigation Analysis* ([[swgde]])
- Groth, *On the Size of Pairing-Based Non-interactive Arguments* (soundness)

Ver también: [[reproducibilidad-cientifica]]
