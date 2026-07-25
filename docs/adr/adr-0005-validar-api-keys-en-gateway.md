# ADR-0005 — Validar API Keys en el API Manager y no en la lógica de negocio

Status: Proposed
Date: 2026-07-18
Author: Mathius

## Decision

La validación de API Keys de los clientes (canales, Banca Web) ocurre exclusivamente en Apigee (el API Gateway definido en ADR-0004). Los microservicios de negocio no implementan lógica propia de validación de API Key.

## Context

Estado actual: ningún microservicio del Core revisado implementa validación de API Key en sus controllers; la seguridad interna actual se basa en JWT propio y roles validados vía `@PreAuthorize` (dependencia `io.jsonwebtoken:jjwt` confirmada en `banquito-core-admin`).

Problema/ASR: evitar duplicar lógica de autenticación de borde en los seis (o más) microservicios y mantenerla centralizada en el componente donde ya se decidió ubicar el gateway (ADR-0004).

Restricciones: esta decisión depende de que ADR-0004 (Apigee) esté implementado; sin gateway, no hay dónde validar la API Key de forma centralizada.

Alternativas consideradas: validar la API Key en cada microservicio mediante un filtro compartido en una librería común (descartada, duplica lógica de seguridad en cada despliegue y dificulta la rotación de llaves); omitir la validación de API Key y confiar únicamente en JWT de usuario (descartada, no todos los consumidores externos — por ejemplo, integraciones de sistema a sistema — tienen un usuario autenticado vía JWT).

Criterio de selección: separación de responsabilidades — el gateway gobierna el acceso de clientes externos (quién puede llamar a la API), el microservicio gobierna la autorización de negocio (qué puede hacer ese usuario ya autenticado, vía JWT/roles).

Impacto: los microservicios deben asumir que toda petición que reciben ya superó la validación de API Key en el borde; no deben rechazar peticiones por ausencia de ese validador interno.

## Opciones consideradas

- **Validación exclusiva en Apigee (adoptada)**: la API Key se valida solo en el gateway (ADR-0004); los microservicios no implementan lógica propia. Ventaja: separación de responsabilidades clara entre acceso de clientes externos (gateway) y autorización de negocio (microservicio, vía JWT/roles).
- **Validar la API Key en cada microservicio mediante un filtro compartido en una librería común**: descartada por duplicar lógica de seguridad en cada despliegue y dificultar la rotación de llaves.
- **Omitir la validación de API Key y confiar únicamente en JWT de usuario**: descartada porque no todos los consumidores externos (por ejemplo, integraciones de sistema a sistema) tienen un usuario autenticado vía JWT.

## Consecuencias

Positivas:
- Evita duplicar lógica de autenticación de borde en los seis (o más) microservicios del Core.
- Centraliza la rotación y gestión de API Keys en un único componente (Apigee).

Negativas:
- Los microservicios quedan funcionalmente dependientes de que ADR-0004 (Apigee) esté implementado; sin gateway, no hay dónde validar la API Key de forma centralizada.
- Un microservicio que reciba tráfico sin pasar por Apigee (por ejemplo, en pruebas locales o por una mala configuración de red) quedaría sin ninguna validación de API Key.
