Bu, internetsiz (air-gapped) bir MicroK8s ortamı için hazırlık, altyapı kurulumu ve görüntü transferi süreçlerini kapsamaktadır.



🏗️ Adım 1: Yerel Kayıt Defteri Ortamını Hazırlama


Bu adımlar, internet bağlantısı olan bir makinede görüntülerin indirilmesi ve internetsiz küme altyapısının hazırlanmasını içerir.



A. Görüntüleri Çekme ve Kaydetme (Harici Ortamda)


Amaç: İhtiyaç duyulan resmi görüntüleri çekip, internetsiz ortama taşımak için tek bir .tar dosyasına paketlemek.

Komutlar:

Bash
# Görüntüleri çekme (Pull)
docker pull nginx:latest
docker pull python:3.9-slim

# Tek bir dosyada kaydetme
docker save -o image.tar nginx:latest python:3.9-slim

# image.tar dosyasını MicroK8s sunucusuna taşıyın (örn: /opt/kube/ dizinine)


B. MicroK8s Kalıcı Depolamayı Etkinleştirme


Amaç: Kayıt defteri verilerinin kalıcı olması için MicroK8s'in hostpath depolamasını hazırlamak.

Komutlar:

Bash
microk8s enable hostpath-storage
# Sonuç: microk8s-hostpath adında StorageClass oluşturulur.


C. Kayıt Defteri Servislerini Tanımlama


Amaç: Kayıt defterini hem küme içinden (ClusterIP) hem de görüntü gönderme (NodePort) amacıyla erişilebilir kılmak.

NodePort Tanımı (registry-nodeport-service.yaml): (Port 30000 kullanıldı)

YAML
# ... (Service metadata)
spec:
  type: NodePort
  selector:
    app: docker-registry
  ports:
  - port: 5000
    targetPort: 5000
    nodePort: 30000 
Bash
kubectl apply -f registry-nodeport-service.yaml


🔌 Adım 2: Görüntüleri MicroK8s Ortamına Aktarma


Bu adımlar MicroK8s düğümünde çalışır ve görüntüleri kayıt defterine push ederken karşılaşılan tüm ağ/protokol sorunlarını çözer.



A. Görüntüleri Yerel Depoya Yükleme


Amaç: Taşıdığınız image.tar dosyasındaki görüntüleri, MicroK8s'in containerd/görüntü deposuna yüklemek.

Komut:

Bash
microk8s ctr images import /opt/kube/image.tar


B. Görüntüleri Etiketleme (NodePort Adresiyle)


Amaç: Push işlemi için, görüntüleri yerel kayıt defterine erişimin sağlandığı localhost:30000 adresiyle etiketlemek.

Komutlar:

Bash
microk8s ctr images tag docker.io/library/nginx:latest localhost:30000/nginx:latest
microk8s ctr images tag docker.io/library/python:3.9-slim localhost:30000/python:3.9-slim


C. Görüntüleri Gönderme (Push)


Amaç: DNS ve HTTPS hatalarını atlayarak görüntüleri kalıcı depolamaya kaydetmek.

Komutlar:

Bash
# --plain-http bayrağı, HTTPS hatasını atlar ve HTTP kullanmaya zorlar.
microk8s ctr images push localhost:30000/nginx:latest --plain-http
microk8s ctr images push localhost:30000/python:3.9-slim --plain-http
# Başarıyla "done" çıktısını almalısınız.


📝 Adım 3: Uygulama Görüntü Adreslerini Güncelleme




A. CronJob Manifestosunu Güncelleme


Amaç: CronJob Pod'larının görüntüleri çekmesi için doğru yerel adresi kullanmasını sağlamak.

İşlem: CronJob'unuzun Deployment veya Pod spesifikasyonunda image: alanını, küme içinden en kararlı erişimi sağlayan ClusterIP Service adresiyle değiştirin.

Kullanılacak Görüntü Adresi:

YAML
# CronJob manifestosu içinde
image: docker-registry-service:5000/python:3.9-slim


B. CronJob'u Uygulama


Amaç: Zamanlanmış işleri çalıştırmaya başlamak.

Komut:

Bash
kubectl apply -f cronjob.yaml
