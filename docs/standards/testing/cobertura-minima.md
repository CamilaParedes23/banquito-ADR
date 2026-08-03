# Estándar — Pruebas unitarias y cobertura mínima

Status: Active; evidence to be attached
Sources: ADR-0011 y ADR-0012 reclasificados; lineamiento explícito del Proyecto Final.

- Aplicar pruebas unitarias a controllers y services de Core y Switch.
- Medir cobertura de línea con JaCoCo.
- Umbral mínimo: 70 % en los paquetes de controllers y services definidos en cada `pom.xml`.
- Ejecutar `mvn clean verify`; el pipeline debe fallar si no se cumple.
- R9-K debe probar como mínimo: entrada exitosa, rechazo funcional, replay equivalente, conflicto idempotente, preparación saliente, confirmación, rechazo con compensación, timeout pendiente, consulta y reverso.
- Las pruebas E2E deben validar saldos, reserva, Nostro/Vostro, asiento, auditoría, outbox, documento, notificación y métricas.
- Adjuntar `target/site/jacoco/index.html` o su artefacto CI por microservicio antes de declarar `Verified`.
