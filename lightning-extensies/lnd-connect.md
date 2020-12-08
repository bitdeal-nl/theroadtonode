# LND Connect

{% hint style="info" %}
Tijd: 2 minuten
{% endhint %}

LND Connect \(vaak [lndconnect](https://github.com/LN-Zap/lndconnect) genoemd\) is een tool ontwikkeld door de makers van Zap wallet. De tool genereert een QR code of stuk tekst waarmee een wallet gekoppeld kan worden aan je node. In dit onderdeel van de guide zullen we enkel lndconnect installeren. Het gebruik komt terug in wallet onderdelen van deze guide, zoals het [Zap \(iOS\)](https://node.bitdeal.nl/ios/zap) onderdeel.

SSH om te beginnen in je Pi. Als het goed is heb je [Go](https://node.bitdeal.nl/raspberry-pi/algemene-dependencies-installeren#golang) al op je machine staan. Dat is mooi, want dan kunnen we direct van start. Het kan een minuutje duren om lndconnect binnen te halen.

```bash
go get -d github.com/LN-Zap/lndconnect
```

Het bovenstaande haalt de broncode van lndconnect binnen. Na het binnenhalen moeten we naar de juiste map en het installeren.

```bash
cd $GOPATH/src/github.com/LN-Zap/lndconnect
```

```bash
make
```
