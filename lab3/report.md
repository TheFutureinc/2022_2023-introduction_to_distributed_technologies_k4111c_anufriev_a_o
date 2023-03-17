Лабораторная работа №3 "Сертификаты и "секреты" в Minikube, безопасное хранение данных."
Общая информация
University: ITMO University
Faculty: FICT
Course: Introduction to distributed technologies
Year: 2022/2023
Group: K4113c
Author: Anufriev Artem Olegovich
Lab: Lab3
Date of create: 17.03.2023
Date of finished:
Ход работы
Создание ConfigMap
ConfigMap позволят определять конфигурационные файлы и переменные, которые потом могут использоваться контейнерами с приложениями. Таким образом, ConfigMap избавляет от необходимости упаковывать конфиги в docker-образ.
Манифест configMap для приложения находится в файле configmap.yaml.
Создание ConfigMap:

 <img width="468" alt="image" src="https://user-images.githubusercontent.com/46056823/225916718-ad9ac9c5-b0ed-41cb-ac7e-343d2a7cfde6.png">

Передача значений ConfigMap в ReplicaSet
Для развертывания приложения был создан манифест replicaset.yaml. Для передачи значений ConfigMap в контейнер в качестве переменных окружений используется поле envFrom с указанием имени созданного ConfigMap.
Также был создан манифест service.yaml, описывающий сервис типа ClusterIP для доступа к подам приложения lab3-app.

 <img width="468" alt="image" src="https://user-images.githubusercontent.com/46056823/225916791-5d98d557-1540-4c60-8086-f9d851e189ef.png">

Развертывание ingress-controller и создание TLS сертификата
Ingress Controller представляет собой специальный балансировщик нагрузки для Kubernetes. Ingress controller позволяет принимать траффик извне кластера и направлять его на поды.
В данной работе был развернут Nginx Ingress controller:
 
 <img width="468" alt="image" src="https://user-images.githubusercontent.com/46056823/225916902-a783eb67-e5a9-4298-b56e-14be072c69eb.png">

После развертывания ingress controller в неймспейсе ingress-nginx появился сервис типа LoadBalancer - ingress-nginx-controller. Именно через этот сервис будет проходить входящий траффик. Траффик будет распределяться на поды, согласно правилам, которые будут описаны объектами Ingress.
 
 <img width="468" alt="image" src="https://user-images.githubusercontent.com/46056823/225916954-5f355e21-a8ad-4343-b142-60e0b05641a2.png">

Для генерации TLS-сертификата была использована утилита mkcert:
 
 <img width="468" alt="image" src="https://user-images.githubusercontent.com/46056823/225916988-07d3e7d5-7562-4fe7-a4b2-52c4d454c87d.png">

Далее TLS-сертификат необходимо импортировать в кластер в виде секрета (Secret):
 
 <img width="468" alt="image" src="https://user-images.githubusercontent.com/46056823/225917035-ee3e8dbb-d2ec-4581-b3a5-e324ec37b513.png">

Создание Ingress в minikube
В файле ingress.yaml приведен манифест Ingress, который описывает dns-имена и правила распределения траффика на поды.

 <img width="468" alt="image" src="https://user-images.githubusercontent.com/46056823/225917062-9ae65d4e-d3ce-47cd-9fed-fc7624bdeef8.png">
 <img width="362" alt="image" src="https://user-images.githubusercontent.com/46056823/225917095-1cb0723a-c738-446b-8484-bf4b00ff78ed.png">

 
После этого приложение может быть открыто в браузере по адресу https://lab3-app.olegovich.com/

<img width="468" alt="image" src="https://user-images.githubusercontent.com/46056823/225917119-1c6df73b-2614-4475-9a7d-ef02b90c4452.png">
Схема организации контейнеров и сервисов
<img width="368" alt="image" src="https://user-images.githubusercontent.com/46056823/225917179-8fa37287-ffb0-40b3-a2db-df923df2378d.png">

 
