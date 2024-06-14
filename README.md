# Kubernetes

- [Kubernetes Giriş](#Kubernetese-Giriş)
- [Monolitik Mimari ve MicroService](#Monolitic-Mimari-ve-MicroService)
- [Docker Swarm ve Kubernetes Arasındaki Farklar](#Docker-Swarm-ve-Kubernetes-Arasındaki-Farklar)

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

![](#https://miro.medium.com/v2/resize:fit:828/format:webp/0*UeutLfH3GOF9-Hwz)








