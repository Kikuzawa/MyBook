1. установить набор правил (поставляется Emerging Threats Open):

```
sudo suricata-update
```

2. Обновляем источники правил

```
sudo suricata-update update-sources
```

3. При необходимости можно включить доступные бесплатные источники:

```
sudo suricata-update enable-source ptresearch/attackdetection
sudo suricata-update enable-source oisf/trafficid
sudo suricata-update enable-source sslbl/ssl-fp-blacklist
```

4. После этого необходимо еще раз обновить правила
