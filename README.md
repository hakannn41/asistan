# Sesli Asistan (Android)

Bir isimle seslenince dinleyen, söylediğiniz komutu telefonda yapan sesli asistan.

## Kurulum

1. **Android Studio**'yu açın (Hedgehog veya daha yeni) → `Open` → bu klasörü seçin. Gradle senkronizasyonunu bekleyin.
2. Telefonda **Geliştirici seçenekleri → USB hata ayıklama**yı açıp telefonu bağlayın, ▶ Run'a basın.
3. Uygulamada:
   - Çağrı ismini yazın (örn. `asistan`, `jarvis`).
   - İsterseniz Claude API anahtarınızı girin (serbest sorular için; console.anthropic.com).
   - **İzinleri ver** düğmesine basın. Açılan ayarda **"Diğer uygulamaların üzerinde göster"** iznini de açın (arka planda uygulama açabilmesi için gerekli).
   - **Asistanı başlat** düğmesine basın.

## Kullanım

Önce çağrı ismini söyleyin ("Dinliyorum" derse komutu söyleyin) ya da tek cümlede söyleyin:

| Komut | Ne yapar |
|---|---|
| "Asistan, WhatsApp'ı aç" | Uygulamayı adıyla açar |
| "Asistan, Instagram'ı aç" | Herhangi bir yüklü uygulamayı adıyla açar |
| "Asistan, WhatsApp'tan Ayşe'ye mesaj gönder merhaba" | Rehberden bulup WhatsApp mesajını yazar ve (erişilebilirlik servisi açıksa) gönderir |
| "Asistan, YouTube'dan tarkan şımarık aç" | YouTube'da arama sonuçlarını açar (ilk videoya kendisi basmaz) |
| "Asistan, Ahmet'i ara" | Rehberden bulup arar |
| "Asistan, Ayşe'ye mesaj gönder merhaba nasılsın" | SMS gönderir |
| "Asistan, saat 7:30'a alarm kur" | Alarm kurar |
| "Asistan, 10 dakika zamanlayıcı kur" | Zamanlayıcı kurar |
| "Asistan, Ankara'nın nüfusu kaç?" | Claude'a sorar, cevabı sesli söyler |
| "Asistan, kendini kapat" | Asistanı durdurur |

## WhatsApp'ta otomatik gönderme

Android, bir uygulamanın başka uygulamada "Gönder"e basmasına yalnızca erişilebilirlik servisiyle izin verir.
Uygulamadaki **"WhatsApp'ı otomatik gönderme"** düğmesine basın ve listeden **Sesli Asistan - WhatsApp gönderme** servisini açın.
Servis kapalıysa mesaj yazılı hâlde hazır gelir, siz gönder tuşuna basarsınız. Servis yalnızca WhatsApp penceresinde ve sesli komuttan sonraki 10 saniye içinde çalışır.
Rehberdeki numaralar 0 ile başlıyorsa Türkiye (+90) kabul edilir.

## Dosyalar

- `WhatsAppSendService.kt`: WhatsApp'ta gönder tuşuna basan erişilebilirlik servisi

- `MainActivity.kt`: ayar ekranı, izinler, başlat/durdur
- `AssistantService.kt`: arka planda sürekli dinleme, çağrı ismi yakalama, sesli cevap
- `CommandHandler.kt`: komutları anlayıp telefonda uygular (yeni komut eklemek için burası)
- `ClaudeClient.kt`: serbest sorular için Claude API bağlantısı (model: `claude-sonnet-5-5`)

## Bilinen sınırlar

- **Dinleme bip sesi:** Android'in konuşma tanıma servisi her dinlemeye başlarken bazı telefonlarda kısa bir ses çıkarabilir. Bunu tamamen önlemek için Picovoice Porcupine gibi cihaz üstü bir "uyandırma kelimesi" motoru eklemek gerekir (ücretsiz anahtarla özel isim eğitilebilir). `AssistantService` içindeki `handleSpeech` yerine bu motor bağlanır.
- **Pil:** Sürekli dinleme pil tüketir. Telefonun pil ayarlarında uygulamayı "kısıtlama" dışına alın.
- **Ses tanıma gizliliği:** Google'ın konuşma tanıma servisi sesi sunucuya gönderebilir (çevrimdışı Türkçe paketi yüklüyse yerelde çalışır).
- **API anahtarı** cihazda saklanır. Yalnızca kendi telefonunuz için uygundur; uygulamayı başkalarına dağıtacaksanız anahtarı bir sunucu üzerinden kullanın.
- Saat komutlarında rakamla söyleyin ("7:30"). "Yedi buçuk" gibi yazıyla söylemeler anlaşılmayabilir.
- Android, uygulamaların arka planda diğer uygulamaları başlatmasını kısıtlar; bu yüzden "diğer uygulamaların üzerinde göster" izni gereklidir.
