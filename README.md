# Otus Homework 24. VLAN.
### Цель домашнего задания
Научиться настраивать VLAN и LACP.
### Описание домашнего задания
в Office1 в тестовой подсети появляются сервера с доп интерфейсами и адресами в internal сети testLAN: 
- testClient1 - 10.10.10.254
- testClient2 - 10.10.10.254
- testServer1- 10.10.10.1 
- testServer2- 10.10.10.1

Равести вланами:  
testClient1 <-> testServer1  
testClient2 <-> testServer2  

Между centralRouter и inetRouter "пробросить" 2 линка (общая inernal сеть) и объединить их в бонд. Проверить работу c отключением интерфейсов
## Выполнение
С помощью Vagrant развернем тестовый стенд. Топология сети представлена на схеме:
![network](network.jpg)

Выполним настройку VLAN на хостах **testClient1** и **testServer1** с операционной системой CenttOS 7. Для этого создадим файл _/etc/sysconfig/network-scripts/ifcfg-vlan10_.
Для сервера **testClient1**:
```bash

```
Для сервера **testServer1** настройка идентичная, но необходимо изменить IP-адрес. Для применения конфигурации необходимо перезапустить служба _NetworkManager_:
```bash
systemctl restart NetworkManager
```
На хостах **testClient2** и **testServer2** с операционной системой Ubuntu 22.04 необходимо настроить конфигурацию netplan. Для testClient2:
```bash

```
Для сервера **testServer1** настройка идентичная, но необходимо изменить IP-адрес. Применим конфигурацию netplan:
```bash
netplan apply
```
После этого хосты **testClient1** и **testServer1** в _VLAN 10_ должны видеть друг друга и не иметь доступа для серверов из _VLAN 20_.

![ping1](ping1.jpg)
  
![ping2](ping2.jpg)
