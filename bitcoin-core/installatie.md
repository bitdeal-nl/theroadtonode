# Installatie

{% hint style="info" %}
Tijd: bijna 2 uur \(waarvan 95% wachten tijdens het `make` commando\)
{% endhint %}

Ga er maar even goed voor zitten, want met dit onderdeel ben je wel even zoet. Het doel is om Bitcoin Core zelf samen te stellen. Wat moeten we daar voor doen?

* **Dependencies installeren**, er is wat software nodig om Bitcoin Core samen te stellen.
* **Broncode ophalen**, door middel van Git kunnen we de nieuwste broncode ophalen.
* **Database installeren**
* **Bitcoin Core samenstellen**

## Dependencies

Voer het volgende uit om de dependencies te installeren. Dependencies zijn precies wat het zegt; afhankelijkheden. Het zelf samenstellen van Bitcoin Core is afhankelijk van andere software.

```bash
sudo apt install git automake autoconf autotools-dev build-essential make pkg-config protobuf-compiler libminiupnpc-dev libprotobuf-dev libdb++-dev libzmq3-dev libsqlite3-dev libboost-thread-dev libboost-test-dev libboost-all-dev libevent-dev libtool libssl-dev libboost-system-dev libboost-filesystem-dev -y
```

## Broncode

Zorg allereerst dat je in de "home directory" zit. Om er zeker van te zijn voer je `cd ~` uit. De tilde \(`~`\) is een afkorting voor `/home/pi` in dit geval. Voer daarna het volgende uit om de broncode binnen te halen en in je home directory neer te zetten. Automatisch zal hier een map aangemaakt worden genaamd bitcoin met daarin de broncode.

```bash
git clone https://github.com/bitcoin/bitcoin
```

Duik de bitcoin map in. Dit is nodig tijdens de volledige installatie van Core.

```bash
cd bitcoin
```

Gezien de [git-flow van Bitcoin Core](https://github.com/bitcoin/bitcoin/blob/master/CONTRIBUTING.md#decision-making-process) kun je er vanuit gaan dat alles in de master branch correct en foutloos functioneert. Het release proces van Bitcoin Core hakt de master branch af op een vaste datum. Alles wat op die branch zat op moment van afhakken, zal meegenomen in de release. Alles dat daarna in de branch komt, zal in een volgende release komen.

Zou je toch een specifieke release willen, kun je het onderstaande commando uitvoeren. Op het moment van schrijven is [v0.21.1](https://github.com/bitcoin/bitcoin/tags) de meest recente versie. Klik je op het linkje en zie je een nieuwere versie? Verander dan het onderstaande getalletje naar iets nieuwers, zoals v0.22.0.

```bash
git checkout v0.21.1
```

## Database

In de repository van Core is een script aanwezig waarmee de database geïnstalleerd kan worden. Dit heeft Bitcoin Core nodig om te werken. Blijf in de huidige map \(`/home/pi/bitcoin`\) zitten en voer het script uit met het onderstaande commando. Alle volgende commando's moeten ook uitgevoerd worden vanuit deze map.

```bash
./contrib/install_db4.sh `pwd`
```

```bash
export BDB_PREFIX='/home/pi/bitcoin/db4'
```

## Bitcoin Core samenstellen

Tot slot het samenstellen van Bitcoin Core.

```bash
./autogen.sh
```

{% hint style="info" %}
Met het volgende commando configureren we de installatie van Bitcoin Core. Het is mogelijk om Bitcoin Core te draaien zonder wallet functionaliteit en dus puur te gebruiken als backend voor je andere wallets. Weet je zeker dat je de ingebouwde wallet functionaliteit van Core niet zal gebruiken, draai dan:

`./configure --enable-upnp-default --without-gui --disable-wallet BDB_LIBS="-L${BDB_PREFIX}/lib -ldb_cxx-4.8" BDB_CFLAGS="-I${BDB_PREFIX}/include"`

Aan dit commando is de instelling `--disable-wallet` meegegeven. Weet je het niet zeker, draai dan onderstaand commando.

Heb je Bitcoin Core geconfigureerd zónder wallet functionaliteit en wil je dit later toch toevoegen, voer dan alles vanaf onderstaand commando nog een keer uit.
{% endhint %}

```bash
./configure --enable-upnp-default --without-gui BDB_LIBS="-L${BDB_PREFIX}/lib -ldb_cxx-4.8" BDB_CFLAGS="-I${BDB_PREFIX}/include"
```

Na op enter drukken van het volgende commando kun je wat voor jezelf gaan doen. **Uit ervaring blijkt deze stap bijna een uur te duren.**

```bash
make -j $(nproc)
```

Als je wat testjes wil draaien om te kijken of alles goed zit, kun je `make check` uitvoeren. Dit is optioneel en duurt zo'n vijftien minuten.

Rond het geheel af met:

```bash
sudo make install
```

Dat was het voor het installeren van Core! Je kunt terug naar de home directory met `cd ~`.