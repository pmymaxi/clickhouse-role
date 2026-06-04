Clickhouse
=========

Данная роль выполняет:
- Устанавливает пакеты ClickHouse
- Настраивает репозиторий
- Применяет шаблоны конфигурации
- Создает базу данных и таблицу
- Открывает и проверяет HTTP и собственные порты (8123/9000)
- Ожидает, когда ClickHouse станет доступен

Role Variables
--------------


|          Variable           |              Description                 |     Default     |
| --------------------------- | ---------------------------------------- | --------------- |
|  `clickhouse_version`       | Версия пакета ClickHouse                 | `22.3.3.44`     |
|  `clickhouse_repo_baseurl`  | URL адрес репозитория                    |                 |
|  `clickhouse_repo_gpgcheck` | Включает проверку подписи пакетов        | `false`         |
|  `clickhouse_repo_enabled`  | включает/выключает репозиторий           | `true`          |
|  `clickhouse_config_dest`   | path до файла конфигурации на instance   |                 |
|  `clickhouse_nativ_port`    | TCP порт службы                          | `9000`          |
|  `clickhouse_nativ_host`    | IP eth службы                            | `127.0.0.1`     |
|  `clickhouse_nativ_delay`   | Время ожидания перед первой проверкой    | `2`             |
|                             | запуска TCP сокета на хосте              |                 |
|  `clickhouse_nativ_timeout` | Максимальное время ожидания              | `30`            |
|  `clickhouse_db_name`       | Имя создаваемой базы данных              | `nginx`         |
|  `clickhouse_table_name`    | Имя создаваемой таблицы в БД             | `my_access_logs`|
|---------------------------- | ---------------------------------------- | --------------- |
|                             |                                          |                 |


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: clickhouse }

License
-------

MIT

Author Information
------------------
Max Maxi is a devops student
