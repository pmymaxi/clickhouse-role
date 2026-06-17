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


|          Variable           |              Description                                                |     Default     |
| --------------------------- | ----------------------------------------------------------------------- | --------------- |
|  `clickhouse_version`       | Версия пакета ClickHouse                                                | `22.3.3.44`     |
|  `clickhouse_repo_baseurl`  | URL адрес репозитория                                                   |                 |
|  `clickhouse_repo_gpgcheck` | Включает проверку подписи пакетов                                       | `false`         |
|  `clickhouse_repo_enabled`  | включает/выключает репозиторий                                          | `true`          |
|  `clickhouse_config_dest`   | path до файла конфигурации на instance                                  |                 |
|  `clickhouse_nativ_port`    | TCP порт службы                                                         | `9000`          |
|  `clickhouse_nativ_host`    | IP eth службы                                                           | `127.0.0.1`     |
|  `clickhouse_nativ_delay`   | Время ожидания перед первой проверкой запуска TCP сокета на хосте       | `2`             |
|  `clickhouse_nativ_timeout` | Максимальное время ожидания                                             | `30`            |
|  `clickhouse_db_name`       | Имя создаваемой базы данных                                             | `nginx`         |
|  `clickhouse_table_name`    | Имя создаваемой таблицы в БД                                            | `my_access_logs`|
|  `clickhouse_use_install`   | Параметр определяющий установку clickhouse через пакетный менеджер yum  | `false`         |
|  `clickhouse_start_systemd` | Параметр запуска с использованием systemd                               | `false`         |


Molecule
-----------
Molecule выполняет deploy тестирование full stack (ClickHouse - Vector+Nginx - Lighthouse) с использованием Docker container.

Выполняются следующие сценария:
scenario:
  name: log_stack
  test_sequence:
    - dependency
    - cleanup
    - destroy
    - syntax
    - create
    - prepare
    - converge
    - idempotence
    - verify
    - idempotence
    - cleanup
    - destroy

Разворачивается следующая инфраструктура:

```text
                    +----------------+
                    |   Lighthouse   |
                    |  Web UI        |
                    +--------+-------+
                             |
                             |
                             v
+-------------+      +---------------+
|   Vector    +----->+  ClickHouse   |
| Log Agent   |      | Logs Storage  |
+------+------+      +-------+-------+
       ^
       |
       |
+------+------+
|    Nginx    |
| Access Logs |
+-------------+
```
Состав инфраструктуры:

  - name: clickhouse-01
    image: clickhouse/clickhouse-server:latest
    description: Clickhouse-server

  - name: web-01
    image: oraclelinux:9
    description: Vector + Nginx

  - name: lighthouse-01
    image: ubuntu:latest
    description: lighthouse

С описанием проекта можно ознакомится в проекте [hw-ansi-04](https://github.com/pmymaxi/hw-ansi-04)

Перед началом запуска ```molecule test```
--------------------------------
При формировании конфигурационного файла Vector через шаблон vector.yaml.j2, используется переменные из переменного окружения.
```yml
user: ${CLICKHOUSE_USER}
password: ${CLICKHOUSE_PASSWORD}
```
Environment определяется и меняется в файле ```molecule.yml``
```yml
env:
  CLICKHOUSE_USER: admin
  CLICKHOUSE_PASSWORD: admin
```
Переменные передаются в переменное окружение docker container , а также для начального определения авторизационных данных в clickhouse-server.

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: clickhouse }

License
-------

MIT

Author Information
------------------
Max Maxi is a devops student
