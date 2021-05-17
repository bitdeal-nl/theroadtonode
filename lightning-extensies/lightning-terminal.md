# Lightning Terminal

{% hint style="info" %}
Tijd: 35 minuten
{% endhint %}

Lightning Terminal \([LiT](https://github.com/lightninglabs/lightning-terminal)\) geeft de gebruiker een mooi schilletje om bestaande tools van Lightning Labs. De tools zijn [Loop](https://lightning.engineering/loop), [Faraday](https://github.com/lightninglabs/faraday) en [Pool](https://lightning.engineering/pool). Op dit moment is Loop goed te gebruiken door middel van LiT. Ook Pool werkt prima, maar is nog in preview fase. Faraday heeft slechts 1 functionaliteit in LiT, maar meer zal toegevoegd worden.

### LiT use cases voor zover

* Een visueel overzicht van de balans van jouw Lightning kanalen.
* Kanalen balanceren via Loop met behulp van submarine swaps.
* Balanceren doe je met on-chain satoshis met Loop In.
* Je betaalt minder fees als je meerdere kanalen tegelijk balanceert.
* Eenvoudig satoshis versturen naar je on-chain wallet met Loop Out.
* Deelname aan de Lightning Network liquiditeit markt met Pool.
* Downloadbaar overzicht van channels in CSV formaat met Faraday.

Een demo video kun je [hier](https://lightning.engineering/static/terminal-2c00f93b062f44e5c2d1db067f7ee8cd.mp4) vinden.

### Benodigdheden

* [Golang](https://docs.theroadtonode.com/raspberry-pi/algemene-dependencies-installeren#golang) **\(update naar de nieuwste versie voordat je begint\)**
* [NodeJS](https://docs.theroadtonode.com/raspberry-pi/algemene-dependencies-installeren#nodejs)
* [Yarn](https://docs.theroadtonode.com/raspberry-pi/algemene-dependencies-installeren#yarn)
* Minimaal 250.000 sats per kanaal om gebruik te kunnen maken van Loop

## LND configureren voor LiT

Als je LND hebt geïnstalleerd zonder speciale installatie tags, dan moet je LND even opnieuw installeren. Aangezien we LND opnieuw installeren met nieuwe tags, dienen de macaroon bestanden \(bestanden die authenticatie verzorgen\) verwijderd te worden. Na het opnieuw opstarten van de verse installatie, zullen de bestanden opnieuw aangemaakt worden.

{% hint style="info" %}
Je channels en wallet blijven bestaan bij een herinstallatie van LND. Net als bij een update.

Heb je Thunderhub draaien? Dan dien je na het opnieuw opstarten van LND ook Thunderhub opnieuw te starten.
{% endhint %}

Stop LND indien deze actief is.

```text
sudo systemctl stop lnd
```

Ga naar de bronbestanden map van LND om LND opnieuw te installeren met de juiste installatie tags.

```text
cd ~/lnd
```

Installeer LND met de juiste tags.

```text
make install tags="autopilotrpc signrpc walletrpc chainrpc invoicesrpc routerrpc watchtowerrpc"
```

Check voor macaroon bestanden.

```text
ls -la ~/.lnd/data/chain/bitcoin/mainnet
```

De macaroon bestanden die je moet verwijderen zijn. `admin.macaroon`, `invoice.macaroon`, `readonly.macaroon` `router.macaroon` .

Verwijder de macaroon bestanden.

```text
rm -i ~/.lnd/data/chain/bitcoin/mainnet/*.macaroon
```

Beantwoord de vragen met `yes` om elk bestand te verwijderen.

Start LND opnieuw. Hiermee worden ook nieuwe macaroons aangemaakt.

```text
sudo systemctl start lnd
```

Unlock je wallet met je wachtwoord.

```text
lncli unlock
```

## Installatie Loop

Om kanalen te kunnen balanceren, hebben we de Loop software nodig. Ga eerst naar je home directory.

```text
cd ~
```

Download de broncode.

```text
git clone https://github.com/lightninglabs/loop.git
```

Ga naar `loop/cmd`.

```text
cd loop/cmd
```

Installeer de `loopd` software.

```text
go install ./...
```

Test of het gelukt is.

```text
loopd
```

In de output zul je lezen dat Loop verbonden is met je LND.

```text
2020-12-01 09:53:40.186 [INF] LOOP: Connected to lnd node '<jouw_node_naam>' with pubkey <public_key_van_jouw_node> (version v0.11.99-beta, build tags 'signrpc,walletrpc,chainrpc,invoicesrpc')
```

Met `Ctrl + C` kun je loopd weer stop zetten.

## Installatie Faraday

Navigeer terug naar je home directory.

```bash
cd ~
```

Download de broncode.

```text
git clone https://github.com/lightninglabs/faraday.git
```

Ga naar de faraday map.

```text
cd faraday
```

Installeer de faraday software.

```text
make && make install
```

Test of het gelukt is.

```text
faraday --version

# Verwachte output lijkt op: faraday version 0.2.2-alpha commit=v0.2.2-alpha-17-gfa65f48c9172e601bbfc89dab7ab68de1564a346
```

Configureer de faraday software en check of het allemaal werkt. Op de plekken "VUL\_USERNAME\_IN" en "VUL\_PASSWORD\_IN" moet je de gegevens invullen die je tijdens de [configuratie](https://docs.theroadtonode.com/bitcoin-core/configuratie-en-starten#authenticatie) van Core hebt aangemaakt.

```bash
faraday --lnd.macaroondir=/home/pi/.lnd/data/chain/bitcoin/mainnet --lnd.tlscertpath=/home/pi/.lnd/tls.cert --lnd.rpcserver=127.0.0.1:10009 --connect_bitcoin --bitcoin.host=127.0.0.1:8332 --bitcoin.user=VUL_USERNAME_IN --bitcoin.password=VUL_PASSWORD_IN
```

Zodra je te zien krijgt dat alles werkt, kun je faraday weer afsluiten met `Ctrl + C`.

## Installatie Pool

Navigeer terug naar je home directory.

```bash
cd ~
```

Download de broncode.

```text
git clone https://github.com/lightninglabs/pool
```

Ga naar de pool map.

```text
cd pool
```

Installeer de Pool software.

```text
make install
```

Test of het gelukt is.

```text
poold
```

Zodra je te zien krijgt dat alles werkt, kun je poold weer afsluiten met `Ctrl + C`.

## Installatie Lightning Terminal

Navigeer terug naar je home directory.

```bash
cd ~
```

Download de broncode van Lightning Terminal.

```text
git clone https://github.com/lightninglabs/lightning-terminal.git
```

Duik de code in.

```text
cd lightning-terminal
```

Installeer Lightning Terminal.

```text
make install
```

## Configuratie

Het configureren van LiT gaat - zoals vele andere zaken in deze guide - via een configuratie bestand. In het bestand staan alle instellingen en tijdens het opstarten van LiT zullen de instellingen toegepast worden.

Navigeer terug naar je home directory.

```bash
cd ~
```

Maak de `.lit` map aan.

```bash
mkdir .lit
```

De configuratie van LND nemen we over in LiT. Aangezien we LND zullen opstarten zodra LiT wordt opgestart \(daarover onder het kopje "automatisering" meer\), kunnen we de LND instellingen kopiëren.

```bash
cp .lnd/lnd.conf .lit/lit.conf
```

Pas het bestand aan.

```bash
nano .lit/lit.conf
```

Er moet veel aangepast worden. Ten eerste moeten alle vierkante haken omgezet worden naar een hashtag. Stond er eerst `[Application Options]`, maak daar dan `# LND - Application Options` van. Ook moet iedere instelling de prefix `lnd.` krijgen. Heb je de guide gevolgd en de [configuratie van LND](https://docs.theroadtonode.com/lightning/configuratie) doorlopen, dan heb je een bestand dat hier op lijkt nadat je de genoemde wijzigingen hebt gemaakt.

```bash
# LND - Application Options
lnd.tlsautorefresh=true
lnd.restlisten=0.0.0.0:8080
lnd.rpclisten=0.0.0.0:10009
lnd.listen=127.0.0.1:9735
lnd.maxpendingchannels=5
lnd.color=ZELF_VERZINNEN_A
lnd.alias=ZELF_VERZINNEN_B

# LND - Tor
lnd.tor.active=true
lnd.tor.v3=true
lnd.tor.streamisolation=true

# LND - Bitcoin
lnd.bitcoin.active=true
lnd.bitcoin.mainnet=true
lnd.bitcoin.node=bitcoind

# LND - bitcoind
lnd.bitcoind.dir=/home/pi/.bitcoin
lnd.bitcoind.rpcuser=DIT_WEET_JE_A
lnd.bitcoind.rpcpass=DIT_WEET_JE_B
lnd.bitcoind.zmqpubrawblock=tcp://127.0.0.1:28332
lnd.bitcoind.zmqpubrawtx=tcp://127.0.0.1:28333
```

Voeg aan de "LND - Application Options" `lnd.lnddir=~/.lnd` toe. Daarna voeg je de onderstaande regels toe voor LiT. Let op: verander "ZELF\_VERZINNEN\_B" in iets anders. Dit is het wachtwoord waarmee je LiT in kunt straks.

```bash
# LiT - Application Options
lnd-mode=integrated
httpslisten=0.0.0.0:8443
uipassword=ZELF_VERZINNEN_B
```

Tot slot voeg je onderaan deze regels toe voor Faraday:

```bash
# Faraday - Application Options
faraday.connect_bitcoin=true

# Faraday - LND
faraday.lnd.macaroondir=/home/pi/.lnd/data/chain/bitcoin/mainnet
faraday.lnd.tlscertpath=/home/pi/.lnd/tls.cert
faraday.lnd.rpcserver=127.0.0.1:10009

# Faraday - Bitcoin
faraday.bitcoin.host=127.0.0.1:8332
faraday.bitcoin.user=DIT_WEET_JE_A
faraday.bitcoin.password=DIT_WEET_JE_B
```

{% hint style="info" %}
Let goed op alle velden die voor jou persoonlijk zijn! Zoals usernames en passwords.
{% endhint %}

Uiteindelijk zal `lit.conf` er ongeveer zo uit zien.

```bash
# LND - Application Options
lnd.tlsautorefresh=true
lnd.restlisten=0.0.0.0:8080
lnd.rpclisten=0.0.0.0:10009
lnd.listen=127.0.0.1:9735
lnd.maxpendingchannels=5
lnd.color=ZELF_VERZINNEN_A
lnd.alias=ZELF_VERZINNEN_B
lnd.lnddir=~/.lnd

# LND - Tor
lnd.tor.active=true
lnd.tor.v3=true
lnd.tor.streamisolation=true

# LND - Bitcoin
lnd.bitcoin.active=true
lnd.bitcoin.mainnet=true
lnd.bitcoin.node=bitcoind

# LND - bitcoind
lnd.bitcoind.dir=/home/pi/.bitcoin
lnd.bitcoind.rpcuser=DIT_WEET_JE_A
lnd.bitcoind.rpcpass=DIT_WEET_JE_B
lnd.bitcoind.zmqpubrawblock=tcp://127.0.0.1:28332
lnd.bitcoind.zmqpubrawtx=tcp://127.0.0.1:28333

# LiT - Application Options
lnd-mode=integrated
httpslisten=0.0.0.0:8443
uipassword=ZELF_VERZINNEN_B

# Faraday - Application Options
faraday.connect_bitcoin=true

# Faraday - LND
faraday.lnd.macaroondir=/home/pi/.lnd/data/chain/bitcoin/mainnet
faraday.lnd.tlscertpath=/home/pi/.lnd/tls.cert
faraday.lnd.rpcserver=127.0.0.1:10009

# Faraday - Bitcoin
faraday.bitcoin.host=127.0.0.1:8332
faraday.bitcoin.user=DIT_WEET_JE_A
faraday.bitcoin.password=DIT_WEET_JE_B
```

## Firewall

W kunnen LiT benaderen over port 8443, vandaar dat we die open zetten.

```text
sudo ufw allow 8443
```

## Automatiseren

Hoe laat je Lightning Terminal \(inclusief LND, Pool, Loop en Faraday\) automatisch opstarten? Daarvoor maken we een Lightning Terminal service bestand aan.

### LiT service aanmaken

```text
sudo nano /etc/systemd/system/lit.service
```

Plak er dit in.

```text
[Unit]
Description=Lightning Terminal
Requires=bitcoind.service
After=bitcoind.service

[Service]
User=pi
ExecStart=/home/pi/go/bin/litd
PIDFile=/home/pi/.lit/lit.pid
Restart=always
TimeoutSec=120
RestartSec=30

[Install]
WantedBy=multi-user.target
```

Sla de wijzigingen op met `Ctrl + X` en bevestig met `Y`.

Breng het systeem op de hoogte van de nieuwe service.

```text
sudo systemctl enable lit
```

### LND service stoppen

Voordat je de nieuwe service kan starten, moet je de LND service stoppen. Dit moet omdat de LiT service LND opstart samen met Loop, Faraday en Pool.

```text
sudo systemctl stop lnd
```

Deactiveer de LND service zodat het in de toekomst niet meer gebruikt wordt.

```text
sudo systemctl disable lnd
```

### Oude services updaten

Aangezien we LND nu opstarten door middel van LiT, is het van belang dat services die voorheen op de LND service leunde, nu op de LiT service leunen. We moeten dus de RTL en Thunderhub service aanpassen als volgt: wijzig `Wants=lnd.service` en `After=lnd.service` naar `Wants=lit.service` en `After=lit.service`. Hieronder staan voorbeelden.

#### RTL service updaten

Als je de RTL service wil aanpassen, voer dan eerst dit commando uit:

```bash
sudo nano /etc/systemd/system/rtl.service
```

Verander dan de "Wants" en "After" regels zoals hierboven beschreven.

```bash
Wants=lit.service
After=lit.service
```

Sla de wijzigingen op met `Ctrl + X` en bevestig met `Y`.

#### Thunderhub service updaten

Als je de Thunderhub service wil aanpassen, voer dan eerst dit commando uit:

```bash
sudo nano /etc/systemd/system/thunderhub.service
```

Herhaal de stappen zoals je de RTL service ook hebt aangepast.

#### Brengt systeem op de hoogte

De Pi moet op de hoogte gebracht worden van de aangepaste services. Dat doe je zo:

```bash
systemctl daemon-reload
```

Het zal om je wachtwoord vragen. Vul dat in en druk op enter.

### LiT service starten

Start de LiT service als volgt.

```text
sudo systemctl start lit
```

Wil je zien of de service is opgestart, voer dan dit uit.

```text
systemctl status lit
```

Wil je een overzicht van de status over meerdere sessies, gebruik dan dit commando.

```text
sudo journalctl -f -u lit
```

### RTL en Thunderhub herstarten

Tot slot dienen de RTL en Thunderhub services herstart te worden.

```bash
sudo systemctl restart rtl
sudo systemctl restart thunderhub
```

## Lightning Terminal gebruiken

Het is goed om te weten dan Pool, Loop en Faraday niet worden opgestart zolang er geen wallet unlocked is. Zodra de LiT service opstart en daarmee dus ook LND aangooit, moet je even `lncli unlock` draaien en je wachtwoord invullen. Pas daarna zullen de andere tools werken.

Open [https://&lt;het](https://<het) **ip adres van je Pi&gt;:8443** in de browser om in te loggen in Lightning Terminal en aan de slag te gaan met de tool.

### Walkthrough

Aan de slag met het LiT dashboard? Volg dan [deze walkthrough](https://github.com/lightninglabs/lightning-terminal/blob/master/doc/WALKTHROUGH.md) van Lightning Labs.

