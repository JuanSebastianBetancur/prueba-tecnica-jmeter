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

---

# Informe de Prueba de "STRESS"


## 1. Resumen

**Objetivo**: Evaluar la capacidad del sistema para manejar cargas altas de usuarios simultáneos, identificando saturación y las condiciones bajo las cuales el rendimiento del sistema se degrada.

**Herramienta Utilizada**: Apache JMeter

## 2. Configuración de la Prueba

- **Endpoint Probado** POST: `https://restful-booker.herokuapp.com/booking`
- **Ramp-Up Period**: 1 segundos
- **Número de Iteraciones**: 1 (una única ejecución por usuario)
- **Tipo de Solicitud**: HTTP POST
- - **Configuración de Usuarios**:
  - 100 usuarios en 1 segundos
  - 200 usuarios en 1 segundos
  - 400 usuarios en 1 segundos
  - 800 usuarios en 1 segundos
  - 1600 usuarios en 1 segundos
  - 3200 usuarios en 1 segundos
  - 6400 usuarios en 1 segundos
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

  # Resultados de la Prueba de Stress

| Usuarios | Promedio (ms) | Máximo (ms) | Mínimo (ms) | Throughput (requests/sec) |
|----------|---------------|-------------|-------------|----------------------------|
| 100      | 348           | 386         | 317         | 74.4                       |
| 200      | 437           | 552         | 340         | 136.7                      |
| 400      | 581           | 952         | 325         | 209.0                      |
| 800      | 3409          | 8251        | 506         | 91.2                       |
| 1600     | 5040          | 13989       | 417         | 111.9                      |
| 3200     | 11712         | 32216       | 656         | 97.5                       |
| 6400     | 23098         | 74944       | 863         | 84.7                       |

#### Usuarios: 100 a 400
- **Rendimiento Estable**: El sistema mantuvo un tiempo de respuesta promedio entre 348 ms y 581 ms siendo eficiente de la carga.
- **Throughput se incrementa**: El throughput aumentó consistentemente, al parecer el sistema respondio bien al incremento gradual de usuarios.
- **Variabilidad Controlada**: La desviación estándar fue baja dando consistencia en los tiempos de respuesta.

#### Usuarios: 800
- **Degradación del Rendimiento**: El tiempo de respuesta promedio aumentó drásticamente a 3409 ms, lo que indica que el sistema comenzó a experimentar problemas de rendimiento.
- **Throughput Reducido**: El throughput disminuyó a 91.2 requests/sec, señalando que el sistema tuvo que esforzarse un poco para mantener la carga.
- **Alta Variabilidad**: La desviación estándar también aumentó mostrando mas inestabilidad en el rendimiento.

#### Usuarios: 1600
- **Incremento de la Inestabilidad**: El tiempo de respuesta promedio subió a 5040 ms, con un tiempo máximo de casi 14 segundos. mostrando una clara degradacion en la respuesta del sistema.
- **Variabilidad Significativa**: La desviación estándar continuó aumentando, indicando una gran variabilidad en los tiempos de respuesta.

#### Usuarios: 3200
- ***Rendimiento Deteriorado**: El tiempo de respuesta promedio se incrementó a 11712 ms, con peticiones que tardaron hasta 32 segundos en completarse.
- **Throughput Estable**: Estable pero con tiempos de respuesta considerablemente altos.
- **Errores Menores**: Aparecieron errores (0.031%), indicando que el sistema empezaba a fallar bajo esta carga.

#### Usuarios: 6400
- **El sistema se degrada**: El tiempo de respuesta promedio alcanzó 23098 ms, con un tiempo máximo de casi 75 segundos. Esto sugiere que el sistema no pudo manejar la carga.
- **Errores Notables**: La tasa de errores aumentó a 0.172% indicando que se excedio la carga del sistema.

#### Conclusiones

- ***Resumen***: Hasta 400 usuarios, el sistema mostró un buen rendimiento con tiempos de respuesta estables y un throughput creciente, A partir de 800 usuarios, el sistema comenzó a mostrar signos de estrés, con un aumento significativo en los tiempos de respuesta y una disminución en el throughput, Con 3200 y 6400 usuarios, el sistema alcanzó su punto de ruptura, con tiempos de respuesta extremadamente altos y la aparición de errores.
- ***Recomendaciones***: Optimizar el sistema para manejar mejor cargas a partir de los 1600 request por ejemplo con un escalado de la infraestructura para mantener el rendimiento bajo condiciones de alta demanda.

---

# Otras pruebas que se recomendaria hacer

## Escalabilidad
Para evaluar cómo el sistema responde al aumento gradual de los recursos disponibles para determinar el impacto en el rendimiento a medida que el sistema escala horizontalmente y verticalmente.

## Por ejemplo teniando en cuenta los siguientes escenarios
- **Escenario 1**: Escalado Horizontal (Incremento de Instancias).
- **Escenario 2**: Escalado Vertical (Incremento de Recursos).
- **Carga Inicial**: 400 usuarios simultáneos.
