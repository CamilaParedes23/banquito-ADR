# ADR-0008 — Externalizar secretos mediante un servicio administrado

Status: Proposed
Date: 2026-07-18
Author: [PENDIENTE DE CONFIRMAR — la Guía Operativa marca este responsable como pendiente]

## Decision

Las credenciales sensibles (contraseñas de base de datos, llaves de firma, credenciales de integración) dejan de vivir en archivos `.env`/`application.yml` versionados y pasan a un servicio de gestión de secretos administrado en el entorno cloud, inyectadas como variables de entorno en tiempo de despliegue.

## Context

Estado actual: los `application.yml` de los seis microservicios del Core usan placeholders con defaults locales (por ejemplo `${ACCOUNT_DB_URL:${DB_URL:jdbc:mysql://localhost:33064/...}}`) y `banquito-core-admin` incluye un `.env.example`, lo que indica que las credenciales reales ya se manejan fuera del código fuente para el entorno local. No se encontró evidencia de un gestor de secretos formal para el entorno cloud en este repositorio.

Problema/ASR: sin un gestor centralizado, las credenciales de entorno cloud dependerían de variables de entorno configuradas manualmente por cada integrante, sin rotación ni auditoría de acceso.

Restricciones: `[PENDIENTE DE CONFIRMAR: proveedor de gestión de secretos a adoptar — Mathius + Infraestructura]`.

Alternativas consideradas: mantener archivos `.env` configurados manualmente por servidor cloud (descartada, no auditable ni rotable, y propensa a divergencia entre entornos); commitear secretos cifrados dentro del repositorio (descartada, alto riesgo si se filtra la llave de cifrado).

Criterio de selección: extiende el patrón ya adoptado en el proyecto (placeholders más variables de entorno) hacia un backend administrado, en lugar de gestión manual dispersa.

Impacto: el pipeline de despliegue cloud debe inyectar los secretos en tiempo de arranque del contenedor; ningún `application.yml` ni `.env` versionado debe contener valores reales de credenciales de producción.

## Opciones consideradas

- **Servicio de gestión de secretos administrado, inyectado como variable de entorno (adoptada)**: extiende el patrón ya adoptado en el proyecto (placeholders + variables de entorno) hacia un backend administrado. Ventaja: permite rotación y auditoría de acceso a credenciales, en vez de gestión manual dispersa.
- **Archivos `.env` configurados manualmente por servidor cloud**: descartada por no ser auditable ni rotable, y por ser propensa a divergencia entre entornos.
- **Commitear secretos cifrados dentro del repositorio**: descartada por el alto riesgo si se filtra la llave de cifrado.

## Consecuencias

Positivas:
- Las credenciales de producción dejan de residir en archivos versionados, reduciendo el riesgo de filtración por el repositorio Git.
- Habilita rotación y auditoría de acceso a secretos, algo que la gestión manual de `.env` no permite.

Negativas:
- El proveedor de gestión de secretos a adoptar queda `[PENDIENTE DE CONFIRMAR: Mathius + Infraestructura]`, lo que bloquea la implementación concreta.
- El responsable de este ADR también queda `[PENDIENTE DE CONFIRMAR — la Guía Operativa marca este responsable como pendiente]`, lo que retrasa su seguimiento.
