# ADR R9-E — Liquidar la comisión después del lote con sobregiro controlado

Status: Accepted
Implementation Status: Verified
Date: 2026-06-18 (reconstructed from evidence)
Author: Lenin
Lifecycle: Microservicios
ASR: ASR-01, ASR-02

## Decision

La reserva inicial cubre solo el monto principal del lote. La comisión definitiva se calcula sobre líneas exitosas y se debita después del procesamiento. Se permite sobregiro únicamente para la comisión de pagos masivos, bajo condiciones estrictas de cuenta jurídica, corriente, activa, matriz, habilitada y dentro de un límite duro.

## Context

V1 exigía el cobro posterior y permitía sobregiro; R9-D reservaba anticipadamente lote y comisión. Se eligió preservar la secuencia heredada, pero incorporando un límite de riesgo y una operación idempotente. Un sobregiro ilimitado y un módulo crediticio completo fueron descartados.

## Options considered

- **Selected — comisión posterior con sobregiro limitado.**
- **Reserva anticipada de lote y comisión:** segura, pero no preserva la secuencia heredada.
- **Sobregiro ilimitado:** descartado por riesgo.
- **Módulo de crédito completo:** descartado por sobreingeniería.

## Consequences

**Positive**
- Preserva compatibilidad funcional y evita sobregiro sin control.
- El cobro puede reintentarse sin duplicar el asiento.

**Negative / trade-offs**
- La configuración del límite se convierte en un control operativo crítico.
- Agrega validaciones específicas al flujo de pagos masivos.

## Evidence

- `banquito-core-estable-R9J/infra/evidencias-r9-e-comision-sobregiro-20260618-150939/ejecucion-completa.txt`
- `ADR_R9_E_COMISION_SOBREGIRO_CONTROLADO.md`
