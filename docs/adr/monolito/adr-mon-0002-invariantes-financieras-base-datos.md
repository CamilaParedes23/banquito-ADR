# ADR-MON-0002 — Aplicar restricciones de base de datos como última línea de defensa financiera

Status: Accepted
Implementation Status: Verified
Date: 2026-07-25
Author: Equipo Banco BanQuito
Lifecycle: Monolito
ASR: ASR-01, ASR-02

## Decision

Las invariantes financieras críticas se validan en la aplicación y se refuerzan en el motor relacional mediante `UNIQUE`, `CHECK`, claves foráneas y tipos restringidos. El UUID transaccional es único y los estados/tipos inválidos no pueden persistirse.

## Context

La Guía de Trazabilidad V1 establece que la base de datos actúa como guardián final ante errores de aplicación, reintentos o despliegues defectuosos. La alternativa de validar solo en Java dejaba abierta la posibilidad de corrupción por scripts, concurrencia o fallos de programación. La decisión afecta el modelo transaccional completo y es costosa de revertir, por lo que se conserva como ADR retrospectivo.

## Options considered

- **Selected — validación en aplicación + constraints relacionales:** defensa en profundidad.
- **Validación solo en la aplicación:** descartada porque no protege accesos alternos ni errores de concurrencia.
- **Triggers para toda regla financiera:** descartada por ocultar lógica y aumentar complejidad operativa.

## Consequences

**Positive**
- Previene duplicidades y estados imposibles incluso si falla una capa superior.
- Mejora integridad referencial y auditabilidad.

**Negative / trade-offs**
- Las migraciones requieren mayor disciplina y pruebas de compatibilidad.
- Una regla incorrecta en el esquema puede bloquear operaciones válidas hasta corregir la migración.

## Evidence

- `Guía de Trazabilidad-Requisitos-BD(1).docx pp. 5-9`
- `banquito-monolito/core/sql/modeloFisicoBD_Core_v5_mariadb1011_cloud_v2.sql`
