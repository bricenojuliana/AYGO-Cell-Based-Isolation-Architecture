# ADR-005: Telemetría por tenant con EMF y CloudWatch

- Estado: Aceptado
- Fecha: 2026-11-14

## Opciones
1. EMF (logs con métricas embebidas) con dimensiones `CellId` y `TenantId`; el detector usa `GetMetricData`.
2. Contadores por tenant en DynamoDB escritos en cada petición.
3. Kinesis con agregación propia.

## Decisión
Opción 1. No agrega escrituras al camino crítico.

## Consecuencias
- (+) Sin infraestructura extra, consultable con herramientas estándar (interoperabilidad).
- (−) Granularidad de 1 min y retraso de publicación: fija un piso al MTTD, que se reporta.
- (−) Costo por cardinalidad (30 tenants × métricas): se incluye en el modelo de costo.
