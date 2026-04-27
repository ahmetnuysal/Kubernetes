## Kubectl Kurulumu

Kubectl kurulumu yaparken paket yönetici kullanmamız işimizi kolaylaştırır.

https://kubernetes.io/docs/tasks/tools/


### 1. Paket Yönetciileri Kurulumu

> Windows için Chocolatey kurulumu

1. https://chocolatey.org
2. ```set-ExecutionPolicy AllSigned``` komutunu çalıştırıp çıkan soruya ```A```de
3. ```Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))``` komutunu çalıştırıyoruz.
4. Doğru kuruldu mu diye kontrol etmek için ```choco -?``` komutunu kullanabiliriz, uzun bir çıktı aldıysak kurulum tamamdır demektir.

NOT: Alternatif olarak "Winget" kullanılabilir.

> Linux ve MacOs içim Homebrew kurulumu

1. https://brew.sh
2. ```/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"```
3. Kurulum yaparken admin/root şifresi sorabilir.
4. Yükleme tamamlandıktan sonra ```brew``` komutu çalıştırılıp çıktıya bakıyoruz.

### 2. Kubectl Kurulumu

> Windows için Choco Kullanarak

1. ```choco install kubernetes-cli``` komutunu çalıştırıyoruz.
2. ```kubectl version``` komutu ile kontrol ediyoruz.

> Windows için Winget Kullanarak

1. ```winget install -e -id kubernetes.kubectl``` komutunu çalıştırıyoruz.
2. ```kubectl version``` komutu ile kontrol ediyoruz.

> MacOs ve Linux için Brew Kullanarak

1. ```brew install kubectl``` komutunu çalıştırıyoruz.
2. ```kubectl version --client``` komutu ile kontrol ediyoruz.
