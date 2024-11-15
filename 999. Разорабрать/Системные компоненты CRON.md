Службы, демоны и планировщик:
1) crontab(демон) - используется для периодического выполнения заданий в определённое время
2) Systemctl – управление запущенными службами (демонами)
3) Service – управление запущенными сервисами

## Systemctl:
systemctl status ssh.service
systemctl start ssh.service
systemctl stop ssh.service
systemctl restart ssh.service
systemctl enable ssh.service
systemctl disable ssh.service

![[Pasted image 20241004190709.png]]