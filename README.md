# 03-sysadmin-07-net
1.linux: ip link

Windows: ipconfig


2. протокол lldp

нужен пакет lldpd

команда для просмотра соседей - lldpctl

3. технология VLAN 802.1q

пакет vlan, далее требуется внести конфиг в файл /etc/network/interfaces , после чего для появления нового интерфейса рестартануть сеть командой  sudo /etc/init.d/networking restart

Пример конфига:

vagrant@vagrant:~$ cat /etc/network/interfaces

source-directory /etc/network/interfaces.d

auto vlan999

iface vlan999 inet static

        address 192.168.199.199

        netmask 255.255.255.0

        vlan_raw_device eth0

4.типы агрегации интерфейсов - статическая (постоянная) либо динамическая на основе протокола LACP

опции для балансировки нагрузки:

balance-rr : пакеты отправляются по всем интерфейсам по принципу round-robin

active-backup : активен только один интерфейс, остальные в роли резервирования

balance-xor : балансировки на основе MAC адресов отправителя - получателя

balance-tlb, balance-alb : адаптивное распределение между интерфейсами на основе загрузки - при tlb влияет только на исходящий трафик.

пример конфига /etc/network/interfaces :

auto bond0

iface bond0 inet static

    address 192.168.199.199
    netmask 255.255.255.0    
    gateway 192.168.199.1
    dns-nameservers 8.8.4.4 8.8.8.8
        slaves eth0 eth1
        bond_mode 0
        
5.
/29 - 8 IP адресов, хостам можно назначить 6 IP адресов (включая шлюз по умолчанию)
из сети /24 можно получить 32 подсети /29

внутри сети 10.10.10.0/24 примеры сетей /29:

10.10.10.0/29 10.10.10.16/29 10.10.10.24/29 10.10.10.80/29 10.10.10.168/29

6.
берем диапазон из подсети 100.64.0.0/10
на 40-50 хостов подойдет маска /26

7.
Linux:

ip neigh

ip neigh del <ip> dev <int> lladdr <mac>

sudo ip neigh flush all


Windows:

arp -a

arp -d <ip>

arp -d *

