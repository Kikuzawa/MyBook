# История
Uniplexed Information and Computing Service (UNICS)  UNIX
Multiplexed Information and Computer Services (MULTICS)

UNIX - семейство переносимых, многозадачных и многопользовательских ОС, которые основаны на AT&T Unix

Первые версии Unix были написаны на ассемблере.
В 69 году Томпсон и Ритчи разработали язык Би.
В 72 году была выпущена вторая редакция Unix, переписанная на языке Би.
В 73 годы на основе Би был разработан компилируемый язык, получивший название С.
В 80 AT&T запретили реализовывать UNIX AT&T – передали исходные коды Berkley

![[Pasted image 20241006093953.png]]
![[Pasted image 20241006093957.png]]

# Развитие UNIX-BASED

[[GNU]] (GNU’s NGNUot UNIX ) — свободная Unix-like операционная система, разрабатываемая Проектом GNU.

![[Pasted image 20241006094101.png]]

# Особенности Unix-Like

**Монолитное ядро** – Unix, Linux Командная строка в пространстве запускаемого процесса

Микроядро – OSX Linux использует монолитное ядро, которое потребляет больше ресурсов, в то время как Windows использует гибридное ядро, которое занимает меньше места, но при этом снижает эффективность работы системы, в отличие от Linux. В Microsoft Windows файлы хранятся в каталогах/папках на разных дисках. В Linux файлы и папки, начиная с корневого каталога, упорядочены в виде древовидной структуры, разветвляясь на различные подкаталоги. Inter Process Communication (IPC)

![[Pasted image 20241006094138.png]]

Модули ядра и загрузка подгружаемых модулей.

## Философия Unix
1. красиво — небольшое
2. каждая программа делает что-то одно, но хорошо
3. храните данные в простых текстовых файлах
4. пишите программы, которые бы работали вместе
5. правило ясности: ясность лучше заумности

![[Pasted image 20241006094222.png]]

# Загрузка UNIX-LIKE

Существует 6 шагов загрузки:
level 0 - завершает работу системы
level 1 - однопользовательский режим работы. Чаще всего используется в целях обслуживания и выполнения других административных задач.
level 2 - многопользовательский режим работы без демонов.
level 3 - многопользовательский режим с поддержкой сети, но без графического интерфейса.
level 4 - не используется.
level 5 - запускается графический интерфейс.
level 6 - перезагружает систему.

![[Pasted image 20241006094306.png]]


Используются [[CHMOD]], [[CHOWN]], [[Cron]], [[IpTables]]


# Пароли | Пользователи

![[Pasted image 20241006094652.png]]


SUID bit – позволяет выполнение программы с правами хозяина файла.

Это – ключевой механизм повышения прав в Unix-системах. Особенности SUID-программ в стандартных конфигурациях Linux:
- Работают с полномочиями пользователя root
- Используются для выполнения безопасных привилегированных операций.
- Используются для штатной смены идентификаторов пользователя: su, sudo, pkexec 
- Программы учитывают идентификатор запустившего их пользователя и различные файлы конфигурации

SUID можно установить на копию /bin/sh для получения возможности выполнять команды с правами root без использования su/sudo. При запуске шелла нужно использовать опцию –p, иначе effective uid будет сброшен.

![[Pasted image 20241006094742.png]]

/etc/sudoers – файл с настройками sudo Редактирование в visudo

![[Pasted image 20241006094757.png]]

Список учетных записей пользователей хранятся в: /etc/passwd

![[Pasted image 20241006094813.png]]

/etc/shadow – хэши паролей пользователей

`smithj:Ep6mckrOLChF.:10063:0:99999:7:::`

Как и в файле passwd, каждое поле в файле shadow отделяется двоеточием:
1. Username, до 8 символов. Совпадает с username в файле /etc/passwd.
2. Пароль, 13 символов (зашифрованный). Пустая запись (то есть, ::) показывает, что для входа пароль не нужен (обычно идея плохая), и запись ``*''
(то есть, :*:) показывает, что вход заблокирован
3. Количество дней (с 1 января 1970), когда пароль был сменен в последний раз.
4. Число дней до смены пароля (0 показывает, что он может быть сменен всегда). 
5. Число дней, после которых пароль должен быть сменен (99999 показывает, что пользователь может не менять пароль фактически никогда).
6. Число дней, в течение которых пользователь получает предупреждения о необходимости пароль сменить (7 для полной недели).
7. Число дней после окончания действия пароля, когда еще можно работать. Если пароль не сменить, после данного срока он выдохнется, и аккаунт будет заблокирован.
8. Число дней, начиная с 1 января 1970, после которых пароль будет заблокирован.
9. Зарезервировано для возможного будущего использования.

Одним из компонентов может быть [[Ядро OSX]]

[[Команды Unix]]