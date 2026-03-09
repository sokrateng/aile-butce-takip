# Aile Bütçesi Takip Uygulaması

Aile gelir ve giderlerini takip etmek için geliştirilmiş modern bir web uygulaması.

## 🌟 Özellikler

### Güvenlik
- 🔒 **Kullanıcı Girişi**: Email/Password ile güvenli giriş
- 🔐 **Veri Güvenliği**: Her kullanıcı sadece kendi verilerini görür
- 🛡️ **Row Level Security**: Supabase RLS ile korumalı veriler

### Bütçe Yönetimi
- 📊 **Dönem Bazlı Raporlama**: Aylık gelir/gider takibi ve karşılaştırma
- 👥 **Kullanıcı Yönetimi**: Aile üyelerini ekleyin, düzenleyin ve takip edin
- 📁 **Kategori Yönetimi**: Gelir ve gider kategorilerini özelleştirin
- ✏️ **İşlem Yönetimi**: İşlemleri ekleyin, düzenleyin, silin
- 🔮 **Tahmini / Kesinleşmiş Kalemler**: Henüz kesinleşmemiş gelir ve giderleri tahmini olarak işaretleyin, raporlarda ayrı görün

### Raporlama
- 📈 **Grafikler**: Pasta ve sütun grafikleri ile görsel raporlar
- 🔍 **Filtreleme**: Kullanıcı, dönem ve durum (kesin/tahmini) bazlı filtreleme
- 📋 **Sıralama**: Tablo kolonlarına göre sıralama
- 📊 **Aylık Karşılaştırma**: Son 4 ayın gelir/gider karşılaştırması (kesin ve tahmini ayrımıyla stacked bar grafik)

### Teknik
- 💾 **Bulut Veritabanı**: Supabase PostgreSQL ile veri saklama
- 🌐 **Cihazlar Arası Senkronizasyon**: Verilerinize her yerden erişin
- 🚀 **Canlı Deployment**: Vercel ile otomatik deployment

## 🚀 Kurulum

### Gereksinimler
- Node.js 16+
- Supabase hesabı
- Vercel hesabı (deployment için)

### Yerel Kurulum

```bash
# Bağımlılıkları yükleyin
npm install

# .env dosyası oluşturun
VITE_SUPABASE_URL=your_supabase_url
VITE_SUPABASE_ANON_KEY=your_supabase_anon_key

# Geliştirme sunucusunu başlatın
npm run dev

# Production build alın
npm run build
```

### Supabase Kurulumu

1. [Supabase](https://supabase.com) hesabı oluşturun
2. Yeni proje oluşturun
3. SQL Editor'da tabloları oluşturun (docs/supabase-setup.sql)
4. Tahmini kalem desteği için migration çalıştırın:
   ```sql
   ALTER TABLE transactions ADD COLUMN is_estimated boolean DEFAULT false;
   ```
5. API anahtarlarını `.env` dosyasına ekleyin

## 🛠️ Teknolojiler

### Frontend
- React 18
- Vite
- TailwindCSS
- Recharts (Grafikler)
- date-fns (Tarih işlemleri)
- Lucide React (İkonlar)

### Backend
- Supabase (PostgreSQL)
- Supabase Auth (Authentication)
- Row Level Security (RLS)

### Deployment
- Vercel (Hosting)
- GitHub (Version Control)

## 📖 Kullanım

1. Uygulamayı açın ve kayıt olun
2. Aile üyelerini ekleyin (Ayarlar → Kullanıcı Yönetimi)
3. Gelir ve gider kategorileri oluşturun (Ayarlar → Kategori Yönetimi)
4. Dashboard'dan işlem ekleyin
   - Henüz kesinleşmemiş kalemler için "Tahmini kalem" kutusunu işaretleyin
   - Kalem kesinleştiğinde düzenleyerek tahmini işaretini kaldırın
5. Grafikler ve raporlarla bütçenizi takip edin
   - Özet kartlarda kesinleşmiş tutarlar ana değer, tahmini tutarlar alt bilgi olarak gösterilir
   - İşlem tablosunda Tümü / Kesin / Tahmini filtreleriyle görüntüleyin

## 🔒 Güvenlik

- Şifreler hash'lenerek saklanır
- Her kullanıcı sadece kendi verilerini görebilir
- Supabase Row Level Security ile korumalı
- HTTPS ile güvenli bağlantı

## 📝 Lisans

MIT

## 🤝 Katkıda Bulunma

Pull request'ler kabul edilir. Büyük değişiklikler için önce issue açarak tartışalım.

## 📧 İletişim

Sorularınız için issue açabilirsiniz.
