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
