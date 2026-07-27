# ADR-0020 — Generar una clave de idempotencia estable en el Switch y propagarla extremo a extremo

Status: Proposed
Implementation Status: Not started
Date: 2026-07-25
Author: Lenin / Mateo
Lifecycle: Microservicios-cloud
ASR: ASR-02, ASR-07
Supersedes: ADR-0019

## Decision

La clave de idempotencia Off-Us se genera una sola vez en el Switch y se propaga sin cambios por broker, contexto Interbancario, cámara simulada, contabilidad y conciliación.

## Context

Generar claves nuevas en cada salto rompe la detección de duplicados y la trazabilidad. El formato exacto y la persistencia todavía deben cerrarse en el contrato R9-K, por lo que permanece Proposed.

## Options considered

- **Proposed — clave estable de origen.**
- **Clave nueva por componente:** descartada.
- **Usar solo UUID del movimiento local:** descartado porque no identifica toda la operación externa.

## Consequences

**Positive**
- Permite replay seguro y conciliación unívoca.

**Negative / trade-offs**
- Todos los adaptadores deben preservar la clave y su semántica.

## Evidence

- `BancoBanQuito-Corev1 RF-06 como antecedente`
- `Alcance R9-K`
