
# Parcial 2 Programación Concurrente
## Memoria del Programa: Simulación de Máquina de Galton

### Enlace al repositorio: https://github.com/itsedupablo/Parcial2_ProgramacionConcurrente

## Introducción
Este proyecto consiste en la simulación de una máquina de Galton, utilizando hilos productores y consumidores para obtener y procesar datos de un archivo CSV. Los resultados de la simulación se muestran en una interfaz gráfica en tiempo real, representando la distribución de los datos recolectados como si se tratara de una campana de Gauss.

## Estructura del Proyecto

### 1. **Clase `GaltonController`**
- **Ubicación**: `com.edupablo.parcial1.fabrica_galton`
- **Descripción**: Es el controlador principal de la aplicación. Inicialmente gestionaba la interfaz web para iniciar los procesos de productores y consumidores, aunque con la transición a `Swing`, la función de iniciar los procesos se movió directamente a la clase `GaltonUI`.
- **Métodos**:
  - `iniciarProcesos`: Inicializaba los procesos de productor y consumidor recibiendo los parámetros como número de hilos. 

### 2. **Clase `GaltonService`**
- **Ubicación**: `com.edupablo.parcial1.fabrica_galton`
- **Descripción**: Servicio que coordina la lógica de inicio de los procesos de productor y consumidor, delegando la creación de hilos y distribución de tareas al `ThreadManager`.

### 3. **Clase `ThreadFactory`**
- **Ubicación**: `com.edupablo.parcial1.fabrica_galton.abstract_factory`
- **Descripción**: Fábrica de hilos que se encarga de crear hilos tanto para productores como para consumidores. Configura los nombres de los hilos dependiendo de su tipo (productor o consumidor).

### 4. **Clase `ThreadManager`**
- **Ubicación**: `com.edupablo.parcial1.fabrica_galton.abstract_factory`
- **Descripción**: Administra la creación y gestión de los hilos de productores y consumidores. Distribuye los datos de entrada (leídos del archivo CSV) a los hilos productores y asigna la tarea de consumo a los consumidores.
- **Métodos**:
  - `iniciar`: Inicializa las fábricas de hilos, distribuye los datos a los productores y arranca los consumidores.
  
### 5. **Clase `Productor`**
- **Ubicación**: `com.edupablo.parcial1.fabrica_galton.abstract_factory`
- **Descripción**: Hilo que simula un productor, tomando elementos de una lista de datos (extraídos del archivo `database.csv`) y colocándolos en el buffer compartido. También se encarga de actualizar la interfaz gráfica mediante el método `actualizarGrafica` de la clase `GaltonUI`.
- **Atributos**:
  - `buffer`: El buffer compartido donde se producen los ítems.
  - `galtonUI`: Se encarga de actualizar la gráfica visualizando la distribución de los datos.

### 6. **Clase `Consumidor`**
- **Ubicación**: `com.edupablo.parcial1.fabrica_galton.abstract_factory`
- **Descripción**: Hilo que simula un consumidor, extrayendo elementos del buffer compartido y procesándolos. Aunque el consumo se realiza en segundo plano, su interacción con la interfaz gráfica es mínima.

### 7. **Clase `BufferCompartido`**
- **Ubicación**: `com.edupablo.parcial1.fabrica_galton.abstract_factory`
- **Descripción**: Implementa un buffer compartido entre los hilos productores y consumidores. Usa una lista de tamaño limitado para coordinar el intercambio de datos entre los productores y consumidores de forma segura.

### 8. **Clase `GaltonUI`**
- **Ubicación**: `com.edupablo.parcial1.fabrica_galton.ui`
- **Descripción**: Esta clase es la responsable de la interfaz gráfica que representa en tiempo real la distribución de los datos producidos, simulando el comportamiento de una máquina de Galton.
- **Métodos**:
  - `actualizarGrafica`: Se llama cada vez que un productor genera un nuevo dato. Este método actualiza las barras de la gráfica que representan los datos distribuidos.
  - `simularCaida`: Simula la caída de una bola en la máquina de Galton, ajustando su posición a izquierda o derecha en base a una probabilidad aleatoria.
  - `dibujarGrafica`: Dibuja la gráfica de barras, mostrando la distribución actual de los datos en pantalla.
  - `dibujarCuadricula`: Agrega una cuadrícula de fondo para facilitar la interpretación de la gráfica.

### 9. **Clase `FabricagaltonApplication`**
- **Ubicación**: `com.edupablo.parcial1.fabrica_galton`
- **Descripción**: Contiene el método `main` que inicializa la interfaz gráfica (`GaltonUI`) y arranca los procesos de productor y consumidor de forma automática.
- **Método**:
  - `main`: Instancia `GaltonUI` y `ThreadManager`, e inicia los hilos de productor y consumidor para simular la caída de datos en la máquina de Galton.

## Aclaraciones

### 1. **Firebase**
Se intentó implementar una base de datos en **Firebase** para almacenar y gestionar los datos producidos por los hilos, lo que habría permitido mantener un registro persistente de los datos generados. Para ello, se añadieron las dependencias necesarias de Firebase al proyecto, pero debido a múltiples errores y problemas de compatibilidad, no se pudo completar su implementación.

### 2. **Interfaz Gráfica con D3.js**
Inicialmente, se consideró el uso de **D3.js** para la representación gráfica de los datos. D3.js es una biblioteca de JavaScript muy poderosa para visualizaciones interactivas y dinámicas en el navegador. Sin embargo, debido a la complejidad de integrar JavaScript con el resto del backend y los errores generados al intentar coordinar los hilos de Java con la interfaz web, se decidió finalmente optar por la implementación de la interfaz gráfica en **Swing**. Esta decisión simplificó el desarrollo y mejoró la visualización en tiempo real sin depender de un entorno web.

## Conclusión

Este proyecto simula la distribución de datos mediante una máquina de Galton, mostrando en tiempo real la distribución de los datos extraídos de un archivo CSV. Aunque no se logró implementar Firebase ni D3.js, la solución final con **Swing** proporciona una visualización funcional y sencilla del comportamiento gaussiano de los datos.
