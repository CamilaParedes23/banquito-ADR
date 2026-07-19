# ADR-0021 — Tratar timeouts remotos como operaciones pendientes de conciliación

Status: Proposed
Date: 2026-07-18
Author: Lenin + Mateo

## Decision

Cuando una llamada del contexto Interbancario hacia la Cámara de Compensación (simulada) agota su tiempo de espera sin respuesta confirmada, la operación no se marca automáticamente como "Fallida" ni se reintenta de forma ciega. Se marca en estado "Pendiente de Conciliación" para revisión y resolución posterior.

## Context

El documento Switch V2 (RF-06) exige registrar "de forma independiente el resultado de cada evento consumido (éxito, fallo de red, rechazo del Core por fondos insuficientes, cuenta inactiva, etc.)" sin que un fallo individual detenga la operación general, el mismo principio de resiliencia que el RF-04 del Switch V1 aplica a líneas individuales de un lote.

Problema/ASR: un timeout de red no significa que la operación falló en el sistema remoto; puede haberse procesado exitosamente del otro lado sin que la confirmación llegue a tiempo. Marcarla automáticamente como "Fallida" y liberar los fondos podría resultar en un pago duplicado si luego se reintenta; marcarla como "Exitosa" sin confirmación podría ocultar una falla real.

Restricciones: depende de que la operación tenga una clave de idempotencia estable (ADR-0020) para poder reconciliarla luego con el estado real en el sistema remoto.

Alternativas consideradas: reintentar automáticamente de inmediato ante cualquier timeout (descartada, riesgo de duplicar el pago si la operación original sí se procesó); marcar como fallida y liberar la posición prefondeada (ADR-0018) de inmediato (descartada, mismo riesgo de duplicidad si la operación remota sí tuvo éxito).

Criterio de selección: un estado intermedio explícito ("Pendiente de Conciliación") es el único que no asume un resultado no confirmado, siguiendo el mismo principio de resiliencia sin pérdida de información que ya rige el procesamiento de lotes en el Switch (V1 y V2).

Impacto: se requiere un proceso, manual o batch, de conciliación que resuelva estas operaciones contra el estado real reportado por la Cámara de Compensación; su diseño exacto queda `[PENDIENTE DE CONFIRMAR: Lenin + Mateo]`.
