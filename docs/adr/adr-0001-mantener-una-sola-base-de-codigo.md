# ADR-0001 — Mantener una sola base de código para local y cloud

Status: Proposed
Date: 2026-07-18
Author: Equipo / PM

## Decision

Cada microservicio mantiene un único repositorio y un único artefacto Maven, sin ramas ni bases de código diferenciadas por entorno. La diferencia entre ejecución local y cloud se resuelve exclusivamente mediante variables de entorno y el perfil de Spring activado por `SPRING_PROFILES_ACTIVE`, nunca duplicando clases o módulos por entorno.

## Context

Cada uno de los seis microservicios del Core (`admin`, `clientes`, `contable`, `documentos`, `notificaciones`, `transaccional`) define su configuración en `application.yml` con valores parametrizados mediante placeholders con default local (por ejemplo `${ACCOUNT_DB_URL:${DB_URL:jdbc:mysql://localhost:33064/...}}`), y añade `application-docker.yml` como configuración adicional para el entorno contenedorizado. El `dockerfile` y el `.env.example` de `banquito-core-admin` fijan `SPRING_PROFILES_ACTIVE=docker` para activar ese perfil en despliegue.

Problema/ASR: el proyecto debe ejecutarse de forma idéntica en las máquinas de los integrantes, en CI y en el entorno cloud, sin duplicar mantenimiento de código entre entornos.

Restricciones: equipo pequeño y tiempo académico limitado; ya existe un pipeline CI (`ci.yml`) que ejecuta `mvn clean verify` sobre la misma base de código.

Alternativas consideradas: mantener ramas Git separadas para local y cloud (descartada por riesgo de divergencia y doble mantenimiento); duplicar clases de configuración por entorno (descartada por violar el principio de una sola fuente de verdad y complicar el pipeline de CI).

Criterio de selección: reutilizar el mecanismo nativo de Spring Boot (perfiles + placeholders de propiedades), que ya está en uso y verificado en los seis microservicios.

Impacto: todo pipeline de CI/CD y despliegue Docker debe fijar explícitamente `SPRING_PROFILES_ACTIVE`; cualquier configuración nueva debe seguir el mismo patrón de placeholder con default local.
