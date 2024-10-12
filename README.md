Для начала добавим комманду отключения taint:
kubectl taint nodes sq node-role.kubernetes.io/control-plane:NoSchedule-
А то kebernetes ругается что все ставится на одну ноду

Cоздаем namespace для Metallb:
kubectl create namespace metallb-system

И поставим metallb:
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.14.8/config/manifests/metallb-native.yaml

Далее ставим namespace api, deploy и services для app1 и app2, настройки для metallb(пул ip-адресов) и configmap'ы:
kubectl -f big_mf.yml

Cтавим nginx ingress controller:
kubectl apply -f nginx_ingress.yml

После запускаем ingress:
kubectl apply -f ingress.yml


Установка NFS storage volume:
Подготавливаем сервер

Устанавливаем nfs-сервер
sudo apt install nfs-kernel-server

Создаем директории приложений
sudo mkdir -p /srv/nfs/app1
sudo mkdir -p /srv/nfs/app2

Меняем разрешения
sudo chown sq:sq /srv/nfs/app1
sudo chown sq:sq /srv/nfs/app2

Редактируем файл конфигурации NFS по пути /etc/exports
/srv/nfs/app1 *(rw,sync,no_subtree_check)
/srv/nfs/app2 *(rw,sync,no_subtree_check)

Перезапускаем службу
sudo exportfs -a
sudo systemctl restart nfs-kernel-server
Проверяем
sudo systemctl status nfs-kernel-server

Далее вносим изменения в деплои
Добавляем 
PersistentVolume
PersistentVolumeClaim

Все эти настройки находятся в additional/big_mf_add.yml