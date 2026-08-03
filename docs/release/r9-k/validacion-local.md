# Banco BanQuito — Evidencia resumida de validación local R9-K

Fecha: 2026-08-03
Estado: validación local aprobada; pendiente QA/staging cloud.

## Repositorios

Los siete repositorios R9-K quedaron en `main`, alineados con `origin/main`, sin cambios locales, sin commits pendientes y sin stash.

## Pruebas unitarias y cobertura

| Servicio | Pruebas | Resultado |
|---|---:|---|
| Admin | 131 | Aprobado; cobertura cumplida |
| Clientes | 125 | Aprobado; cobertura cumplida |
| Contable | 142 | Aprobado; cobertura cumplida |
| Documentos | 38 | Aprobado; cobertura cumplida |
| Notificaciones | 83 | Aprobado; cobertura cumplida |
| Transaccional | 302 | Aprobado; cobertura cumplida |
| Seguridad | Compilación | Build exitoso; sin pruebas configuradas |

## E2E de 10 operaciones

Resultado: `E2E_R9K_10_APROBADO`.

- Matriz: 2,000,000.00 → 1,999,030.00
- Nostro: 1,000,000.00 → 999,530.00
- Vostro: 1,000,000.00 → 999,790.00
- Reserva: 1,450.00; On-Us 500.00; Off-Us 470.00; liberado 480.00
- Instrucciones: 10; 7 ejecutadas; 2 rechazadas; 1 reversada
- Salientes: 6; 3 liquidadas; 2 rechazadas; 1 reversada; 0 pendientes
- Entrantes: 3; 1 liquidada; 1 rechazada; 1 reversada
- Auditoría: 21
- Outbox: 4
- Pendientes de conciliación al cierre: 0

## E2E de 100 operaciones

Resultado: `E2E_R9K_100_APROBADO`.

- Líneas: 100; 60 On-Us; 40 Off-Us
- Off-Us: 30 liquidadas; 8 rechazadas; 2 reversadas; 0 pendientes
- Monto total: 6,054.50
- On-Us: 2,432.70
- Off-Us neto liquidado: 2,591.20
- Liberado: 1,030.60
- Matriz final: 1,994,976.10
- Nostro final: 997,408.80
- Vostro final: 1,000,000.00
- Instrucciones: 100; 90 ejecutadas; 8 rechazadas; 2 reversadas
- Auditoría: 192
- Métrica de pendientes al cierre: 0
- Duración: 328.303 s
- Throughput secuencial: 0.305 líneas/s

## Escenarios interbancarios cubiertos

- `CONFIRM`: `PREPARED → SETTLED`
- `REJECT`: `PREPARED → REJECTED`
- `PENDING_CONFIRM`: `PREPARED → PENDING_RECONCILIATION → SETTLED`
- `CONFIRM_REVERSE`: `PREPARED → SETTLED → REVERSED`
- `PENDING_REJECT`: `PREPARED → PENDING_RECONCILIATION → REJECTED`
- Replay idempotente equivalente sin doble movimiento
- Conflicto idempotente con payload diferente
- Reverso mediante asiento compensatorio, sin borrar el asiento original

## Versiones Flyway verificadas

- Admin V7
- Clientes V4
- Contable V5
- Seguridad V9
- Notificaciones V5
- Transaccional V10

No se utilizó `flyway clean`. Las restauraciones de migraciones históricas se realizaron en Git para preservar el contenido validado y evitar cambios de checksum.

## Pendiente de cierre

1. Crear tags `r9-k-core-rc1` sobre los commits del BOM.
2. Construir imágenes y registrar digests.
3. Desplegar QA/staging.
4. Validar health, Flyway, OAuth2, secretos, conectividad e integraciones externas.
5. Ejecutar E2E10 y E2E100 en nube.
6. Aprobar evidencia cloud y crear el tag final `r9-k`.
