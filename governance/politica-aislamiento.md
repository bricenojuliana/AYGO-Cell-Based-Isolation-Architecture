# Política de aislamiento automático de tenants y su mapeo a COBIT 2019

Poner a un cliente en cuarentena es una decisión con efecto sobre el negocio: cambia dónde se atiende a un cliente que paga. Por eso el aislamiento automático necesita reglas de gobierno, no solo un algoritmo.

## Política

1. **Propósito.** Proteger el nivel de servicio de todos los tenants frente al comportamiento anómalo de uno, sin dejar de atender a ese tenant.
2. **Alcance de la automatización.** El detector puede poner en cuarentena por sí solo; **no** puede liberar. La liberación la hace un operador después de revisar el caso.
3. **Condiciones para actuar.** Las tres a la vez: puntaje z > 3 sobre la demanda del tenant durante 2 ventanas de 1 minuto seguidas, la celda del tenant con síntomas (p99 > 500 ms o más del 1 % de peticiones fallidas), y ninguna otra cuarentena activa en esa celda.
4. **Modos de operación.** `off`, `observe` (solo registra) y `act` (registra y aísla), definidos en un parámetro de SSM. Un cambio de umbrales o de modo es un cambio controlado.
5. **Trazabilidad.** Cada decisión queda en la bitácora con ventana, tenant, variable, puntaje, umbral, celda de origen y destino, razón y duración.
6. **Revisión.** Cada cuarentena se revisa: si el tenant tenía un pico legítimo se registra como falsa alarma y se ajusta el umbral; si es recurrente se trata como problema.
7. **Intervención humana.** El operador puede poner en cuarentena o liberar a mano por CLI; también queda en la bitácora.

## Mapeo a COBIT 2019

| Objetivo de gobierno o gestión | Cómo lo atiende la arquitectura |
|---|---|
| EDM03 Asegurar la optimización del riesgo | El radio de impacto se acota por diseño (celdas) y el apetito de riesgo se expresa en umbrales aprobados |
| APO09 Acuerdos de servicio gestionados | El SLO por tenant (p99 ≤ 500 ms, ≤ 1 % de errores) define cuándo un tenant está afectado y cuándo actuar |
| APO12 Riesgo gestionado | El escenario del vecino ruidoso y la falla de celda se identifican, se miden y se mitigan con controles automáticos |
| BAI06 Cambios de TI gestionados | Reenrutar a un tenant es un cambio estándar preaprobado; cambiar umbrales o el modo del detector pasa por control de cambios |
| DSS02 Solicitudes de servicio e incidentes gestionados | La detección y la cuarentena reducen el MTTD y el MTTR del incidente; la bitácora es el registro del incidente |
| DSS03 Problemas gestionados | Las cuarentenas recurrentes del mismo tenant se analizan como problema (causa raíz, capacidad, contrato) |
| DSS04 Continuidad gestionada | La falla de una celda deja operando a las demás (E4) |
| MEA01 Monitoreo del desempeño y la conformidad | Métricas por tenant y celda en CloudWatch y las falsas cuarentenas como indicador del control |
