# ADR-0021 — Tratar timeouts remotos como pendientes de conciliación

Status: Accepted
Implementation Status: In progress
Date: 2026-07-25
Last Updated: 2026-08-03
Author: Lenin / Mateo
Lifecycle: Microservicios-cloud
ASR: ASR-01, ASR-02, ASR-05, ASR-07

## Decision

Un timeout o respuesta desconocida del banco remoto cambia la transferencia saliente a `PENDING_RECONCILIATION`. No se confirma, rechaza, libera ni reenvía con una llave nueva. El Switch consulta el estado remoto usando `sourceRoutingCode + paymentLineUuid` y resuelve después como `SETTLED` o `REJECTED`.

## Context

El banco remoto puede haber acreditado aunque la respuesta HTTP se pierda. Liberar fondos o reenviar a ciegas puede duplicar el pago.

## Options considered

- **Selected — estado pendiente y consulta idempotente.**
- **Reintento inmediato con nueva clave:** descartado por duplicidad.
- **Fallo definitivo y compensación automática:** descartado por resultado remoto desconocido.

## Consequences

**Positive**
- Evita asumir un resultado financiero no confirmado.
- Hace visible la deuda operativa mediante métricas.

**Negative / trade-offs**
- Mantiene fondos comprometidos hasta resolver la conciliación.

## Implementation evidence

- Estado `PENDING_RECONCILIATION`.
- Endpoint interno `/pending-reconciliation`.
- Consulta `/api/v1/interbank/transfers/status`.
- Gauge `banquito.interbank.pending.reconciliation`.
