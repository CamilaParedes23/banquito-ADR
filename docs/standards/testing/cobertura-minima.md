# Estándar — Pruebas unitarias y cobertura mínima

Status: Active; evidence to be attached
Sources: ADR-0011 y ADR-0012 reclasificados; lineamiento explícito del Proyecto Final.

- Aplicar pruebas unitarias a controllers y services de Core y Switch.
- Medir cobertura de línea con JaCoCo.
- Umbral mínimo: 70 % en el alcance definido por el curso.
- Ejecutar el control en `mvn verify` y bloquear el pipeline si no se cumple.
- Adjuntar reportes por microservicio antes de declarar `Verified`.
