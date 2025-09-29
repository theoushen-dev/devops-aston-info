Задание 3

# ТЗ

- Написать скрипт, который удовлетворяет следующим условиям:
- создает папку /opt/app
- создает файл /opt/app/log.txt
- каждые 17 секунд записывает в файл /opt/app/log.txt случайную строку 
- длиной до 20 символов (каждая итерация - с новой строки)


[*optional] - добавить скрипт в "автозагрузку" - после перезапуска перационной системы скрипт должен стартовать автоматически

[*optional] - настроить ротацию log-файла с помощью logrotate


• Пошаговую инструкцию по выполнению задания сохранить в файле task3.md




# Скрипт: 
``` sudo nano opt/app/logger.sh ```
```
#!/bin/bash


APP_DIR="/opt/app"
LOG_FILE="$APP_DIR/log.txt"

mkdir -p "$APP_DIR"

touch "$LOG_FILE"

while true; do
    
    LENGTH=$((RANDOM % 20 + 1))

    RANDOM_STRING=$(head /dev/urandom | tr -dc A-Za-z0-9 | head -c $LENGTH)
    echo "$RANDOM_STRING" >> "$LOG_FILE"

    sleep 17
done
```

# Пояснения
Скрипт создает директорию APP_DIR, в ней файл LOG_FILE с длинной RANDOM_STRING.

# Запуск

1. Пермишен на исполнение

    ``` sudo chmod +x {name_skript} ``` 

# Запуск в ручную:
    
    ``` sudo ./logger.sh ```

# Автозапуск 

1. Создайте файл сервиса.

    ``` sudo nano /etc/systemd/system/logger.service ```

2. Добавьте в файл конфигурацию сервиса.
Вставьте следующий текст в редактор:

    ``` 
    [Unit]
    Description=My Custom Logger Service
    After=network.target

    [Service]
    ExecStart=/bin/bash /opt/app/logger.sh
    Restart=always
    User=root

    [Install]
    WantedBy=multi-user.target
    ```

3. Сохраните и закройте файл.

. Активируйте и запустите сервис:


- Перезагрузить конфигурацию systemd
   
    ` sudo systemctl daemon-reload`

- Включить автозапуск сервиса

    ```sudo systemctl enable logger.service```
`
- Запустить сервис немедленно
    
    ```sudo systemctl start logger.service```

- Проверьте статус сервиса.

    ``` sudo systemctl status logger.service ``` 
