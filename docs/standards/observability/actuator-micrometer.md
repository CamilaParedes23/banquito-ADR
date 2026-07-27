# Estándar — Actuator y Micrometer

Status: Active
Source: ADR-0013 reclasificado.

- Exponer health/readiness y métricas técnicas por microservicio.
- Usar Spring Boot Actuator y Micrometer donde aplique.
- No exponer información sensible en endpoints de administración.
- Mapear latencia, errores, CPU, memoria, JVM, pools y dependencias críticas.
- Centralizar tableros y alertas en la plataforma cloud durante el cierre de infraestructura.
