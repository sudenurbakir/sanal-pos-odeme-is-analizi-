# 1. Üye İş Yeri Kabulü, MID/TID ve PCI Kapsam Gereksinimleri

## 1.1. Üye İş Yeri (Merchant) ve MID/TID İş Kuralları

| Kural Kodu | Kural Adı | Açıklama |
| :--- | :--- | :--- |
| **BR-01** | Tekil Üye İş Yeri Kuralı | Sisteme başvuran her işletme Vergi Kimlik Numarası (VKN) üzerinden doğrulanır. Sistemde aktif kaydı bulunan bir VKN için ikinci bir ana **MID** oluşturulamaz. |
| **BR-02** | MID Ataması | Başvurusu onaylanan üye iş yerine sistem tarafından otomatik ve benzersiz bir **MID** (Merchant ID) atanır ve iş yeri statüsü "Aktif" duruma getirilir. |
| **BR-03** | Çoklu Kanal / TID Yapısı | Bir MID altında, işletmenin ödeme alacağı her satış kanalı veya noktası (Web Sitesi, Mobil Uygulama vb.) için ayrı bir **TID** (Terminal ID) tanımlanır. |
| **BR-04** | İşlem Ön Koşulu | Hem ilgili **MID** hem de işlemin geldiği **TID** "Aktif" statüde olmadan hiçbir ödeme isteği kabul edilmez; sistem işlemi doğrudan reddeder. |

## 1.2. Kart Verisi Güvenliği ve PCI Kapsam Kuralları 

Teknik mimari detaylarına girilmeden, ödeme akışı boyunca kart verilerinin korunmasına yönelik iş gereksinimleri aşağıda tanımlanmıştır:

| Kural Kodu | Kural Adı | Açıklama |
| :--- | :--- | :--- |
| **PCI-01** | CVV/CVC Saklama Yasağı | İşlem anında alınan güvenlik kodu (CVV/CVC), yetkilendirme/tahsilat isteği iletildikten sonra veritabanında, işlem kayıtlarında veya sistem loglarında kesinlikle saklanmaz. |
| **PCI-02** | Kart Numarası (PAN) Maskeleme | Kart numarası hiçbir ekranda, dekontta veya operasyon kaydında açık (maskesiz) gösterilmez; yalnızca maskelenmiş formatta (örneğin `XXXX **** **** XXXX`) görüntülenir. |
| **PCI-03** | Son Kullanma Tarihi ve Kart Sahibi Adı | Kart üzerindeki ad bilgisi mutabakat/iade referansı amacıyla işlem kaydında tutulabilir; son kullanma tarihi ise yalnızca yetkili akışlarda ve kısıtlı görünürlükte işlenir. |
| **PCI-04** | Hassas Veri Log Kısıtı | Maskesiz kart numarası ve CVV/CVC verileri uygulama hata veya erişim loglarına yazılmaz. |
