==========================================================================
PROJE ADI: Socket Programming - Data Transmission with Error Detection
KONU: Hata Tespit Yöntemleri ve Veri İletişim Simülasyonu
TARİH: 17.12.2025
==========================================================================

1. PROJE HAKKINDA
--------------------------------------------------------------------------
Bu proje, veri iletişiminde kullanılan hata tespit yöntemlerinin (Error 
Detection) ve veri bozulması (Data Corruption) senaryolarının simüle 
edildiği bir ağ uygulamasıdır. 

Proje, ödevde belirtilen Client-Server-Client mimarisine uygun olarak 
3 ana bileşenden oluşmaktadır:
1. Client 1 (Commander): Veriyi gönderen ve hata denetim kodunu ekleyen taraf.
2. Server (Jammer): Veriyi taşıyan ve kasıtlı olarak bozan ara düğüm.
3. Client 2 (Artillery): Veriyi alan ve bütünlüğünü doğrulayan taraf.

Sistem, terminal üzerinden interaktif bir "Savaş Oyunu" konseptiyle 
görselleştirilmiştir.

2. DOSYA YAPISI VE GÖREVLERİ
--------------------------------------------------------------------------

A) utils.py (Algoritma Kütüphanesi)
   - Projenin matematiksel işlem merkezidir.
   - Ödev gereksinimlerinde belirtilen algoritmaları içerir:
     * Parity Bit (Tek/Çift Parity) 
     * 2D Parity (Matris Tabanlı Kontrol) 
     * CRC (Cyclic Redundancy Check - Manuel Polinom Bölme) 
   - Hazır kütüphane fonksiyonları yerine algoritmalar manuel kodlanmıştır.

B) commander.py (Client 1 - Gönderici) 
   - Kullanıcıdan metin (askeri emir) girişi alır .
   - Kullanıcının seçtiği yönteme göre (Parity, 2D Parity, CRC) kontrol 
     bitsini üretir .
   - Veriyi "VERİ|YÖNTEM|KOD" formatında paketleyip sunucuya iletir .
   - Soket yapısı: TCP/IP, Non-persistent connection.

C) jammer.py (Server - Ara Düğüm ve Bozucu) 
   - Client 1'den gelen paketi dinler ve yakalar .
   - Kullanıcıya "Hata Enjeksiyonu" menüsü sunar :
     1. Bit Flip: Rastgele bir bitin ters çevrilmesi .
     2. Burst Error: Belirli bir aralıktaki verinin bozulması .
     3. No Error: Verinin olduğu gibi iletilmesi.
   - Bozulan (veya sağlam) paketi Client 2'ye iletir .

D) artillery.py (Client 2 - Alıcı) 
   - Sunucudan gelen paketi alır.
   - Paketi "split" ederek veri ve kontrol kodunu ayırır .
   - Gelen veriye göre kontrol kodunu yeniden hesaplar .
   - Gelen kod ile hesaplanan kodu karşılaştırarak "DATA CORRECT" veya 
     "DATA CORRUPTED" sonucunu ekrana basar .

3. GEREKSİNİMLERİN KARŞILANMA DURUMU
--------------------------------------------------------------------------
[✓] Socket Bağlantısı: İki istemci ve bir sunucu arasında TCP bağlantısı kuruldu.
[✓] Metin Girişi: Kullanıcıdan dinamik veri girişi sağlandı.
[✓] Yöntem Seçimi: Parity, 2D Parity ve CRC seçenekleri eklendi.
[✓] Paket Formatı: İletişim "DATA|METHOD|HASH" formatında sağlandı.
[✓] Hata Enjeksiyonu: Sunucu tarafında Bit Flip ve Burst Error uygulandı.
[✓] Doğrulama: Alıcı tarafında veri bütünlüğü kontrol edilip sonuç raporlandı.

4. KURULUM VE ÇALIŞTIRMA (HOW TO RUN)
--------------------------------------------------------------------------
Projenin çalışması için Python 3 ve 'colorama' kütüphanesi gereklidir.

Gerekli Kütüphane Kurulumu:
$ pip install colorama

Çalıştırma Sırası (3 Farklı Terminalde):
Sistemin doğru çalışması için aşağıdaki sıra takip edilmelidir:

1. Terminal -> python artillery.py  (Alıcı dinlemeye başlar)
2. Terminal -> python jammer.py     (Sunucu köprüyü kurar)
3. Terminal -> python commander.py  (Gönderici emri girer)

5. KULLANILAN TEKNOLOJİLER
--------------------------------------------------------------------------
- Dil: Python 3
- Kütüphaneler: socket, random, time, binascii
- Görselleştirme: colorama (Renkli terminal çıktıları için)

--------------------------------------------------------------------------


==============================================================================
COMMAND LINE WARFARE: THE JAMMER - OYUN REHBERİ
==============================================================================

1. OYUNUN HİKAYESİ VE AMACI
------------------------------------------------------------------------------
Savaş alanındasın. Karargah (Commander), cephedeki Topçu Birliği'ne (Artillery) 
kritik atış koordinatları göndermek zorunda. Ancak düşman hattında bir 
Sinyal Karıştırıcı (Jammer) var ve bu mesajları yakalayıp bozmaya çalışıyor.

Amacımız: 
Veri iletiminde kullanılan "Hata Tespit Yöntemlerini" (Parity, CRC vb.) 
kullanarak, düşmanın mesajı bozup bozamadığını simüle etmek.

2. ROLLER (KİM NE YAPAR?)
------------------------------------------------------------------------------
[A] COMMANDER (Komutan - Client 1):
    Görevi: Emir vermek ve bu emri şifrelemek.
    Yeteneği: Emri korumak için Zayıf (Parity) veya Güçlü (CRC) güvenlik 
    yöntemi seçebilir.

[B] JAMMER (Düşman - Server):
    Görevi: Hatta sızmak.
    Yeteneği: Mesajı yakalar ve içeriğini değiştirebilir (Tek bir harfi veya 
    biti bozabilir).

[C] ARTILLERY (Topçu - Client 2):
    Görevi: Emri uygulamak.
    Yeteneği: Gelen mesajın bozulup bozulmadığını kontrol eder. 
    - Eğer mesaj sağlamsa: "HEDEF VURULDU" yazar.
    - Eğer mesaj bozuksa: "SİNYAL BOZUK, ATIŞ İPTAL" yazar.

3. ADIM ADIM OYNANIŞ
------------------------------------------------------------------------------
Oyunu oynamak için 3 ayrı terminal penceresine ihtiyacınız var.

ADIM 1: BAĞLANTILARI KUR
   Sırasıyla şu kodları terminallerde çalıştır:
   1. Terminal: python artillery.py  (Topçu mevziye yerleşir)
   2. Terminal: python jammer.py     (Düşman pusuya yatar)
   3. Terminal: python commander.py  (Komutan masaya oturur)

ADIM 2: EMİR GÖNDER (Commander Ekranı)
   - Bir emir yaz (Örn: "SALDIR", "102,45 KOORDINAT").
   - Bir güvenlik yöntemi seç:
     * [1] Parity: Düşman kolayca kandırabilir.
     * [3] CRC-16: En güvenli yöntemdir, en ufak hatayı yakalar.

ADIM 3: SABOTAJ YAP (Jammer Ekranı)
   - Komutanın mesajı düşman ekranına düşer.
   - Bir saldırı yöntemi seç:
     * [1] Sızma Yok: Mesajı elleme, olduğu gibi geçsin.
     * [2] Bit Flip: Mesajın binary kodunda tek bir biti değiştir.
     * [3] Burst Error: Mesajı karala (büyük hasar ver).

ADIM 4: SONUCU İZLE (Artillery Ekranı)
   - Topçu birliği hesaplamayı yapar.
   - Eğer seçilen koruma yöntemi (CRC vb.), düşmanın yaptığı sabotajı 
     fark ederse ekran KIRMIZI olur (Saldırı başarısız, veri güvenliği çalıştı).
   - Eğer mesaj bozulmamışsa ekran YEŞİL olur (Hedef vuruldu).

4. ÖRNEK SENARYOLAR
------------------------------------------------------------------------------
Senaryo A: Güvenli İletişim
   Komutan "ATES" yazar, CRC seçer. Düşman "Sızma Yok" der.
   SONUÇ: YEŞİL (Hedef Vuruldu).

Senaryo B: Başarılı Savunma
   Komutan "ATES" yazar, CRC seçer. Düşman "Bit Flip" yapar.
   SONUÇ: KIRMIZI (Sinyal Bozuk!). CRC yöntemi hatayı yakaladı.

------------------------------------------------------------------------------
İyi Eğlenceler Komutan!
==============================================================================
