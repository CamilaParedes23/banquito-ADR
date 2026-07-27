# ADR-0016 — Representar Nostro/Vostro fuera de las cuentas de clientes

Status: Proposed
Implementation Status: Not started
Date: 2026-07-25
Author: Lenin
Lifecycle: Microservicios-cloud
ASR: ASR-01, ASR-07

## Decision

Modelar Nostro/Vostro como posiciones/cuentas institucionales del contexto Interbancario y nunca como cuentas de clientes.

## Context

Las reglas, titulares y estados de cuentas de clientes no corresponden a relaciones interbancarias. La opción propuesta extiende el patrón institucional del Core V2 y preserva trazabilidad contable.

## Options considered

- **Proposed — cuentas/posiciones institucionales.**
- **Bandera en ACCOUNT:** descartada por contaminación del modelo.
- **Tabla aislada sin vínculo contable:** descartada por pérdida de trazabilidad.

## Consequences

**Positive**
- Evita aplicar reglas retail/corporativas a bancos.

**Negative / trade-offs**
- Requiere integración explícita con el plan contable.

## Evidence

- `BancoBanQuito-Core-V2.pdf pp. 5-6, 15`
