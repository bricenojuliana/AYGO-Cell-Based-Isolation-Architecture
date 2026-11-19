# ADR-009: Generador de carga en lazo abierto e inyección de fallas propia

- Estado: Aceptado (reemplaza la versión con Locust)
- Fecha: 2026-11-19

## Contexto
La métrica principal es la latencia p99 de los tenants vecinos durante una saturación. Un generador en lazo cerrado como Locust (cada usuario espera la respuesta antes de enviar la siguiente) baja su tasa justo cuando el sistema se degrada y oculta la cola de latencia; es el problema de *coordinated omission*.

## Decisión
Carga con un generador propio en lazo abierto (Python con `aiohttp`). Cada tenant envía peticiones en instantes de un proceso de Poisson a tasa fija, sin esperar respuestas; el tenant ruidoso multiplica su tasa entre el segundo 120 y el 420. Cada petición se registra con su instante de envío, estado, latencia total y latencia de la celda (cabecera `x-cell-ms`).

Se ejecuta desde un portátil con las credenciales temporales del lab, porque CloudShell cierra la sesión por inactividad y la matriz dura horas. Todas las configuraciones se miden desde el mismo cliente.

Las fallas se inyectan con banderas en SSM Parameter Store que leen las celdas, para no depender de AWS FIS, que necesita un rol de servicio propio.

## Consecuencias
- (+) La tasa ofrecida no depende de la salud del sistema, así que el p99 medido es honesto.
- (+) Un comando reanudable; inicio y fin de la falla con marca de tiempo exacta (necesaria para MTTD y MTTR).
- (−) La latencia absoluta incluye la red del cliente; se comparan configuraciones entre sí.
- (−) Las fallas son simuladas en la aplicación, no en la infraestructura.
