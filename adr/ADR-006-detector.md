# ADR-006: Z-score EWMA desplegado; Isolation Forest evaluado offline

- Estado: Aceptado
- Fecha: 2026-11-14 (aceptado el 2026-11-19)

## Contexto
Por cada tenant y minuto hay pocas variables de demanda (peticiones por segundo, capacidad consumida, fracción de la celda). La decisión que dispara el detector, poner a un cliente en cuarentena, debe poder explicarse y auditarse. scikit-learn con numpy y scipy se acercan al límite de 250 MB de una Lambda en zip, y un modelo grande (por ejemplo, de series de tiempo preentrenado) necesitaría GPU o un contenedor aparte.

## Opciones
1. Z-score con línea base EWMA por tenant (Python puro).
2. Isolation Forest desplegado como imagen de contenedor.
3. Modelo preentrenado de series de tiempo.

## Decisión
Opción 1 en línea, detrás de la interfaz `AnomalyDetector`. Isolation Forest se evalúa offline con exactamente las mismas ventanas que vio el detector durante los experimentos, con la verdad conocida de las fallas inyectadas, para justificar con datos que el modelo simple basta. La opción 3 se descarta: no aporta precisión sobre datos tabulares tan pequeños y sí costo, latencia y opacidad.

## Consecuencias
- (+) El detector desplegado es simple y explicable: el puntaje z, la variable y el umbral quedan en la bitácora.
- (+) Agregar otro detector es una clase nueva (QAS08).
- (−) La comparación de MTTR de extremo a extremo se hace solo con z-score.
