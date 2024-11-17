
# Parcial 2 Programación Concurrente
## Memoria del Programa: Simulación de Máquina de Galton

### Enlace al repositorio: https://github.com/itsedupablo/Parcial2_ProgramacionConcurrente

## Introducción
Este proyecto consiste en la simulación de una máquina de Galton, utilizando hilos productores y consumidores para obtener y procesar datos de un archivo CSV. Los resultados de la simulación se muestran en una interfaz gráfica en tiempo real, representando la distribución de los datos recolectados como si se tratara de una campana de Gauss.
Tiene la misma funcionalidad que el proyecto del Parcial 1 con la diferencia de que se hace uso de técnicas de programación paralela y distribuida.

### **ACLARACIÓN** No se ha hecho uso de RabbitMQ 
-----

## Estructura del Proyecto
# Arquitectura del programa: Simulación de una Máquina de Galton

Este programa simula el funcionamiento de una máquina de Galton, representando la distribución gaussiana mediante la caída de bolas que interactúan con obstáculos y se agrupan en columnas. Está compuesto por varias clases organizadas en paquetes según su funcionalidad:

---

## 1. **Punto de entrada**
### `FabricaGaltonApplication`
- Clase principal que configura y ejecuta la aplicación Spring Boot.
- Realiza las siguientes tareas:
  - **Carga de datos:** Lee un archivo CSV y genera una lista de datos (`cargarDatosDesdeCSV`).
  - **Gestión de hilos:** Utiliza `ThreadManager` para coordinar productores y consumidores.
  - **Interfaz gráfica:** Inicializa `GaltonUI` para la representación visual de la simulación.

## 2. **Gestión de hilos y concurrencia**
### Paquete: `com.edupablo.parcial2.fabrica_galton.abstract_factory`

#### **`ThreadManager`**
- Gestiona la creación y ejecución de hilos productores y consumidores.
- Divide la lista de datos entre los hilos productores y coordina la comunicación a través de una cola compartida (`BlockingQueue`).

#### **`Productor`**
- Produce elementos desde una lista y los inserta en la cola compartida.
- Actualiza la interfaz gráfica (`GaltonUI`) al generar un nuevo dato.

#### **`Consumidor`**
- Extrae elementos de la cola compartida, simulando su "consumo".
- Integra pausas configurables (`tiempoEspera`) para emular tiempos de procesamiento.

#### **`ThreadFactory`**
- Implementa un patrón Factory para crear instancias de hilos (`Thread`).
- Personaliza la configuración y el nombre de los hilos creados.

## 3. **Controlador Web**
### Paquete: `com.edupablo.parcial2.fabrica_galton.controller`

#### **`WebFluxController`**
- Proporciona un endpoint REST para acceder a los datos generados.
- Utiliza programación reactiva con `Flux` para emitir datos de forma continua.

## 4. **Interfaz gráfica**
### Paquete: `com.edupablo.parcial2.fabrica_galton.ui`

#### **`GaltonUI`**
- Representa la interfaz gráfica principal.
- Utiliza un panel (`JPanel`) personalizado para dibujar bolas y columnas.
- Administra la animación de bolas que caen y se apilan en columnas, simulando la distribución gaussiana.

#### **`Bola`**
- Representa las bolas que caen en la máquina.
- Calcula su posición y simula un movimiento aleatorio hacia la izquierda o derecha.

#### **`Columna`**
- Representa las columnas donde las bolas se agrupan tras caer.
- Permite apilar y dibujar bolas para visualizar la simulación.

## 5. **Resumen del flujo de trabajo**
1. **Inicialización:** La clase principal carga los datos del archivo CSV y configura los componentes necesarios.
2. **Producción y consumo:** Los hilos productores generan elementos y los colocan en la cola, mientras que los consumidores los procesan.
3. **Interacción gráfica:** Los datos generados por los productores actualizan la representación visual en `GaltonUI`.
4. **Distribución visual:** Las bolas caen aleatoriamente, simulan interacciones con obstáculos, y se agrupan en columnas para formar una curva de distribución gaussiana.

## 6. **Tecnologías utilizadas**
- **Spring Boot:** Para la gestión del ciclo de vida de la aplicación y la exposición de endpoints REST.
- **Java Swing:** Para la representación gráfica interactiva.
- **Concurrencia en Java:** Uso de `BlockingQueue`, `Thread`, y patrones de fábrica para la gestión de hilos.
- **WebFlux:** Para ofrecer capacidades reactivas en el controlador REST.
---
## Aclaraciones

### 1. **No se ha hecho uso de RabbitMQ**
Debido a una serie de problemas a nivel de compilación al intentar conectar con el localhost (sospecho que ha sido problema de mi ordenador pero como no lo puedo confirmar y no he conseguido ver el modo de implementar Rabbit sin que me de errores, he optado por hacer un programa funcional aunque prescindiendo de este recurso). 
He añadido una una carpeta llamada errores donde he subido capturas del problema que tenía. 

### 2. **Interfaz Gráfica con javax swing**
Al igual que en el Parcial 1 se ha optado por utilizar las librerías de UI en lugar de html por temas de simplicidad y problemas con el docker. La diferencia es que en esta ocasión si se han creado bolas que se van añadiendo al gráfico para la visualización de la distribución normal en lugar de un historigrama
