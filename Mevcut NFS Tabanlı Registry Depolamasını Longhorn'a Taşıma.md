Eğer registry zaten NFS ile etkinse (PVC StorageClass: nfs-client veya benzeri), veri kaybı olmadan migrate etmek için şu adımları izleyin. Bu işlem downtime gerektirir (yaklaşık 10-15 dakika).

Registry'yi Durdurun (Scale Down):
text
microk8s kubectl scale deployment registry --replicas=0 -n container-registry
Pod'lar durur, ama PVC kalır.
Veriyi Yedekleyin (NFS Volume'dan):
Registry verisi /var/lib/registry dizininde saklanır. Mevcut PVC'yi mount eden bir geçici pod oluşturun ve veriyi tar'layın.
Geçici pod YAML'ı oluşturun (backup-pod.yaml):
YAML

apiVersion: v1
kind: Pod
metadata:
  name: backup-pod
  namespace: container-registry
spec:
  volumes:
  - name: registry-storage
    persistentVolumeClaim:
      claimName: registry-storage  # Mevcut NFS PVC adınız
  containers:
  - name: backup
    image: busybox
    command: ['sleep', '3600']
    volumeMounts:
    - mountPath: /data
      name: registry-storage
Uygulayın: microk8s kubectl apply -f backup-pod.yaml.
Pod'u çalıştırın ve veriyi export edin (host'unuza kopyalayın):
text

microk8s kubectl exec -it backup-pod -n container-registry -- tar czf /data/registry-backup.tar.gz -C /data .
microk8s kubectl cp container-registry/backup-pod:/data/registry-backup.tar.gz ./registry-backup.tar.gz
Yedeği güvenli bir yere alın (örneğin host dizinine).
Eski PVC'yi Silin:
text

microk8s kubectl delete pvc registry-storage -n container-registry
NFS volume'u serbest bırakılır (veri NFS sunucusunda kalır, ama Kubernetes'ten ayrılır).
Registry'yi Longhorn ile Yeniden Etkinleştirin:
text

microk8s disable registry  # Addon'u tamamen kaldırın (eğer gerekirse)
microk8s enable registry:storageclass=longhorn
Yeni PVC (registry-storage) Longhorn ile oluşur (20Gi varsayılan; boyut için addon YAML'ını edit'leyin).
Veriyi Geri Yükleyin:
Yeni PVC için geçici pod oluşturun (restore-pod.yaml, backup-pod'a benzer, ama boş Longhorn volume ile):
YAML

# backup-pod.yaml ile aynı, ama volume claimName: registry-storage (yeni)
Uygulayın ve veriyi import edin:
text

microk8s kubectl cp ./registry-backup.tar.gz container-registry/restore-pod:/data/
microk8s kubectl exec -it restore-pod -n container-registry -- tar xzf /data/registry-backup.tar.gz -C /data
Pod'u silin: microk8s kubectl delete pod restore-pod -n container-registry.
Registry'yi Yeniden Başlatın:
text

microk8s kubectl scale deployment registry --replicas=1 -n container-registry
Durumu kontrol edin: microk8s kubectl get all -n container-registry.
