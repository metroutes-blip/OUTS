# Meter Reader (OUTS)

Offline web app for out-card meter reading. The plain files in this folder are the whole app; there is no build step for the web version.

## Android app (install file)
The same files also ship as an Android app, `MeterReader.apk`. This is built from the `android-app` branch.

- **Build:** the *Android app* workflow (`.github/workflows/android.yml`) wraps the web files in a [Capacitor](https://capacitorjs.com) shell and signs the APK. It runs on every push to `android-app`; the APK is kept as a build artifact on the run's page.
- **Release:** bump `APP_VERSION` in `app.js`, push, then push a matching tag (`git tag v9.4 && git push origin v9.4`). The APK is published at `https://github.com/metroutes-blip/outs/releases/latest/download/MeterReader.apk`.
- **Install or update on a phone:** open that link, allow *Install unknown apps* when asked, tap Install. Updating keeps bundles and reads.
- **Signing key:** repo secrets `ANDROID_KEYSTORE_B64` (a PKCS12 keystore with one key, base64) and `ANDROID_KEYSTORE_PASSWORD`. The LVB keystore works as-is. Every release must be signed with the same key, or phones have to uninstall (losing their data) before they can update.
- **In the app:** code that differs is behind `NATIVE` in `app.js`. Backups save to *Documents / MeterReader*, and *Auto* backup rewrites the day's file there. The bundle email opens the phone's mail app with the CSV and report attached. Back goes to the previous screen. There is no service worker or Refresh button. Leaflet is bundled so the map screen opens offline; map tiles still need a connection.
- **Moving from the web version:** data does not carry over from the browser. Finish or email open bundles first, or reload the CSV in the app.
