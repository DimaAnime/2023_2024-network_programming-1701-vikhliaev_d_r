## Отчет по лабораторной работе №2 "Развертывание дополнительного CHR, первый сценарий Ansible"

University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming)

Year: 2023/2024

Group: 1701

Author: Vikliaev Dmitriy Romanovich

Lab: Lab1

Date of create: 26.10.2023

Date of finished: --.--.2023

## Цель работы
С помощью Ansible настроить несколько сетевых устройств и собрать информацию о них. Правильно собрать файл Inventory.

## Ход работы

####  Настройка OVPN Client на 2-х CHR
![ovpn_2_chr](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab2/ovpn_2_chr.PNG)

#### ping
![ping_chr_server](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab2/ping_chr_to_server.PNG)

### Ansible

#### Содержимое файла inventory
```conf
[routers]
chr1 ansible_host=10.15.0.9 rid=10.10.1.1
chr2 ansible_host=10.15.0.5 rid=10.10.1.2

[routers:vars]
ansible_connection=ansible.netcommon.network_cli
ansible_network_os=community.routeros.routeros
ansible_user=admin
ansible_ssh_pass=admin
```
#### Получение ифромации о хостах
```console
{
    "_meta": {
        "hostvars": {
            "chr1": {
                "ansible_connection": "ansible.netcommon.network_cli",
                "ansible_host": "10.15.0.9",
                "ansible_network_os": "community.routeros.routeros",
                "ansible_ssh_pass": "admin",
                "ansible_user": "admin",
                "rid": "10.10.1.1"
            },
            "chr2": {
                "ansible_connection": "ansible.netcommon.network_cli",
                "ansible_host": "10.15.0.5",
                "ansible_network_os": "community.routeros.routeros",
                "ansible_ssh_pass": "admin",
                "ansible_user": "admin",
                "rid": "10.10.1.2"
            }
        }
    },
    "all": {
        "children": [
            "ungrouped",
            "routers"
        ]
    },
    "routers": {
        "hosts": [
            "chr1",
            "chr2"
        ]
    }
}
```

#### Ansible пинг
![ping_pong](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab2/ping_pong.png)

#### 7. Конфигурационный файл для playbook

```yml
- name: Configure
  hosts: routers
  gather_facts: false
  tasks:
    - name: add user
      routeros_command:
        commands :
          - /user add name=valery password=remote group=full
    - name: set NTP client
      routeros_command:
        commands :
          - /system ntp client set enabled yes primary-ntp [:resolve @.ru.pool.ntp.org] secondary-ntp [:resolve 1.ru.pool.ntp.org]
    - name: set OSPF
      routeros_command:
        commands :
          - /ip address add address={{rid}}/24 interface=ether1
          - /routing ospf network add network=10.10.1.0/24 area=backbone

- name: Get information
  hosts: routers
  gather_facts: false
  tasks:
    - name: get configs
      register: configuration
      community.routeros.facts:
        gather_subset:
          - config
    - name: collect OSPF topology
      register: ospf
      routeros_command:
        commands :
          - /routing ospf neighbor print
    - name: Print configuration
      debug:
        msg: "{{ configuration }}"
    - name: Print ospf
      debug:
        msg: "{{ ospf }}"
```
#### Результат работы playbook
Gjkysq результы вынесен в файл [ok.txt](ok.txt)

####  Проверка связанности роутеров

Пинг между роуторами

![ping1](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab2/ping1.png)

![ping2](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab2/ping2.png)
