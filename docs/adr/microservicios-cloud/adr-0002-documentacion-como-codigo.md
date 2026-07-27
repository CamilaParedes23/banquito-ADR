# ADR-0002 — Gestionar la documentación arquitectónica como código

Status: Accepted
Implementation Status: Verified
Date: 2026-07-25
Author: Kevin
Lifecycle: Microservicios-cloud
ASR: ASR-04, ASR-06

## Decision

Los ADR, ASR, estándares, diagramas y evidencias se mantienen en Markdown versionado en Git y se publican mediante MkDocs. Documentos ofimáticos pueden ser insumos o entregables, pero no sustituyen la fuente de verdad.

## Context

Las decisiones históricas estaban dispersas en documentos sin diff ni trazabilidad. El repositorio `banquito-ADR` materializa la alternativa seleccionada y permite revisión por Pull Request, historial y navegación unificada.

## Options considered

- **Selected — Markdown + Git + MkDocs.**
- **Word/Google Docs como fuente única:** descartado por divergencia y falta de diff.
- **Confluence como fuente única:** descartado; puede funcionar como espejo.

## Consequences

**Positive**
- Preserva el razonamiento y facilita revisión.
- El Decision Log se integra al ciclo de cambios.

**Negative / trade-offs**
- Exige disciplina editorial y actualización de enlaces/índices.
- El equipo debe mantener herramientas de publicación documental.

## Evidence

- `banquito-ADR/README.md`
- `banquito-ADR/mkdocs.yml`
- `banquito-ADR/docs/adr/index.md`
