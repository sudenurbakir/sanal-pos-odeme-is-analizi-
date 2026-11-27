
# 3. Sayısal Senaryolar, Uç Durumlar, UAT Planı ve Değişiklik Talepleri

## 3.1. Çalışılmış Sayısal Senaryolar (Komisyon ve Net Tutar)

Varsayılan üye iş yeri komisyon oranı **%2,5** olarak baz alınmıştır:

| Senaryo ID | İşlem Tutarı | Komisyon Oranı | Hesaplanan Komisyon | Üye İş Yeri Net Tutarı | Notlar |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **N-01** | 1.000,00 TL | %2,5 | 25,00 TL | 975,00 TL | Standart tam tutarlı işlem |
| **N-02** | 333,33 TL | %2,5 | 8,33 TL *(8,33325)* | 325,00 TL | Kuruş yuvarlama (2 ondalık basamak) kontrolü |
| **N-03** | 0,01 TL | %2,5 | 0,00 TL *(0,00025)* | 0,01 TL | Minimum işlem tutarı alt sınır kontrolü |

## 3.2. Uç Durum Kaydı (Edge Case Register)

| Kayıt ID | Senaryo Tanımı | Beklenen Sistem Davranışı | İlgili Kural |
| :--- | :--- | :--- | :--- |
| **EC-01** | Ödeme ekranında bilgi girildikten sonra sepete geri dönülmesi | PCI kapsamı dışındaki form alanları kullanıcı tercihine bağlı olarak korunur; CVV ve maskesiz kart verisi asla saklanmaz. | PCI-01, PCI-02 |
| **EC-02** | Zaman aşımı sırasında müşterinin sayfayı yenileyip tekrar ödeme göndermesi | Aynı işlem anahtarıyla gelen ikinci istek yeni tahsilat başlatmaz; mevcut işlemin güncel statüsünü döner. | ERR-01, ERR-02 |
| **EC-03** | Minimum tutar (`0,01 TL`) veya maksimum işlem limitinin sınırında işlem gelmesi | Alt sınır kabul edilir; üst limitin `0,01 TL` üzerindeki istekler yetkilendirmeye gitmeden reddedilir. | MSG-01 |

## 3.3. Sınır Düzeyinde Kullanıcı Kabul Testi (Boundary UAT) Planı

| Test ID | Test Başlığı | Test Adımları / Girdi | Beklenen Sonuç |
| :--- | :--- | :--- | :--- |
| **UAT-01** | Minimum Tutar Sınırı Testi | Aktif MID ve TID ile `0,01 TL` tutarında ödeme isteği gönderilir. | Sistem tutarı kabul eder, kuruş yuvarlama hatası oluşmadan işlemi tamamlar. |
| **UAT-02** | Üst Limit Aşım Testi | Tanımlı maksimum işlem limitinin `0,01 TL` üzerinde tutar gönderilir. | Sistem limit aşımı uyarısı döner ve bankaya tahsilat isteği iletmez. |
| **UAT-03** | Pasif MID / TID Engelleme Testi | Statüsü "Pasif" olan bir TID üzerinden ödeme isteği başlatılır. | İşlem ön koşul kontrolünde reddedilir (`BR-04`). |
| **UAT-04** | Timeout ve Mükerrer İstek Testi | Banka yanıtı geciktirilir (timeout) ve aynı sipariş anahtarıyla 2. istek atılır. | İlk işlem belirsiz/reversal akışına alınır; 2. istek mükerrer tahsilat oluşturmaz. |
| **UAT-05** | Sepete Geri Dönüş ve PCI Kontrolü | Ödeme adımında form doldurulup sepete dönülür ve tekrar ödeme adımına gelinir. | İzin verilen alanlar korunur; CVV alanı boş gelir ve kayıtlarda yer almaz. |

## 3.4. Gereksinim Kalite İncelemesi ve Değişiklik Talepleri (CR)

Doküman kalite kontrolü sırasında tespit edilen eksiklik ve tutarsızlıklar aşağıdaki değişiklik talepleriyle kapatılmıştır:

| CR No | İlgili Bölüm | Tespit Edilen Bulgu | Yapılan Değişiklik ve Gerekçesi | Durum |
| :--- | :--- | :--- | :--- | :--- |
| **CR-01** | Bölüm 2.1 (`ERR-01`) | Zaman aşımı (timeout) durumunda doğrudan "Ödemeniz reddedildi" mesajı dönülmesi, bankada onaylanmış olabilecek işlemlerde mükerrer çekim riski yaratıyordu. | Timeout anında işlemin `"Timeout / Durum Belirsiz"` statüsüne çekilmesi, ters işlem (**Reversal**) tetiklenmesi ve kullanıcıya temkinli uyarı mesajı gösterilmesi kuralı eklendi. | Kapatıldı |
| **CR-02** | Bölüm 3.2 (`EC-01`) | Sepetten geri dönüşlerde ödeme ekranındaki form verilerinin davranışına dair bir kural bulunmuyordu. | PCI kurallarına aykırı olmayan alanların kullanıcı onayıyla korunması, CVV ve maskesiz kart numarasının ise kesinlikle saklanmaması kuralı eklendi. | Kapatıldı |
