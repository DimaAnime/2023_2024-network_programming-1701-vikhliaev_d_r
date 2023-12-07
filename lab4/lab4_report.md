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


По умолчанию все пакеты автоматически отбрасываются.

Изменения в файле basic.p4.

1. Добавление в парсер заголовки ipv4 и ethernet:

```
parser MyParser(packet_in packet,
                out headers hdr,
                inout metadata meta,
                inout standard_metadata_t standard_metadata) {

    state start {
        /* TODO: add parser logic */
        transition parse_ethernet;
        #transition accept;
    }
    
    state parse_ethernet {
        packet.extract(hdr.ethernet);
        transition select(hdr.ethernet.etherType) {
            TYPE_IPV4 : parse_ipv4;
            default : accept;
        }    
    }

    state parse_ipv4 {
        packet.extract(hdr.ipv4);
        transition accept;
    }
}
```

2.
- установка входного порта для пересылки ipv4;
- обновлие исходного mac-адреса и адреса назначения;
- изменение значения TTL;
- добавление таблицы маршрутизации и условия проверки заголовка IPv4.
```
control MyIngress(inout headers hdr,
                  inout metadata meta,
                  inout standard_metadata_t standard_metadata) {
    action drop() {
        mark_to_drop(standard_metadata);
    }

    action ipv4_forward(macAddr_t dstAddr, egressSpec_t port) {
        /* TODO: fill out code in action body */
        standard_metadata.egress_spec = port;
        hdr.ethernet.srcAddr = hdr.ethernet.dstAddr;
        hdr.ethernet.dstAddr = dstAddr;
        hdr.ipv4.ttl = hdr.ipv4.ttl - 1;
    }

    table ipv4_lpm {
        key = {
            hdr.ipv4.dstAddr: lpm;
        }
        actions = {
            ipv4_forward;
            drop;
            NoAction;
        }
        size = 1024;
        default_action = NoAction();
    }

    apply {
        /* TODO: fix ingress control logic
         *  - ipv4_lpm should be applied only when IPv4 header is valid
         */
        if (hdr.ipv4.isValid()) {
            ipv4_lpm.apply();
        }
    }
}
```

3. Дополнение Deparser - выбор порядка вставки полей при сборке заголовка пакета обратно.

```
control MyDeparser(packet_out packet, in headers hdr) {
    apply {
        /* TODO: add deparser logic */
        packet.emit(hdr.ethernet);
        packet.emit(hdr.ipv4);
    }
}
```

проверка связанности устройств

![](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab4/mininet_ping2.png)

Полученная схема связи

![](https://github.com/DimaAnime/2023_2024-network_programming-1701-vikhliaev_d_r/blob/main/lab4/sheme1.png)
