# ADR-0004 — Usar Apigee como API Manager del despliegue cloud

Status: Proposed
Date: 2026-07-18
Author: Mathius

## Decision

El tráfico externo (Banca Web, canales) hacia los microservicios del Core y del Switch en el entorno cloud se enruta a través de Apigee como API Manager. Los microservicios no se exponen directamente a internet.

## Context

Estado actual: ninguno de los seis microservicios del Core revisados declara una dependencia de gateway propio en su `pom.xml`; exponen APIs REST/gRPC internas sin capa de borde en el código. El documento de requisitos "Switch de Pagos Masivos V2" (RF-05, "Gestión de Tráfico y Seguridad") exige explícitamente un API Gateway como único punto de entrada para la Banca Web, con políticas de *rate limiting*.

Problema/ASR: sin un gateway centralizado, cada microservicio tendría que replicar autenticación de borde, *rate limiting* y observabilidad de tráfico externo por su cuenta.

Restricciones: el despliegue cloud gestionado está fuera del alcance verificable desde este repositorio local. `[PENDIENTE DE CONFIRMAR: proveedor cloud exacto, configuración de productos/proxies Apigee — Mathius/Luis]`.

Alternativas consideradas: gateway propio basado en Spring Cloud Gateway u otro framework (descartada por la sobrecarga operativa de mantener infraestructura de gateway propia en un proyecto académico); exposición directa de cada microservicio sin gateway (descartada, incumple el RF-05 del Switch V2 y amplía la superficie de ataque).

Criterio de selección: Apigee es la herramienta de API Management ya asignada al equipo de despliegue cloud (Mathius/Luis) y cubre *rate limiting*, gestión de productos de API y punto único de entrada sin código adicional en cada microservicio.

Impacto: los microservicios deben delegar la autenticación de borde y el control de tráfico al gateway (ver ADR-0005) y no deben reimplementar esa validación internamente.

## Opciones consideradas

- **Apigee como API Manager (adoptada)**: punto único de entrada para tráfico externo, con *rate limiting* y gestión de productos de API. Ventaja: herramienta ya asignada al equipo de despliegue cloud, sin código adicional en cada microservicio.
- **Gateway propio (por ejemplo, Spring Cloud Gateway)**: descartada por la sobrecarga operativa de mantener infraestructura de gateway propia en un proyecto académico.
- **Exposición directa de cada microservicio sin gateway**: descartada por incumplir el RF-05 del Switch V2 y ampliar la superficie de ataque.

## Consecuencias

Positivas:
- Centraliza autenticación de borde, *rate limiting* y observabilidad de tráfico externo en un único componente, evitando que cada microservicio la replique.
- Cumple explícitamente el RF-05 del documento "Switch de Pagos Masivos V2".

Negativas:
- El despliegue cloud gestionado queda fuera del alcance verificable desde este repositorio local; la configuración exacta de productos/proxies Apigee está `[PENDIENTE DE CONFIRMAR: proveedor cloud exacto — Mathius/Luis]`.
- Introduce una dependencia crítica externa: si Apigee falla o está mal configurado, todo el tráfico externo hacia Core y Switch se ve afectado.
