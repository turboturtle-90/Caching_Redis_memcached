# Домашнее задание к занятию "«Кэширование Redis/memcached» - `Смирнов Максим`

### Задание 1. Кеширование
Приведите примеры проблем, которые может решить кеширование.

`Примеры проблем решаемых кэшированием`

1.	Увеличение производительности. Классический пример – кэш процессора. Процессор сам по себе быстрый, но его взаимодействие с оперативной памятью уже гораздо медленнее. Если задача требует частого взаимодействия, то производительность будет определяться собственно его скоростью и соответственно будет низкой. Кэширование на уровне процессора это отличное решение. Таким образом процессор не передает весь объем данных на оперативную память и обратно и производительность больше не ограничивается этим узких каналом передачи.

2.	Снижение нагрузки на базы данных. При обращениях с тяжелыми запросами или просто большом количестве обращений нагрузка на базу данных растет. Растет соответственно время ответа, расход ресурсов. Поэтому целесообразно частые или тяжелые запросы выполнить и записать в кэш. При их получении они будут отдаваться оттуда, не нагружая базу данных. В результате происходит ускорение ответа и экономия ресурсов.

---


Задание 2. Memcached
Установите и запустите memcached.

Приведите скриншот systemctl status memcached, где будет видно, что memcached запущен.

Задание 3. Удаление по TTL в Memcached
Запишите в memcached несколько ключей с любыми именами и значениями, для которых выставлен TTL 5.

Приведите скриншот, на котором видно, что спустя 5 секунд ключи удалились из базы.

Задание 4. Запись данных в Redis
Запишите в Redis несколько ключей с любыми именами и значениями.`

Через redis-cli достаньте все записанные ключи и значения из базы, приведите скриншот этой операции.

`Балансировка на 4 уровне по roubdrobin`
![1-level4.jpg](https://github.com/turboturtle-90/Homework_clustering_and_load_balancing/blob/7705286e7aea06eb754ecf0cc6f2510aa3f7173f/1-level4.jpg)

`Текст конфига /etc/haproxy/haproxy.cfg в части балансировки roundronib на 4 уровне приведен ниже :`

```                                                         
listen web_tcp

        bind :1325
        balance roundrobin
        server s1 127.0.0.1:8888 check inter 3s
        server s2 127.0.0.1:9999 check inter 3s
```

### Задание 2

`Ссылка на .cfg`

https://github.com/turboturtle-90/Homework_clustering_and_load_balancing/blob/593b469a59cef8dcb0deedeb24426b0453281f6e/haproxy.cfg-assignment2 

`Балансировка на 7 уровне по roubdrobin с весовыми множителями и обработкой только при адресации к домену example.com`
![2-weighted-level7.jpg](https://github.com/turboturtle-90/Homework_clustering_and_load_balancing/blob/593b469a59cef8dcb0deedeb24426b0453281f6e/2-weighted-level7.jpg)


`Текст конфига /etc/haproxy/haproxy.cfg в части балансировки roundronib на 7 уровне приведен ниже :`

```                                                         
frontend example  # секция фронтенд
        mode http
        bind :8088
        #default_backend web_servers
        acl ACL_example.com hdr(host) -i example.com
        use_backend web_servers if ACL_example.com

backend web_servers    # секция бэкенд
        mode http
        balance roundrobin
        option httpchk
        http-check send meth GET uri /index.html
        server s1 127.0.0.1:8888 check weight 2
        server s2 127.0.0.1:9999 check weight 3
        server s3 127.0.0.1:9999 check weight 4
```
---
