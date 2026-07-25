# ADR-0007 — Transformar Identity mediante una transición gradual

Status: Proposed
Date: 2026-07-18
Author: Mathius

## Decision

La migración del modelo de identidad propio (JWT + roles locales) al esquema OAuth2 administrado (ADR-0006) se ejecuta de forma incremental, microservicio por microservicio, permitiendo que ambos mecanismos coexistan temporalmente en lugar de un corte total simultáneo en todos los servicios.

## Context

Estado actual: los seis microservicios del Core ya tienen pruebas unitarias y flujos de seguridad basados en JWT propio funcionando y verificados (`mvn test` ejecutado exitosamente sobre `banquito-core-admin` y `banquito-core-documentos` en este proyecto).

Problema/ASR: reemplazar la identidad de golpe en todos los microservicios simultáneamente arriesga romper flujos ya validados y dificulta el rollback si algo falla durante el despliegue cloud.

Restricciones: equipo académico pequeño, sin ventana de mantenimiento formal fuera de las entregas del curso; cada microservicio debe seguir siendo desplegable de forma independiente durante la transición.

Alternativas consideradas: corte total ("big bang") migrando todos los microservicios a OAuth2 en un solo release (descartada, alto riesgo y difícil de revertir si un servicio falla); mantener JWT propio indefinidamente sin migrar (descartada, contradice directamente ADR-0006).

Criterio de selección: una transición gradual permite validar el nuevo esquema en un microservicio piloto antes de propagarlo al resto, reduciendo el radio de impacto de un fallo de migración.

Impacto: durante la transición coexisten dos mecanismos de validación de identidad en el ecosistema; el orden y calendario de migración por microservicio queda `[PENDIENTE DE CONFIRMAR: secuencia de migración — Mathius]`.

## Opciones consideradas

- **Migración incremental, microservicio por microservicio (adoptada)**: permite validar el nuevo esquema en un microservicio piloto antes de propagarlo. Ventaja: reduce el radio de impacto de un fallo de migración y preserva la posibilidad de rollback.
- **Corte total ("big bang") migrando todos los microservicios a OAuth2 en un solo release**: descartada por su alto riesgo y dificultad de revertir si un servicio falla.
- **Mantener JWT propio indefinidamente sin migrar**: descartada por contradecir directamente ADR-0006.

## Consecuencias

Positivas:
- Cada microservicio puede migrar sin arriesgar los flujos de seguridad ya validados en el resto del ecosistema.
- Permite rollback acotado a un solo microservicio si la migración falla, en lugar de un incidente generalizado.

Negativas:
- Durante la transición coexisten dos mecanismos de validación de identidad (JWT propio y OAuth2 administrado), lo que añade complejidad operativa temporal.
- El orden y calendario de migración por microservicio queda `[PENDIENTE DE CONFIRMAR: secuencia de migración — Mathius]`, lo que retrasa la planificación del corte completo.
