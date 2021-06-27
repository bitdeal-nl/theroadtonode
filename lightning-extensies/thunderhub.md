# Thunderhub

{% hint style="info" %}
Tijd: 20 minuten
{% endhint %}

Thunderhub is net als Ride The Lightning een beheertool voor jouw node. Bezoek [de website](https://www.thunderhub.io/) om alle features te ontdekken.

### Benodigdheden

* NPM of [Yarn](https://docs.theroadtonode.com/raspberry-pi/algemene-dependencies-installeren#yarn)
* [nodejs](https://docs.theroadtonode.com/raspberry-pi/algemene-dependencies-installeren#nodejs)

## Broncode

Download de broncode van Thunderhub.

```bash
git clone https://github.com/apotdevin/thunderhub.git
```

Ga naar de code.

```bash
cd thunderhub
```

Pak de laatste versie/tag/release.

```text
git checkout v0.12.20
```

Haal alle benodigde software dependencies binnen.

```bash
npm install
```

Of als je liever Yarn gebruikt, voer dan `yarn`uit.

## Configuratie

Maak het bestand `.env.local`:

```bash
nano .env.local
```

Plak het volgende erin:

```bash
# -----------
# Interface Configs
# -----------
THEME='dark'
CURRENCY='sat'

# -----------
# Account Configs
# -----------
ACCOUNT_CONFIG_PATH='/home/pi/.thunderhub/config.yaml'
```

Sla het op met `Ctrl + X` en bevestig met `Y`. Dit is een minimale setup qua configuratie. Meer parameters die je kunt gebruiken vind je in het `.env` bestand.

Nu gaan we terug naar je home directory en maken daar een map aan met de naam .thunderhub. In deze map maken we een config bestand aan voor Thunderhub.

```bash
cd ~
mkdir .thunderhub
cd .thunderhub
nano config.yaml
```

Plak dit erin:

```bash
masterPassword: 'password' # Default password unless defined in account
defaultNetwork: 'mainnet' # Default network unless defined in account
accounts:
  - name: '<kies_een_naam>'
    serverUrl: '127.0.0.1:10009'
    # network: Leave without network and it will use the default network
    lndDir: '/home/pi/.lnd'
```

Sla het op met `Ctrl + X` en bevestig met `Y`. Het masterPassword kun je naar wens aanpassen en heb je nodig om in te loggen in Thunderhub in je browser straks. Nadat je Thunderhub voor de eerste keer hebt opgestart, wordt dit wachtwoord herschreven met een hashed waarde.

## Installatie

We gaan weer terug naar de map met de Thunderhub software:

```bash
cd ~
cd thunderhub
```

Installeer Thunderhub:

```bash
npm run build
```

Als je de app met Yarn wilt installeren, voer dan het volgende uit:

```bash
yarn build
```

## Firewall

Zet port 4000 open.

```bash
sudo ufw allow 4000 comment "Port voor Thunderhub"
```

Mocht je Thunderhub van buiten je netwerk willen gebruiken, moet je port 4000 op je router opengooien en verkeer doorsturen naar je Pi.

## Automatiseren

Hoe laat je Thunderhub automatisch opstarten?  
Daarvoor maken we een Thunderhub service bestand aan:

```bash
sudo nano /etc/systemd/system/thunderhub.service
```

Plak er dit in.

```bash
[Unit]
Description=Thunderhub
Wants=lnd.service
After=lnd.service

[Service]
User=pi
WorkingDirectory=/home/pi/thunderhub
ExecStart=/usr/bin/npm start -- -p 4000
Restart=always
TimeoutSec=120
RestartSec=30

[Install]
WantedBy=multi-user.target
```

{% hint style="info" %}
Mocht je gebruik maken van LiT, vervang dan `lnd.service` met `lit.service`
{% endhint %}

Sla het weer op met `Ctrl + X` en bevestig met `Y`. De applicatie wordt gestart op poort 4000. Standaard is dit poort 3000, maar deze poort wordt ook gebruikt voor de [Ride The Lightning](ride-the-lightning.md) applicatie.

Het systeem moet op de hoogte gesteld worden van de nieuwe service en kan daarna gestart worden.

```bash
sudo systemctl enable thunderhub
```

```bash
sudo systemctl start thunderhub
```

Wil je zien of alles goed is opgestart, voer dan dit uit:

```bash
systemctl status thunderhub
```

Wil je een overzicht van de status over meerdere sessie, gebruik dan dit:

```bash
sudo journalctl -f -u thunderhub
```

## Gebruiken

Ga naar `http://[het ip adres van je Pi]:4000` in je browser om Thunderhub te openen.  
Gebruik het wachtwoord `password` om in te loggen tenzij je een ander wachtwoord hebt ingevuld in het `config.yaml` bestand.

## Updaten

Stop de Thunderhub service.

```bash
sudo systemctl stop thunderhub
```

Ga naar de applicatie directory.

```bash
cd ~/thunderhub
```

Update de repository met de laatste wijzigingen via Git.

```bash
git fetch --all
```

Toon de laatste versie/tag/release.

```text
git describe --tags `git rev-list --tags --max-count=1`
```

Haal de wijzigingen op van de laatste versie.

```bash
git checkout <OUTPUT VAN DE VORIGE STAP> #bijvoorbeeld v0.12.20
```

Installeer de software.

```text
npm install
npm run build
```

Start de Thunderhub service.

```bash
sudo systemctl start thunderhub
```

Thunderhub is nu bijgewerkt!

