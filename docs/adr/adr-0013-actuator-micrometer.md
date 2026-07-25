# ADR-0013 — Instrumentar servicios con Actuator y Micrometer

Status: Proposed
Date: 2026-07-18
Author: Mathius

## Decision

Todos los microservicios del Core exponen Spring Boot Actuator con métricas Micrometer para observabilidad (health checks, métricas de aplicación). No se implementa un mecanismo de observabilidad alternativo o construido a medida.

## Context

Estado actual: `spring-boot-starter-actuator` está declarado en los seis `pom.xml` del Core (`admin`, `clientes`, `contable`, `documentos`, `notificaciones`, `transaccional`), lo que trae Micrometer de forma transitiva como motor de métricas.

Problema/ASR: el despliegue cloud (Apigee como gateway, más el orquestador de contenedores) necesita endpoints de salud y métricas estandarizados para health checks y observabilidad, sin construir instrumentación personalizada en cada uno de los microservicios.

Restricciones: `[PENDIENTE DE CONFIRMAR: backend/plataforma de métricas al que se exporta Micrometer en el entorno cloud — Mathius]`.

Alternativas consideradas: logging manual sin métricas estructuradas (descartada, no permite alertas ni dashboards agregados entre microservicios); instrumentación propia por servicio, por ejemplo endpoints `/health` custom (descartada, redundante frente al estándar de Spring Boot ya disponible sin código adicional).

Criterio de selección: Actuator/Micrometer es el estándar de facto de Spring Boot, ya presente en los seis servicios sin configuración adicional, y compatible con la mayoría de plataformas de observabilidad cloud.

Impacto: los endpoints de Actuator deben protegerse adecuadamente detrás del API Gateway (ADR-0004) para no exponer información operativa interna a clientes externos no autorizados.

## Opciones consideradas

- **Spring Boot Actuator + Micrometer (adoptada)**: health checks y métricas de aplicación estandarizados. Ventaja: estándar de facto de Spring Boot, ya presente en los seis microservicios sin configuración adicional, y compatible con la mayoría de plataformas de observabilidad cloud.
- **Logging manual sin métricas estructuradas**: descartada por no permitir alertas ni dashboards agregados entre microservicios.
- **Instrumentación propia por servicio (por ejemplo, endpoints `/health` custom)**: descartada por ser redundante frente al estándar de Spring Boot ya disponible sin código adicional.

## Consecuencias

Positivas:
- Health checks y métricas quedan disponibles de forma uniforme en los seis microservicios sin código de instrumentación adicional.
- Facilita observabilidad agregada (alertas, dashboards) sobre todo el ecosistema Core.

Negativas:
- El backend/plataforma de métricas al que se exporta Micrometer en el entorno cloud queda `[PENDIENTE DE CONFIRMAR: Mathius]`, lo que deja incompleta la cadena de observabilidad.
- Los endpoints de Actuator exponen información operativa interna (salud, métricas, posiblemente variables de entorno) que debe protegerse explícitamente detrás del API Gateway (ADR-0004); una mala configuración podría filtrar esos datos a clientes externos no autorizados.
