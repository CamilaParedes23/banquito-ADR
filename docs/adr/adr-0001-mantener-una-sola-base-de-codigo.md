# ADR-0001 — Mantener una sola base de código para local y cloud

Status: Proposed
Date: 2026-07-18
Author: Lenin

## Decision

Cada microservicio mantiene un único repositorio y un único artefacto Maven, sin ramas ni bases de código diferenciadas por entorno. La diferencia entre ejecución local y cloud se resuelve exclusivamente mediante variables de entorno y el perfil de Spring activado por `SPRING_PROFILES_ACTIVE`, nunca duplicando clases o módulos por entorno.

## Context

Cada uno de los seis microservicios del Core (`admin`, `clientes`, `contable`, `documentos`, `notificaciones`, `transaccional`) define su configuración en `application.yml` con valores parametrizados mediante placeholders con default local (por ejemplo `${ACCOUNT_DB_URL:${DB_URL:jdbc:mysql://localhost:33064/...}}`), y añade `application-docker.yml` como configuración adicional para el entorno contenedorizado. El `dockerfile` y el `.env.example` de `banquito-core-admin` fijan `SPRING_PROFILES_ACTIVE=docker` para activar ese perfil en despliegue.

Problema/ASR: el proyecto debe ejecutarse de forma idéntica en las máquinas de los integrantes, en CI y en el entorno cloud, sin duplicar mantenimiento de código entre entornos.

Restricciones: equipo pequeño y tiempo académico limitado; ya existe un pipeline CI (`ci.yml`) que ejecuta `mvn clean verify` sobre la misma base de código.

Alternativas consideradas: mantener ramas Git separadas para local y cloud (descartada por riesgo de divergencia y doble mantenimiento); duplicar clases de configuración por entorno (descartada por violar el principio de una sola fuente de verdad y complicar el pipeline de CI).

Criterio de selección: reutilizar el mecanismo nativo de Spring Boot (perfiles + placeholders de propiedades), que ya está en uso y verificado en los seis microservicios.

Impacto: todo pipeline de CI/CD y despliegue Docker debe fijar explícitamente `SPRING_PROFILES_ACTIVE`; cualquier configuración nueva debe seguir el mismo patrón de placeholder con default local.

## Opciones consideradas

- **Base de código única con perfiles de Spring (adoptada)**: un solo repositorio y artefacto Maven por microservicio; la diferencia entre entornos se resuelve con `SPRING_PROFILES_ACTIVE` y placeholders. Ventaja: reutiliza el mecanismo nativo de Spring Boot ya en uso y verificado en los seis microservicios, sin mantenimiento duplicado.
- **Ramas Git separadas para local y cloud**: descartada por riesgo de divergencia entre ramas y doble esfuerzo de mantenimiento.
- **Clases de configuración duplicadas por entorno**: descartada por violar el principio de una sola fuente de verdad y complicar el pipeline de CI.

## Consecuencias

Positivas:
- Un único artefacto se ejecuta de forma idéntica en máquinas locales, CI y cloud, sin recompilar ni duplicar código.
- El pipeline CI (`ci.yml`) valida la misma base de código que se despliega, reduciendo el riesgo de divergencia.

Negativas:
- Todo pipeline de CI/CD y despliegue Docker queda obligado a fijar explícitamente `SPRING_PROFILES_ACTIVE`; un olvido en ese paso puede activar el perfil incorrecto.
- Cualquier configuración nueva debe seguir el mismo patrón de placeholder con default local, lo que exige disciplina del equipo para no romper el mecanismo.
