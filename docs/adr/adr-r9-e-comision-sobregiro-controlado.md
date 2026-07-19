# ADR R9-E — Liquidar la comisión después del lote con sobregiro controlado

Status: Accepted
Date: [Pendiente de confirmar con el historial del proyecto]
Author: Equipo Banco BanQuito

## Decision

La reserva inicial de un lote de pagos masivos cubrirá únicamente el monto principal del lote. El Switch informará una comisión máxima durante la reserva y enviará la comisión definitiva después del procesamiento, calculada sobre las líneas exitosas. El Core debitará esa comisión directamente de la cuenta matriz y separará contablemente el ingreso neto y el IVA.

El sobregiro se permitirá exclusivamente para la comisión de pagos masivos cuando la cuenta corresponda a una persona jurídica, sea corriente, esté activa, esté habilitada como cuenta matriz, tenga sobregiro habilitado y disponga de un límite positivo. El límite será estricto: si la comisión lo supera, la operación se rechazará sin efectos parciales y podrá reintentarse con la misma correlación bajo las reglas de idempotencia existentes. Esta autorización no aplicará a retiros, P2P, fondeo del lote ni otras operaciones.

## Context

La versión heredada del Switch calcula la comisión al terminar el procesamiento, con base en las líneas exitosas, y contempla que su cobro pueda llevar la cuenta matriz a sobregiro. Core V2 trasladó al Core la liquidación financiera y la separación contable entre comisión e IVA, pero no eliminó expresamente esa secuencia heredada.

En R9-D se reservaban anticipadamente el monto del lote y la comisión. Aunque esa alternativa evitaba cualquier sobregiro, no representaba el cálculo definitivo posterior al procesamiento. También se evaluaron un sobregiro ilimitado y la construcción de un módulo crediticio completo. El sobregiro ilimitado introducía un riesgo no gobernado, mientras que un módulo de crédito con intereses, mora, cartera y aprobaciones excedía el alcance del proyecto.

La entidad y la tabla `CUENTA` ya contienen `PERMITE_SOBREGIRO` y `LIMITE_SOBREGIRO`; por ello, la opción seleccionada reutiliza capacidades existentes y agrega un control de riesgo específico. La comisión deja de consumir la reserva del lote, se conserva la intención funcional heredada y no se introducen intereses, mora ni productos crediticios. Los registros históricos de R9-D se preservan y la migración V7 permanece aditiva y compatible.
