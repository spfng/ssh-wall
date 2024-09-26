# ssh wall

Description:

Набор инструментов для создания децентрализованной p2p-сети между хостами с использованием bash, ssh и nginx (или любого другого веб-сервера).

Главная идея заключается в обмене приватными ssh-ключами между хостами с использованием секретной фразы, по которой будет доступен ключ для скачивания, после чего хосты получают доступ друг к другу и возможность обмена данными.

Разместив это на каждом хосте -- каждый хост будет предоставлять доступ к ssh-ключу, доступ к которому может получить лишь администратор сети, зная секретную фразу.

Каждый хост может получать ssh-ключи других хостов в сети.

Далее, каждый хост, при подключении к новой сети, будет загружать ключи с других хостов оттуда.

И далее, все хосты друг с другом могут синхронизировать данные, получаемые из других сетей.

Получается так, что хост A знает ключ хоста B, а хост B подключен к сети, где есть хост C.

Хост A синхронизирует данные с хостом B и теперь хост A знает ключ хоста C, при этом не имея к нему прямого доступа из сети.

Можно оставить записку на хосте A, чтобы выполнить команду на хосте C, и когда хост B синхронизирует данные с хостом A, а далее хост C получит данные от хоста B, то хост C увидит записку, которая предназначена ему и сможет выполнить запрошенную команду хостом A.

Таким образом мы получаем реализацию децентрализованной p2p-сети, где каждый хост это и master, и slave одновременно, где все участники равноценны между собой и могут сообщаться друг с другом, будучи при этом даже не подключены в одну локальную сеть.

Quick Start:

`echo 192.168.0.0/24 | ./ssh-wall-announce.sh | ./ssh-wall-subscribe.sh | ./ssh-wall-command.sh @./examples/hello_world.sh`

1) разложит подсеть на IP-адреса

2) просканирует каждый IP-адрес на открытый 80 порт

3) попробует скачать приватный ssh-ключ с каждого хоста

4) зайдёт на каждый подконтрольный хост и выполнит заданную команду или скрипт (`./examples/hello_world.sh`)

How-To Use:

`echo 192.168.0.0/24 | ./ssh-wall-announce.sh` -- раскладывает подсети на IP-адреса и сканирует на наличие открытого 80 порта.

`echo 192.168.0.1 | ./ssh-wall-subscribe.sh` -- скачивает приватный ssh-ключ с подконтрольго хоста.

`echo 192.168.0.1 | ./ssh-wall-command.sh` -- выполняет команду или скрипт на удалённом хосте.

Examples:

`./hello_world.sh` -- каждый хост напишет свои IP-адреса (`hostname -I`) и `Hello world!`.

---
ssh wall
