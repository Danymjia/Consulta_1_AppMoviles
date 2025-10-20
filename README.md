# Consulta 1 — Ionic Angular + Capacitor (Android)

Guía práctica para configurar icono personalizado, Splash Screen y AndroidManifest.xml. Incluye pasos, verificación y solución de problemas, con espacios para capturas.

## Índice
- **[Requisitos previos](#requisitos-previos)**
- **[Icono personalizado](#icono-personalizado)**
- **[Splash Screen](#splash-screen)**
- **[AndroidManifest.xml](#androidmanifestxml)**
- **[Sincronización y build](#sincronización-y-build)**
- **[Verificación visual (capturas)](#verificación-visual-capturas)**
- **[Solución de problemas](#solución-de-problemas)**

---

## Requisitos previos
- Node, npm, Ionic CLI y Android Studio instalados.
- Proyecto con Capacitor inicializado.
- Archivo de configuración actual: `capacitor.config.ts`

```ts
// c:/Users/narva/Downloads/APLICACIONES MOVILES/moviles/consulta_1/capacitor.config.ts
import type { CapacitorConfig } from '@capacitor/cli';

const config: CapacitorConfig = {
  appId: 'tu.dominio.app',
  appName: 'Mi App',
  webDir: 'www',
  plugins: {
    SplashScreen: {
      launchShowDuration: 3000,
      launchAutoHide: true,
      backgroundColor: '#ffffff',
      androidSplashResourceName: 'splash',
      androidScaleType: 'CENTER_CROP',
      showSpinner: false,
      splashFullScreen: true,
      splashImmersive: true
    }
  }
};
```

---

## Icono personalizado

- **Requisitos de imagen**
  - `resources/icon.png` — PNG 1024x1024, preferible fondo transparente.

- **Generación de recursos**
  ```bash
  npm i -D @capacitor/assets
  npx capacitor-assets generate --android
  ```

- **Archivos generados (Android)**
  - `android/app/src/main/res/mipmap-*/ic_launcher.png`
  - `android/app/src/main/res/mipmap-anydpi-v26/ic_launcher.xml`

- **Verificación**
  - Revisa que cambien las fechas de modificación de los archivos `mipmap-*`.

- **Captura sugerida**
  - [Inserte aquí la captura del icono en el launcher]

---

## Splash Screen

- **Requisitos de imagen**
  - `resources/splash.png` — PNG 2732x2732, contenido centrado (área segura ~1200x1200).

- **Generación de recursos**
  ```bash
  npx capacitor-assets generate --android
  ```
  - Se crean `android/app/src/main/res/drawable*/splash.png` y variantes `drawable-port-*`.

- **Tema de lanzamiento (Android ≤ 30)**
  Archivo: `android/app/src/main/res/values/styles.xml`
  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <resources>
    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
      <item name="colorPrimary">@color/colorPrimary</item>
      <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
      <item name="colorAccent">@color/colorAccent</item>
    </style>

    <style name="AppTheme.NoActionBar" parent="Theme.AppCompat.DayNight.NoActionBar">
      <item name="windowActionBar">false</item>
      <item name="windowNoTitle">true</item>
      <item name="android:background">@null</item>
    </style>

    <style name="AppTheme.NoActionBarLaunch" parent="Theme.AppCompat.DayNight.NoActionBar">
      <item name="android:windowBackground">@drawable/splash</item>
    </style>
  </resources>
  ```

- **Tema de lanzamiento (Android 12+, API 31)**
  Archivo: `android/app/src/main/res/values-v31/styles.xml` (créalo si no existe)
  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <resources>
    <style name="AppTheme.NoActionBarLaunch" parent="Theme.SplashScreen">
      <item name="windowSplashScreenBackground">@android:color/white</item>
      <item name="windowSplashScreenAnimatedIcon">@drawable/splash</item>
      <item name="postSplashScreenTheme">@style/AppTheme.NoActionBar</item>
      <item name="windowSplashScreenIconBackgroundColor">@null</item>
    </style>
  </resources>
  ```

- **`AndroidManifest.xml` debe apuntar al tema de launch**
  Archivo: `android/app/src/main/AndroidManifest.xml`
  ```xml
  <activity
    android:name="io.ionic.starter.MainActivity"
    android:theme="@style/AppTheme.NoActionBarLaunch"
    android:exported="true"
    android:launchMode="singleTask"
    android:configChanges="orientation|keyboardHidden|keyboard|screenSize|locale|smallestScreenSize|screenLayout|uiMode">
    <intent-filter>
      <action android:name="android.intent.action.MAIN" />
      <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
  </activity>
  ```

- **Plugin opcional para control por código**
  ```bash
  npm i @capacitor/splash-screen
  npx cap sync android
  ```
  Y en `src/app/app.component.ts`:
  ```ts
  import { SplashScreen } from '@capacitor/splash-screen';
  // SplashScreen.show({ autoHide: false });
  // ... inicialización ...
  // SplashScreen.hide();
  ```

- **Captura sugerida**
  - [Inserte aquí la captura del splash en arranque]

---

## AndroidManifest.xml

- **Ubicación**: `android/app/src/main/AndroidManifest.xml`
- **Importante (AGP moderno)**: No usar `package` en `<manifest>`. Definir `namespace` en `android/app/build.gradle`.
  ```gradle
  android {
    namespace "tu.dominio.app" // Debe coincidir con appId de capacitor
  }
  ```
- **Strings**: `android/app/src/main/res/values/strings.xml`
  ```xml
  <resources>
    <string name="app_name">Mi App</string>
    <string name="title_activity_main">Mi App</string>
  </resources>
  ```

- **Captura sugerida**
  - [Inserte aquí la captura del AndroidManifest.xml con el tema de launch]

---

## Sincronización y build

```bash
ionic build
npx cap sync android
npx cap open android
```
En Android Studio:
- Build > Clean Project
- Build > Rebuild Project
- Ejecutar en emulador o dispositivo

---

## Verificación visual (capturas)
- **[icono]** Captura del icono en el launcher del emulador/dispositivo.
- **[splash]** Captura del splash en arranque en frío.
- **[manifest]** Captura del `AndroidManifest.xml` mostrando `AppTheme.NoActionBarLaunch`.

> Nota: Para ver el splash, realiza un arranque en frío: fuerza detención o desinstala y vuelve a abrir la app.

---

## Solución de problemas
- **Se ve el icono antiguo**
  - Regenera con `npx capacitor-assets generate --android`.
  - Desinstala la app y reinstala (o incrementa `versionCode` en `build.gradle`).

- **No se ve la imagen de splash (Android 12+)**
  - Crea `values-v31/styles.xml` con `windowSplashScreenAnimatedIcon=@drawable/splash`.
  - Asegúrate de que exista `res/drawable-*/splash.png`.

- **Error: package en AndroidManifest**
  - Quitar `package` de `<manifest>` y usar `namespace` en `build.gradle`.

- **Autocompletado/IDE**
  - Reinicia TS server (VS Code) y asegúrate de `npm install` sin errores.

---

## Metadatos del proyecto
- `appId`: `tu.dominio.app`
- `appName`: `Mi App`
- `webDir`: `www`
- Splash configurado en `capacitor.config.ts` y estilos Android.

---

Documentación generada automáticamente. Actualiza las capturas donde se indica para evidencias.
