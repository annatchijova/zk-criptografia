# Arquitectura de un prover

## ¿Qué problema resuelve?
Generar una prueba ZK es computacionalmente caro: el prover debe ejecutar el circuito y luego comprimir criptográficamente esa ejecución en una prueba corta. La arquitectura del prover determina si esto es viable en segundos sobre un laptop o requiere un clúster — una cuestión práctica decisiva para que el peritaje ZK sea usable en la práctica judicial.

## Idea intuitiva
El prover es como un notario que debe redactar un acta certificada de un proceso de miles de pasos: primero ejecuta el proceso completo (generación del [[witness-generation|witness]]), luego lo "sella" aplicando operaciones matemáticas pesadas (exponenciaciones en curvas elípticas, transformadas de Fourier sobre campos finitos). El sello es lo caro, no la ejecución.

## Definición formal
Dado un circuito $C$ con $n$ restricciones, un prover típico (Groth16, Plonk, Halo2, STARK) realiza:
1. **Witness generation**: evaluación de $C$, costo $O(n)$.
2. **FFT/NTT** para interpolación polinómica: $O(n \log n)$.
3. **MSM (multi-scalar multiplication)** en curvas elípticas (SNARKs) o compromisos hash (STARKs): $O(n)$ operaciones de grupo, dominantes en la práctica.

## Complejidad / garantías / limitaciones
- **Tiempo**: $O(n \log n)$ asintótico; en la práctica, ~1–10 s para circuitos de $10^6$ restricciones en hardware moderno.
- **Memoria**: $O(n)$; circuitos grandes (> $2^{24}$ restricciones) exigen técnicas de *streaming* o recursión.
- **Paralelización**: MSM y NTT son masivamente paralelizables (GPU/FPGA dan speedups de 5–20×). Es el foco de librerías como `arkworks`, `bellman` e `icicle`.
- **Limitaciones**: el prover necesita acceso al witness completo, así que debe correr donde residen los datos privados o bajo cómputo delegado confiable. En SNARKs con *trusted setup* (Groth16), además se requiere el CRS.

## Aplicaciones concretas
- Provers en wallets móviles (identidad, pagos privados).
- Servidores de prueba para rollups (validity proofs).
- Prover local del perito: prueba credencial + posesión de evidencia en su propio equipo.

## Relación con Midnight / Compact (si aplica)
Midnight usa pruebas tipo Halo2/Plonk sobre la curva BLS12-381; el prover corre del lado del cliente (en la wallet o dapp), preservando que los datos privados jamás toquen la cadena. Ver [[verifier-architecture]].

## Relación con mi proyecto (si aplica)
El perito corre el prover localmente: prueba "soy el colegiado Nº X habilitado" (o incluso "soy un colegiado habilitado", con membresía de conjunto) y "poseo evidencia con hashes comprometidos on-chain". Latencia objetivo < 10 s en hardware común, requisito de usabilidad bajo Daubert (el método debe ser *aplicable* y reproducible).

## Referencias
- Ben-Sasson et al., *SNARKs for C* (Groth16 engineering). https://eprint.iacr.org/2013/879
- Ingonyama, ICICLE (GPU acceleration). https://github.com/ingonyama-zk/icicle
- Midnight docs. https://docs.midnight.network
