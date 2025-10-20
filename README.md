# Consulta 1 — Ionic Angular + Capacitor Android

## Índice
- **[Requisitos previos](#requisitos-previos)**
- **[Icono personalizado](#icono-personalizado)**
- **[Splash Screen](#splash-screen)**
- **[AndroidManifest.xml](#androidmanifestxml)**
- **[Sincronización y build](#sincronización-y-build)**
- **[Capturas](#capturas)**

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

  - `resources/icon.png` — PNG 1024x1024, preferible fondo transparente.
 
  - <img width="512" height="512" alt="icon" src="https://github.com/user-attachments/assets/e9e8c7c6-aea8-46f1-8160-99f11c69c18f" />

- **Generación de recursos**
  ```bash
  npm i -D @capacitor/assets
  npx capacitor-assets generate --android
  ```

- **Archivos generados (Android)**
  - `android/app/src/main/res/mipmap-*/ic_launcher.png`
  - `android/app/src/main/res/mipmap-anydpi-v26/ic_launcher.xml`
    
---

## Splash Screen

  - `resources/splash.png` — PNG 2732x2732, contenido centrado (área segura ~1200x1200).
 
  - <img width="385" height="807" alt="image" src="https://github.com/user-attachments/assets/cb51c74c-1992-4dc5-9f95-1bd8b87e74eb" />

- **Generación de recursos**
  ```bash
  npx capacitor-assets generate --android
  ```

- **Tema de lanzamiento**
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
  
---

## AndroidManifest.xml

- **Ubicación**: `android/app/src/main/AndroidManifest.xml`
  
- **Strings**: `android/app/src/main/res/values/strings.xml`
  ```xml
  <resources>
    <string name="app_name">Mi App</string>
    <string name="title_activity_main">Mi App</string>
  </resources>
  ```
---

## Sincronización y build

```bash
ionic build
npx cap sync android
npx cap open android
```

---

## Capturas
- **[icono]**

<img width="376" height="194" alt="image" src="https://github.com/user-attachments/assets/cc65513e-fa81-4c5d-a739-4ba51a80b1b3" />

- **[splash]** Captura del splash en arranque en frío.

<img width="385" height="807" alt="image" src="https://github.com/user-attachments/assets/9b947d56-cc8c-48e0-8cd2-85d7ee6d7840" />

- **[spp]**

<img width="411" height="899" alt="image" src="https://github.com/user-attachments/assets/32ad5a89-80d1-42aa-bbf8-0545b4d61ca6" />

<img width="466" height="902" alt="image" src="https://github.com/user-attachments/assets/1242a93c-7336-4497-9bef-a70a329aba08" />

<img width="432" height="908" alt="image" src="https://github.com/user-attachments/assets/67125177-b87d-468b-b1cd-ece19cd0d5ed" />



