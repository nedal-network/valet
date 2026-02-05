1️⃣ Kapcsold be a systemd-resolved-et
```bash
sudo systemctl enable --now systemd-resolved
```

2️⃣ Állítsd vissza a resolv.conf symlinket
```bash
sudo rm -f /etc/resolv.conf
sudo ln -s /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
```

Ellenőrzés:
```bash
ls -l /etc/resolv.conf
```

Ezt kell látnod:
```bash
/etc/resolv.conf -> /run/systemd/resolve/stub-resolv.conf
```
3️⃣ Mondd meg az NetworkManager-nek, hogy resolved-et használjon

/etc/NetworkManager/conf.d/00-use-resolved.conf
```ini
[main]
dns=systemd-resolved
```
```bash
sudo systemctl restart NetworkManager
```

4️⃣ Lokális dnsmasq (rendszerszintű!)
```bash
sudo dnf install dnsmasq
sudo systemctl enable --now dnsmasq
```

/etc/dnsmasq.d/test.conf
```ini
address=/.test/127.0.0.1
```

dnsmasq figyeljen localhoston:
```bash
sudo ss -lnptu | grep :53
```

5️⃣ systemd-resolved routing domain

/etc/systemd/resolved.conf
```ini
[Resolve]
DNS=127.0.0.1
Domains=~test
```
```bash
sudo systemctl restart systemd-resolved
```

6️⃣ Ellenőrzés
```bash
resolvectl status
```
Jó állapot:
```
Current DNS Server: 127.0.0.1
DNS Domain: ~test
```
