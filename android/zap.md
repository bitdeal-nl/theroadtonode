# Zap

{% hint style="info" %}
Tijd: 10 minuten
{% endhint %}

De [Zap: Bitcoin Lightning Wallet](https://play.google.com/store/apps/details?id=zapsolutions.zap)-app is nog in ontwikkeling maar daarom niet minder bruikbaar. Hij heeft een mooie interface en je kunt hem aan je eigen node hangen via Tor. Om Tor te kunnen gebruiken op Android heb je ook [Orbot: Tor for Android](https://play.google.com/store/apps/details?id=org.torproject.android) nodig.

## Voorbereiding

Omdat we een headless server draaien, hebben we geen QR-code die we straks kunnen scannen dus moeten we een connectiestring plakken. Deze connectiestring maken we met [lndconnect](https://node.bitdeal.nl/lightning-extensies/lnd-connect) op je Pi. SSH je Pi in en typ:

```bash
lndconnect --host=xxx.onion --port=10009 --nocert -j
```

Waar **xxx.onion** staat, vul je natuurlijk [jouw onion-adres](https://node.bitdeal.nl/lightning/tor-aanpassen#onion-adressen) in voor de **RPC** API van LND. Er zal een lap tekst verschijnen dat iets weg heeft van het volgende:

```bash
lndconnect://xxx.onion:10009?macaroon=heel_veel_tekens
```

Kopieer die lap tekst vanuit SSH en plak die bijvoorbeeld in een Google Docs document \(want we willen die tekst straks op onze telefoon hebben\).

## Orbot

Als je de app installeert en voor het eerst opent, wordt je gevraagd om een wallet in te stellen.

1. Download & installeer Zap \(open deze nog niet!\)
2. Download & installeer Orbot
3. Open Orbot
4. Schakel VPN-modus in
5. Voeg Zap toe als Tor-gebruikende app
6. Tap op de grote Start knop
7. Tap nu op het zap icoon onder in het scherm \(Zap zal nu geopend worden\)

## Zap

1. Tap op Wallet instellen
2. Kies voor Maak verbinding met eigen node
3. Toestaan

Direct daarna verschijnt een scherm waarmee jij jouw node kunt connecten. Open nu op je telefoon dit Google Docs document en kopieer de gehele string \(`lndconnect://xxx.onion:10009?macaroon=heel_veel_tekens`\). Switch nu terug naar Zap en tap op `PLAK`. Je krijgt nu een melding of je zeker weet of je wilt verbinden met `xxx.onion`. Tap nu weer op `OKE`.

Je krijgt nu een melding dat je verbonden bent met MAINNET met echte Bitcoin! Tap weer op `OKE`.

Je bent nu verbonden met je eigen Node.

Via `Beheer...` kun je naam van je wallet aanpassen. Standaard krijgt die de naam van je Tor-adres namelijk.

