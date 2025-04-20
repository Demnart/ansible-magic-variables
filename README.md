# 📘 Ansible Magic Variables & Common Facts

## 🧠 Системные факты (`ansible_`)
| Переменная                        | Описание                              | Пример                      |
|----------------------------------|----------------------------------------|-----------------------------|
| `ansible_hostname`               | Имя хоста                              | `worker1`                   |
| `ansible_fqdn`                   | Полное доменное имя                    | `worker1.localdomain`       |
| `ansible_user_id`                | Имя текущего пользователя              | `root`                      |
| `ansible_distribution`           | Название дистрибутива                  | `Ubuntu`                    |
| `ansible_distribution_version`   | Версия ОС                              | `24.04`                     |
| `ansible_distribution_release`   | Кодовое имя релиза                     | `noble`                     |
| `ansible_architecture`           | Архитектура                            | `x86_64`                    |
| `ansible_os_family`              | Тип ОС                                 | `Debian`, `RedHat`, ...     |
| `ansible_processor_cores`        | Кол-во ядер на сокет                   | `4`                         |
| `ansible_processor_vcpus`        | Кол-во логических ядер                 | `8`                         |
| `ansible_memtotal_mb`            | Всего RAM в MB                         | `15894`                     |
| `ansible_default_ipv4.address`   | Основной IP                            | `192.168.122.100`           |
| `ansible_selinux.status`         | SELinux статус                         | `disabled` / `enforcing`    |

## 🔁 Переменные цикла (`loop`)
| Переменная            | Описание                                  |
|----------------------|--------------------------------------------|
| `item`               | Текущий элемент                            |
| `ansible_loop.index` | Индекс итерации (с 1)                      |
| `ansible_loop.last`  | `true`, если последний элемент             |

## 🧩 Инвентарь и группы
| Переменная                       | Описание                              |
|----------------------------------|----------------------------------------|
| `inventory_hostname`            | Имя хоста из inventory                 |
| `inventory_hostname_short`      | Короткое имя                          |
| `group_names`                   | Список групп хоста                    |
| `groups['mygroup']`             | Все хосты в группе `mygroup`          |
| `hostvars[inventory_hostname]`  | Все переменные хоста                  |
| `hostvars['control']['ansible_default_ipv4']['address']` | IP другого хоста |

## ⚙️ Контекст и окружение
| Переменная               | Описание                                 |
|--------------------------|-------------------------------------------|
| `ansible_play_hosts`     | Все хосты в текущем `play`                |
| `ansible_playbook_dir`   | Путь до playbook                          |
| `ansible_role_name`      | Имя роли (внутри роли)                   |
| `ansible_env.HOME`       | Переменные окружения (`$HOME`, `$PATH`)  |

## 📦 `register` и результаты задач
| Переменная         | Описание                                     |
|--------------------|-----------------------------------------------|
| `result.stdout`    | Стандартный вывод команды                     |
| `result.stderr`    | Ошибки                                        |
| `result.rc`        | Код возврата                                  |
| `result.changed`   | Были ли изменения (`true`/`false`)            |

## 💡 Примеры использования
```yaml
- debug:
    msg: "OS: {{ ansible_distribution }} {{ ansible_distribution_release }}"

- name: Только для Ubuntu 24.04
  when: ansible_distribution == "Ubuntu" and ansible_distribution_release == "noble"

- name: Проверка архитектуры
  when: ansible_architecture == "x86_64"

- name: IP текущего хоста
  debug:
    msg: "My IP is {{ ansible_default_ipv4.address }}"

- name: IP другого хоста
  debug:
    msg: "{{ hostvars['control']['ansible_default_ipv4']['address'] }}"
