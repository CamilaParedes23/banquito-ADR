# Banco BanQuito — Manifiesto de release R9-K RC1

Fecha de consolidación: 2026-08-03
Estado: código sincronizado en `main`; pendiente validación QA/staging cloud y creación del tag `r9-k-core-rc1`.

## BOM de repositorios

| Repositorio | Rama | Commit |
|---|---|---|
| banquito-core-admin-3p | main | d798d0fead2ba7b655dd7fb2d88e61cdb7af5cd2 |
| banquito-core-contable-3p | main | 2bcae05d2612bcd23550c68c9b2d4c48574902b4 |
| banquito-core-seguridad-3p | main | c64eed57fade309621c5a60c4453308fed9358c4 |
| banquito-core-clientes-3p | main | c59d29ad3c75e694e3185b3745b16f6754a71c1d |
| banquito-core-documentos-3p | main | 9e97facc3d6ba44f690c024ae277b2e0a89e2857 |
| banquito-core-notificaciones-3p | main | 44076128cc6fb0dbbedc6d4d6534d72712884900 |
| banquito-core-transaccional-3p | main | 43a0c547fb7ebe9dc88109e88e88c8ff5b5f4937 |

## Versiones Flyway esperadas

| Servicio | Versión |
|---|---:|
| Admin | 7 |
| Clientes | 4 |
| Contable | 5 |
| Seguridad | 9 |
| Notificaciones | 5 |
| Transaccional | 10 |

## Artefactos externos de release

Estos binarios no deben incorporarse al repositorio documental. Deben conservarse en almacenamiento de artefactos y referenciarse por nombre y checksum.

| Artefacto | SHA-256 |
|---|---|
| BanQuito_R9K_NostroVostro_BanQuill_RC1.zip | ae723de894c70509fe7b16a0acce82656ad5d9caa0686aaca725c24c43e0fb86 |
| BanQuito_API_Interbancaria_R9K_BanQuill_v1_CORREGIDA.yaml | 33325da3f6b9838a9f0a436638db178a0923386ef8f5723f46773e7811fa8593 |
| BanQuito_R9K_Handoff_Infra_QA_v1.md | 9e85077906bf0bb1dafcc6c829f0409400420239956d6f0ef6dcf2f645556b69 |
| BanQuito_R9K_Seed_Completo_E2E_v6.zip | 5a86b92e93150a02007fda113aacba9b9fd5f2e882589b5b82154ba4cc1c7b6b |
| BanQuito_R9K_Gate_E2E_Final_v3.zip | 4e75c148c047d84e7cdf4d0995a7dd577b65387c8319d205e584f75361bb83ea |
| BanQuito_R9K_Evidencia_Contable_v1.zip | 0cbeaecbd2eefeaa0fc50d42879288926e59abad98ebae6884c9be4fdae867d3 |

## Decisiones arquitectónicas R9-K

- ADR-0015: bounded context interbancario como módulo explícito dentro de Transaccional, sin nuevo microservicio en R9-K.
- ADR-0016: Nostro/Vostro fuera de cuentas de clientes y bajo Contable.
- ADR-0017: plan contable único para operaciones On-Us y Off-Us.
- ADR-0018: modelo bilateral prefondeado mientras no exista una cámara/BCE integrada; requiere trigger de revisión.
- ADR-0020: `paymentLineUuid` como clave idempotente estable end-to-end.
- ADR-0021: timeout remoto como `PENDING_RECONCILIATION`, sin liberación automática.

## Documentos canónicos a versionar

- `docs/contracts/interbank-core-v1.yaml`
- `docs/contracts/switch-core-interbank-lifecycle-v1.yaml`
- `docs/functional/r9-k-interbank-core.md`
- `docs/runbooks/r9-k/handoff-infra-qa.md`
- `docs/release/r9-k/manifest.md`
- estándares de idempotencia, observabilidad y cobertura.

## Pendientes para promoción

1. Revalidación local sobre los commits publicados.
2. Tag `r9-k-core-rc1` en cada repositorio.
3. Build e imágenes por commit/digest.
4. Despliegue QA/staging.
5. Health, Flyway, OAuth2, integraciones y E2E cloud.
6. Evidencia de aceptación.
7. Tag final `r9-k` y promoción de los mismos digests.
