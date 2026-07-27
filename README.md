# banquito-ADR

Fuente de verdad documental de arquitectura para Banco BanQuito.

## Evolución cubierta

1. **Monolito:** Core V1 y Switch V1.
2. **Microservicios:** Core R9-J y Switch V2.
3. **Microservicios-cloud:** Google API Gateway, Google OAuth y evolución hacia secretos/servicios administrados.

## Estructura

```text
docs/
├── adr/
│   ├── monolito/
│   ├── microservicios/
│   ├── microservicios-cloud/
│   ├── index.md
│   └── ADR-0000-template.md
├── asr/
├── standards/
├── governance/
├── functional/
└── evidence/
```

## Publicación local

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements-docs.txt
mkdocs serve
```

Validación previa a commit:

```powershell
mkdocs build --strict
```

## Gobierno

- Un ADR contiene una sola decisión con alternativas y trade-offs.
- `Status` y `Implementation Status` son ejes distintos.
- Los requisitos, estándares y pantallas se documentan fuera del Decision Log.
- Un ADR Accepted se reemplaza con otro ADR; no se reescribe para cambiar la historia.
