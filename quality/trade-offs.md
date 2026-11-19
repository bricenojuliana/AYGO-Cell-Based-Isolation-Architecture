# Análisis de *trade-offs* (estilo ATAM, abreviado)

| Decisión | Atributo que mejora | Atributo que empeora | Punto de sensibilidad | Riesgo o no-riesgo |
|---|---|---|---|---|
| Celdas en vez de pool (ADR-001) | Disponibilidad: radio de impacto ≤ 1/N | Costo y holgura: cada celda tiene menos margen para picos que un pool del mismo tamaño total | Tamaño de la celda frente a la intensidad del pico (E1 a 50× contra E1S a 200×) | Riesgo: con picos moderados el pool absorbe mejor |
| Router compartido (ADR-003) | Simplicidad, cambio de topología sin redeploy | Desempeño (latencia extra) y un punto compartido | Concurrencia del router; caché de rutas de 10 s | No-riesgo si el router no reintenta (falla rápido) |
| Cuarentena por reenrutamiento (ADR-007) | Confiabilidad: MTTR de segundos tras la decisión | Lecturas de historial más lentas para el tenant aislado | TTL de la caché de rutas | No-riesgo: el infractor sigue atendido |
| Detector z-score (ADR-006) | Explicabilidad y auditabilidad | Detección de patrones complejos | Umbral (z > 3) y k = 2 ventanas | Riesgo: MTTD con piso de unos 2 a 3 min por la granularidad de CloudWatch |
| Salvaguardas del lazo (ADR-010) | Exactitud (sin castigar a víctimas) | Un segundo infractor en la misma celda requiere operador | Regla de una cuarentena por celda | No-riesgo para los escenarios evaluados |
| Capacidad acotada (ADR-004) | Reproducibilidad y costo predecible | Realismo frente a producción elástica | Capacidad de ráfaga de DynamoDB | Amenaza a la validez externa |
