---
layout: post
title:  "CC:Pen-Testing"
date:   2021-08-05 14:32:04 +0700
categories: [OSCP, THM]
---


Bu aşamada temel olarak belirli yöntemler kullanılmaktadır. ```Sosyal Medya, Shodan, Nmap, Nessus, Dig``` gibi araçlar kullanılarak sistemler hakkında bilgi toplanır ve OSINT yöntemleri kullanılarak da şirket çalışanları ve kurum hakkında bilgi toplanabilir. Hedefimiz bir Tryhackme platformunda bulunan **CC:Pen Testing** makinesi. 

Bu makine sistem içerisinde 5 ayrı makine mevcut. Biz hepsini tek tek değil genel bir makine çözüm haritası üzerinde durarak ilerleyeceğiz.

## Enumeration

Zafiyet tarama sistem üzerinde bulunan açıkları tespit etme kısmıdır. Çalışan servislerin yanlış yapılandırılması ya da güvenli olmayan uygulamalar üzerinde bulunan zafiyetler örnek verilebilir. Bu kısımda ```Nmap, Sqlmap, Nikto``` tarzı araçlar kullanılmaktadır.

Makinemizde nmap taraması yaptığımızda ```80``` portunun açık olduğunu görüyoruz.
80 portu açık olduğunda aşağıdaki seçenekleri gözden geçirebilirsiniz;

* Site kaynak kodu incelenebilir.
* ```gobuster``` aracı ile dizin taraması yapılabilir.
* ```Nikto``` ile web sitesi üzerindeki zaafiyetler hakkında bilgi alınabilir.
* ```Searchsploit``` ile ilgili uygulamaya ait zaafiyetler sorgulanabilir.
* Site **wordpress** tabanlı ise ```wpscan``` aracı kullanılabilir.
* ```Metasploit``` ile ilgili zaafiyet araması yapılabilir.
* İlgili zaafiyet için ```Nmap``` script havuzuna bakılabilir.
---
## Exploiting

Bu kısımda öncesinde topladığımız bilgiler ve bulunan zafiyetler kullanılarak sistem üzerinde bir yetki elde edilmeye çalışılmaktadır. Amaç sistemler üzerinde bulunan güvenlik duvarı, saldırı tespit ve engelleme sistemleri gibi önlemleri atlatarak sistem üzerinde bulunan kaynaklara erişim sağlamaktır.

Hangi zaafiyeti kullanacağımızı tespit ettikten sonra aşağıdaki adımlar gözden geçirilebilir;

* ```Msfvenom``` ile payload oluşturulabilir.
* ```netcat``` kullanımı ile shell alınabilir.

## Privilege Escalation

Yetki yükseltme aşamasında amaç ele geçirilen bir sistem üzerinde elde edilen kullanıcıdan daha yetkili bir kullanıcı hesabını ele geçirmektir. Bu adımda hedef sistem üzerinde çalışan uygulamalar, çekirdek sürümü ya da kullanıcı parola özetleri (Password hash) tespit etme gibi birçok yöntem uygulanmaktadır.

Makinede başarılı bir şekilde ```shell``` aldıktan sonra aşağıdaki seçeneklere göz atılabilir;

* Makine üzerinde dizin kontrolü yapılabilir.
* Dizinlerde ki gizli dosyalar tespit edilebilir.
* Hash türlerinden birini içeren bir dosya varsa ```john, hashcat``` gibi araçlar kullanılabilir.
* Sistemde sql açığı var ise ```sqlmap, sqlite3``` aracı kullanılabilir.
* sudo -l komutunun çıktısına bakılabilir.













