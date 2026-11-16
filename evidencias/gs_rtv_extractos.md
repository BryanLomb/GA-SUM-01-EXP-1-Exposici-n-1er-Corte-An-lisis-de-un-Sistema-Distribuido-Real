# Extractos documentales del RTV para el bloque de García Sangoquiza

**Origen:** `RTV-PROYECTO_FINAL.md`, facilitado localmente por el integrante. Autores indicados: Kevin Garcia, Diego Giraldo, Steven Pozo y Nixon Piguave, Universidad Técnica Estatal de Quevedo. No consta fecha de publicación. Consulta de esta copia: 16-11-2026.

**SHA-256 del original completo:** `79dd3b4dd921ce1a62a773d55e8d928d3e3a9d18bd0e812920bc5de8df17d286`.

Estos son extractos, no una transcripción completa. Los puntos suspensivos entre corchetes indican omisiones. Se omiten imágenes con configuraciones ajenas al objeto de la exposición. Las referencias a líneas corresponden al original y permiten contrastar esta selección. No se presenta este archivo como evidencia de lectura realizada por el estudiante.

## Identidad y alcance — título y § 3

Título, línea 1:

> DESARROLLO DE UNA APLICACIÓN WEB PARA EL CENTRO DE REVISIÓN TÉCNICA VEHICULAR DE VALENCIA

§ 3, línea 47:

> El CRTVMV es una institución orientada a garantizar la seguridad vial, la protección del medio ambiente y el cumplimiento de la normativa vigente, mediante la prestación de servicios técnicos especializados destinados a la inspección, control y verificación del parque automotor del cantón […].

El alcance institucional es un centro, pero el texto no proporciona una cifra de vehículos/día ni de usuarios concurrentes. No se convierte el objetivo de mejora del resumen en un resultado medido.

## Arquitectura — § 3.3, línea 66

> El sistema adopta una arquitectura de tres capas bajo el paradigma cliente-servidor, donde cada capa tiene responsabilidades claramente delimitadas y se comunica con las adyacentes a través de interfaces bien definidas, favoreciendo la escalabilidad, mantenibilidad y separación de responsabilidades.

> La capa de presentación está implementada con Angular como framework de desarrollo de una SPA (Single Page Application), lo que permite una experiencia de usuario fluida sin recargas de página. Angular se comunica exclusivamente con el backend mediante peticiones HTTP/HTTPS a los endpoints RESTful expuestos, enviando y recibiendo datos en formato JSON. Todas las peticiones autenticadas incluyen un token JWT en el encabezado Authorization.

> La capa de lógica de negocio está construida con Spring Boot sobre Java, exponiendo una API REST que centraliza las reglas del negocio, la validación de datos y el control de acceso. Spring Security intercepta cada petición entrante para validar el token JWT antes de permitir el acceso a los recursos protegidos. La comunicación con la base de datos se realiza a través de Spring Data JPA para operaciones estándar y mediante procedimientos almacenados para operaciones críticas que requieren atomicidad y rendimiento optimizado.

> La capa de persistencia está gestionada por PostgreSQL, un motor de base de datos relacional cuyo esquema está normalizado hasta la tercera forma normal (3NF). Se implementan restricciones de integridad referencial, índices optimizados para consultas frecuentes, y triggers para automatizar operaciones que deben mantener la consistencia de datos históricos.

**Límite de interpretación:** tres capas no prueban tres máquinas, réplicas ni microservicios. En la exposición se asumen nodos separados para backend y BD. No se ha verificado el despliegue instalado.

## Llegada y turno — entrevista, línea 163

> Al llegar, el guardia llena una ficha con el motivo de la visita del propietario. El vehículo ingresa, se estaciona y el propietario se dirige a recepción para generar un turno. El turno se genera con el pago pendiente, por lo que el usuario debe acercarse a la recaudadora para cancelar el trámite. Luego, el recepcionista asigna una línea de revisión. […] Finalmente, la digitadora genera el resultado de la revisión, recepción entrega el documento junto con las llaves del vehículo, identificadas con el número de placa.

**Elección para el ejemplo:** turno al llegar, no reserva anticipada. El documento también usa “citas” en otros apartados; se conserva la entrevista como referencia del flujo de atención y se advierte la diferencia en la preparación oral.

## Requisitos funcionales — § 5.1

| Código | Texto de la descripción | Línea |
|---|---|---|
| RF-10 | El sistema deberá clasificar el vehículo de acuerdo con la norma INEN 2656. | 202 |
| RF-11 | El sistema deberá asignar la línea de revisión técnica correspondiente según la clasificación del vehículo. | 203 |
| RF-13 | El sistema deberá permitir el avance del vehículo a la siguiente etapa únicamente cuando la etapa anterior haya sido completada. | 205 |
| RF-14 | El sistema deberá registrar el estado actual del vehículo durante el proceso de revisión. | 206 |
| RF-15 | El sistema deberá registrar el resultado final de la revisión técnica vehicular. | 207 |
| RF-23 | El sistema deberá asociar cada resultado de prueba con el equipo utilizado. | 215 |
| RF-24 | El sistema deberá verificar que el equipo utilizado se encuentre en estado operativo antes de registrar una prueba. | 216 |
| RF-25 | El sistema deberá verificar que el equipo utilizado tenga la calibración vigente antes de registrar una prueba. | 217 |
| RF-38 | El sistema deberá impedir la emisión del certificado de revisión técnica vehicular cuando el pago no haya sido registrado. | 233 |
| RF-39 | El sistema deberá generar el certificado de revisión técnica vehicular cuando el resultado sea aprobado. | 234 |

## Simultaneidad y tiempo de respuesta — § 5.2.4

RNF-07, línea 274:

> El sistema deberá responder a las operaciones críticas en un tiempo máximo de tres segundos.

RNF-08, línea 275:

> El sistema deberá permitir la ejecución de múltiples revisiones técnicas vehiculares de forma simultánea en diferentes líneas.

Son requisitos. No hay un ensayo adjunto que permita convertirlos en resultados alcanzados.

## Avance entre etapas — CU-13 y CU-14

CU-13, líneas 1093–1099, fragmentos:

> 1. [Inspector RTV] Completa el registro de resultados de la prueba en la etapa actual.
>
> 2. [Inspector RTV] Solicita avance a la siguiente etapa mediante el botón 'Siguiente Etapa'.
>
> 3. [Sistema] Verifica que todos los resultados obligatorios de la etapa actual estén registrados.
>
> 4. [Sistema] Verifica que los equipos utilizados están operativos y calibrados.
>
> 6. [Sistema] Actualiza el estado de la etapa a 'Completada' y registra timestamp.
>
> 7. [Sistema] Habilita la siguiente etapa en el flujo de revisión.

CU-14, líneas 1158–1161:

> 1. [Inspector RTV] Solicita el avance a la siguiente etapa de revisión.
>
> 2. [Sistema] Verifica que todos los resultados obligatorios de la etapa actual estén registrados.
>
> 3. [Sistema] Si existen resultados pendientes, bloquea el avance y notifica al inspector.
>
> 4. [Sistema] Si la etapa está completa, aprueba el avance a la siguiente etapa.

**Representación elegida:** etapas genéricas `i → i+1` y `j → j+1`. La guía individual agrupa visual, mecánica y gases, pero CU-13 enumera pruebas sin formalizar esa agrupación en tres etapas. No se inventa un orden más detallado en la figura. “Bloquea el avance” significa una regla de negocio, no necesariamente un mutex o una barrera entre hilos.

## Asincronía como recomendación, no implementación

Línea 1709, recomendaciones:

> Implementar un sistema de colas de mensajes (como Apache Kafka o RabbitMQ) para el procesamiento asíncrono de notificaciones por correo electrónico.

Por eso no se dibuja un broker como componente instalado y no se declara que el correo ya sea asíncrono.
