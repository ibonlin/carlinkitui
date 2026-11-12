# Crossland Launcher

Opel Crossland için modern, koyu temalı araç ana ekranı. Bu proje, tasarladığımız
HTML/CSS/JS arayüzünü (`app/src/main/assets/launcher/index.html`) tam ekran bir
Android **WebView launcher** uygulamasının içine gömer. Carlinkit (ve benzeri)
Android AI Box'lara **varsayılan ana ekran (launcher)** olarak kurulmak üzere
yapılandırılmıştır.

> **Not:** APK bu ortamda derlenemedi (Android SDK yok + Google sunucuları kapalı).
> Bu, Android Studio'da veya komut satırında **senin derleyeceğin hazır projedir.**
> Derleme için tek ihtiyaç: Android SDK (Android Studio otomatik kurar) ve internet.

---

## 1. Derleme

### Android Studio ile (en kolay)
1. Android Studio → **File > Open** → bu klasörü seç.
2. İlk açılışta Gradle sync başlar; AGP ve Android SDK bileşenlerini otomatik indirir.
3. Üstten **Build > Build Bundle(s)/APK(s) > Build APK(s)**.
4. Çıkan APK: `app/build/outputs/apk/debug/app-debug.apk`

### Komut satırı ile
```bash
# Android SDK kuruluysa ve ANDROID_HOME tanımlıysa:
./gradlew assembleDebug
# Çıktı: app/build/outputs/apk/debug/app-debug.apk
```
`app-debug.apk` debug anahtarıyla otomatik imzalanır — ayrıca imzalamaya gerek yok,
doğrudan kurulabilir.

---

## 2. Kutuya Kurma

**Yöntem A — USB (en basit):**
1. `app-debug.apk`'yı bir USB belleğe kopyala.
2. Kutuya tak, kutunun **dosya yöneticisi**nden APK'ya dokun.
3. İlk seferde "bilinmeyen kaynaklara izin ver" isteyebilir → izin ver.

**Yöntem B — ADB:**
```bash
adb install app-debug.apk
```
Kutuda geliştirici seçenekleri + USB/kablosuz ADB açık olmalı. Çoğu Carlinkit
kutusu kablosuz ADB destekler (Ayarlar içinde IP görünür).

---

## 3. Varsayılan Launcher Yapma
- Kutuda **Ayarlar > Uygulamalar > Varsayılan uygulamalar > Ana ekran** → **Crossland** seç.
- Ya da **Home** tuşuna bas; çıkan seçicide **Crossland** → **Her zaman**.

## 4. Geri Dönüş (güvenli)
- Varsayılanı tekrar kutunun **kendi launcher'ına** çevir (aynı menü), ya da
- Uygulamayı kaldır: `adb uninstall net.adda.crosslandlauncher`
> Bu uygulama yalnızca Carlinkit kutusuna kurulur. **Aracın NAC sistemine veya
> firmware'ine hiçbir müdahale yoktur** — Opel ekranının orijinal yazılımı değişmez.

---

## Şu an NE çalışır / NE çalışmaz (dürüst durum)

**Çalışır:**
- Tam ekran modern ana ekran, canlı saat/tarih, akıcı dokunmatik etkileşimler.
- Uygulama çekmecesi ve dock → kutudaki gerçek uygulamaları açmak için bağlanabilir
  (CarPlay/Spotify/YouTube vb. — Adım: her butona ilgili uygulamanın `package name`'i).
- Varsayılan launcher olarak kutunun açılış ekranı olur.

**Henüz çalışmaz / ayrı iş:**
- Widget'lardaki veriler şimdilik **demo** (klima, menzil, şarkı, navigasyon sahte değerler).
- **Araç verisi (klima, menzil, lastik basıncı):** Carlinkit kutusu araç CAN verisine
  erişmez; bu değerler gerçeğe bağlanamaz (sadece görsel kalır).
- **Müzik "şimdi çalıyor":** Android `MediaSession` üzerinden, müzik **kutudaki bir
  Android uygulamasından** çalıyorsa okunabilir (sonraki aşama). CarPlay içeriği
  sandbox'ta olduğu için oradan okunamaz.
- **Hava durumu / navigasyon:** kutu internetteyse bir API'den beslenebilir (sonraki aşama).

Yani bu paket: **gerçek, kurulabilir modern bir launcher.** Widget'ları canlı veriye
bağlamak sıradaki adım ve her biri kutunun neyi açtığına göre ayrı ayrı ele alınır.

---

## Dosya Yapısı
```
app/src/main/
├── AndroidManifest.xml          # HOME + LAUNCHER olarak tanımlı
├── java/.../MainActivity.java   # tam ekran WebView, ekranı açık tutar
├── assets/launcher/index.html   # ARAYÜZ (burayı düzenleyerek tasarımı değiştir)
└── res/                         # ikon, tema, renkler
```
Arayüzü değiştirmek için sadece `assets/launcher/index.html` dosyasını düzenle ve
yeniden derle. (Prototipi tarayıcıda da açıp düzenleyebilirsin.)

Paket adı: `net.adda.crosslandlauncher`
minSdk 26 (Android 8.0+) · targetSdk 34 · Gradle 8.7 · AGP 8.5.2
