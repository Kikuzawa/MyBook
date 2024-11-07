### 1. Установка SSH-сервера

Если SSH-сервер еще не установлен, выполните команду:

```bash
sudo apt update
sudo apt install openssh-server
```

# 2. Настройка конфигурации SSH

Откройте файл конфигурации SSH:

```bash
sudo nano /etc/ssh/sshd_config
```

## Рекомендуемые изменения:

- **Измените порт** (по умолчанию 22) на более высокий, чтобы уменьшить количество автоматических атак:
    
    ```plaintext
    Port 2222
    ```
    
- **Отключите вход по паролю** и используйте только ключи:
    
    ```plaintext
    PasswordAuthentication no
    ```
    
- **Отключите вход для пользователя root**:
    
    ```plaintext
    PermitRootLogin no
    ```
    
- **Настройте доступ для определенных пользователей или групп**:
    
    ```plaintext
    AllowUsers yourusername
    ```
    
- **Включите аутентификацию по ключу**:
    
    ```plaintext
    PubkeyAuthentication yes
    ```
    
- **Отключите анонимный доступ**:
    
    ```plaintext
    PermitEmptyPasswords no
    ```
    

## Сохраните изменения и закройте редактор.

# 3. Генерация SSH-ключей

На клиентской машине сгенерируйте пару ключей, если у вас ее еще нет:

```bash
ssh-keygen -t rsa -b 4096
```

Скопируйте публичный ключ на сервер:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub yourusername@yourserver_ip -p 2222
```

# 4. Перезапуск SSH-сервера

После внесения изменений в конфигурацию перезапустите SSH-сервер:

```bash
sudo systemctl restart ssh
```

# 5. Настройка брандмауэра

Убедитесь, что брандмауэр настроен правильно. Например, если вы изменили порт на 2222:

```bash
sudo ufw allow 2222/tcp
sudo ufw enable
```

# 6. Дополнительные меры безопасности

- **Установите Fail2Ban** для защиты от брутфорс-атак:
    
    ```bash
    sudo apt install fail2ban
    ```
    
- **Настройте двухфакторную аутентификацию** (например, с помощью Google Authenticator).
    
- **Регулярно обновляйте систему**:
    
    ```bash
    sudo apt update && sudo apt upgrade
    ```
    

# 7. Проверка состояния SSH

Убедитесь, что SSH работает и настроен правильно:

```bash
sudo systemctl status ssh
```