# Ride The Lightning

{% hint style="info" %}
Tijd: 15 minuten
{% endhint %}

De Lightnig Network CLI \(lncli\) is wel geinig, maar niet heel praktisch. Daarom gaan we Ride The Lightning \(RTL\) installeren. Een gebruiksvriendelijk voorkantje voor LND.

{% hint style="info" %}
Let op: dit onderdeel is afhankelijk van de [NodeJS installatie](https://node.bitdeal.nl/raspberry-pi/algemene-dependencies-installeren#nodejs). Je kunt niet verder als je NodeJS niet geinstalleerd hebt op de Raspberry Pi.
{% endhint %}

## Installatie

Tijd om de broncode van RTL op te halen.

```bash
git clone https://github.com/Ride-The-Lightning/RTL
```

Duik de code in.

```bash
cd RTL
```

Installeer nu RTL. Let op, dit kan zo'n 10 minuten duren.

```bash
npm install --only=prod
```

## Configuratie

Ook RTL moet ingesteld worden. Maak het configuratiebestand aan \(nog steeds in de /home/pi/RTL map\).

```bash
nano RTL-Config.json
```

Plak er dit in.

```javascript
{
    "multiPass": "password",
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
            "lnNode": "JOUW_ALIAS",
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

Vul bij `JOUW_ALIAS` de [alias](https://node.bitdeal.nl/lightning/configuratie) van jouw Lightning node in. Sla het op met `control + X` en bevestig met `Y`.

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

Sla het weer op met `control + X` en bevestig met `Y`.

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

Ga via je favoriete browser naar `het ip adres van je Pi:3000`. Bij mij is dat 192.168.1.6:3000. De RTL interface zal verschijnen en vragen om een wachtwoord. Het standaard wachtwoord is "password". Na de eerste keer inloggen mag je zelf een wachtwoord instellen. Mocht LND net opgestart zijn, zal RTL ook vragen om dát wachtwoord.

## Bereikbaar over Tor

Dit blijkt ietwat lastig te zijn. Meer info volgt.
