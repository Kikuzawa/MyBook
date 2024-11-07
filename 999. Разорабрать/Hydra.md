**Hydra** — инструмент для проведения брутфорс-атак на различные протоколы. Поддерживает FTP, SSH, HTTP(S), SMB, RDP и другие.

Команда:
hydra -l user -P password_list.txt ftp://192.168.1.100 (перебор паролей для FTP).

**Устанавливается при помощи в Debian**: sudo apt-get install hydra

- Запуск атаки на SSH:
	hydra -l <username> -P <password_list> ssh://<target_ip>
	hydra -l root -P /path/to/passwords.txt ssh://192.168.1.100
- Запуск атаки на FTP:
	hydra -l <username> -P <password_list> ftp://<target_ip>
	hydra -l admin -P /path/to/passwords.txt ftp://192.168.1.100 -L
 - Использовать файл со списком логинов.
	hydra -L /path/to/usernames.txt -P /path/to/passwords.txt ssh://192.168.1.100


![[Pasted image 20241004201715.png]]

