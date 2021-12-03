## Laporan Hasil Pratikum 2 - Sistem Administrasi Server 

Nadila Chusnul K - 1202190020 \
Anastasya Rahma Juniarti - 120219058\
Kelompok 10 

Masuk windows terminal dan konek menggunakan ssh
   ![A1](aset/p1.jpg)
    
Cek lxc yang ada
 ```bash
    lcx-ls - f
 ```
   ![A1](aset/p2.jpg)

Stop ubuntu landing dan ubuntu php7.4 karena akan dihapus/destroy
```bash
    lxc-stop -n ubuntu_landing
    lxc-stop -n ubuntu_pho7.4
 ```
 ![A1](aset/p3.jpg)

Hapus clone terlebih dulu
```bash
    lxc-destroy ubuntu_landing_backup
    lxc-destroy ubuntu_landing
    lxc-destroy ubuntu_php7.4_backup
    lxc-destroy ubuntu_php7.4
 ```
![A1](aset/p4.jpg)

Buat lxc ubuntu_landing focal dan ubuntu_php7.4 focal dan start kedua lxc
```bash
sudo lxc-create -n ubuntu_landing -t download -- --dist ubuntu --release focal --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
```
![A1](aset/p5.jpg)

## 1. Rubah LXC landing dengan ubuntu focal (destroy n create, same ip, same name)
Masuk ubuntu landing dan setting ip
```bash
    lxc-attach ubuntu_landing
```
![A1](aset/p6.jpg)
```bash
    nano /etc/netplan/10-lxc.yaml
```
![A1](aset/p7.jpg)
![A1](aset/p8.jpg)

Restart kemudian cek ip
```bash
    nano /etc/netplan/10-lxc.yaml
    netplan apply
    ip a
```
![A1](aset/p9.jpg)
kemudain exit

Install ssh server 
```bash
    apt-get install openssh-server -y
```
![A1](aset/p10.jpg)

Konfigurasi SSH
```bash
    nano /etc/ssh/sshd_config
```
![A1](aset/p11.jpg)
![A1](aset/p12.jpg)

Set password dan restart ssh
```bash
    service sshd restart
    passwd
```
![A1](aset/p13.jpg)

Test SSH server
```bash
    ssh root@10.0.3.103
```
![A1](aset/p14.jpg)

## 2. 2. Rubah LXC php7 dengan ubuntu focal (destroy n create, same ip, same name)

Masuk ubuntu_php7.4
```bash
    lxc-attach -n ubuntu_php7.4
```
![A1](aset/p15.jpg)

Setting ip
```bash
    nano /etc/netplan/10-lxc.yaml
```
![A1](aset/p16.jpg)
![A1](aset/p17.jpg)

Restart agar ip terganti
```bash
    netplan apply
```
![A1](aset/p18.jpg)

Install ssh server 
```bash
    apt install openssh-server 
```
![A1](aset/p19.jpg)

konfig sshd
```bash
    nano /etc/ssh/sshd_config
```
![A1](aset/p20.jpg)
![A1](aset/p21.jpg)

Set password dan restart ssh
```bash
    service sshd restart
    passwd
```
![A1](aset/p22.jpg)

Test ssh server
```bash
    ssh root@10.0.3.101
    lxc-ls -f
```
![A1](aset/p23.jpg)
![A1](aset/p24.jpg)

## 3. vm.local/

Buat file install Laravel
```bash
    nano install-laravel.yml
```
![A1](aset/p25.jpg)
![A1](aset/p26.jpg)

Buat direktori roles/php/tasks dan roles/php/handlers
```bash
    mkdir -p roles/php/tasks
    mkdir -p roles/php/handlers
```
![A1](aset/p27.jpg)

Edit roles/php/tasks/main.yml
```bash
    nano roles/php/task/main.yml
```
![A1](aset/p28.jpg)
![A1](aset/p29.jpg)
![A1](aset/p30.jpg)

Edit roles/php/handlers/main.yml
```bash
    nano roles/php/handlers/main.yml
```
![A1](aset/p31.jpg)
![A1](aset/p32.jpg)

Buat direktory roles/lv/tasks, roles/lv/templates, roles/lv/handlers
```bash
    mkdir -p roles/lv/tasks
    mkdir -p roles/lv/templates
    mkdir -p roles/lv/handlers
```
![A1](aset/p33.jpg)

Edit roles/lv/tasks/main.yml
```bash
    mkdir -p roles/lv/tasks/main.yml
```
![A1](aset/p34.jpg)
![A1](aset/p35.jpg)
![A1](aset/p36.jpg)
![A1](aset/p37.jpg)

Edit roles/lv/templates/env.template
```bash
    nano roles/lv/templates/env.template
```
![A1](aset/p38.jpg)
![A1](aset/p39.jpg)
![A1](aset/p40.jpg)
![A1](aset/p41.jpg)

Edit roles/lv/templates/lv.conf
```bash
    nano roles/lv/templates/lv.conf
```
![A1](aset/p42.jpg)
![A1](aset/p43.jpg)

Edit roles/lv/handlers/main.yml
```bash
    nano roles/lv/handlers/main.yml
```
![A1](aset/p44.jpg)
![A1](aset/p45.jpg)

Run ansible dengan ansible-playbook -i hosts install-laravel.yml -k
```bash
    ansible-playbook -i hosts install-laravel.yml -k
```
![A1](aset/p46.jpg)
![A1](aset/p47.jpg)

Test laravel di browser
```bash
    vm.local
```
![A1](aset/p48.jpg)

## 4.vm.local/blog

Buat ansible untuk mengintall wordpress lalu edit
```bash
    nano install-wp.yml
```
![A1](aset/p49.jpg)
![A1](aset/p50.jpg)

Buat direktory roles/wp/tasks, roles/wp/templates, roles/wp/handlers
```bash
    mkadir -p roles/wp/tasks  mkadir -p roles/wp templates
    mkadir -p roles/wp/handlers
```
![A1](aset/p51.jpg)

Edit direktory roles/wp/tasks/main.yml
```bash
    nano roles/wp/tasks/main.yml
```
![A1](aset/p52.jpg)
![A1](aset/p53.jpg)
![A1](aset/p54.jpg)
![A1](aset/p55.jpg)

Edit roles/wp/templates/wp.conf
```bash
    nano roles/wp/templates/wp.conf
```
![A1](aset/p56.jpg)
![A1](aset/p57.jpg)
![A1](aset/p58.jpg)
![A1](aset/p59.jpg)
![A1](aset/p60.jpg)

Edit roles/wp/templates/wp.local
```bash
    nano roles/wp/templates/wp.local
```
![A1](aset/p61.jpg)
![A1](aset/p62.jpg)

Edit roles/wp/handlers/main.yml
```bash
    nano roles/wp/handlers/main.yml
```
![A1](aset/p63.jpg)
![A1](aset/p64.jpg)

Run ansible install-wp.yml
```bash
    ansible-playbook -i hosts install-wp .yml -k
```
![A1](aset/p66.jpg)
![A1](aset/p66.jpg)

Test di browser
```bash
  vm.local/blog/wp-admin/install.php
```
![A1](aset/p67.jpg)
![A1](aset/p68.jpg)
![A1](aset/p69.jpg)
![A1](aset/p70.jpg)
![A1](aset/p71.jpg)
![A1](aset/p72.jpg)

## Soal Tambahan
5. Rubah konfigurasi php7.4 yang semula menggunakan socket menjadi menggunakan port (127.0.0.1:9001)

Untuk vm.local (laravel)
```bash
  nano roles/lv/templates/lv.conf
```
![A1](aset/p73.jpg)
![A1](aset/p74.jpg)

Untuk vm.local/blog (wordpress)
```bash
  nano roles/wp/templates/wp.local
```
![A1](aset/p75.jpg)
![A1](aset/p76.jpg)

Ecit /etc/php/7.4/fpm/www.conf
```bash
  nano /etc/php/7.4/fpm/www.conf
```
![A1](aset/p77.jpg)
![A1](aset/p78.jpg)

Run ansible Laravel
```bash
    ansible-playbook -i hosts install-laravel.yml -k
```
![A1](aset/p79.jpg)
![A1](aset/p80.jpg)

Test di browser
```bash
    vm.local
```
![A1](aset/p81.jpg)

Run ansible wordpress
```bash
    ansible-playbook -i hosts install-wp.yml -k
```
![A1](aset/p82.jpg)
![A1](aset/p83.jpg)

Test di browser
```bash
    vm.local/blog
```
![A1](aset/p84.jpg)