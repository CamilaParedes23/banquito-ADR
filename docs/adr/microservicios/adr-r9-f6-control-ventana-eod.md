# ADR R9-F.6 — Restringir el EOD a la ventana operativa con excepciones auditadas

Status: Accepted
Implementation Status: Verified
Date: 2026-06-19
Author: Lenin
Lifecycle: Microservicios
ASR: ASR-01, ASR-04

## Decision

Accounting consulta la ventana `CORE_CONTABLE` en Admin mediante gRPC. El EOD ordinario solo corre sobre la jornada activa, dentro de la ventana. Las ejecuciones fuera de fecha u horario requieren autorización administrativa, motivo obligatorio y auditoría `RUN_EOD_WINDOW_OVERRIDE`.

## Context

La fecha contable está desacoplada de la fecha física. Duplicar la ventana en Accounting crearía dos fuentes de verdad; permitir cierres arbitrarios reduciría la auditabilidad. Se mantiene Admin como autoridad de calendario y se admiten excepciones explícitas para escenarios controlados.

## Options considered

- **Selected — ventana centralizada en Admin + excepciones auditadas.**
- **Duplicar parámetros en Accounting:** descartado por divergencia.
- **Permitir cierres manuales libres:** descartado por riesgo operativo.

## Consequences

**Positive**
- Evita cierres fuera de jornada sin evidencia.
- Los rechazos no dejan efectos parciales.

**Negative / trade-offs**
- Accounting depende de Admin para ejecutar EOD.
- Las excepciones legítimas requieren intervención de un administrador.

## Evidence

- `banquito-core-estable-R9J/infra/PRUEBA_R9_F_6_CIERRE_UX_CONTROL.ps1`
- `banquito-core-estable-R9J/infra/PRUEBA_REGRESION_HERENCIA_R9_F_6.ps1`
