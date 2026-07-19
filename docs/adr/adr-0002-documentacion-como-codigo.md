# ADR-0002 — Gestionar la documentación arquitectónica como código

Status: Proposed
Date: 2026-07-18
Author: Kevin

## Decision

Toda la documentación de arquitectura del proyecto (ADR, Decision Log, diagramas de contexto y evidencia) se versiona como Markdown en el repositorio Git `banquito-architecture`, con publicación opcional mediante MkDocs Material. Word, PDF y Confluence pueden usarse como espejo o insumo de trabajo, pero nunca como fuente de verdad.

## Context

Estado actual: los dos ADR históricos (R9-E, R9-F.6) y la guía operativa llegaron como `.docx`/`.pdf` sueltos, sin control de versiones ni historial de cambios verificable. El repositorio `banquito-architecture` se creó en esta misma actividad para organizar ese material según la estructura acordada (`docs/adr`, `docs/architecture`, `docs/security`, `docs/testing`, `docs/interbank`, `docs/evidence`).

Problema/ASR: sin versionado, no es posible auditar quién cambió una decisión, cuándo, ni por qué, y el contenido puede divergir entre integrantes que trabajan sobre copias locales del mismo documento.

Restricciones: el equipo ya usa Git/GitHub para el código de los seis microservicios del Core; la Guía Operativa ADR Banquito (sección 8) ya define esta estructura de repositorio como recomendación institucional.

Alternativas consideradas: mantener los documentos en Word/Google Docs compartido (descartada, sin diff ni revisión por Pull Request); usar Confluence como fuente única (descartada explícitamente por la guía operativa, que solo la permite como portal o espejo).

Criterio de selección: coherencia con el flujo de trabajo ya adoptado para el código (Git, Pull Request, revisión) y alineación directa con la recomendación de la Guía Operativa.

Impacto: cada ADR nuevo se publica mediante Pull Request documental (ver flujo Git de la guía) y el Decision Log (`docs/adr/index.md`) debe actualizarse en el mismo PR que introduce el ADR.
