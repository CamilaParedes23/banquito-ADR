# ADR-0017 — Mantener un único plan contable para operaciones On-Us y Off-Us

Status: Accepted
Implementation Status: In progress
Date: 2026-07-25
Last Updated: 2026-08-03
Author: Lenin
Lifecycle: Microservicios-cloud
ASR: ASR-01, ASR-07

## Decision

Registrar operaciones On-Us y Off-Us en el mismo Plan Único de Cuentas y Microservicio Contable. Se agregan cuentas institucionales Nostro/Vostro bajo la jerarquía existente; no se crea un libro mayor paralelo.

## Context

Un segundo libro mayor duplicaría EOD, balance, reversos y regla de suma cero. El Core Contable ya es custodio de asientos y saldos institucionales.

## Options considered

- **Selected — libro mayor único.**
- **Libro interbancario separado:** descartado por duplicación y conciliación adicional.
- **Contabilidad en el Switch:** descartada por violar la responsabilidad del Core.

## Consequences

**Positive**
- Cuadre corporativo único y trazabilidad completa.
- Reutiliza partida doble, reversos y EOD.

**Negative / trade-offs**
- Accounting valida liquidez de posiciones Nostro/Vostro además de suma cero.

## Implementation evidence

- Asiento saliente: débito `FONDOS_RESERVADOS_PM`, crédito `NOSTRO_<BANCO>_<MONEDA>`.
- Asiento entrante: débito `VOSTRO_<BANCO>_<MONEDA>`, crédito `CLIENTES_PASIVO`.
- Validación de saldo no negativo en `AccountingService`.
