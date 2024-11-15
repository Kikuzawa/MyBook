```
iptables -t mangle -I PREROUTING -p tcp -m tcp --dport 80 -m mark ! --mark 0x1/0x1 -j NFQUEUE --queue-num 0
```