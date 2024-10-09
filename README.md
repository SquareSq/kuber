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
