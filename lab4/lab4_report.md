# Отчет по лабораторной работе №4 "Базовая 'коммутация' и туннелирование используя язык программирования P4"
University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Network Programming](https://itmo-ict-faculty.github.io/network-programming/)

Year: 2023/2024

Group: 1701

Author: Vikliaev Dmitriy Romanovich

Lab: Lab4

Date of create: 02.12.2023

Date of finished: --.--.2023


## Цель работы: 
Изучить синтаксис языка программирования P4 и выполнить 2 обучающих задания от Open network foundation для ознакомления на практике с P4.

## Ход работы:

### Подготовка

- клонирование рипозитория https://github.com/p4lang/tutorials/tree/master
- установка virtualbox
- установка vargrant
- Перейти в папку vm-ubuntu-20.04
- Используя Vagrant развернуть тестовую среду vagrant up

В результате установки запустилась виртуальная машина с аккаунтами login/password vagrant/vagrant и p4/p4

![](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab4/launch_vm.png)


### Задание Implementing Basic Forwarding

проверка несвязанности устройств

![](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab4/mininet_ping.png)

проверка связанности устройств

![](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab4/mininet_ping2.png)
