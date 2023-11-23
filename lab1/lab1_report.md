University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming)
Year: 2023/2024
Group: 1701
Author: Vikliaev Dmitriy Romanovich
Lab: Lab1
Date of create: 28.09.2023
Date of finished: 26.10.2023

# Лабораторная работа №1 "Установка CHR и Ansible, настройка VPN"

## Описание
Данная работа предусматривает обучение развертыванию виртуальных машин (VM) и системы контроля конфигураций Ansible а также организации собственных VPN серверов.

## Цель работы
Целью данной работы является развертывание виртуальной машины на базе платформы Yandex Cloud с установленной системой контроля конфигураций Ansible и установка CHR в VirtualBox

## Ход работы
В результате работы будет настроен vpn между клииентом и сервером по протаколу openvpn, передача данных будет осуществлятся по протоколу транспортного уровня tcp.

### Схема связи между клиентом и сервером
![sheme](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab1/sheme.png)

В начеле работы была создана виртуальная машина в облачной среде Yandex Cloud с операционной системой "Ubuntu Server 22.04", для использования в качестве vpn сервера.
### IP адресс сервера
![ip_address_server](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab1/foreign_ip_server.PNG)

Далее, на локальном пк, с помощью программы virtualbox, была установлена операцтонная система "CHR RouterOS 6.49", для  использования в качестве vpn клиента.
В качестве протокола vpn был выбран openvpn.

На локальной виртульной ОС "Ubuntu 22.04", с помощью программы "easyrsa3", был создан удостоверяющий центр(CA). Затем на клиенте и сервере были зозданы открытый и закрытый ключи, на основе которых были подписаны сертификаты CA.

Для настройки openvpn сервера был сформирован файл конфигурации.

### Файл конфигурации сервера
![server_conf](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab1/server.conf.png)

В результате запуска программы openvpn был открыт виртуальный сетевой тунель (TUN). 

### Открытие сетегого тунеля на сервере
![ip_server](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab1/ip%20server.PNG)

### Настройки конфигурации клиента
![Client_conf](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab1/conf_client.PNG)

### ip адресс клиента vpn
![Client_ip](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab1/client_ip.PNG)

### ping от клиента на сервер и от сервера клиенту через vpn
![ping_client](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab1/ping.PNG)
![ping_server](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab1/server_ping.PNG)


