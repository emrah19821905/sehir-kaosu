# Şehir Kaosu – Android (WebView)

## Açma
Android Studio → Open → bu klasör. (Gradle wrapper otomatik oluşur; elle: `gradle wrapper --gradle-version 8.14`)
Gerekenler: JDK 17, Android SDK 36. Sürüm uyarısı çıkarsa Studio'nun "Upgrade Assistant" aracını kullanın.

## Debug APK (test)
./gradlew assembleDebug
→ app/build/outputs/apk/debug/app-debug.apk

## Yükleme anahtarı (bir kez oluşturun, yedekleyin!)
keytool -genkeypair -v -keystore sehirkaosu-release.jks -alias sehirkaosu \
  -keyalg RSA -keysize 2048 -validity 10000
cp keystore.properties.example keystore.properties   # şifreleri doldurun

## Release
./gradlew bundleRelease     → app/build/outputs/bundle/release/app-release.aab   (Google Play)
./gradlew assembleRelease   → app/build/outputs/apk/release/app-release.apk      (doğrudan dağıtım)

## Play Console kontrol listesi
- Play App Signing'i açın, AAB'yi yükleyin; her yüklemede versionCode'u artırın.
- targetSdk = 36 (zorunlu). İnternet izni yok → Data safety: veri toplanmıyor.
- İçerik derecelendirmesi: şiddet içeriği (silah/yaya vurma) beyan edilmeli.
- Gizlilik politikası URL'si, 512x512 ikon ve ekran görüntüleri gerekir.
- Tablet/katlanabilir (>=600dp) cihazlarda Android 16 yatay kilidi yok sayar; oyun duyarlı olduğu için sorun olmaz.

## GitHub'dan APK indirme
1. Projeyi GitHub'a yükleyin (klasörün kendisi repo kökü olmalı).
2. Actions sekmesi → "Build APK" → son çalışma → Artifacts → `sehir-kaosu-apk` (zip içinde APK).
3. Kalıcı link için: `git tag v1.0.0 && git push --tags` → Releases sayfasında APK görünür.
4. İmzalı release için Settings → Secrets → Actions: KEYSTORE_BASE64 (`base64 -w0 sehirkaosu-release.jks`), KEYSTORE_PASSWORD, KEY_ALIAS, KEY_PASSWORD.
Secret yoksa sadece debug APK üretilir (telefona kurulabilir).
