# AWS Sınavı

## Sınav Konuları

1. Ölçeklendirilebilir, hataya dayanıklı ve düşük maliyetli mimari oluşturma (60%)
2. Güvenli VPN tasarlama (AWS Kaynaklarını kullanan uygulamalar), IA Politikalarının nasıl kullanılacağı.  (20%)
3. Uygulama (örneğin: iki katmanlı bir mimariyi nasıl uygularsınız) / Dağıtım (kaynakları AWS'ye nasıl geçirirsiniz) (% 10)
4. Sorun Giderme (örnek: arızalı bir VM'yi nasıl geri(restore) yüklersiniz) (% 10)

## Sınav Tipleri
#### 1. Ölçeklendirilebilir, hataya dayanıklı ve düşük maliyetli mimari oluşturma

Bu bölüm sınav puanının %60'ını kapsadağından, en önemli kısımdır. Bilmeniz gereken en önemli noktalar şunlardır:

**AWS EC2, Load Balancer, and Storage Options:**

Bu bölümde sınavda, ödeme planları, instance'ların storage gibi özellikleri sorulur.

**Hatırlanması gereken önemli noktalar:**

Üç ödeme planı arasındaki farkı öğrenin:
* **İsteğe Bağlı instances:** Saat başına ödeme

Kullanım Durumları: development/ test ortamı

* **Reserved instances:** 1 ya da 3 yıllık anlaşma (60% varan indirim)

Kullanım Durumları: üretime hazır uygulamar, uzun süre aktif olacak uygulamalar

* **Spot Instances:** fiyat teklifi olanlar.

Kullanım Durumları: sonlandırılmış uygulamaların instance'larda bir sorun teşkil etmemesi.

Spot Instance: Spot instance fiyatı, teklif fiyatının üstüne çıkarsa veya yeterli kapasite yoksa, AWS EC2 instance bir sonlandırma bildirimi alır ve iki dakika içinde sonlandırılır.

Üç tip kiralama seçeneği vardır:
* **Shared Tenancy:** Birden fazla instance aynı donanımda çalışır (default option)
* **Dedicated instance:** Bir dedicated donanımı yalnızca tek müşteri instance'sı çalıştırır.
* **Dedicated host:** Full kapasite EC2 ile kullanıcıya adanmış fiziksel dedicated server. Bu daha çok Lisans nedenleri ile kullanılır.

Instance store datası, an Amazon EC2 instance'ı restarlandığı veya sonlandığı zaman kaybolur. Bu geçici veri geçerlidir.

 AMI’lerin dört tipi vardır:
* **AWS tarafından yayınlanan AMI'ler:** Bunlar AWS tarafından sürdürülür, yönetilir ve çok güvenilirdir.
* **AWS Marketplace:** AMI’yi Bitnami, Drupal gibi diğer sağlayıcılardan satın alabilirsiniz. Uygulamaları yüklemeniz gerekmez.
* **Mevcut Instance AMI’leri:** EC2'den mevcut bir AMI.
* **Sanal Serverdan upload edilen AMI'ler:**  Bunlar, AWS Import / Export Service aracılığıyla içe aktarılan veya Verilen AMI'lerdır.
**EFS** birimi birden fazla sunucuya monte edilebilir.

**EBS** birimi tek bir sunucuya monte edilebilir.

Public IP bir instance'nin stop/start esnasında değiştirilir. IP değişikliğini önlemek için, instance'nıza bir Elastic IP ilişkilendirin.
Bir EC2 instance'ına takılan Elastic IP herhangi bir ücrete tabi olmayacaktır! Ancak, ilişkilendirilmimişse değilse ücretlendirilirsiniz.

Yeni başlattığınız Windows instance'ına, AWS'nin rastgele ürettiği bir şifre ile erişebilirsiniz.

**Bootstrapping:** Bir instance önyüklemesinde bir script dosyasını çalıştırmanıza izin verir. Genellikle, belirli paketlerin kurulumunu veya Chef / Puppet yapılandırmayı içerir.

**Enhanced Networking:** Ağ bağlantısı geliştiren AWS EC2'deki bir özelliktir. Not: Yalnızca belirli EC2 türleri bunu destekler ve sadece VPC'de etkinleştirilebilir.

**Termination Protection** Bir EC2 instance'ının yanlışlıkla silinmesini önler.

**Placement Group:**  Bir grupta birden çok AWS EC2 instance'ını yerleştirebilirsiniz, bu da daha düşük bir ağ gecikmesi sağlayacaktır.

Dört tip EBS Volume vardır:
* **Cold HDD** Kullanım Durumu: Az veri erişimine sahip uygulamalar. Max IOPS 250.
* **Throughput HDD** Kullanım Durumu: data warehouse, log processing. Max IOPS 500.
* **General Purpose SSD** Kullanım Durumu: OS boot volume, databases. Max IOPS 10,000
* **Provisioned IOPS SSD** Kullanım Durumu: Çok hızlı veri erişimine ihtiyaç duyan uygulamalar, large databases Max IOPS 20,000.
---

#### 2. AWS Databases

**OLTP** – Online Transaction Processing (database types: AWS RDS, AWS Aurora)

**OLAP** – Online Analytic Processing (AWS Redshift)

**RPO (Recovery Point Objective)** – Kabul edilebilir data kaybı.

**RTO (Recovery Time Objective)** – Gelecekte uygulamanızın hayatta çalışır durumda olacağı zaman.

* Otomatik DB Snapshots ile karşılaştırıldığında manuel DB Snapshots silinmez!

* Hataya dayanıklı ve yüksek kullanılabilir bir veritabanı mimarisi oluşturmak için Multi-AZ' ile çalışın.
Veritabanına bağlanmak için uygulamanızda DNS adını kullanın. Veritabanı başarısız olursa, AWS kayıtları günceller, böylece uygulamanızı etkilemez.

* Yoğun trafik içeren web siteleri için Read Replicas kullanın.

* AWS Redshift toplu içe aktarma komutunu kullanın, ham SQL Queries'den çok daha verimlidir.

* Amazon DynamoDB, AWS Managed NoSQL veritabanıdır.

* Bir Amazon DynamoDB'nin yazma verimliliğini primary key değerini rastgele hale getirerek artırın.

* AWS Aurora, AWS tarafından geliştirilen ve AWS RDS'den daha hızlı ve daha ucuz olan veritabanı motorudur.
---

#### 3. AWS Storage, S3, Glacier
Bu bölümde çeşitli depolama seçenekleri, veri depolama güvenliği ve depolama seçeneklerinin uygulanması / dağıtımı üzerinde duruluyor.

**Anahtar Noktalar:**

* Block level (EBS) ve file storage (EFS) burada saklanır.
* AWS S3 bir nesne deposudur, S3'te kaydedilen her şey veri nesneleri olarak saklanır.
* Her nesne MetaData (Amazon tarafından yaratılan) ve Data (custom data) içerir.
* Bir nesnenin büyüklüğü 0 - 5 TB arasında olabilir.
* S3 Nesneleri bir bölge(region) içindeki birden fazla cihazda çoğaltılır!
* S3 S3 Nesneleri “Bucket” adı verilen bir yerde saklanır, Bucket'ı root folder gibi düşünün.
* Yanlışlıkla nesnenin silinmesini önlemek için, sürüm ve MFA'yı etkinleştirin.
* S3 verileri diğer bölgelerde(regions) çoğaltılabilir, bu genellikle uyum için yapılır. Not: Sadece yeni nesneler çoğaltılacaktır.
* Bucket isimleri tüm  AWS Account'larında benzersiz ve eşsizdir.
* Bucket  3 ila 63 karakter içerebilir ve sayılar, kısa çizgiler, noktalar yazılabilir.

**S3'ün uyumluluk modeli**

* Yeni bir nesne oluşturduğunuz zaman en son, çıkan objeyi alırsınız. (Read After Write Consistency)PUTS to new object
* Var olan bir nesneyi PUT veya DELETE yaptığımızda,  AWS, nihai uyumluluk sağlar, değişikliklerin etkilenmesi biraz zaman alabilir.

* S3 storage'in üç tip sınıfı vardır:**
* **S3 Standard:** 99.99% availability, sık sık erişim yapılan datalar için idealdir.
* **S3 Infrequent Access:** 99.9% availability, daha az erişim yapılan datalar için ideldir ve S3 Standard'dan daha ucuzdur, minimum object size 128kb
* **S3 RRS (Reduced Redundancy Storage):** 99.99% availability, en ucuzudur. data kaybının önemli olmadığı nesneler için idealdir.
AWS Glacier data arşivlemesi için idealdir. Glacier data arşivlemesinde datalar hemen aktif ve hazır olmayabilir. Dataları almak biraz zaman alabilir.
---

#### 4. Elastic Load Balancer
Anahtar Noktalar:

İki tip Load Balancer vardır :

* **Internet Facing:** public IP'si olan
* **Internal Load Balancer:** routes traffic (VPC ile)

 Load Balancer Kategorileri:
* **Application Load Balancer**
  * Layer 7'de çalışır.
  * WebSockets and Secure WebSockets' destekler.
  * SNI'ı destekler.
  * IPv6'yı destekler.
* **Network Load Balancer** ( NLB, Layer 4'te kullanılmak üzere önerilen Load Balancer'dır)
  * Layer 4'de çalışır.
  * Source IP'yi korur.
  * Diğer Load Balancer'lar ile kıyasla azaltılmış gecikme(latency) sağlar.
  * Saniye başına milyonlarca request işler.
  * Classic Load Balancer
  * Layer 4'de Çalışır.
  * Cross Zone Load Balancing özellikleri.

* **Idle Connection Timeout:** Load Balancer'ın, client ile EC2 instance arasındaki bağlantıyı sonlandırmasıyla oluşan hareketsizlik süresidir ve default olarak 60 saniyedir.
* Enable Cross Zone Load Balancing, trafiği, kayıtlı AWS instance'ların havuzuna eşit olarak dağıtmak için.
* Connection Draining, Load Balancer'ların hatalı instance'lara trafik göndermesini engeller.
* **Proxy Protocol:** User Agent veclients IP'si almak için , etkinleştirilmesi gereklidir. (sadece Network Load Balancer ve Classic Load Balancer)
* **Sticky Sessions**: Kullanıcının ilk requestini alacak olan EC2 instance'sine gönderilmesini garanti bir şekilde şağlar.
* **Auto Scaling Group:** EC2 instance'larınızın sayısını CPU, RAM, Scheduled Scaling bağlı olarak artırmaya veya azaltmaya olanak sağlar.(genellikle X zamanındaki yükte bir artış olacağını tahmin ettiğinizde kullanılır: Pazarlama Kampanyası)
---

#### 5. Güvenli Bir VPN Tasarlamak
Bu bölümde, sınav, bilginizi bir mimarinin güvenlik yönüyle test edecektir. Güvenli bir mimariyi inşa etmek, bir mimarın zorlu bir işi olabilir. AWS, güvenli bir mimari uygulamanıza yardımcı olabilecek birçok hizmet veya özelliğe sahiptir.

**Hatırlamak için anahtar noktalar:**

* VPC (Virtual Private Cloud) AWS'de müşteriye bağlı sanal bir ağ.
* VPC birden fazla bölge(region) ile ilişkilendirilemez.
* VPC or Subnets IP address aralığı(range) VPC oluşturulduktan sonra değiştirilemez.

Yeni bir VPC oluşturulduğunda şu kaynaklara sahiptir:
* Subnets.
* Route Tables.
* Dynamic Host Configuration Protocol.
* Security Groups.
* Network Access Control
* Bir (alt ağ)subnet'in, VPC'in bir bölümlemesidir.

* Bir subnet en az 16 (/28 netmask) IP address'e en fazla 65,536 (/16 netmask) IP adress'e sahip olabilir.
* Bir subnetin 5 IP adresi AWS tarafından kullanılıyor, bu yüzden 16 IP adresli bir subnet oluşturursanız, kullanılabilecek yalnızca 11 IP adresi kalır (16-5 = 11).

Üç tip subnet vardır:
* **Public:** İnternet erişimi var ve trafik IGW'ye yönlendirilir (Internet Gateway)
* **Private:** IGW'ye yönlendirilmez.
* **VPN-only:** Traffik bir VPG'ye yönlendirilir. (Virtual Private Gateway).

* Bir subnet yalnızca uygun bir bölge (availability zone) ilişkilendirilebilir.
* Bir subnetin public'e açılması için (internet erişimine sahip olmak), tüm yerel olmayan trafiğini IGW'ye yönlendirmesi gerekecektir.
* VPC Endpoints, VPC'nizden bir internet bağlantısı, NAT Gateway vb. Gerekmeden özel bir ağ üzerinden S3 gibi AWS servisleriyle uçtan uca bir bağlantı oluşturmanızı sağlar.
* VPC eşleme bağlantısı, diğer VPC’lerin instance'larının birbirine bağlamanıza olanak tanır.
* Bağlantılar bir istek(request) / kabul(accept) protokolü ile başlatılır.
* Bire bir ilişkili ve bağlantılıdır.
* Diğier bölgedeki VPC'lere aktarım yapamaz
* Geçişli rooting i desteklemez

İki tip Firewall vardır:
* **Security Group.**
* **Network Access Control Lists (ACLs)**

* 
İnternete erişmesi gereken private subnet ağdaki instance'lar için NAT instance veya NAT Gateway'e bağlanmaları gerekir.
* NAT Instances müşteri tarafından yönetilir.
* NAT Gateway AWS tarafından yönetilir ve otomatik olarak ölçeklendirilir.
---
