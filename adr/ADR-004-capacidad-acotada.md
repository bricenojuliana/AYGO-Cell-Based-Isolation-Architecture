# ADR-004: Capacidad acotada por celda para saturación reproducible

- Estado: Aceptado
- Fecha: 2026-11-14 (valores fijados el 2026-11-19 con la fase 0)

## Contexto
Con capacidad elástica sin límite, el tenant ruidoso no afecta a nadie pero dispara el costo; con el presupuesto del Learner Lab no se puede llegar a esa escala.

## Decisión
Cada celda usa concurrencia reservada en Lambda (20; el pool 80) y DynamoDB en modo aprovisionado (25 RCU/WCU; el pool 100). La fase 0 confirmó 1000 ejecuciones concurrentes en la cuenta, así que la reserva total (160) deja holgura para el router y el detector. El cálculo está en `docs/CAPACITY.md` del prototipo.

## Consecuencias
- (+) La saturación aparece con poca carga y es repetible; el costo es predecible (unos USD 0,16 por hora en tablas).
- (−) La capacidad de ráfaga de DynamoDB (300 s de holgura acumulada) favorece a las tablas grandes; por eso se prueban dos intensidades de ruido (E1 a 50× y E1S a 200×).
- (−) Escenario más extremo que producción; se discute como amenaza a la validez.
