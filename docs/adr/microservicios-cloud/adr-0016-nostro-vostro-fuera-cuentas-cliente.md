# ADR-0016 — Representar Nostro/Vostro fuera de las cuentas de clientes

Status: Accepted
Implementation Status: In progress
Date: 2026-07-25
Last Updated: 2026-08-03
Author: Lenin
Lifecycle: Microservicios-cloud
ASR: ASR-01, ASR-07

## Decision

Modelar Nostro/Vostro como cuentas institucionales vinculadas al Plan Único de Cuentas en `core-accounting-service`. Nunca se registrarán en la tabla `CUENTA` de clientes. Las cuentas externas solo se conservan como referencias de transferencia.

## Context

Las cuentas de clientes tienen titular, producto, estado y reglas retail/corporativas. Una posición corresponsal representa un activo o pasivo entre bancos y requiere naturaleza contable, moneda, contraparte y control de liquidez.

## Options considered

- **Selected — `CUENTA_INSTITUCIONAL` + `CUENTA_CONTABLE`.**
- **Bandera en `CUENTA`:** descartada por contaminación del modelo de clientes.
- **Tabla sin vínculo al libro mayor:** descartada por pérdida de cuadre y trazabilidad.

## Consequences

**Positive**
- Evita aplicar reglas de clientes a relaciones interbancarias.
- Integra posiciones al EOD y balance de comprobación.

**Negative / trade-offs**
- Cada contraparte y moneda requiere una cuenta funcional configurada.

## Implementation evidence

- Tipos `NOSTRO` y `VOSTRO` en `TipoCuentaInstitucionalEnum`.
- Metadatos `ROUTING_CODE_CONTRAPARTE`, `MONEDA` y `NUMERO_CUENTA_CORRESPONSAL`.
- `V5__nostro_vostro_institutional_accounts.sql`.
