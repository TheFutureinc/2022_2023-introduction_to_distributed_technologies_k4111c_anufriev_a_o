Лабораторная работа №1 "Установка Docker и Minikube, мой первый манифест."
Общая информация
University: ITMO University
Faculty: FICT
Course: Introduction to distributed technologies
Year: 2022/2023
Group: 
Author: Anufriev Artem Olegovich
Lab: Lab1
Date of create: 17.03.2023
Date of finished:
Ход работы
Запуск кластера minikube
Для создания кластера minikube была использована следующая команда:
 <img width="468" alt="image" src="https://user-images.githubusercontent.com/46056823/225920308-479d6321-d672-4902-b34a-3c61a392e06d.png">

Данная команда использует Docker в качестве драйвера для запуска Kubernetes кластера. После создания кластера, необходимо проверить возможность подключения к нему.


Запуск пода в Kubernetes кластере
После запуска кластера и проверки подключения, был создан манифест pod.yaml. Для развертывания пода была использована следущая команда:
<img width="464" alt="image" src="https://user-images.githubusercontent.com/46056823/225920257-f21e42db-8e50-4073-97ea-c7a1fdab9ec3.png">

Убедимся, что pod стартовал успешно:
 <img width="262" alt="image" src="https://user-images.githubusercontent.com/46056823/225920236-6abe1de9-c255-4952-a310-9dfe98fcd641.png">

Создание сервиса
Для обеспечения доступа к поду, был создан сервис vault типа NodePort:
 <img width="460" alt="image" src="https://user-images.githubusercontent.com/46056823/225920219-3ccfd469-22cc-4794-889c-e49706b2296a.png">

Для получения созданного сервиса выполним следующую команду:
 <img width="392" alt="image" src="https://user-images.githubusercontent.com/46056823/225920199-5a12e6d1-763a-403b-a128-be91a2cf52c7.png">

Получение доступа к контейнеру
 <img width="395" alt="image" src="https://user-images.githubusercontent.com/46056823/225920175-73e82b76-560b-416b-bff5-0dd5e531bc4e.png">

Данная команда пробрасывает порт хоста в контейнер Vault. После выполнения данной команды, приложение будет доступно по адресу http://localhost:8200.
 <img width="468" alt="image" src="https://user-images.githubusercontent.com/46056823/225920155-b34ececd-645f-4e83-b6b0-8fb6d5ac9252.png">


Получение токена для доступа в Vault
Получим логи контейнера и отфильтруем их:
<img width="299" alt="image" src="https://user-images.githubusercontent.com/46056823/225920135-a9804e58-84aa-476a-a6ef-493035d3f607.png">

 

Вставим найденный токен в форму авторизации на странице приложения. При успешной авторизации откроется главная страница приложения Vault:	
 <img width="468" alt="image" src="https://user-images.githubusercontent.com/46056823/225920114-f3737ede-48cc-4337-819d-877c23617d84.png">


Схема организации контейнеров и сервисов
<img width="468" alt="image" src="https://user-images.githubusercontent.com/46056823/225920094-b99226c0-6225-4019-9623-637c7fdad035.png">

