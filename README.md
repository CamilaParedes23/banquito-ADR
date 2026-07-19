# banquito-architecture

Repositorio documental de arquitectura del proyecto Banco BanQuito (Core + Switch de Pagos Masivos).

Contiene los Architectural Decision Records (ADR) del proyecto y la documentación de arquitectura, seguridad, pruebas, interbancario y evidencias asociadas.

## Fuente de verdad

Este repositorio (Markdown versionado en Git) es la única fuente de verdad documental. La publicación recomendada es un sitio generado con MkDocs Material a partir de `docs/`.

## Estructura

```
banquito-architecture/
├── README.md
├── mkdocs.yml
└── docs/
    ├── index.md
    ├── adr/
    │   ├── index.md              # Decision Log
    │   ├── ADR-0000-template.md  # Plantilla compacta
    │   └── adr-XXXX-*.md
    ├── architecture/
    ├── security/
    ├── testing/
    ├── interbank/
    └── evidence/
        └── historico/            # Versiones originales (pre-normalización) de los ADR históricos
```

## Cómo se gestionan los ADR

Ver `docs/adr/index.md` (Decision Log) y la Guía Operativa para la Gestión de ADR del proyecto para el flujo completo, formato institucional (Decision + Context únicamente), numeración, estados y criterios de calidad.

## Responsables

Ver la tabla de organización del equipo en la Guía Operativa. Kevin es el custodio de ADR (numeración, plantilla, Decision Log, revisión editorial y publicación).
