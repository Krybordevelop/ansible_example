# PostgreSQL HA: Patroni + etcd + HAProxy

Проект разворачивает кластер из трёх серверов:

- Patroni управляет PostgreSQL, streaming replication и автоматическим failover.
- etcd хранит состояние кластера и выполняет роль DCS для Patroni.
- HAProxy на каждом узле направляет запись на текущий primary, а чтение — на доступные replicas.

## Перед запуском

1. В [host.ini](./host.ini) замените IP-адреса и пользователя SSH.
2. В [group_vars/all.yml](./group_vars/all.yml) задайте уникальные пароли. Для production лучше вынести их в Ansible Vault.
3. Убедитесь, что все три сервера используют Ubuntu 22.04+ (или совместимый Debian-based дистрибутив), имеют SSH-доступ и находятся в одной сети.
4. Откройте между узлами TCP-порты `2379-2380`, `5432`, `5433` и `8008`.

## Запуск

```bash
ansible-playbook -i host.ini deploy.yml
```

После развёртывания:

- `psql -h <любой-узел> -p 5000` — запись в текущий primary через HAProxy;
- `psql -h <любой-узел> -p 5001` — чтение с доступных replicas через HAProxy;
- `psql -h <узел> -p 5432` — прямое подключение к PostgreSQL на узле;
- `curl http://<узел>:8008/cluster` — состояние Patroni;
- `etcdctl --endpoints=http://<узел>:2379 endpoint health` — состояние etcd.

Для приложения рекомендуется использовать адрес балансировщика или VIP перед HAProxy. В текущем варианте HAProxy слушает локально на каждом из трёх узлов; отдельный floating IP можно добавить через keepalived. Не забудьте открыть TCP-порты `5000-5001` в firewall.

## Важно

Первый запуск создаёт кластер с текущим набором узлов. Не меняйте `postgres_cluster_name` и не удаляйте данные etcd/PostgreSQL без понимания последствий. Пароли в примере демонстрационные и обязательны к замене.
