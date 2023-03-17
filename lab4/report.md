# Лабораторная работa №4 "Сети связи в Minikube, CNI и CoreDNS"

## Общая информация

University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)

Year: 2022/2023

Group: K4113c

Author: Koshevskii Lev Sergeevich

Lab: Lab4

Date of create: 11.01.2023

Date of finished: 

## Ход работы

### Запуск minikube кластера с несколькими нодами (Nodes) и CNI плагином calico

Для запуска кластера minikube с 2 нодами и плагином CNI, была использована следующая команда: 

![image](https://user-images.githubusercontent.com/46699832/211786351-fcd5e055-e727-4106-8f2c-66067cc6501a.png)

После запуска кластера были созданы 2 ноды:

![image](https://user-images.githubusercontent.com/46699832/211786406-41077cea-bfbd-44ea-ada7-8a120b327a86.png)

Развертывание calico:

![image](https://user-images.githubusercontent.com/46699832/211786556-99005ad7-fa0a-4949-a878-13a7b87260c4.png)

### Подготовка IP pools

При запуске кластера minikube c CNI calico, был создан ресурс IPPool - default-ipv4-ippool.
Данный IPPool не имеет конкретных правил по назначению ip-адресов на ноды, поэтому его необходимо удалить, чтобы он мешал последующей работе.

Удаление дефолтного IP пула:

![image](https://user-images.githubusercontent.com/46699832/211789689-167c3934-582e-4970-808e-b21d2f37ebda.png)

Добавление меток для нод:

![image](https://user-images.githubusercontent.com/46699832/211787017-7644eede-6701-4e11-bbf3-3c6405376ccf.png)

После добавления меток на ноды, были разработаны манифесты IPPools, представленные в файле ip-pools.yaml.

IPPool region-east-ippool будет назначать подам, запланированных на ноде с меткой zone=east, IP-адреса из адресного пространства 10.244.0.0/24.

IPPool region-west-ippool будет назначать подам, запланированных на ноде с меткой zone=west, IP-адреса из адресного пространства 10.244.1.0/24.

Создание IP пулов в кластере:

![image](https://user-images.githubusercontent.com/46699832/211789826-bbfa4b27-76c0-44a5-bd91-9d53bf87ef1f.png)
### Проверка назначения IP адресов

Для проверки созданных IP-пулов необходимо развернуть тестовое приложение и посмотреть, на каких нодах развернулись поды и какие IP-адреса этим подам были присвоены.

Развертывание тестового приложения:

![image](https://user-images.githubusercontent.com/46699832/211787415-d6403ba3-08b8-41d4-9f66-e794d72b4e1f.png)

Проверка IP-адресов:

![image](https://user-images.githubusercontent.com/46699832/211787477-22156e70-255b-4007-925a-f91c3b025577.png)

Как видно из вывода, один под разместился на ноде multinode-cluster, которой была присвоена метка zone=east, и получилось IP-адрес из адресного пространства: 10.244.0.0/24. Второй же под запустился на ноде multinode-cluster-m02 с меткой zone=west и получил IP-адрес из пула адресов 10.244.1.0/24.

Вместе с deployment был также развернут сервис типа LoadBalancer для доступа к приложению. Получим доступ к нему через туннель minikube и проверим ответы приложения в браузере.

Запуск туннеля minikube:

![image](https://user-images.githubusercontent.com/46699832/211787580-cea1db5d-cde2-4076-ba7c-a62286fccd5f.png)

Тестирование приложения в браузере:

![image](https://user-images.githubusercontent.com/46699832/211787637-0e89ccff-93f4-47f3-a1bf-11688575e746.png)

![image](https://user-images.githubusercontent.com/46699832/211787660-f45b9201-b873-4b9a-9711-9fec695d49d0.png)

Пинг:
![image](https://user-images.githubusercontent.com/46699832/211787745-52e321e1-340a-4111-a308-f1289dc42616.png)

### Схема организации контейнеров и сервисов

![image](https://user-images.githubusercontent.com/46699832/211807296-4e301a13-a3e6-4820-8b62-77afe0240ba1.png)




