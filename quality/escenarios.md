# Escenarios de atributos de calidad

Formato de Bass, Clements y Kazman (estímulo y entorno, respuesta, medida). Los resultados salen de `results/` en el repo del prototipo. Se ejecutó una corrida por condición: la cuenta del laboratorio se desactivó a mitad de la matriz por el volumen de invocaciones Lambda de la prueba de carga, así que los valores son descriptivos.

| ID | Atributo | Estímulo y entorno | Respuesta | Medida | Resultado |
|---|---|---|---|---|---|
| QAS01 | Desempeño | 30 tenants a 2 rps cada uno, sin fallas (E0) | El router enruta sin degradar | Sobrecosto del router ≤ 15 ms en p50 (H3) | No verificable: el pool también pasa por el router y p50 fue igual en pool y celdas (137 ms). Desde el cliente solo hay una cota superior de ≈ 112 ms, que incluye la red |
| QAS03 | Disponibilidad | Falla completa de una celda (E4) | Solo se afectan sus tenants | % de tenants afectados ≤ 1/N + 5 % con N = 3 celdas (≤ 38 %); el pool como comparación | Pool: 100 % afectados (2 corridas). Celdas: no se alcanzó a ejecutar antes del cierre de la cuenta |
| QAS04 | Confiabilidad | Un tenant genera 50× o 200× su tasa (E1, E1S) | El detector lo identifica y lo pone en cuarentena | MTTD ≤ 3 min, MTTR ≤ 5 min, 0 falsas cuarentenas | Cumplido en E1 (50×): MTTD 178 s, MTTR 240 s, 0 falsas cuarentenas. Sin el detector, celdas no se recuperó durante la falla. E1S no se ejecutó |
| QAS05 | Seguridad | Un tenant intenta leer pedidos de otro | Petición rechazada | 0 accesos cruzados; keys guardadas solo como hash HMAC | Cumplido: smoke test en AWS (404) y pruebas unitarias |
| QAS07 | Mantenibilidad | Se corrige un defecto del dominio | Cambia un solo módulo | Dominio sin dependencias de AWS; cobertura ≥ 85 % | Cumplido: cobertura 92 %, `ruff` sin errores |
| QAS08 | Modificabilidad | Se cambia el algoritmo de detección | Se implementa una nueva clase `AnomalyDetector` | 1 clase nueva, 0 cambios en router o celdas | Cumplido por diseño (interfaz `AnomalyDetector`) |
| QAS09 | Testeabilidad | Se ejecuta la suite sin AWS | Todas las pruebas corren local | Sin credenciales (moto y dobles), < 2 min | Cumplido |
| QAS10 | Reusabilidad | Se despliega pool o celdas | Mismo artefacto de código | Un solo template de celda instanciado N veces | Cumplido: el pool es una celda con 4× capacidad |
| QAS11 | Soportabilidad | Un operador investiga una cuarentena | Correlaciona tenant, celda y decisión | La bitácora registra puntaje, variable, umbral, ventana y razón | Cumplido: bitácora `AUDIT` en la tabla de control |

QAS02 (escalabilidad lineal), QAS06 (contrato OpenAPI) y el escenario de consulta pesada se dejaron fuera del alcance por el plazo.
