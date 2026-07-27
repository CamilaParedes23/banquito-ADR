# ADR-0017 — Mantener un único plan contable para operaciones On-Us y Off-Us

Status: Proposed
Implementation Status: Not started
Date: 2026-07-25
Author: Lenin
Lifecycle: Microservicios-cloud
ASR: ASR-01, ASR-07

## Decision

Registrar On-Us y Off-Us en el mismo Plan Único de Cuentas y Microservicio Contable, agregando cuentas institucionales bajo la jerarquía existente. No crear un libro mayor paralelo.

## Context

Dos libros mayores duplicarían EOD, balance y regla de suma cero. La decisión preserva una sola fuente contable corporativa, pero todavía debe validarse con el modelo físico de R9-K.

## Options considered

- **Proposed — libro mayor único.**
- **Libro separado interbancario:** descartado por duplicación y conciliación adicional.

## Consequences

**Positive**
- Cuadre corporativo único.

**Negative / trade-offs**
- Accounting recibe nuevas responsabilidades y contratos interbancarios.

## Evidence

- `BancoBanQuito-Core-V2.pdf RF-09 y Anexo 1`
