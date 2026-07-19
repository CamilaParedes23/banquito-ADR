# ADR R9-E — Comisión posterior y sobregiro controlado en pagos masivos

## Estado

Aceptado para R9-E.

## Contexto

La versión heredada del Switch calcula la comisión al finalizar el procesamiento, con base en las líneas exitosas, y contempla que su cobro pueda llevar la cuenta matriz a sobregiro. Core V2 asigna al Core la liquidación financiera y la separación contable entre ingreso e IVA, pero no elimina explícitamente esa regla heredada.

R9-D reservaba anticipadamente el monto del lote y la comisión. Ese comportamiento impedía sobregiros, pero no representaba la secuencia heredada de comisión definitiva posterior al procesamiento.

La entidad y la tabla `CUENTA` ya contienen `PERMITE_SOBREGIRO` y `LIMITE_SOBREGIRO`, por lo que no es necesario introducir un módulo crediticio nuevo.

## Decisión

1. La reserva inicial cubre únicamente el monto del lote.
2. El Switch informa una comisión máxima al crear la reserva y posteriormente envía la comisión definitiva calculada sobre las líneas exitosas.
3. La comisión se debita directamente de la cuenta matriz y el Core separa el neto e IVA.
4. El sobregiro se admite exclusivamente para la comisión de pagos masivos cuando la cuenta:
   - pertenece a una persona jurídica;
   - es corriente;
   - está ACTIVA;
   - está habilitada como cuenta matriz de pagos masivos;
   - tiene sobregiro habilitado y un límite positivo.
5. El límite es duro. Si la comisión lo supera, la operación se rechaza sin efectos parciales, la comisión queda pendiente y la reserva no puede liberarse ni cerrarse.
6. El cobro puede reintentarse con la misma correlación. Accounting devuelve el asiento existente cuando el payload coincide y rechaza un replay conflictivo.
7. El cupo no aplica a retiros, P2P, fondeo del lote ni otras operaciones.

## Consecuencias

- Se conserva la intención de V1 de liquidar la comisión después del procesamiento.
- Se incorpora un control de riesgo adicional: no existe sobregiro ilimitado.
- No se implementan intereses, mora, cartera, aprobaciones multinivel ni límites por producto.
- La comisión deja de consumir la reserva del lote.
- Los registros R9-D históricos se conservan; la migración V7 es aditiva y compatible.

## Alternativas descartadas

- **Cobertura anticipada de lote y comisión:** segura, pero se aparta de la secuencia heredada.
- **Sobregiro ilimitado:** cumple literalmente la regla heredada, pero introduce riesgo no gobernado.
- **Módulo completo de crédito/sobregiro:** excede el alcance del Core académico y constituye sobreingeniería.
