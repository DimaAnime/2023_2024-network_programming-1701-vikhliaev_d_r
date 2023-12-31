# Отчет по лабораторной работе №3 "Развертывание Netbox, сеть связи как источник правды в системе технического учета Netbox"
University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Network Programming](https://itmo-ict-faculty.github.io/network-programming/)

Year: 2023/2024

Group: 1701

Author: Vikliaev Dmitriy Romanovich

Lab: Lab3

Date of create: 24.11.2023

Date of finished: --.--.2023

## Цель работы: 
с помощью Ansible и Netbox собрать всю возможную информацию об устройствах и сохранить их в отдельном файле.

## Ход работы:

### Поднятие Netbox на VM

#### Установка posegresql и создание базы данных netbox

![ppstgresql](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab3/postgresql.png)


#### Установка Redis

![ppstgresql](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab3/redis.png)


#### Установка и настройка NetBox

Содержание файла конфигурации с сгенерированным секретным ключом 

![configpy](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab3/configpy.PNG)


Настройка gunicorn, запуск netbox сервиса в режиме daemon.

![gunicron](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab3/gunicron.jpg)


Установка nginx
Содержание nginx.conf

![nginx_conf](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab3/nginx_conf.PNG)

Открытие доступа к графическому интерфейсу netbox

![netbox_interphase](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab3/netbox_interphase.png)


### Заполнение информации о CHR в Netbox

![chr_netbox](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab3/chr_netbox.png)

###  Сохранение всех данных из Netbox в отдельный файл

inventory файл для сбора данных об устройствах с api tokenom netbox

![api_token](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab3/api_token.PNG)

информация об устройствах: [inventory](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab3/inventory.yml)

### Изменение имени устройства, добавление IP адреса на устройство

playbook для изменения имени устройства и добавление интерфейса

![playbook](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab3/playbook.PNG)

![](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab3/ansible_work.jpg)

![chr1](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab3/chr1.PNG)
![chr2](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab3/chr2.PNG)

### Написать сценарий, позволяющий собрать серийный номер устройства и вносящий серийный номер в Netbox

playbook

![](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab3/serial_number_playbook.PNG)

Результат

![](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab3/ansible_serial_num.png)

chr1

![](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab3/serial_num_1.jpg)

chr2

![](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab3/serial_num_2.jpg)


### схема связи

![](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab3/shame.drawio.png)




