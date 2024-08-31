# Informe de Prueba de Carga

---

## 1. Resumen

**Objetivo**: Evaluar el rendimiento del sistema bajo diferentes cargas de usuarios (100, 500, y 2500 usuarios) con un período de rampa de 10 segundos.

**Herramienta Utilizada**: Apache JMeter

**Entorno de Pruebas**:
- **Servidor**: (especifica el tipo de servidor o ambiente de pruebas)
- **Endpoint Probado** GET: `https://restful-booker.herokuapp.com/booking/2`
- **Endpoint Probado** POST: `https://restful-booker.herokuapp.com/booking`
- **Configuración de Usuarios**:
  - 100 usuarios en 10 segundos
  - 500 usuarios en 10 segundos
  - 2500 usuarios en 10 segundos

---

## 2. Configuración de la Prueba

- **Número de Usuarios**: 100, 500, 2500
- **Ramp-Up Period**: 10 segundos
- **Número de Iteraciones**: 1 (una única ejecución por usuario)
- **Tipo de Solicitud**: HTTP GET, HTTP POST
- **Contenido de la Solicitud en solicitud POST**: 
  ```json
  {
    "firstname": "Jim",
    "lastname": "Brown",
    "totalprice": 111,
    "depositpaid": true,
    "bookingdates": {
      "checkin": "2018-01-01",
      "checkout": "2019-01-01"
    },
    "additionalneeds": "Breakfast"
  }
  ```  
## 3. Resultados:

## Resultados GET

| Usuarios | Promedio (ms) | Máximo (ms) | Mínimo (ms) | Throughput (requests/sec) |
|----------|---------------|-------------|-------------|----------------------------|
| 100      | 351           | 438         | 319         | 9.7                        |
| 500      | 348           | 468         | 316         | 48                         |
| 2500     | 2820          | 21393       | 319         | 100.5                      |

## Resultados POST

| Usuarios | Promedio (ms) | Máximo (ms) | Mínimo (ms) | Throughput (requests/sec) |
|----------|---------------|-------------|-------------|----------------------------|
| 100      | 88            | 79          | 11          | 10.1                       |
| 500      | 87            | 91          | 88          | 50.2                       |
| 2500     | 104           | 94          | 89          | 101.9                      |

  
## 4. Analisis resultados  
El sistema se comportó de manera estable tanto para las peticiones POST como GET, a excepción de la petición GET para 2500 usuarios, donde se observó una degradación en el rendimiento del sistema. En este caso, algunas peticiones tardaron hasta 21 segundos en completarse, y la desviación estándar fue superior a 2 segundos, lo que indica una alta variabilidad en los tiempos de respuesta. Cabe destacar que, a pesar de esta degradación, todas las peticiones para esta cantidad de usuarios fueron resueltas sin errores.
