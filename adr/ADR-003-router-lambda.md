# ADR-003: Router en Lambda que invoca las celdas con `lambda:Invoke`

- Estado: Aceptado
- Fecha: 2026-11-14

## Opciones
1. Router Lambda con invocación directa de la Lambda de la celda.
2. Un API Gateway por celda y ruteo por DNS o en el cliente.
3. Router en contenedor (ECS/Fargate).

## Decisión
Opción 1. La ruta `tenant → celda` vive en DynamoDB con caché en memoria (10 s; la autenticación, 60 s). La identidad del tenant sale de la API key (hash HMAC con un *pepper*), nunca de la URL. El router no reintenta: si la celda está saturada responde 429 o 503 de inmediato, para que una celda lenta no consuma la concurrencia compartida del router.

## Consecuencias
- (+) Simple, sin servidores que operar, funciona con `LabRole`.
- (−) Dos invocaciones Lambda por petición (latencia y costo, medidos en H3 y H4).
- (−) Durante el TTL de la caché el router puede usar una ruta vieja; la cuarentena tarda hasta 10 s en verse, lo que entra en el MTTR medido.
