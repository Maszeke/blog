---
title: Implementasi Loadbalancing Server Dengan Reverse Proxy Apache di Centos 7
author: ~
date: '2020-12-10'
slug: Implementasi Loadbalancing Server Dengan Reverse Proxy Apache di Centos 7
description : Selain web server, Apache juga dapat dikonfigurasi sebagai Reverse Proxy untuk membuat cluster load balancing dari dua atau lebih server web. Fungsionalitas ini dapat ditambahkan ke Apache melalui modul mod_proxy.
categories: ['Sisjarkom', 'Loadbalancer']
tags: ['Sisjarkom', 'Loadbalancer']
---

Selain web server, Apache juga dapat dikonfigurasi sebagai **Reverse Proxy** untuk membuat cluster load balancing dari dua atau lebih server web. Fungsionalitas ini dapat ditambahkan ke Apache melalui modul **mod_proxy**.

## Persiapan

Untuk dapat mengikuti tutorial ini, Anda harus mempersiapkan beberapa hal sebagai berikut :

#### Menyiapkan Sistem Operasi

Dalam implementasinya tidak dianjurkan untuk menggunakan loadbalancing pada satu komputer yang sama. Namun, sebagai latihan tutorial ini menggunakan tiga buah virtual machine dengan virtualbox dengan spesifikasi sebagai berikut :

| Hostname              | IP Address    | OS              |
|-----------------------|---------------|-----------------|
| Centos 7 loadbalancer | 192.168.43.71 | Centos 7 64 bit |
| Centos 7 Server 1     | 192.168.43.92 | Centos 7 64 bit |
| Centos 7 Server 2     | 192.168.43.65 | Centos 7 64 bit |

#### Install Essential Tools

**net-tools**

```{}
yum install net-tools
```

**nano**

```{}
yum install nano
```

#### Install Apache

Install apache server untuk ketiga virtual machine yang sudah disediakan sebelumnya dengan perintah 

**Install apache**
```{}
yum install httpd 
```

**Enable service httpd**
```{}
systemctl enable httpd
```

**Start service httpd**
```{}
systemctl start httpd
```

**Buka firewall service http/port 80**
```{}
firewall-cmd --permanent --add-service=http
```

**Reload Firewall**
```{}
firewall-cmd --reload
```

Pastikan ketiga server tersebut bisa diakses dari luar VM. Akses http://(ip / domain)/ pada browser komputer. contoh http://192.168.43.71/

## Konfigurasi Loadbalancer

Apache HTTP Server membutuhkan modul **mod_proxy** agar dapat berfungsi sebagai Load Balancer. Modul **mod_proxy** sudah tersedia dalam paket httpd, oleh karena itu secara otomatis terinstall Bersama Apache HTTP Server pada CentOS 7.

Gunakan perintah berikut untuk memverifikasi ketersediaan **mod_proxy** pada server loadbalancer.

```{}
httpd –M | grep proxy
```

Buat sebuah file konfigurasi pada server loadbalancer dengan perintah berikut

```{}
nano /etc/httpd/conf.d/lbsisjarkom.conf
```

Masukkan script konfigurasi berikut ini

```{}
<proxy balancer://mycluster>
    BalancerMember http://192.168.43.92:80
    BalancerMember http://192.168.43.65:80
    ProxySet lbmethod=byrequests
</proxy>

<Location /balancer-manager>
    SetHandler balancer-manager
    allow from all
</Location>

ProxyPass /balancer-manager !
ProxyPass / balancer://mycluster/
ProxyPassReverse / balancer://mycluster/
```

Restart service httpd dengan perintah berikut

```{}
systemctl restart httpd
```

Akses Loadbalancer melalui browser 

```{}
http://192.168.43.71/
```

Akses loadbaancer manager

```{}
http://192.168.43.71/balancer-manager
```

## Konfigurasi Server 1 dan Server 2

Buat sebuah file html pada server 1 dan server 2 dengan perintah berikut

```{}
nano /var/www/html/index.html
```

Masukkan Script berikut kedalam file index.html yang sudah dibuat


```html
<html>
    <head>
        <title>Server 1</title>
    </head>
    <body>
        <h1> Selamat Datang di Server 1</h1>
    </body>
</html>
```

***server 2 menyesuaikan***

Akses Loadbalancer melalui browser 

```{}
http://192.168.43.71/
```

Reload browser, Loadbalancer akan mengakses server 1 dan server 2 secara bergantian

## Bagaimana jika salah satu server mati ?

Matikan salah satu server dengan perintah berikut

```{}
systemctl stop httpd
```

Maka loadbalancer akan mengakses server yang masih aktif dan server yang mati statusnya akan error

## Bagaimana jika kedua server mati ?

Matikan kedua server dan reload browsernya maka status kedua server menjadi error dan akan muncul status error 503 Service Unavailable

## Mengamankan Balancer Manager

Buat user baru pada sistem

```{}
htpasswd -c /etc/httpd/htpasswd admin
```

Ganti kode pada file konfigurasi loadbalancer

```{}
nano /etc/httpd/conf.d/lbsisjarkom.conf
```

Ganti

```{}
<Location /balancer-manager>
    SetHandler balancer-manager
    allow from all
</Location>
```

Menjadi

```{}
<location "/balancer-manager"> 
    SetHandler balancer-manager 
    AuthType "basic" 
    AuthName "balancer-manager" 
    AuthUserFile /etc/httpd/htpasswd 
    Require valid-user 
</location>
```

Restart service httpd dengan perintah berikut

```{}
systemctl restart httpd
```

Akses Loadbalancer manager, maka nanti akan muncul jendela untuk melakukan auntentikasi
