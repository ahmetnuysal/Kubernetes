# Kubernetes

- [Kubernetes Giriş](#Kubernetese-Giriş)
- [Monolitik Mimari ve MicroService](#Monolitic-Mimari-ve-MicroService)
- [Docker Swarm ve Kubernetes Arasındaki Farklar](#Docker-Swarm-ve-Kubernetes-Arasındaki-Farklar)
- [Kubernetes Mimarisi](#Kubernetes-Mimarisi)

## Kubernetes Giriş

Kubernetes, konteynerli uygulamaların dağıtımını, ölçeklendirilmesini ve yönetimini
otomatikleştirmek için tasarlanmış açık kaynaklı bir konteyner düzenleme motorudur. Kubernetes açık kaynaklı bir platformdur, yani kaynak kodu herkesin kullanması, değiştirmesi
ve yeniden dağıtması için serbestçe kullanılabilir.

Kubernetes yapılandırma için YAML ve JSON destekler.

Docker'da uygulamalarımızı konteynerlerin içine yerleştiririz. Ancak
Kubernetes'te, uygulamanın trafiğine bağlı olarak sayıları genellikle binlerce veya daha fazla
olan konteynerleri daha büyük ölçekte yönetiriz.

Docker'da, konteynerler içeren bir gemi hayal edin.
Şimdi, Kubernetes'te aynı gemiyi hayal edin, ancak bu sefer bir dümeni var. Tıpkı bir kaptanın
geminin rotası hakkında karar vermek için geminin dümenini kullanması gibi, Kubernetes de
konteynerleri yönetmek için "gemi dümeni" görevi görür.

> Kubernetes'in Alternatifleri

- Docker Swarm
- Apache Mesos
- Openshift
- Nomad

> Kubernetes geldikten sonra, aşağıdaki gibi birçok görevi otomatikleştirdi;

- Konteynerlerin yoğun veya normal saatlere göre otomatik ölçeklendirilmesi.
- Birden fazla konteynerin yük dengelemesi.
- Konteynerlerin kümedeki mevcut düğümlere otomatik olarak dağıtılması.
- Konteynerler arızalanırsa kendi kendini iyileştirme.

> Kubernetes Özellikleri

- Otomatik Ölçeklendirme: Kubernetes, büyük ölçekli üretim ortamları için yatay ve dikey ölçeklendirme olmak üzere iki tür otomatik ölçeklendirmeyi destekler ve bu da uygulamaların kesinti süresini azaltmaya yardımcı olur.
- Otomatik İyileştirme: Kubernetes otomatik iyileştirmeyi destekler, yani konteynerler herhangi bir sorun nedeniyle başarısız olursa veya durdurulursa, Kubernetes bileşenlerinin yardımıyla (önümüzdeki günlerde konuşulacak), konteynerler otomatik olarak onarılacak veya iyileşecek ve tekrar düzgün bir şekilde çalışacaktır.
- Yük Dengeleme: Kubernetes, yük dengeleme yardımıyla trafiği iki veya daha fazla konteyner arasında dağıtır.
- Platform Bağımsız: Kubernetes, ister şirket içi, ister Sanal Makineler veya herhangi bir Bulut olsun, her türlü altyapı üzerinde çalışabilir.
- Hata Toleransı: Kubernetes, düğüm veya pod arızalarını bildirmeye ve mümkün olan en kısa sürede yeni podlar veya konteynerler oluşturmaya yardımcı olur
- Geri alma: Önceki sürüme geçiş yapabilirsiniz.
- Konteynerlerin Sağlık İzlemesi: Monitörün sağlığını düzenli olarak kontrol edin ve herhangi bir konteyner arızalanırsa yeni bir konteyner oluşturun.
- Orkestrasyon: Üç konteynerin farklı ağlar üzerinde çalıştığını varsayalım (Şirket içi, Sanal Makineler ve Bulutta). Kubernetes bir küme oluşturabilir üçünün de farklı ağlardan çalışan kapsayıcıları vardır.


## Monolitik Mimari ve MicroService

- Monolitik mimari tüm görevleri yerine getiren tek bir mutfak gibiyken, mikro hizmet
mimarisi birlikte çalışan birden fazla özel restoran gibidir.
- Monolitlerin kurulumu ve yönetimi genellikle daha kolayken mikro hizmetler daha
fazla esneklik ve ölçeklenebilirlik sunar.
- Monolitlerde tek bir hata noktası olabilirken, mikro hizmetler hataya karşı daha
dayanıklıdır çünkü bir mikro hizmetteki hata diğerlerini etkilemek zorunda değildir.
Sonunda Kubernetes, iş açısından iyi olan mikro hizmet tabanlı mimariye ulaşmaya yardımcı
olur.

## Docker Swarm ve Kubernetes Arasındaki Farklar

![image](https://github.com/ahmetnuysal/Kubernetes/assets/85068070/bb9a154f-285e-4e92-8bd0-da0e46a8efb5)

## Kubernetes Mimarisi

Kubernetes, bir 'Kubernetes Kümesi' oluşturan Master Node ve Worker node'un bulunduğu istemci-sunucu mimarisini takip eder. İhtiyaca göre birden fazla işçi düğümüne ve Ana düğüme sahip olabiliriz.

Kontrol Düzlemi: API sunucusu, etcd, zamanlayıcı ve denetleyici yöneticisi dahil olmak üzere kontrol düzlemi bileşenleri genellikle bir Kubernetes kümesinin ana düğüm(ler)inde bulunur. Bu bileşenler kümeyi bir bütün olarak yönetmek ve kontrol etmekten sorumludur.

> Master Node

Master Node, daha önce tartıştığımız her şeyi yönetmeye yardımcı olan dört ana bileşene sahiptir:

1. API Sunucusu: Basit bir ifadeyle, kubectl'i ana düğüme yükledikten sonra geliştiriciler pod oluşturmak için komutları çalıştırır. Böylece komut API Sunucusuna gider ve API Sunucusu da bunu podların oluşturulmasına yardımcı olacak bileşene iletir. Başka bir deyişle, API Sunucusu, API Sunucusunun işleri uygulamak için hiyerarşik yaklaşımı izlediği herhangi bir Kubernetes görevi için bir giriş noktasıdır.

2. Etcd: Etcd, Ana düğümün ve İşçi düğümünün (tüm küme) Pod IP'leri, Düğümler, ağ yapılandırmaları vb. gibi tüm bilgilerini depolayan bir veritabanı gibidir. Etcd verileri anahtar-değer çiftinde depolar. Veriler vb. içinde depolanmak üzere API Sunucusundan gelir.

3. Denetleyici Yöneticisi: Denetleyici Yöneticisi, Kubernetes kümesinin API Sunucusundan kümenin istenen durumu gibi verileri/bilgileri toplar ve ardından API Sunucusuna talimatlar göndererek ne yapılacağına karar verir.

4. Zamanlayıcı: API Sunucusu Denetleyici Yöneticisinden bilgileri topladıktan sonra, API Sunucusu pod sayısının artırılması vb. gibi ilgili görevi gerçekleştirmesi için Zamanlayıcıya bildirimde bulunur. Bildirim aldıktan sonra, Zamanlayıcı sağlanan iş üzerinde harekete geçer. Dört bileşeni de gerçek zamanlı bir örnekle anlayalım.

> Worker Node

Worker Node (İşçi Düğümü), konteynerleri yöneten ve onlarla ilgilenen ve kaynakları planlanan konteynerlere atamak için talimatlar veren Ana Düğüm ile iletişim kuran aracıdır. Bir Kubernetes kümesi, kaynakları gerektiği gibi ölçeklendirmek için birden fazla işçi düğümüne sahip olabilir. Worker Node, konteynerleri yönetmeye ve Master Node ile iletişim kurmaya yardımcı olan dört bileşen içerir:

1. Kubelet: kubelet, Pod'ları yöneten ve pod'un çalışıp çalışmadığını düzenli olarak kontrol eden Worker Node'un birincil bileşenidir. Podlar düzgün çalışmıyorsa, kubelet yeni bir pod oluşturur ve bir öncekiyle değiştirir çünkü başarısız olan pod yeniden başlatılamaz, bu nedenle podun IP'si değişebilir. Ayrıca, kubelet podlarla ilgili ayrıntıları Ana Düğümde bulunan API Sunucusundan alır.
2. Kube-proxy: kube-proxy, pod IP'si vb. gibi tüm kümenin tüm ağ yapılandırmasını içerir. Kube-proxy, ağ yapılandırması altında gelen yük dengeleme ve yönlendirme ile ilgilenir. Kube-proxy podlar hakkındaki bilgileri Master Node üzerinde bulunan API Server'dan alır.
3. Podlar: Pod, uygulamanın konuşlandırıldığı bir konteyner veya birden fazla konteyner içeren çok küçük bir birimdir. Pod, konteynerlere uygun IP'yi dağıtan bir Genel veya Özel IP aralığına sahiptir. Her podun altında bir konteyner olması iyidir.
4. Konteyner Motoru: Konteynere çalışma zamanı ortamını sağlamak için Konteyner Motoru kullanılır. Kubernetes'te Konteyner motoru, konteynerleri oluşturmaktan ve yönetmekten sorumlu olan konteyner çalışma zamanı ile doğrudan etkileşime girer. Piyasada CRI-O, containerd, rkt(rocket), vb. gibi birçok Konteyner motoru bulunmaktadır. Ancak Docker en çok kullanılan ve güvenilen Konteyner Motorlarından biridir. Bu nedenle, önümüzdeki gün Kubernetes kümesini kurarken bunu kullanacağız.


#### Ana Düğüm - Alışveriş Merkezi Yönetimi:
● Bir alışveriş merkezinde, her şeyle ilgilenen bir yönetim ofisiniz vardır. Kubernetes'te bu Master Node'dur.
● Ana Düğüm, tıpkı alışveriş merkezi yönetiminin alışveriş merkezinin sorunsuz çalışmasını sağlaması gibi, kümedeki tüm faaliyetleri yönetir ve koordine eder.

#### kube-apiserver - Merkezi Kontrol Masası:
● Kube-apiserver'ı alışveriş merkezinin merkezi kontrol masası olarak düşünün. Tüm taleplerin (mağaza açılışları veya müşteri soruları gibi) yönlendirildiği yerdir.
● Alışveriş merkezi yönetiminin mağazalarla iletişim kurması gibi, kube-apiserver da tüm Kubernetes bileşenleriyle iletişim kurar.

#### etcd - Ana Kayıtlar:
● etcd, mağaza konumları ve saatleri gibi önemli bilgileri içeren alışveriş merkezinin ana kayıtlarıyla karşılaştırılabilir.
● Yapılandırma ve küme durumu verilerini depolayan bir anahtar-değer deposudur. kube-controller-manager - Görev Yöneticileri:
● Güvenlik ve bakım gibi farklı alışveriş merkezi departmanları için özel görev yöneticilerine sahip olduğunuzu düşünün.
● Kubernetes'te kube-controller-manager, istenen sayıda Pod'un çalışmasını sağlamak gibi çeşitli görevleri yerine getirir.

#### kube-scheduler - Zamanlayıcı Yöneticisi:
● Kube zamanlayıcısını, hangi çalışanların (Pod'ların) nerede (hangi Worker Node üzerinde) çalışması gerektiğine karar veren bir yönetici olarak düşünün.
● Eşit dağılım ve verimli kaynak tahsisi sağlar.

#### Kubelet - Mağaza Yöneticileri:
● Her mağazada (Worker Node), çalışanların (Pod'lar) doğru çalışmasını sağlayan bir mağaza yöneticiniz (Kubelet) vardır.
● Kubelet, Master Node ile iletişim kurar ve deposundaki Pod'ları yönetir.

#### kube-proxy - Müşteri Hizmetleri Masası:
● kube-proxy her mağazada bir müşteri hizmetleri masası gibi davranır. Müşteri sorularını (ağ talepleri) ele alır ve bunları doğru çalışana (Pod) yönlendirir.
● Yük dengeleme ve yönlendirme için ağ kurallarını korur.

#### Konteyner Çalışma Zamanı - Çalışan Eğitimi:
● Her mağazada, görevlerini yerine getirmek için eğitime ihtiyaç duyan çalışanlarınız (Pod'lar) var.
● Konteyner çalışma zamanı (Docker gibi), çalışanların (Pod'lar) görevlerini yerine getirmeleri için gerekli eğitimi (çalışma zamanı ortamı) sağlar.
