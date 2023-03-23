Лабораторная работa №4 "Сети связи в Minikube, CNI и CoreDNS"
Общая информация
University: ITMO University
Faculty: FICT
Course: Introduction to distributed technologies
Year: 2022/2023
Group: 
Author: Anufriev Artem Olegovich
Lab: Lab4
Date of create: 17.03.2023
Date of finished:
Ход работы
Запуск minikube кластера с несколькими нодами (Nodes) и CNI плагином calico
Для запуска кластера minikube с 2 нодами и плагином CNI, была использована следующая команда:
 
 <img width="385" alt="image" src="https://user-images.githubusercontent.com/46056823/227168220-a7df24ae-86db-4365-8148-c484569c78f8.png">


После запуска кластера были созданы 2 ноды:

<img width="348" alt="image" src="https://user-images.githubusercontent.com/46056823/227168262-8cf5d3a1-a7e4-4b5c-b039-3d4a5805cd06.png">

 
Развертывание calico:

<img width="406" alt="image" src="https://user-images.githubusercontent.com/46056823/227168284-3919ee52-f7ec-460f-a0d7-6e1c1ca69984.png">

 
Подготовка IP pools
При запуске кластера minikube c CNI calico, был создан ресурс IPPool - default-ipv4-ippool. Данный IPPool не имеет конкретных правил по назначению ip-адресов на ноды, поэтому его необходимо удалить, чтобы он мешал последующей работе.
Удаление дефолтного IP пула:

<img width="468" alt="image" src="https://user-images.githubusercontent.com/46056823/227168307-5cc26590-d44e-4d58-b778-ab199957bbf7.png">

 
Добавление меток для нод:

<img width="396" alt="image" src="https://user-images.githubusercontent.com/46056823/227168349-ea3953e7-3516-41d2-ab9d-2aebb83a2699.png">

 
После добавления меток на ноды, были разработаны манифесты IPPools, представленные в файле ip-pools.yaml.
IPPool region-east-ippool будет назначать подам, запланированных на ноде с меткой zone=east, IP-адреса из адресного пространства 10.244.0.0/24.
IPPool region-west-ippool будет назначать подам, запланированных на ноде с меткой zone=west, IP-адреса из адресного пространства 10.244.1.0/24.
Создание IP пулов в кластере:

<img width="468" alt="image" src="https://user-images.githubusercontent.com/46056823/227168374-a4f66889-19fa-4173-94db-7d2b3ad37f4a.png">

 


Проверка назначения IP адресов
Для проверки созданных IP-пулов необходимо развернуть тестовое приложение и посмотреть, на каких нодах развернулись поды и какие IP-адреса этим подам были присвоены.
Развертывание тестового приложения и проверка IP адресов:

<img width="468" alt="image" src="https://user-images.githubusercontent.com/46056823/227168415-98632ec3-9ebb-4ece-ac2e-aa096294d279.png">

 
Как видно из вывода, один под разместился на ноде multinode-cluster, которой была присвоена метка zone=east, и получилось IP-адрес из адресного пространства: 10.244.0.0/24. Второй же под запустился на ноде multinode-cluster-m02 с меткой zone=west и получил IP-адрес из пула адресов 10.244.1.0/24.
Вместе с deployment был также развернут сервис типа LoadBalancer для доступа к приложению. Получим доступ к нему через туннель minikube и проверим ответы приложения в браузере.
Запуск туннеля minikube:

<img width="468" alt="image" src="https://user-images.githubusercontent.com/46056823/227168453-2c449509-da05-4020-8e17-e124e33f3210.png">

 
Тестирование приложения в браузере:

<img width="468" alt="image" src="https://user-images.githubusercontent.com/46056823/227168477-a17c42d3-6a6b-49f4-8bae-b913eb4995e6.png">

 
Пинг:

<img width="435" alt="image" src="https://user-images.githubusercontent.com/46056823/227168505-e86f1002-18ea-4a1e-8c28-12d4307b362f.png">

 

Схема организации контейнеров и сервисов

<img width="468" alt="image" src="https://user-images.githubusercontent.com/46056823/227168526-1a94b30b-0026-4814-8fb5-24dbadd24844.png">

 
