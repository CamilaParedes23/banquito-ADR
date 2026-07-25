# ADR-0016 — Representar Nostro/Vostro fuera de las cuentas de clientes

Status: Proposed
Date: 2026-07-18
Author: Lenin

## Decision

Las cuentas Nostro (lo que BanQuito mantiene en otros bancos) y Vostro (lo que otros bancos mantienen en BanQuito) se modelan como cuentas institucionales dentro del contexto Interbancario (ADR-0015), nunca como registros en la tabla de cuentas de clientes.

## Context

El documento Core V2 ya distingue explícitamente `ACCOUNT` (cuentas de clientes, pasivo del banco) de `INSTITUTIONAL_ACCOUNT` (cuentas internas del banco, como `1.1.0.01 Banco Central / Cámara de Compensación` en su plan de cuentas). El documento Switch V2 introduce la Cuenta de Encaje como "cuenta de alta liquidez que cada banco comercial está obligado por ley a mantener en el Banco Central" — conceptualmente análoga a una relación Nostro.

Problema/ASR: si Nostro/Vostro se modelaran como filas de la tabla de cuentas de clientes, se les aplicarían por error reglas de negocio pensadas para clientes (estados Activa/Inactiva/Bloqueada/Suspendida, titularidad Natural/Jurídica) que no tienen sentido para una relación con otro banco.

Restricciones: debe reutilizar el patrón ya validado de `INSTITUTIONAL_ACCOUNT` del Core V2 en vez de crear un tercer tipo de cuenta sin relación con el catálogo contable existente.

Alternativas consideradas: agregar un indicador `ES_NOSTRO_VOSTRO` a la tabla de cuentas de clientes existente (descartada, contamina el modelo de cuentas de clientes con semántica institucional); crear una tabla completamente aislada sin relación con el catálogo contable (descartada, rompería la trazabilidad con `ACCOUNTING_ACCOUNT`/`JOURNAL_ENTRY` ya definida en Core V2).

Criterio de selección: coherencia con el patrón `INSTITUTIONAL_ACCOUNT` ya aceptado y con el anexo de modelo contable del Core V2 (cuenta `1.1.0.01`).

Impacto: cualquier asiento contable que involucre Nostro/Vostro se genera con el mismo mecanismo de partida doble del Microservicio Contable, referenciando una cuenta institucional, nunca una cuenta de cliente.

## Opciones consideradas

- **Nostro/Vostro como cuentas institucionales dentro del contexto Interbancario (adoptada)**: reutiliza el patrón `INSTITUTIONAL_ACCOUNT` ya validado en Core V2. Ventaja: coherencia con el anexo de modelo contable del Core V2 (cuenta `1.1.0.01`), sin crear un tercer tipo de cuenta.
- **Agregar un indicador `ES_NOSTRO_VOSTRO` a la tabla de cuentas de clientes existente**: descartada por contaminar el modelo de cuentas de clientes con semántica institucional (estados Activa/Inactiva/Bloqueada/Suspendida, titularidad Natural/Jurídica no aplican a una relación con otro banco).
- **Crear una tabla completamente aislada sin relación con el catálogo contable**: descartada por romper la trazabilidad con `ACCOUNTING_ACCOUNT`/`JOURNAL_ENTRY` ya definida en Core V2.

## Consecuencias

Positivas:
- Evita aplicar por error reglas de negocio pensadas para clientes a una relación con otro banco.
- Mantiene la trazabilidad contable completa, ya que todo asiento Nostro/Vostro pasa por el mismo mecanismo de partida doble del Microservicio Contable.

Negativas:
- Depende de que el contexto Interbancario (ADR-0015) y su forma exacta queden definidos antes de poder modelar estas cuentas institucionales.
- Introduce acoplamiento explícito con el catálogo contable existente (`ACCOUNTING_ACCOUNT`), por lo que cualquier cambio futuro en ese catálogo debe considerar su impacto sobre Nostro/Vostro.
