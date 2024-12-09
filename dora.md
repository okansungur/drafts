##### Tarihçe:
DORA grubu başlangıçta bağımsız bir araştırma kuruluşuydu. Ancak 2018'de Google Cloud, DORA'yı ve aralarında Nicole Forsgren, Gene Kim ve Jez Humble liderliğindeki ekibin de bulunduğu araştırma 
varlıklarını satın aldı. 
##### DORA'daki Önemli Kişiler:
Nicole Forsgren: DORA'nın kurucu ortağı ve State of DevOps raporlarının ve DORA ölçümlerinin arkasındaki kilit araştırmacılardan biridir. BT performansı ve organizasyon kültürü konusunda tanınmış bir uzmandır.

Gene Kim: The Phoenix Project, The DevOps Handbook ve DevOps uygulamalarına ilişkin diğer etkili kitapların ortak yazarıdır.
Jez Humble: Sürekli Teslimat ve DevOps El Kitabı kitaplarının ortak yazarı ve yazılım dağıtım otomasyonu konusunda tanınmış bir otorite.

DORA ölçümleri, Yazılım geliştirme performansını değerlendirmek için pusula görevi görür
Takım performansını değerlendirmek için 4 anahtar kriter belirlenmiştir.
Hızlıca online bir değerlendirme yapılması için link     https://dora.dev/quickcheck/

##### 1-Kod değişiklikler için teslimat süresi (Lead Time for Changes): Kodun oluşturma aşamasından deploymenta kadar olan süredir 
Development pipeline sağlıklı olup olmadığı
Ekibin ne kadar çevik olduğunu gösterir; yalnızca değişiklikleri uygulamanın ne kadar sürdüğünü değil,
aynı zamanda ekibin kullanıcıların sürekli gelişen ihtiyaçlarına ne kadar duyarlı olduğunu da gösterir

Buna göre performans kriterleri :

Elit : <1 saat
Yüksek Seviye: 1 gün ile 1 hafta arası
Orta seviye: 1 ay ile 6 ay aralığı
Düşük Seviye: 6 aydan fazla

https://services.google.com/fh/files/misc/state-of-devops-2021.pdf)
 Accelerate State of DevOps 2021 report (Hızlandırılmış DevOps Durum raporu 2021 den alınmıştır)

##### 2-Dağıtım Sıklığı (Deployment Frequency)   Kod canlı ortama ne sıklıkla çıkar
Değişiklikleri ne sıklıkta gönderdiğiniz ve yazılım teslimatınızın ne kadar tutarlı olduğunun ölçümü sağlanır
Elit : Günde birkaç 
Yüksek Seviye: Haftada 1 den  Ayda 1 aralığına kadar
Orta seviye: Ayda1 den   6 ayda 1 aralığına kadar
Düşük Seviye: Her 6 ayda 1 kere veya daha az

##### 3-Ortalama Kurtarma Süresi (Mean Time to Recovery):
Ortalama Kurtarma Süresi (MTTR), bir hizmet kesintisi olduğunda ekibin hizmeti geri yüklemesi için gereken ortalama süredir. Bu ölçüm ile  yazılımın kararlılığı ve ve ekibin zorluklar karşısında aldığı tutum da gözlemlenebilir
 Elit : 1 saatten az
Yüksek Seviye: 1 günden az
Orta seviye: 1 ginle 1 hafta aralığı
Düşük Seviye: 6 aydan fazla

##### 4-Değişim Hata Oranı (Change Failure Rate): 

Değişim Hata Oranı kesinti süresine, hizmetin bozulmasına veya geri almalara neden olan sürümlerin yüzdesidir
Ekibin güvenilir bir kod sunabilme becerisi ölçülür.Başarısızlıklar ölçülmesiyle nerelerde iyileştirmelerin yapılabileceği ortaya çıkarılır.
Elit : 0-15%
Yüksek, Orta  ve düşük  16-30%



##### DORA metriklerinin  ölçümü için bazı yazılımlar:  Her birinin birbirine göre avantaj ve dezavantajları mevcuttur.

1-Typo  ($16/month/user)
2-LinearB  ($15 per dev per month)
3-Apache DevLake (Open Source)
4- Waydev ($449 per year)
4-Gitlab ($29 per month)
5-Jira (Free)
6-Sleuth ($30 per month)

Burada  önemli olan kurumların kendi CI/CD tarafları ile  uyumlu çalışabilecek maliyeti  düşük  bir çözüm bulmalarıdır. DORA Metrikleri bireysel performansı değil kurumun performansının ölçmekle ilgilidir. Bu  süreçteki darboğazları ve iyileştirmeleri belirlemekle ilgilidir.

DORA metriklerini ölçmek için en iyi araç, organizasyonunuzun mevcut yazılım geliştirme süreçlerine ve ihtiyaçlarına bağlıdır. Eğer düşük maliyetli ve basit bir çözüm arıyorsanız, GitHub Actions veya GitLab CI/CD gibi ücretsiz CI/CD araçları, temel DORA metriklerini izlemek için iyi bir başlangıçtır. Hatalar ve arıza süreleri için Sentry veya Prometheus gibi açık kaynak araçları da uygun maliyetli çözümler sunar.
Jira, doğrudan DORA metriklerini izlemek için yeterli olmayabilir, ancak doğru entegrasyonlar ve araçlar ile DORA metriklerinin ölçülmesini destekleyebilir. CI/CD araçları ve izleme araçları ile entegre edilerek, Jira'yı kullanarak DORA metriklerini takip etmek mümkündür. Jira, özellikle proje yönetimi ve hata takibi konusunda güçlüdür, ancak metriklerin doğru bir şekilde ölçülmesi için ek yazılım ve entegrasyonlara ihtiyaç duyulabilir.
Jenkins, doğru yapılandırmalar ve entegrasyonlar ile DORA metriklerini ölçmek için oldukça etkili bir araç olabilir. Dağıtım sıklığı, geri dönüş süresi, değişiklik başarısızlık oranı ve hizmeti yeniden kurma süresi gibi metrikler, Jenkins’in sunduğu otomasyon ve izleme özellikleriyle takip edilebilir. Ancak, bazı metrikler için ek araçlar ve entegrasyonlar gerekebilir. Jenkins'in gücü, CI/CD süreçlerini izlemek ve otomatikleştirmek olduğundan, DORA metriklerinin ölçülmesi de bu süreçle entegre şekilde verimli bir şekilde yapılabilir.
Burada dikkat edilmesi gereken bir diğer nokta  örneğin MTTR  (Ortalama Kurtarma Süresi ) kriterinde kesinti olarak neyi tanımlıyoruz, kesinti başlangıç zamanı ve bitiş zamanı  için bir monitörleme aracı  kullanılıyormu (Prometheus, Datadog, Sentry, New Relic, Prometheus,Grafana).Bu hesaplamalar nasıl yapılmalı veya mevcut durumda nasıl yapılıyor. Bunlar incelenmeli ve kurum genelinde belkide  uygulamada bazı  güncellemeler yapılmalı. Çünkü bu kriterlerin belirlenmesinde doğru veri alınması oldukça önemlidir.
Typo doğrudan DORA metriklerini takip etmek için tasarlanmış bir araç olmasa da, doğru entegrasyonlar ve yapılandırmalarla dağıtım sıklığı, geri dönüş süresi, değişiklik başarısızlık oranı ve hizmeti yeniden kurma süresi gibi DORA metriklerini takip etmek mümkündür. CI/CD araçları, version control sistemleri ve izleme araçları ile yapılacak entegrasyonlar, Typo'yu DORA metriklerinin izlenmesi için etkili bir platform haline getirebilir. Ancak, bazı metriklerin doğru şekilde ölçülmesi için hata izleme ve izleme sistemlerinin de kullanılması gerekebilir.
Apache DevLake, DORA metriklerini izlemek için uygun bir araçtır, çünkü yazılım geliştirme süreçlerinden (CI/CD, sürüm kontrol, hata yönetimi vb.) veri toplama ve analiz etme yeteneklerine sahiptir. Dağıtım sıklığı, geri dönüş süresi, değişiklik başarısızlık oranı ve hizmeti yeniden kurma süresi gibi metrikler, DevLake üzerinden kolayca izlenebilir. Ancak, DORA metriklerinin doğru şekilde ölçülmesi için doğru araçlarla entegrasyonlar ve yapılandırmalar gereklidir. DevLake, bu entegrasyonlar sayesinde verimli bir şekilde DORA metriklerini ölçebilir ve raporlayabilir.
