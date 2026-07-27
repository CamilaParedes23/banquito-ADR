# ADR-0021 — Tratar timeouts remotos como pendientes de conciliación

Status: Proposed
Implementation Status: Not started
Date: 2026-07-25
Author: Lenin / Mateo
Lifecycle: Microservicios-cloud
ASR: ASR-01, ASR-02, ASR-05, ASR-07

## Decision

Un timeout hacia la cámara simulada no se interpreta como éxito ni fracaso definitivo. La operación pasa a `PENDING_RECONCILIATION` y solo un proceso de conciliación resuelve su estado.

## Context

El sistema remoto puede haber procesado la operación aunque la respuesta no llegue. Reintentar a ciegas o liberar fondos puede duplicar pagos. La decisión depende de ADR-0020 y del diseño de conciliación aún pendiente.

## Options considered

- **Proposed — estado pendiente y conciliación.**
- **Reintento inmediato:** descartado por duplicidad.
- **Fallo definitivo y liberación:** descartado por resultado remoto desconocido.

## Consequences

**Positive**
- Evita asumir resultados no confirmados.

**Negative / trade-offs**
- Introduce estados operativos pendientes y fondos inmovilizados hasta conciliar.

## Evidence

- `Switch V2 RF-06`
- `Alcance R9-K`
