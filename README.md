# Postfix-Rules-Config

Bu rehberde, Postfix'te e-posta içeriği ve başlığına göre filtreleme oluşturmak için gerekli adımları göstereceğiz. Bu, spam ve istenmeyen e-postaları engellemek için yararlı olabilir.


## Postfix Erişim Haritası Oluşturma ve Güncelleme

Öncelikle /etc/postfix/access dosyasını düzenleyin:

```
sudo nano /etc/postfix/access
```


E-posta adresini ekleyin ve reddedilecek şekilde yapılandırın:

```
spammer@example.com REJECT
```

Daha sonra, access dosyasını postmap ile güncelleyin:

```
sudo postmap /etc/postfix/access
```

Postmap komutunu kullanarak access.db dosyasını oluşturun.

```
sudo postmap /etc/postfix/access
```

## Postfix main.cf

Postfix'in ana yapılandırma dosyası olan /etc/postfix/main.cf dosyasını düzenleyin:

```
sudo nano /etc/postfix/main.cf
```


Aşağıdaki satırı ekleyin veya düzenleyin:

```
smtpd_recipient_restrictions = check_sender_access hash:/etc/postfix/access, permit_sasl_authenticated, permit_mynetworks, reject_unauth_destination
```


## Başlık (header) Kontrolleri Ekleme

header_checks dosyasını oluşturun ve düzenleyin:

```
sudo nano /etc/postfix/header_checks
```

```
/^Subject:.*Yusuf/ REJECT
```

## Gövde (body) Kontrolleri Ekleme

```
sudo nano /etc/postfix/body_checks
```

```
/Yusuf/ REJECT
```

##main.cf Dosyasını Güncelleme

main.cf dosyasını açın ve header_checks ve body_checks yapılandırmalarını ekleyin:

```
sudo nano /etc/postfix/main.cf
```

```
header_checks = regexp:/etc/postfix/header_checks
body_checks = regexp:/etc/postfix/body_checks
```

## Postfix'i Yeniden Başlatma

```
sudo systemctl restart postfix
```


## Test


Konuya göre bir engelleme işlemi yapmıştık. Gördüğünüz gibi başarılı şekilde çalıştı.

```
Jun 27 21:39:43 localhost postfix/cleanup[2712]: 906F418067A: reject: header Subject: Yusuf from mail-yb1-f174.google.com[209.85.219.174]; from=<testmaili34@gmail.com> to=<ugur@ugurcomptech.com.tr> proto=ESMTP helo=<mail-yb1-f174.google.com>: 5.7.1 message content rejected
```

Mail bazlı bir engelleme işlemi yapmıştık. Gördüğünüz gibi başarılı şekilde çalıştı.

```
Jun 27 21:41:17 localhost postfix/smtpd[3524]: NOQUEUE: reject: RCPT from mail-yb1-f176.google.com[209.85.219.176]: 554 5.7.1 <ugurcomptech@gmail.com>: Sender address rejected: Access denied; from=<ugurcomptech@gmail.com> to=<ugur@ugurcomptech.com.tr> proto=ESMTP helo=<mail-yb1-f176.google.com>
```


## NOT

Bu güvenlik yöntemi basit bir önlem alma yoludur. Daha karışık yapılara ihtiyacınız var ise SpamAssasin gibi servisler kullanabilirsiniz.















