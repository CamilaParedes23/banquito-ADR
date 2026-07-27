# ADR-MON-0004 — Separar credenciales de clientes y usuarios operativos del banco

Status: Accepted
Implementation Status: Verified
Date: 2026-07-25
Author: Equipo Banco BanQuito
Lifecycle: Monolito
ASR: ASR-03

## Decision

Las credenciales de Banca Web y los usuarios internos del Core se almacenan y administran en modelos separados. Los roles de cliente no comparten la misma entidad ni ciclo de vida con cajeros, operadores o administradores.

## Context

El monolito debía servir a clientes empresariales y personal del banco. Un único modelo de usuario habría ampliado el impacto de errores de autorización y facilitado asignaciones de privilegios cruzadas. La Guía de Trazabilidad identifica la separación `WEB_CREDENTIAL` / `CORE_USER` como primera barrera de ciberseguridad.

## Options considered

- **Selected — identidades separadas por población:** reduce mezcla de privilegios.
- **Una única tabla de usuarios con roles:** descartada por elevar el riesgo de escalamiento accidental.
- **Proveedores de identidad completamente separados desde V1:** descartada por complejidad y alcance de la fase.

## Consequences

**Positive**
- Mejora separación de funciones y control de privilegios.
- Permite ciclos de activación y políticas distintas para clientes y empleados.

**Negative / trade-offs**
- Duplica parte de la lógica de gestión de identidad y auditoría.
- La consolidación futura en un IAM requiere migración y mapeo explícito.

## Evidence

- `Guía de Trazabilidad-Requisitos-BD(1).docx pp. 6-7`
- `banquito-monolito/core/README.md`
