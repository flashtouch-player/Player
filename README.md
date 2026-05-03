# Mi APK — Capacitor + ExoPlayer (Media3) — Compilación automática

Scaffold listo para empaquetar tu `index.html` como APK Android usando **Capacitor 6** y un **plugin nativo con ExoPlayer/Media3** (HLS, DASH, DRM Widevine, PiP, etc.).

## 🚀 Compilación online con GitHub Actions (sin instalar nada)

1. Crea un repositorio nuevo en https://github.com/new
2. Sube **todo el contenido de esta carpeta** al repo (puedes arrastrar el ZIP descomprimido a la web de GitHub o usar `git`).
3. Ve a la pestaña **Actions** del repo → el workflow **Build APK** se lanza solo en cada push.
4. Cuando termine (≈5 min) abre el run → sección **Artifacts** → descarga `app-debug-apk` → dentro está tu `app-debug.apk`.

> 💡 Antes de subir, sustituye `www/index.html` por **TU** `index.html`. Cualquier `<video>` que quieras nativo, reemplázalo por `ExoPlayer.play({...})` (ver abajo).

## 📂 Sustituir tu HTML
Copia tu `index.html` y todos sus assets (CSS, JS, imágenes) dentro de `www/`. Asegúrate de mantener `www/exoplayer-bridge.js` para usar el reproductor nativo.

## 🎬 Llamar al reproductor nativo desde tu HTML
```html
<script type="module">
  import { ExoPlayer } from './exoplayer-bridge.js';

  ExoPlayer.play({
    url: 'https://test-streams.mux.dev/x36xhzz/x36xhzz.m3u8',
    title: 'Mi canal',
    type: 'hls',                  // 'hls' | 'dash' | 'progressive' | 'auto'
    // drmLicenseUrl: '...',      // opcional
    // drmScheme: 'widevine'      // 'widevine' | 'playready'
  });
</script>
```
API disponible:
- `ExoPlayer.play({ url, title?, type?, drmLicenseUrl?, drmScheme? })`
- `ExoPlayer.stop()` / `pause()` / `resume()` / `enterPip()`

En navegador hace fallback a `<video>` para que puedas probar; en el APK se abre el reproductor nativo Media3/ExoPlayer.

## 🛠️ Compilación local (opcional)
```bash
npm install
npx cap add android
# (el workflow integra el plugin automáticamente — para local mira .github/workflows/build-apk.yml
#  y replica los pasos "Integrate ExoPlayer plugin module")
npx cap sync android
cd android && ./gradlew assembleDebug
# APK en: android/app/build/outputs/apk/debug/app-debug.apk
```

## 📦 APK firmado para producción
El workflow genera un APK **debug** (válido para instalar y probar). Para Play Store necesitas `assembleRelease` + firma. Avísame y lo añado.
