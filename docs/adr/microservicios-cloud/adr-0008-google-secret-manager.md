# ADR-0008 — Adoptar Google Secret Manager con IAM y Workload Identity para secretos operativos

Status: Proposed
Implementation Status: Not started
Date: 2026-07-25
Author: Infraestructura cloud + responsables de microservicios
Lifecycle: Microservicios-cloud
ASR: ASR-03

## Decision

Las credenciales de base de datos, SMTP, client secrets y llaves de firma se almacenarán en Google Secret Manager y serán consumidas por las cargas mediante IAM de mínimo privilegio y Workload Identity. No se versionarán secretos operativos ni se incorporarán dentro de imágenes.

## Context

El proyecto final exige un baúl de secretos cloud. La variante actual aún usa variables/configuración local y el equipo reporta la externalización como pendiente. La decisión debe aprobarse antes del despliegue final porque afecta identidad de workloads, pipelines y rotación.

## Options considered

- **Proposed — Google Secret Manager + IAM + Workload Identity.**
- **Secretos en variables estáticas del pipeline:** descartado por rotación y exposición.
- **Archivos `.env` en servidores:** descartado por falta de gobierno.

## Consequences

**Positive**
- Elimina credenciales del repositorio y habilita auditoría/rotación.

**Negative / trade-offs**
- Requiere diseñar IAM, nombres, versiones, rotación y recuperación.
- Un error de permisos puede impedir el arranque de múltiples servicios.

## Evidence

- `Pendiente de implementación; declaración del equipo 2026-07-25`
