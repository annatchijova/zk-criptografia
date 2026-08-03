# Compact: el lenguaje de contratos de Midnight

## ¿Qué problema resuelve?
Escribir circuitos ZK a mano (R1CS, Halo2) exige criptografía avanzada. Compact permite a un desarrollador TypeScript definir contratos con privacidad sin entender el sistema de pruebas subyacente.

## Idea intuitiva
Es como TypeScript con dos mundos: lo que declarás en el `ledger` es público y on-chain; lo que entra por *witnesses* es privado y se procesa off-chain. El compilador vigila la frontera: si intentás filtrar un dato privado al estado público sin declararlo con `disclose()`, la compilación falla. Privacidad por defecto, revelación explícita.

## Definición formal
Compact es un lenguaje fuertemente tipado, estático y acotado (sin bucles no acotados), de sintaxis similar a TypeScript, que compila a circuitos ZK + API TypeScript. Elementos de un programa `.compact`:

- `pragma language_version` — versión del lenguaje.
- `import CompactStandardLibrary;` — librería estándar.
- `export ledger x: T;` — estado público on-chain.
- `witness f(...): T;` — callback privado provisto por el runner TypeScript.
- `export circuit nombre(args): T { ... }` — función central; las exportadas que tocan el ledger generan claves de verificación.
- `constructor(...)` — inicialización (máximo uno).
- `pure circuit` — circuito sin efectos sobre ledger ni witnesses.

Ejemplo mínimo:

```compact
pragma language_version 0.23;
import CompactStandardLibrary;

export ledger registro: Bytes<32>;

witness hashEvidencia(): Bytes<32>;
witness credencialPerito(): Bytes<32>;

export circuit registrar(sal: Bytes<32>): [] {
  const h = hashEvidencia();
  assert(h == persistentHash([hashEvidencia(), sal]), "evidencia inválida");
  registro = disclose(persistentHash([credencialPerito(), h]));
}
```

El compilador genera prover/verifier keys y tipos TypeScript (`Contract`, `Witnesses`, `Circuits`, `Ledger`).

## Complejidad / garantías / limitaciones
- Garantía clave: el compilador rechaza cualquier flujo de dato privado → público no envuelto en `disclose()` (error "potential witness-value disclosure").
- Limitación: lenguaje acotado; no hay recursión ni loops dinámicos (los circuitos deben ser de tamaño fijo). Ver [[limitaciones]].
- En 2025 el lenguaje pasó a llamarse **Minokawa** (Linux Foundation Decentralized Trust), versión 0.18, compilador 0.26.

## Aplicaciones concretas
- KYC sin exponer documentos, votación privada, scoring crediticio (ejemplo oficial ZKLoan).

## Relación con mi proyecto
Es el núcleo del prototipo: un circuito que prueba (1) posesión de credencial de perito habilitado y (2) conocimiento del hash de la evidencia de víctimas, publicando solo un commitment. Ver [[circuitos-compact]], [[tipos]].

## Referencias
- https://docs.midnight.network/compact/reference/compact-reference
- https://docs.midnight.network/tutorials/counter/smart-contract
- https://forum.midnight.network/t/major-compact-milestone-and-release/501
