<h1>Logsum: Linux Log Analiz ve Özetleme Aracı</h1>
<p>Bu bash betiği, belirtilen bir dizindeki <code>.log</code> uzantılı dosyaları analiz eder, hata (ERROR) satırlarını raporlar ve dosyaları yedekler.</p>

<h2>Özellikler</h2>
<ul>
    <li><strong>Argüman Kontrolü:</strong> Hedef dizin yolunu parametre olarak alır. Dizin belirtilmezse veya mevcut değilse hata verip güvenli çıkış yapar.</li>
    <li><strong>Otomatik Çalışma Alanı:</strong> Çıktılar için <code>~/log_rapor</code> dizinini kullanır. Dizin zaten varsa temizler ve baştan oluşturur.</li>
    <li><strong>Log İstatistikleri:</strong> Hedef dizindeki toplam <code>.log</code> dosyası sayısını ekrana yazdırır.</li>
    <li><strong>Boyut Analizi:</strong> En yüksek boyuta sahip log dosyasını bulur ve insan okuyabileceği formatta boyutunu raporlar.</li>
    <li><strong>Hata (ERROR) Taraması:</strong> Tüm log dosyalarında <code>ERROR</code> kelimesi geçen satırları sayar. Sonuçları dosya bazında <code>~/log_rapor/hata_ozeti.txt</code> içerisine kaydeder.</li>
    <li><strong>Durum Değerlendirmesi:</strong> Tüm dosyalardaki toplam hata sayısını denetler. 100'den fazla hata varsa <code>DURUM: KRİTİK</code>, değilse <code>DURUM: NORMAL</code> uyarısı verir.</li>
    <li><strong>Otomatik Arşivleme:</strong> Analiz edilen log dosyalarını günün tarihiyle (<code>log_yedek_YYYYAAGG.tar.gz</code>) sıkıştırarak rapor dizinine yedekler.</li>
</ul>

<h2>Kullanım</h2>
<p>Betiğe çalıştırma izni verin:</p>
<pre><code>chmod +x logsum.sh</code></pre>

<p>Hedef dizini belirterek çalıştırın:</p>
<pre><code>./logsum.sh /var/log/uygulama_loglari</code></pre>

<h2>Çıktılar</h2>
<p>İşlem tamamlandıktan sonra <code>~/log_rapor/</code> dizininde oluşacak dosyalar:</p>
<ul>
    <li><code>hata_ozeti.txt</code>: Her bir log dosyasındaki hata satırı sayıları.</li>
    <li><code>log_yedek_YYYYAAGG.tar.gz</code>: İlgili dizindeki logların sıkıştırılmış tam yedeği.</li>
</ul>
