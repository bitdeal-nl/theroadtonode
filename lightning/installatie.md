# Installatie

{% hint style="info" %}
Tijd: 5 minuten
{% endhint %}

[Lighting Network Daemon](https://github.com/lightningnetwork/lnd#lightning-network-daemon) \(LND\) is een implementatie van het Lightning Network protocol. Het is een tweede laag op Bitcoin, waarbij bitcoind \(of software equivalent\) de eerste laag vormt. Maar als je LND wil installeren dan wist je dat waarschijnlijk al.

{% hint style="info" %}
Let op: dit onderdeel is afhankelijk van de [Golang installatie](https://node.bitdeal.nl/raspberry-pi/algemene-dependencies-installeren#golang). Je kunt niet verder als je Golang niet geinstalleerd hebt op de Raspberry Pi.
{% endhint %}

Zorg dat je in de home directory bent.

```bash
cd ~
```

Haal de broncode binnen.

```bash
git clone https://github.com/lightningnetwork/lnd
```

Ga de LND map in.

```bash
cd lnd
```

En installeer LND.

```bash
make install
```
