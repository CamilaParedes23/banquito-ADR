# Especificación funcional — Experiencia interbancaria en canales

Status: Draft for R9-K
Source: ADR-0022 reclasificado.

Los canales Banca Web Personas y Ventanilla/Backoffice deberán:

- distinguir visualmente operaciones On-Us y Off-Us;
- informar tiempos de liquidación diferentes;
- mostrar estados como pendiente de conciliación;
- consultar la clasificación y el estado desde el backend, sin replicar reglas financieras;
- conservar los límites del sistema: los frontends son canales externos al Core.

El detalle visual y el canal exacto donde se habilitará cada función requieren validación UX/funcional, no un ADR.
