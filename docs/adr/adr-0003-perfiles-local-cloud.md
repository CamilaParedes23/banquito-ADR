# ADR-0003 — Separar configuración local y cloud mediante perfiles

Status: Proposed
Date: 2026-07-18
Author: Mathius

## Decision

Cada microservicio expone dos configuraciones activables por perfil de Spring: la configuración base/local (`application.yml`, con defaults para ejecución en la máquina del desarrollador) y el perfil `docker` (`application-docker.yml`), activado mediante `SPRING_PROFILES_ACTIVE=docker` en el contenedor. No se crean perfiles adicionales sin justificación registrada en un ADR posterior.

## Context

Confirmado en los seis microservicios del Core: cada uno tiene `application.yml` más `application-docker.yml`, y `banquito-core-admin` fija `ENV SPRING_PROFILES_ACTIVE=docker` en su `dockerfile` y en `.env.example`.

Problema/ASR: el mismo artefacto compilado debe correr en Docker Compose local, en CI y en el entorno cloud, sin recompilar ni mantener binarios distintos (relacionado con ADR-0001).

Restricciones: Spring Boot ya es el framework elegido para los seis microservicios; el mecanismo de perfiles debe ser el estándar del framework, no una solución custom.

Alternativas consideradas: variables de entorno puras sin perfiles de Spring (descartada, pierde la segmentación declarativa de propiedades por archivo); un único `application.yml` con todos los valores de todos los entornos mezclados (descartada, mezclaría configuración de desarrollo con valores sensibles de otros entornos en el mismo archivo).

Criterio de selección: uso del mecanismo estándar de Spring Boot (`spring.profiles.active`), ya validado y en uso en los seis microservicios sin necesidad de librerías adicionales.

Impacto: todo microservicio nuevo debe seguir el mismo patrón de dos archivos de configuración; el pipeline de despliegue cloud es responsable de fijar el perfil correcto en tiempo de arranque.

## Opciones consideradas

- **Perfiles de Spring (`application.yml` + `application-docker.yml`, adoptada)**: segmentación declarativa de propiedades por archivo, activada por `SPRING_PROFILES_ACTIVE`. Ventaja: mecanismo estándar de Spring Boot, ya validado en los seis microservicios sin librerías adicionales.
- **Variables de entorno puras sin perfiles de Spring**: descartada por perder la segmentación declarativa de propiedades por archivo.
- **Un único `application.yml` con todos los valores de todos los entornos mezclados**: descartada por mezclar configuración de desarrollo con valores sensibles de otros entornos en el mismo archivo.

## Consecuencias

Positivas:
- El mismo artefacto compilado corre en Docker Compose local, en CI y en el entorno cloud, sin recompilar ni mantener binarios distintos.
- La configuración de cada entorno queda declarativamente separada y fácil de auditar por archivo.

Negativas:
- Todo microservicio nuevo debe seguir el mismo patrón de dos archivos, lo que exige disciplina de equipo para no romper la convención.
- El pipeline de despliegue cloud es responsable de fijar el perfil correcto en tiempo de arranque; un error de configuración en ese paso puede activar el perfil equivocado.
