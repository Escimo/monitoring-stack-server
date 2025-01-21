# Система мониторинга на базе Prometheus и Grafana

## Содержание
1. Обзор системы
2. Требования
3. Установка
4. Конфигурация
5. Основные компоненты
6. Обслуживание
7. Устранение неполадок

## 1. Обзор системы
Система мониторинга построена на базе следующих компонентов:
- Prometheus - сбор и хранение метрик
- Grafana - визуализация данных
- Alertmanager - управление оповещениями
Автоматическая настройка отслеживания экспортеров:
- Node Exporter - метрики системы
- cAdvisor - метрики контейнеров
- MySQL Exporter - метрики MySQL
- Blackbox Exporter - мониторинг HTTP эндпоинтов

Все зависимости устанавливаются автоматически.
Компоненты развернуты в Docker контейнерах (используется Docker-Compose)

## 2. Требования
- Ubuntu 24.04 ARM
- Ansible для развертывания

## 3. Установка

### Подготовка окружения
```bash
# Клонирование репозитория
git clone https://github.com/fossasoftware/containerized-monitoring-server.git
cd containerized-monitoring-server

# Настройка inventory
cp inventory/hosts.yml.example inventory/hosts.yml
# Отредактируйте inventory/hosts.yml и укажите ваш сервер
```

### Развертывание
```bash
# Проверка доступности хоста
ansible all -m ping

# Запуск плейбука
ansible-playbook site.yml
```

## 4. Конфигурация

### Prometheus
- Путь к конфигурации: `/opt/monitoring/prometheus/prometheus.yml`
- Период хранения данных: 30 дней
- Интервал сбора метрик: 15 секунд

### Alertmanager
- Путь к конфигурации: `/opt/monitoring/alertmanager/alertmanager.yml`
- Настроена отправка уведомлений в Telegram
- Группировка алертов по: alertname, job, severity

### Grafana
- URL доступа: http://your-server:3000
- Логин по умолчанию: admin
- Пароль по умолчанию: admin
- Предустановленные дашборды:
  - Node Exporter Dashboard
  - MySQL Overview
  - Blackbox Monitoring

## 5. Основные компоненты

### Node Exporter
- Порт: 9100
- Метрики: CPU, память, диск, сеть
- Алерты:
  - HighCPUUsage (>85%)
  - CriticalCPUUsage (>95%)
  - HighMemoryUsage (>85%)
  - HighDiskUsage (>85%)

### MySQL Exporter
- Порт: 9104
- Основные метрики:
  - Количество соединений
  - Скорость выполнения запросов
  - Статус репликации
  - Размер базы данных

### Blackbox Exporter
- Проверка доступности HTTP эндпоинтов
- Мониторинг SSL сертификатов
- Время отклика сервисов

## 6. Обслуживание

### Обновление компонентов
```bash
cd /opt/monitoring
docker-compose pull
docker-compose up -d
```

### Проверка состояния
```bash
docker-compose ps
docker-compose logs -f [service-name]
```

### Резервное копирование
В текущей конфигурации резервное копирование не настроено.

## 7. Устранение неполадок

### Проверка логов
```bash
# Логи Prometheus
docker-compose logs prometheus

# Логи Alertmanager
docker-compose logs alertmanager

# Логи Grafana
docker-compose logs grafana
```

### Часто встречающиеся проблемы

1. Prometheus не собирает метрики:
   - Проверьте доступность экспортеров
   - Проверьте конфигурацию prometheus.yml
   - Проверьте файрволл

2. Не приходят уведомления:
   - Проверьте конфигурацию Alertmanager
   - Проверьте токен и chat_id Telegram
   - Проверьте логи Alertmanager

3. Grafana не отображает данные:
   - Проверьте подключение к Prometheus
   - Проверьте правильность запросов в дашбордах
   - Проверьте права доступа

### Полезные команды
```bash
# Перезапуск всех сервисов
docker-compose restart

# Проверка конфигурации Prometheus
docker-compose exec prometheus promtool check config /etc/prometheus/prometheus.yml

# Проверка правил алертов
docker-compose exec prometheus promtool check rules /etc/prometheus/alerts.yml
```
