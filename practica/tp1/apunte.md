## [Volver atrás](./tp1.md)

<div align="center">
<h1>Apunte - TP 1. Herramientas de Diagnóstico de Redes</h1>
</div>

## Bibliografía

- OPPENHEIMER, P., 2011, Top-Down Network Design (3d ed). CISCO Press.
    - Capítulo 2. Sección “Network Performance” (pp. 32-44)
    - Capítulo 3. Sección “Checking the Health of the Existing Internetwork” (pp. 71-81)

## Chapter 2: Analyzing Technical Goals and Tradeoffs 

### Network Performance Definitions

La siguiente lista provee definiciones para los objetivos de rendimiento de red que pueden usarse cuando se analizan requisitos:

- Capacidad (bandwidth): la capacidad de transporte de datos de un circuito o red, medido generalmente en bps.
- Utilización: el porcentaje de la capacidad total disponible que está en uso.
- Utilización óptima: la utilización promedio máxima antes de que la red se considere saturada.
- Throughput: cantidad de datos sin errores transferidos exitosamente entre nodos por segundo.
- Carga ofrecida: suma de todos los datos que todos los nodos de la red tienen listos para enviar en un momento dado.
- Precisión: la cantidad de tráfico útil que se transmite correctamente en relación al tráfico total.
- Eficiencia: análisis de cuánto esfuerzo se requiere para producir una determinada cantidad de throughput.
- Latencia: tiempo entre que una trama está lista para ser transmitida desde un nodo y la entrega de dicha trama en otro punto de la red.
- Jitter: cantidad de tiempo que varía el retardo promedio.
- Tiempo de respuesta: la cantidad de tiempo entre una solicitud y la respuesta a dicha solicitud.

### Optimum Network Utilization

