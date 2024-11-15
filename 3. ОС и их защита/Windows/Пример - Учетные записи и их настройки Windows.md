### 1. Создать учетную запись с административными правами
```powershell
# Создать учетную запись с административными правами
$adminUsername = "AdminDemo"
$adminPassword = "P@ssw0rd123!" | ConvertTo-SecureString -AsPlainText -Force
New-LocalUser -Name $adminUsername -Password $adminPassword -FullName "Admin Demo" -Description "Administrator account" -PasswordNeverExpires:$true
Add-LocalGroupMember -Group "Administrators" -Member $adminUsername
```

![[Pasted image 20241006120135.png]]

### 2. Создание уникального пароля и записать в хранилище
### 2.1 Генерация уникального пароля
```powershell
$adminUsername = "AdminDemo"
$adminPassword = "SimpleP@ssw0rd"
```
### 2.2. Преобразование пароля в безопасный формат
```powershell
$securePassword = ConvertTo-SecureString $adminPassword -AsPlainText -Force
```
### 2.3. Обновление пароля существующего пользователя
```powershell
Set-LocalUser -Name $adminUsername -Password $securePassword
```
### 2.4. Сохранение учетных данных в хранилище
```powershell
$credentialTarget = "AdminDemo_Credentials" # Уникальное имя для хранения
$null = cmdkey /add:$credentialTarget /user:$adminUsername /pass:$adminPassword
```
### Информация для пользователя
```powershell
Write-Host "Пароль учетной записи '$adminUsername' обновлен."
Write-Host "Пароль: $adminPassword"
```

### Проверка сохранённых учетных данных
```powershell
cmdkey /list
```

![[Pasted image 20241006120158.png]]

### Проверка через графический интерфейс

Если вы предпочитаете использовать графический интерфейс:

1. **Откройте Панель управления**:
   - Нажмите `Win + R`, введите `control`, и нажмите Enter.

2. **Перейдите в раздел "Учетные данные"**:
   - В Панели управления выберите **"Учетные записи пользователей"**.
   - Затем выберите **"Управление учетными данными"**.

3. **Просмотрите сохранённые учетные данные**:
   - В разделе **"Учетные данные Windows"** вы увидите список всех сохранённых учетных данных. Ищите запись с именем `AdminDemo_Credentials` для подтверждения сохранения.


![[Pasted image 20241006120254.png]]

### 3. Отключить учетную запись «Гость»
```powershell
# Отключить учетную запись «Гость»
Set-LocalUser -Name "Guest" -AccountDisabled $true
Get-LocalUser -Name "Guest" | Select-Object Name, Enabled
```

![[Pasted image 20241006120308.png]]

### 4. Включить контроль учетных записей (UAC)
Для настройки уровня уведомлений UAC можно изменить реестр через PowerShell:
```powershell
# Включить UAC и установить уровень уведомления
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name "EnableLUA" -Value 1
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name "ConsentPromptBehaviorAdmin" -Value 5
```

![[Pasted image 20241006120322.png]]

### Скрипт настройки параметров безопасности пароля:

```powershell
# Скрипт для настройки параметров безопасности на Windows Server 2019

# 1. Настройка политики паролей
function Configure-PasswordPolicy {
    # Вести журнал паролей - 5 последних паролей
    secedit /export /cfg C:\work\secpol.cfg
    (Get-Content C:\work\secpol.cfg).replace('PasswordHistorySize = 24', 'PasswordHistorySize = 5') | Set-Content C:\work\secpol.cfg
    secedit /configure /db secedit.sdb /cfg C:\work\secpol.cfg /quiet

    # Максимальный срок действия паролей - 90 дней
    net accounts /maxpwage:90

    # Минимальная длина пароля - 8 символов
    net accounts /minpwlen:8

    # Пароль должен отвечать требованиям сложности - Включен
    secedit /export /cfg C:\work\secpol.cfg
    (Get-Content C:\work\secpol.cfg).replace('PasswordComplexity = 0', 'PasswordComplexity = 1') | Set-Content C:\work\secpol.cfg
    secedit /configure /db secedit.sdb /cfg C:\work\secpol.cfg /quiet

    Write-Host "Политика паролей настроена"
}

# 2. Настройка политики блокировки учетной записи
function Configure-LockoutPolicy {
    # Пороговое значение блокировки - 5 ошибок
    net accounts /lockoutthreshold:5

    # Продолжительность блокировки УЗ - 15 мин
    net accounts /lockoutduration:15

    # Разрешить блокировку УЗ администратора - Включено
    secedit /export /cfg C:\work\secpol.cfg
    (Get-Content C:\work\secpol.cfg).replace('LockoutBadCount = 0', 'LockoutBadCount = 1') | Set-Content C:\work\secpol.cfg
    secedit /configure /db secedit.sdb /cfg C:\work\secpol.cfg /quiet

    Write-Host "Политика блокировки настроена"
}

# Основной скрипт

# 1. Настроить политику паролей
Configure-PasswordPolicy

# 2. Настроить политику блокировки учетной записи
Configure-LockoutPolicy

Write-Host "Все параметры безопасности настроены успешно"
```

### Описание настроек:

1. **Политика паролей**:
   - Вести журнал паролей (5 последних паролей) — это параметр, который ограничивает повторное использование старых паролей.
   - Максимальный срок действия паролей — 90 дней.
   - Минимальная длина пароля — 8 символов.
   - Требования к сложности пароля включены.

2. **Политика блокировки учетной записи**:
   - Порог блокировки после 5 неудачных попыток входа.
   - Продолжительность блокировки — 15 минут.
   - Блокировка учетной записи администратора включена.

### Как запустить скрипт:

1. Сохраните скрипт в файл, например, `security_policy.ps1`.
2. Запустите PowerShell с правами администратора.
3. Выполните команду для запуска скрипта:
   ```powershell
   .\security_policy.ps1
```

![[Pasted image 20241006120400.png]]

### Настройка логирования


![[Pasted image 20241006120411.png]]
