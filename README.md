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

## NOT

Bu güvenlik yöntemi basit bir önlem alma yoludur. Daha karışık yapılara ihtiyacınız var ise SpamAssasin gibi servisler kullanabilirsiniz.















