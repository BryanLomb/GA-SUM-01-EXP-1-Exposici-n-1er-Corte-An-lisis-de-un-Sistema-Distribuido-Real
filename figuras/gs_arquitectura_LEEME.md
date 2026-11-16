# Diagrama RTV — García Sangoquiza

- `gs_arquitectura.drawio`: versión editable en draw.io / diagrams.net, con 54 elementos y 6 conectores nativos. No es una imagen incrustada.
- `gs_arquitectura.png`: imagen generada desde la fuente TikZ para compartir e insertar en diapositivas. Contiene los mismos componentes, comunicaciones y advertencias de la versión draw.io; las tipografías y los saltos de línea pueden variar.
- `gs_arquitectura_tikz.tex`: fuente editable en TikZ, compilable como figura independiente.
- `gs_arquitectura_LEEME.md`: fuentes, supuestos, instrucciones y declaración de asistencia.

## Formato solicitado por el SGA

`InstruccionesSGA.txt`, línea 18, permite elaborar la arquitectura en draw.io o TikZ. Sin embargo, la lista de archivos de entrega, línea 49, exige expresamente el diagrama editable `.drawio`. Se incluyen ambos formatos para satisfacer esa lista y conservar la fuente que genera la imagen.

Esta es una entrega parcial del diagrama: no contiene todavía las tres diapositivas, las notas ni todos los demás materiales de la exposición.

## Abrir y editar el archivo draw.io

1. Acceder a <https://app.diagrams.net/> o abrir la aplicación de escritorio draw.io.
2. Elegir abrir un diagrama existente desde el dispositivo y seleccionar `gs_arquitectura.drawio`.
3. Seleccionar las cajas, textos o conectores para modificarlos. Los límites discontinuos representan el despliegue físico asumido.
4. Conservar el archivo `.drawio` al guardar. Para obtener una imagen nueva, usar **Exportar como → PNG**, con fondo blanco, tema claro y una escala suficiente para leer los textos. Puede exportarse SVG para conservar calidad vectorial.

El archivo se comprobó mediante validación del XML, resolución de los identificadores de los conectores y apertura en diagrams.net. La imagen PNG incluida procede de la compilación de TikZ, revisada visualmente.

## Qué representa

Arquitectura cliente-servidor de tres capas: Angular en los puestos, API REST con Spring Boot y Spring Security, y persistencia en PostgreSQL. Las tecnologías y relaciones se basan en los apartados 3.3, 5.6.10 y 5.6.11 de `RTV-PROYECTO_FINAL.md`. Se omiten servicios externos para mantener el foco en el flujo de revisión; la figura no pretende inventariar todas las integraciones del proyecto.

**Supuesto académico explícito:** el backend y la base de datos se sitúan en nodos separados, accesibles desde los puestos por red. El documento aportado incluye diagramas de despliegue, pero no se ha contrastado el entorno instalado; una cadena JDBC con `localhost` en el documento tampoco demuestra un servidor de datos remoto. Por ello, los límites físicos se dibujan discontinuos y se identifican como supuestos.

Las líneas A y B y los vehículos X e Y son ejemplos, no cifras de operación real. RNF-08 exige revisiones simultáneas en líneas diferentes; no acredita un ensayo de carga ni confirma paralelismo de CPU. RF-13 exige terminar una etapa antes de avanzar; RF-15, RF-38 y RF-39 sustentan la reunión del resultado y las condiciones de certificación. Las flechas entre componentes representan comunicación de software, no el recorrido físico de un vehículo.

No se atribuyen al proyecto balanceadores, microservicios, colas de mensajes, réplicas de base de datos ni una configuración concreta de hilos. La notificación asíncrona con Kafka o RabbitMQ figura como recomendación futura en el documento y no se representa como implementación existente.

## Generar la imagen

Dependencias: TeX Live con `standalone`, TikZ/PGF, `babel` español y Latin Modern; Poppler (`pdftoppm`) para exportar PNG. Desde la raíz del proyecto:

```sh
mkdir -p /tmp/opencode/rtv-diagrama
pdflatex -interaction=nonstopmode -halt-on-error -output-directory=/tmp/opencode/rtv-diagrama figuras/gs_arquitectura_tikz.tex
pdftoppm -png -singlefile -r 240 /tmp/opencode/rtv-diagrama/gs_arquitectura_tikz.pdf figuras/gs_arquitectura
```

El archivo TikZ es autónomo: **no** se incluye directamente con `\input` en Beamer, porque contiene preámbulo. La imagen se inserta con `\includegraphics`.

