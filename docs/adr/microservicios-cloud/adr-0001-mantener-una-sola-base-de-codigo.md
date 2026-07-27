# ADR-0001 — Mantener una base de código única parametrizada por entorno

Status: Accepted
Implementation Status: Verified
Date: 2026-07-25
Author: Lenin
Lifecycle: Microservicios-cloud
ASR: ASR-06
Supersedes: ADR-0003 como decisión independiente

## Decision

Cada microservicio mantiene un único repositorio y artefacto. La variación local, Docker y cloud se resuelve mediante variables de entorno y perfiles de Spring; no se crean ramas, clases ni binarios distintos por ambiente.

## Context

La misma base debe compilarse en estaciones locales, CI y despliegue cloud. El código R9-J utiliza placeholders y perfiles de Spring. Durante la depuración se concluyó que ADR-0003 describe el mecanismo táctico de esta misma decisión y no amerita un ADR separado.

## Options considered

- **Selected — un artefacto y configuración externa por entorno.**
- **Ramas/bases de código por ambiente:** descartadas por divergencia.
- **Clases duplicadas por entorno:** descartadas por mantenimiento doble.

## Consequences

**Positive**
- Una sola fuente de verdad y un pipeline común.
- Reduce divergencia entre desarrollo y despliegue.

**Negative / trade-offs**
- La configuración externa y los perfiles deben gobernarse con disciplina.
- Un error de variables puede activar valores incorrectos sin cambiar el artefacto.

## Evidence

- `banquito-core-google-oauth-minimal/*/src/main/resources/application*.yml`
- `banquito-core-google-oauth-minimal/*/.github/workflows/*.yml`
