# Ubuntu
. Установка SSH
1. sudo apt-get install ssh
2. Запуск сервиса sudo systemctl start ssh
3. Добавление сервиса в автозагрузку sudo systemctl enable ssh
4. Проверить статус работы сервиса sudo systemctl status sshd
5. Создание резервной копии конфига sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.factory- defaults
6. Копирование открытых ключей (ИТ и ИБ) c ПК администраторов на ПК пользователя (ключи предварительно необходимо скачать с данной страницы - `SSH-ключи (ИБ и ИТ)) ssh-copy-id -i <path-to-key> <admin account>@<new-pc-ip> пример: ssh-copy-id -i path/to/name.pub admin- msk@10.1.1.47`
7. Команда для редактирования конфига sudo nano /etc/ssh/sshd_config 
8. Настроенный файл конфигурации - (отправлю в чат)можно скопировать напрямую на ПК пользователя или скопировать-вставить настройки в конфигурационный файл 
9. Перезагрузить сервис для применение изменений sudo systemctl restart ssh