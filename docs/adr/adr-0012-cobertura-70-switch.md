# ADR-0012 — Medir al menos 70 % de cobertura en controllers y services del Switch

Status: Proposed
Date: 2026-07-18
Author: Julio

## Decision

El o los microservicios del Switch de Pagos Masivos aplican la misma regla que el Core (ADR-0011): cobertura mínima del 70 % en los paquetes `controller` y `service`, verificada con JaCoCo en `mvn verify`, con el mismo criterio de falla del build en CI.

## Context

Mismo requisito institucional que ADR-0011, aplicado ahora al Switch de Pagos Masivos.

Estado actual: `[PENDIENTE DE CONFIRMAR — el repositorio del Switch de Pagos Masivos no está presente en el workspace de trabajo revisado (`banquito-core-admin`, `-clientes`, `-contable`, `-documentos`, `-notificaciones`, `-transaccional` no incluyen un servicio "switch"); no fue posible verificar si ya existe configuración de JaCoCo ni pruebas unitarias sobre ese código]`.

Problema/ASR: mismo requisito institucional que ADR-0011 — cobertura mínima del 70 % en Controladores y Servicios, aplicado a todos los microservicios del proyecto, incluido el Switch.

Restricciones: responsable Julio (pruebas unitarias del Switch); verificación fuera del alcance de este repositorio hasta que el código del Switch esté disponible para revisión.

Alternativas consideradas: ninguna evaluada todavía por falta de acceso al código del Switch.

Criterio de selección: coherencia con ADR-0011, para mantener un único criterio de calidad de pruebas en todo el ecosistema Banco BanQuito (Core y Switch), tal como exige el requisito institucional original.

Impacto: Julio debe confirmar si el o los microservicios del Switch ya tienen JaCoCo configurado con esta regla y su estado de cobertura actual; este ADR permanece `Proposed` hasta esa confirmación.

## Opciones consideradas

- **Aplicar la misma regla que ADR-0011 al Switch (adoptada)**: 70 % mínimo en `controller`/`service`, verificado por JaCoCo en `verify`. Ventaja: mantiene un único criterio de calidad de pruebas en todo el ecosistema Banco BanQuito (Core y Switch), tal como exige el requisito institucional original.
- **Ninguna alternativa evaluada todavía**: por falta de acceso al código del Switch en el workspace revisado, no fue posible comparar esta regla contra otras configuraciones de cobertura.

## Consecuencias

Positivas:
- Mantiene un único estándar de calidad de pruebas entre Core y Switch, evitando criterios divergentes entre equipos.

Negativas:
- El estado actual del Switch (repositorio, configuración de JaCoCo, cobertura existente) es `[PENDIENTE DE CONFIRMAR — el repositorio del Switch de Pagos Masivos no está presente en el workspace de trabajo revisado]`, por lo que no se puede verificar si la regla ya se cumple o si exige trabajo adicional.
- El ADR permanece en `Proposed` hasta que Julio confirme el estado del código del Switch, lo que retrasa su adopción formal.
