# Firewall

Ook voor LND moeten de nodige ports opengezet worden. Dit zijn **8080** voor de REST API, **9735** voor connecties met andere Lightning nodes en **10009** voor de RPC API.

```bash
sudo ufw allow 8080
sudo ufw allow 9735
sudo ufw allow 10009
```

Het is belangrijk om port 9735 ook open te zetten op je router en verkeer door te sturen naar je Pi. Voor de andere twee ports is dat niet per se nodig, tenzij je van buitenaf met je node wil communiceren.

