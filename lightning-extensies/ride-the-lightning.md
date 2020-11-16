# Ride The Lightning

{% hint style="info" %}
Tijd: 20 minuten
{% endhint %}

De Lightnig Network CLI \(lncli\) is wel geinig, maar niet heel praktisch. Daarom gaan we Ride The Lightning \(RTL\) installeren. Een gebruiksvriendelijk voorkantje voor LND.

## NodeJS

RTL is gebouwd met NodeJS. Net als dat Golang binnengehaald moest worden voor LND, gaan we nu NodeJS binnenhalen voor RTL. We willen de nieuwste versie, dus draaien eerst een scriptje.

```bash
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
```

Nadat het scriptje zijn werk gedaan heeft, kunnen we NodeJS installeren.

```bash
sudo apt install nodejs -y
```

Check of je de juiste versie hebt met:

```bash
node --version
# Verwachte output: v14.15.0
```

## Installatie

Tijd om de broncode van RTL op te halen.

```bash
git clone https://github.com/Ride-The-Lightning/RTL
```

Duik de code in.

```bash
cd RTL
```

Installeer nu RTL. Let op, dit kan zo'n 15 minuten duren.

```bash
npm install --only=prod
```

## Configuratie

Ook RTL moet ingesteld worden. Maak het configuratiebestand aan \(nog steeds in de /home/pi/RTL map\).

```bash
nano RTL-Config.json
```

Plak er dit in.

```bash
{
  "port": "3000",
  "defaultNodeIndex": 1,
  "SSO": {
    "rtlSSO": 0,
    "rtlCookiePath": "",
    "logoutRedirectLink": ""
  },
  "nodes": [
    {
      "index": 1,
      "lnNode": "Bitdeal.nl",
      "lnImplementation": "LND",
      "Authentication": {
        "macaroonPath": "/home/pi/.lnd/data/chain/bitcoin/mainnet",
        "configPath": "/home/pi/.lnd/lnd.conf"
      },
      "Settings": {
        "userPersona": "OPERATOR",
        "themeMode": "NIGHT",
        "themeColor": "TEAL",
        "bitcoindConfigPath": "/home/pi/.bitcoin/bitcoin.conf",
        "enableLogging": true,
        "fiatConversion": false,
        "lnServerUrl": "https://127.0.0.1:8080"
      }
    }
  ]
}
```

Sla het op met `Control + X` en bevestig met `Y`.

## Automatiseren

```bash
sudo nano /etc/systemd/system/rtl.service
```

Plak er dit in.

```bash
[Unit]
Description=Ride The Lightning Daemon
Wants=lnd.service
After=lnd.service

[Service]
User=pi
ExecStart=/usr/bin/node /home/pi/RTL/rtl.js
Restart=always
TimeoutSec=120
RestartSec=30

[Install]
WantedBy=multi-user.target
```

Sla het weer op met `Control + X` en bevestig met `Y`.

Het systeem moet op de hoogte gesteld worden van de nieuwe service en kan daarna gestart worden.

```bash
sudo systemctl enable rtl
```

```bash
sudo systemctl start rtl
```

Wil je zien of alles goed is opgestart, voer dan dit uit:

```bash
systemctl status rtl
```

Wil je een overzicht van de status over meerdere sessie, gebruik dan dit:

```bash
sudo journalctl -f -u rtl
```

## Gebruik

Ga via je favoriete browser naar [http://localhost:3000](http://localhost:3000). De RTL interface zal verschijnen en vragen om een wachtwoord. Mocht LND net opgestart zijn, zal RTL ook vragen om dat wachtwoord.

## Bereikbaar over Tor

Dit blijkt ietwat lastig te zijn. Meer info volgt.

