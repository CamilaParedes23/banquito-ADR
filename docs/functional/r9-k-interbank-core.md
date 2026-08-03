# R9-K — Implementación Core Nostro/Vostro

Status: Implemented and validated locally; pending QA/staging cloud verification.

## Límites

- El Switch clasifica y orquesta.
- El Core emisor reserva/consume, registra la transferencia y la posición Nostro.
- El Core receptor valida/acredita y registra la posición Vostro.
- Accounting conserva el único libro mayor.
- `reservationUuid` nunca cruza entre bancos.
- El broker permanece en el Switch.

## Estados

- `PREPARED`: efecto emisor registrado; aún no hay resultado remoto definitivo.
- `SETTLED`: banco receptor confirmó liquidación.
- `REJECTED`: rechazo definitivo; en salidas se compensó el efecto local.
- `PENDING_RECONCILIATION`: respuesta remota desconocida; no liberar ni duplicar.
- `REVERSED`: liquidación compensada con registros inmutables.

## Evidencia por línea

Emisor: instrucción, transferencia, consumo Off-Us, movimiento de reserva, asiento Nostro, auditoría y referencias remotas.

Receptor: transferencia, crédito de cuenta, asiento Vostro, auditoría, outbox, comprobante, documento y notificación.

## Contratos

- Externo: `docs/contracts/interbank-core-v1.yaml`.
- Interno Switch/Core: `docs/contracts/switch-core-interbank-lifecycle-v1.yaml`.

## Seguridad técnica

La migración registra `bank-banquill-interbank-client` en estado `INACTIVO`. El equipo de infraestructura o el seed E2E deberá provisionar el hash real del secreto desde el mecanismo seguro del ambiente y activar el cliente. No se versiona ningún secreto funcional en Git.

## Verificación

Validado localmente:

1. Flyway Admin V7, Accounting V5, Security V9 y Account V10.
2. Pruebas unitarias y reglas de cobertura.
3. Seed E2E v6 con posiciones prefondeadas.
4. Entrada, salida, replay, conflicto, rechazo, timeout, conciliación y reverso.
5. E2E10 y E2E100 con cuadre contable y cero pendientes al cierre.
6. Métricas Prometheus y auditoría.

Pendiente:

1. Despliegue QA/staging.
2. Validación de secretos, IAM, imágenes y digests.
3. Integración cloud con Switch y Banco BanQuill o simulador certificado.
4. E2E cloud y evidencia de aceptación.