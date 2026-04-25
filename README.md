Garzón Post2 U8 — Flutter DevTools, Isolates y Firebase Performance

Aplicación desarrollada en Flutter como parte de la Unidad 8 Post-Contenido 2: Optimización de rendimiento y experiencia fluida, en la asignatura Aplicaciones Móviles — Ingeniería de Sistemas, Universidad de Santander (UDES), 2026.

Descripción

La aplicación simula la carga y procesamiento de un catálogo extenso de productos en formato JSON, con el fin de analizar el impacto que tienen las operaciones pesadas sobre el hilo principal.

Se implementan dos enfoques:

Procesamiento directo en el main thread (provocando bloqueos)
Procesamiento optimizado utilizando Isolates mediante compute()

Adicionalmente, se integran herramientas de monitoreo como Flutter DevTools y Firebase Performance Monitoring para medir el comportamiento real de la aplicación.

Requisitos
Flutter SDK 3.16 o superior
Dart 3.2 o superior
Android Studio con soporte Flutter
Emulador o dispositivo físico (API 26+)
Proyecto Firebase previamente configurado

Configuración y ejecución
Clonar el repositorio:
git clone https://github.com/dylandjg8413-gif/Garzon-post2-u8
Instalar dependencias:
flutter pub get
Configurar Firebase:
flutterfire configure
Ejecutar la aplicación:
flutter run

Tecnologías utilizadas
Tecnología	Propósito
Flutter + Dart	Desarrollo de la aplicación
Isolates (compute)	Ejecución de procesos en segundo plano
dart:developer	Instrumentación y análisis de ejecución
Firebase Performance	Medición de métricas reales
DevTools	Análisis de renderizado y rendimiento

Estructura del proyecto
lib/
├── models/
│   ├── product_model.dart       → Modelo de datos
│   └── catalog_generator.dart   → Generador de datos simulados
├── screens/
│   └── catalog_screen.dart      → Interfaz principal
├── firebase_options.dart        → Configuración de Firebase
└── main.dart                    → Punto de entrada

Optimizaciones implementadas

1. Procesamiento en hilo principal (escenario inicial)

Inicialmente, el parse del JSON se realizaba directamente en el hilo principal utilizando jsonDecode().

Esto generaba:

Bloqueos temporales de la interfaz
Caídas en la tasa de frames
Interrupciones visibles en animaciones

2. Uso de Isolates con compute()

Para mejorar el rendimiento, el procesamiento fue trasladado a un Isolate secundario:

final productos = await compute(parseProductos, jsonString);

✔ Evita bloquear el hilo principal
✔ Permite mantener la UI fluida
✔ Mejora la experiencia del usuario

3. Instrumentación con Timeline

Se agregaron trazas manuales para analizar el comportamiento del sistema:

Generación de datos
Procesamiento del JSON
Tiempo total de carga

Esto permite identificar cuellos de botella dentro de Flutter DevTools.

4. Monitoreo con Firebase Performance

Se implementaron métricas personalizadas para medir:

Tiempo de carga del catálogo
Cantidad de elementos procesados
Duración de operaciones críticas

Análisis de métricas

Antes de la optimización
Bajo rendimiento durante la carga
Frames con tiempos elevados
Congelamiento momentáneo de la interfaz
Experiencia de usuario poco fluida

Después de la optimización
Mejora notable en la fluidez
Procesamiento en segundo plano
Animaciones continuas sin interrupciones
Reducción del impacto en CPU

Resultados obtenidos
Reducción de bloqueos en la interfaz
Mejor uso de recursos del sistema
Mayor estabilidad en la ejecución
Experiencia de usuario optimizada

Conclusiones

El uso de Isolates en Flutter permite manejar operaciones costosas sin afectar la experiencia visual.
Combinado con herramientas de monitoreo, se logra identificar y optimizar puntos críticos del sistema.

Este proyecto demuestra la importancia de separar tareas pesadas del hilo principal para construir aplicaciones eficientes y escalables.

---

## Capturas de pantalla

### DevTools Timeline — Antes de compute() (frames rojos)
![DevTools antes](Saved%20Pictures/screenshot_devtools_before.png)

### DevTools Timeline — Después de compute() (mejorado)
![DevTools después](Saved%20Pictures/screenshot_devtools_after.png)

### Logcat con Firebase Performance
![Firebase Logcat](Saved%20Pictures/screenshot_firebase_logcat.png)

---
