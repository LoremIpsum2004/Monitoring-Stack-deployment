# Monitoring Stack with Prometheus, Grafana, and Node Exporter

Это <ins>учебный проект</ins>, который позволяет быстро развернуть простую, работающую "из коробки" систему для мониторинга linux-машин. На мониторинг-хосте используется **Prometheus** + **Grafana**, на мониторируемых хостах - **Node Exporter**. Благодаря **Grafana Provision**, осуществляется первичная настройка *data sources* и *dashboard*. В качестве дашборда используется [Node Exporter Full](https://grafana.com/grafana/dashboards/1860-node-exporter-full/) (rev. 45).

<img width="1915" height="916" alt="изображение" src="https://github.com/user-attachments/assets/2fae8150-80d4-4df9-9488-f4de2d486cfe" />

---

## 📌 Структура проекта

```
├── deploy.yml
├── inventory.ini
├── README.md
└── roles/
    └── metrics_server_deploy/
        ├── defaults/
        ├── files/
        │   ├── 1860_rev45.json      # source: grafana.com/grafana/dashboards/
        │   ├── dashboards.yml
        │   ├── datasources.yml
        │   └── prometheus.yml
        ├── handlers/
        ├── tasks/
        │   ├── deploy_monitoring.yml
        │   ├── deploy_node_exporter.yml
        │   └── main.yml
        └── templates/
```

---

## 🛠 Предварительные требования

1. **Ansible** (на master-машине)
2. **Docker**
3. **Python**
4. Доступ по SSH к целевым машинам

---

## 🚀 Установка и запуск

### 1. Клонируйте репозиторий
```bash
git clone <...>
cd Kasper
```

### 2. Настройте инвентарь
Отредактируйте файл `inventory.ini` или используйте свой.
```ini
[monitoring_servers]
monitoring_server ansible_connection=local ansible_become_method=sudo ansible_become_user=root ansible_become_pass=toor

[node_exporter_servers]
node_exporter_server ansible_host=192.168.32.132 ansible_user=lessons ansible_ssh_pass=lessons ansible_become_method=sudo ansible_become_user=root ansible_become_pass=toor
```

### 3. Запустите плейбук
```bash
ansible-playbook -i inventory.ini deploy.yml
```

---

## 📊 Компоненты

| Компонент          | Описание                                                                 | Порт  |
|--------------------|-------------------------------------------------------------------------|-------|
| **Node Exporter**  | Собирает системные метрики (CPU, память, диск и т.д.)                  | 9100  |
| **Prometheus**     | Собирает и хранит метрики, опрашивает Node Exporter                     | 9090  |
| **Grafana**        | Визуализирует метрики из Prometheus                                     | 3000  |

---
## 🛠 TODO
- [ ] Избавиться от хардкодинга
- [ ] Реализовать переменные (имена файлов, сетевые порты...)
- [ ] Добавить функционал systemd сервиса (сейчас используются docker контейнеры)
