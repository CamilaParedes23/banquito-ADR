# Requisitos Arquitectónicamente Significativos (ASR)

Un ASR es un requisito o fuerza que modifica de manera medible la estructura, los límites, los estilos o las decisiones de la solución. No todo RNF es un ASR y algunos requisitos funcionales —por ejemplo Off-Us— sí lo son porque obligan a cambiar la arquitectura.

| ID | ASR | Criterio de éxito | ADR relacionados |
|---|---|---|---|
| ASR-01 | Integridad financiera y auditabilidad | Ninguna operación deja saldos, asientos, posiciones o EOD inconsistentes; todo cambio es rastreable. | ADR-MON-0002, ADR-MON-0003, ADR R9-E, ADR R9-F.6, ADR-0009, ADR-0016, ADR-0017, ADR-0018, ADR-0021 |
| ASR-02 | Idempotencia financiera | Un replay, timeout o mensaje duplicado no produce dos afectaciones financieras. | ADR-MON-0001, ADR-MON-0002, ADR R9-E, ADR-0020, ADR-0021 |
| ASR-03 | Seguridad e identidad gobernada | Autenticación, autorización, API Keys y secretos se administran con límites claros y mínimo privilegio. | ADR-MON-0003, ADR-MON-0004, ADR-0004, ADR-0007, ADR-0008 |
| ASR-04 | Trazabilidad extremo a extremo | Una operación se correlaciona entre HTTP, gRPC, mensajería, logs y contabilidad. | ADR R9-F.6, ADR-0002, ADR-0009 y estándares de observabilidad |
| ASR-05 | Escalabilidad, resiliencia y disponibilidad | Los picos y fallos parciales no detienen el ecosistema ni comprometen la consistencia. | ADR-MON-0001, ADR-0004, ADR-0009, ADR-0010, ADR-0021 |
| ASR-06 | Testabilidad, mantenibilidad y deployability | El mismo código se prueba, versiona y despliega de forma repetible, con evidencia. | ADR-0001, ADR-0002 y estándares de pruebas/configuración |
| ASR-07 | Interoperabilidad bancaria | BanQuito procesa On-Us y Off-Us sin mezclar cuentas de clientes, posiciones interbancarias y responsabilidades del Switch. | ADR-0015, ADR-0016, ADR-0017, ADR-0018, ADR-0020, ADR-0021 |

## Cadena de trazabilidad

`Necesidad / requisito → ASR → alternativas y trade-offs → ADR → C4 / código / infraestructura → evidencia`
