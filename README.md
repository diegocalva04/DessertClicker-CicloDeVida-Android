# Dessert Clicker: ciclo de vida de una actividad

Aplicación Android desarrollada en Kotlin y Jetpack Compose siguiendo el codelab oficial de Google **[Etapas del ciclo de vida de la actividad](https://developer.android.com/codelabs/basic-android-kotlin-compose-activity-lifecycle?hl=es-419)**.

El proyecto parte de la rama `starter` del repositorio oficial [Dessert Clicker](https://github.com/google-developer-training/basic-android-kotlin-compose-training-dessert-clicker) y reproduce su solución final sin rediseños ni funcionalidades adicionales. Al tocar el postre, la aplicación incrementa las unidades vendidas y los ingresos, cambia la imagen según el progreso y permite compartir el resultado.

## Objetivos del ejercicio

- Observar las transiciones del ciclo de vida de `MainActivity` en Logcat.
- Implementar `onCreate()`, `onStart()`, `onResume()`, `onRestart()`, `onPause()`, `onStop()` y `onDestroy()`.
- Comprender la destrucción y recreación de una actividad ante un cambio de configuración.
- Conservar los cinco valores de estado de la interfaz mediante `rememberSaveable`.

## Tecnologías y requisitos

- Kotlin y Jetpack Compose con Material 3.
- Android Gradle Plugin 8.8.0 y Gradle 8.12 (incluido mediante Gradle Wrapper).
- JDK 17.
- Android Studio con Android SDK 35 instalado.
- Dispositivo o emulador con Android 7.0 (API 24) o superior.

## Abrir y ejecutar

1. Clona el repositorio o descarga su contenido.
2. Abre la carpeta raíz en Android Studio.
3. Espera a que finalice la sincronización de Gradle.
4. Selecciona un dispositivo físico o virtual con API 24 o superior.
5. Ejecuta la configuración `app`.

También se puede comprobar la compilación desde una terminal en la raíz del proyecto:

```powershell
.\gradlew.bat clean testDebugUnitTest lintDebug assembleDebug
```

El APK de depuración se genera en `app/build/outputs/apk/debug/app-debug.apk`.

## Ciclo de vida y Logcat

`MainActivity` registra cada devolución de llamada con la etiqueta `MainActivity`:

| Método | Momento observado por Android |
|---|---|
| `onCreate()` | Crea e inicializa una nueva instancia de la actividad. |
| `onStart()` | Hace visible la actividad. |
| `onResume()` | Entrega el foco y permite interactuar con ella. |
| `onPause()` | Retira el foco, aunque la actividad aún puede estar visible. |
| `onStop()` | La actividad deja de ser visible. |
| `onRestart()` | Una actividad detenida vuelve a iniciarse. |
| `onDestroy()` | La instancia se destruye al finalizar o recrearse. |

Para ver únicamente estos mensajes, abre Logcat y usa el filtro:

```text
tag:MainActivity
```

## Cambios de configuración y estado

Una rotación destruye y vuelve a crear la actividad. Con `remember`, los valores sobreviven a recomposiciones, pero no a esa recreación. El codelab corrige la pérdida de datos sustituyendo `remember` por `rememberSaveable` en exactamente cinco variables:

- `revenue`
- `dessertsSold`
- `currentDessertIndex`
- `currentDessertPrice`
- `currentDessertImageId`

Así, Compose puede restaurar los ingresos, la cantidad vendida, el precio y la imagen correspondiente después del cambio de configuración.

## Etapas e historial

El historial Git se creó desde cero, sin importar el historial del repositorio de Google, y conserva la evolución real del ejercicio:

1. Preparación de una copia independiente de la rama `starter`.
2. Registro de `onCreate()`.
3. Registro de `onStart()`.
4. Registro de las cinco devoluciones de llamada restantes.
5. Migración de las cinco variables a `rememberSaveable`.
6. Comparación del código y la configuración con la rama `main` oficial.
7. Documentación de la implementación y de las validaciones disponibles.

Consulta los identificadores y mensajes exactos con:

```bash
git log --oneline --reverse
```

## Validación realizada

El 15 de octubre de 2026 se verificó localmente lo siguiente:

- La rama `starter` compiló antes de aplicar los cambios.
- La compilación final, el análisis Lint y la tarea de pruebas unitarias terminaron correctamente.
- Se generó un APK de depuración de 9.544.820 bytes.
- Lint informó `0 errors` y `26 warnings`; las advertencias proceden de recursos y versiones conservados de la solución oficial.
- `app/src` y `app/build.gradle.kts` no presentan diferencias frente a la rama `main` oficial.
- No existen pruebas unitarias en el proyecto oficial, por lo que Gradle informó `testDebugUnitTest NO-SOURCE`.

No se ejecutaron las pruebas interactivas de Atrás, Inicio/Recientes, selector de compartir ni rotación. No había dispositivo conectado ni AVD configurado; la imagen de sistema necesaria tampoco estaba instalada. Por tanto, este repositorio no presenta secuencias de Logcat ni resultados visuales como si se hubieran observado. Los pasos pendientes están detallados en [docs/INFORME_VALIDACION.md](docs/INFORME_VALIDACION.md).

## Licencia y procedencia

El código y los recursos originales conservan sus avisos de copyright y la licencia Apache 2.0 incluida en [LICENSE](LICENSE). La estructura, recursos y solución se mantienen fieles al material educativo oficial de Google.
