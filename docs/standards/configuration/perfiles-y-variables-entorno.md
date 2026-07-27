# Estándar — Perfiles y variables por entorno

Status: Active
Source: ADR-0003 reclasificado; implementa ADR-0001.

1. Mantener un solo artefacto por microservicio.
2. Usar placeholders y variables de entorno para valores externos.
3. Utilizar perfiles de Spring únicamente para agrupar configuración de ambiente; no duplicar código.
4. No almacenar secretos operativos en `application.yml`, `.env` versionado ni imágenes.
5. El pipeline debe fijar explícitamente el perfil y validar variables requeridas.
