# Informe de validación

Fecha de validación: 15 de octubre de 2026.

## Alcance

Este informe distingue los controles realmente ejecutados de las pruebas que requieren un dispositivo Android. No se reproducen como resultados propios las secuencias de ejemplo publicadas en el codelab.

## Entorno detectado

- Windows con JDK 17.
- Android SDK instalado en el equipo, incluidas las plataformas 34, 35, 36 y posteriores.
- Herramientas de plataforma y emulador disponibles.
- Aceleración WHPX disponible.
- Ningún dispositivo devuelto por `adb devices -l`.
- Ningún AVD devuelto por `emulator -list-avds`.
- Ninguna imagen de sistema instalada inicialmente.

Se intentó obtener `system-images;android-35;google_apis;x86_64` para habilitar las pruebas instrumentadas manuales. El paquete publicado tiene un tamaño aproximado de 1,74 GB y la descarga no mostró avance útil en el tiempo disponible, por lo que se detuvo sin crear un AVD.

## Verificaciones ejecutadas

### Proyecto inicial

La rama `starter` se compiló antes de modificar `MainActivity.kt`:

```text
gradlew.bat assembleDebug
BUILD SUCCESSFUL
```

Esto confirmó la resolución de dependencias, el procesamiento de recursos, la compilación Kotlin y el empaquetado inicial.

### Compilación final, pruebas unitarias y Lint

Se ejecutó desde una compilación limpia:

```text
gradlew.bat clean testDebugUnitTest lintDebug assembleDebug
BUILD SUCCESSFUL
```

Resultados comprobados:

- Compilación Kotlin: correcta.
- Procesamiento del manifiesto y recursos: correcto.
- Empaquetado del APK de depuración: correcto.
- APK generado: `app/build/outputs/apk/debug/app-debug.apk` (9.544.820 bytes).
- Pruebas unitarias: `NO-SOURCE`; el proyecto oficial no incluye casos de prueba locales.
- Android Lint: 0 errores y 26 advertencias.

Las advertencias de Lint corresponden al proyecto oficial: nivel objetivo no más reciente, versiones de dependencias conservadas, etiqueta redundante, recursos vectoriales grandes, rutas vectoriales extensas, sugerencia de plurales y recomendación de estados primitivos de Compose. No se corrigieron porque el objetivo es reproducir la solución oficial, no modernizarla.

### Inspección de implementación

Se verificó en el código que:

- Existen los siete métodos del ciclo de vida solicitados.
- Cada método invoca la implementación correspondiente de la superclase.
- Los siete mensajes `Log.d()` coinciden con el codelab.
- La etiqueta es `MainActivity`.
- Las cinco variables indicadas usan `rememberSaveable`.
- La lógica de clic, progresión de postres, ingresos y uso compartido permanece intacta.
- Los recursos gráficos, textos, manifiesto y configuración Gradle se conservaron.

### Comparación oficial

Se exportó la rama `main` del repositorio oficial y se compararon `app/src` y `app/build.gradle.kts` con el resultado. La comparación final no encontró diferencias. No fue necesario modificar versiones ni introducir compatibilidad adicional.

## Pruebas interactivas no ejecutadas

Las siguientes comprobaciones requieren una instancia real de Android y quedaron pendientes:

1. Inicio de la aplicación y observación real de `onCreate`, `onStart` y `onResume` en Logcat.
2. Clics sobre el postre y comprobación visual de contador, ingresos e imagen.
3. Botón Atrás y observación de `onPause`, `onStop` y `onDestroy`.
4. Inicio/Recientes y observación de la secuencia de segundo plano y retorno.
5. Apertura y cierre del selector de compartir y observación de `onPause`/`onResume`.
6. Rotación antes de la corrección para observar la pérdida real de estado.
7. Rotación después de la corrección para observar la restauración real de los cinco valores.

No se adjuntan capturas ni registros de esas pruebas porque no se ejecutaron.

## Procedimiento pendiente en un dispositivo

1. Abrir el proyecto en Android Studio y arrancar un dispositivo API 24 o superior.
2. Ejecutar `app` y filtrar Logcat por `tag:MainActivity`.
3. Repetir los tres casos de navegación del codelab.
4. Para evidenciar el defecto previo, cambiar temporalmente al commit anterior a `rememberSaveable`, interactuar y rotar el dispositivo.
5. Volver a la rama `main`, repetir la interacción y confirmar que contador, ingresos e imagen permanecen sincronizados tras la rotación.

## Conclusión

El proyecto compila, genera el APK y coincide con la solución oficial en código fuente y configuración. La validación de ejecución sobre Android queda pendiente por falta de un dispositivo o AVD disponible; no se considera realizada ni aprobada en este informe.
