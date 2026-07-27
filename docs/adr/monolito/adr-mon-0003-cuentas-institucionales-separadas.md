# ADR-MON-0003 — Separar las cuentas institucionales de las cuentas de clientes

Status: Accepted
Implementation Status: Verified
Date: 2026-07-25
Author: Equipo Banco BanQuito
Lifecycle: Monolito
ASR: ASR-01, ASR-03

## Decision

Las cuentas internas del banco —ingresos, impuestos, bóveda y otras posiciones institucionales— se modelan fuera de la tabla de cuentas de clientes. No se exponen ni se someten a las mismas reglas de titularidad y navegación de Banca Web.

## Context

El Switch V1 exige separar comisión e IVA y el Core debe conservar cuentas institucionales. Modelarlas como cuentas de clientes habría permitido aplicar estados, titularidad y exposición de canales que no corresponden al banco. La guía formal identifica esta separación como una medida de seguridad, cumplimiento y prevención de fraude.

## Options considered

- **Selected — tabla/aggregate institucional separado:** límites de dominio y exposición claros.
- **Una única tabla con bandera institucional:** descartada por riesgo de filtrado insuficiente y reglas mezcladas.
- **Contabilidad solo como valores parametrizados:** descartada por perder trazabilidad de movimientos.

## Consequences

**Positive**
- Reduce el riesgo de que un canal de clientes vea saldos internos del banco.
- Permite reglas y auditoría específicas para cuentas institucionales.

**Negative / trade-offs**
- Introduce repositorios, consultas y reconciliaciones diferenciadas.
- Los reportes consolidados deben integrar dos modelos de cuentas.

## Evidence

- `Guía de Trazabilidad-Requisitos-BD(1).docx pp. 6-9`
- `BancoBanQuito-RequisitosFuncionales-SwitchPagosMasivos(1).pdf pp. 4, 8`
