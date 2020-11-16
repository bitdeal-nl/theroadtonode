# Installatie

{% hint style="info" %}
Tijd: bijna 2 uur \(waarvan 95% wachten\)
{% endhint %}

Ga er maar even goed voor zitten, want met dit onderdeel ben je wel even zoet. Het doel is om Bitcoin Core zelf samen te stellen. Wat moeten we daar voor doen?

* **Dependencies installeren**, er is wat software nodig om Bitcoin Core samen te stellen.
* **Broncode ophalen**, door middel van Git kunnen we de nieuwste broncode ophalen.
* **Database installeren, indien je van de Bitcoin Core wallet gebruik wilt maken**
* **Bitcoin Core samenstellen**

## Dependencies

Voer het volgende uit om de dependencies te installeren.

```bash
sudo apt install git automake autoconf autotools-dev build-essential make pkg-config protobuf-compiler libminiupnpc-dev libprotobuf-dev libdb++-dev libzmq3-dev libsqlite3-dev libboost-thread-dev libboost-test-dev libboost-all-dev libevent-dev libtool libssl-dev libboost-system-dev libboost-filesystem-dev -y
```

## Broncode

Voer het volgende uit om de code binnen te halen.

```bash
git clone https://github.com/bitcoin/bitcoin
```

```bash
cd bitcoin
```

Voor de mensen die iets van Git afweten: ja, we blijven op de master branch zitten. Hier zit ondersteuning voor [onion v3 adressen](https://bitcoinmagazine.nl/2020/10/bitcoin-core-tor-v3/) in, wat pas in een latere versie echt uitgebracht wordt. Wil je dat niet, kun je met het volgende commando kiezen welke versie van Core je wil. Op het moment van schrijven is [0.20](https://github.com/bitcoin/bitcoin/tags) de meest recente versie.

```bash
# Dit is dus optioneel
git checkout 0.20
```

## Database

Deze stap is optioneel en alleen noodzakelijk indien je ook gebruik wilt maken van de Bitcoin Core wallet. Als je wel wenst gebruik te maken van de Bitcoin Core wallet, moet je bij de volgende stap bij de `./configure` command de parameters `--disable-wallet` weglaten.
In de repository van Core is een script aanwezig waarmee de database geïnstalleerd kan worden. Voer het script uit met:

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

```bash
./configure --enable-upnp-default --without-gui --disable-wallet BDB_LIBS="-L${BDB_PREFIX}/lib -ldb_cxx-4.8" BDB_CFLAGS="-I${BDB_PREFIX}/include"
```

Na op enter drukken van het volgende commando kun je wat voor jezelf gaan doen. Uit ervaring blijkt deze stap bijna anderhalf uur te duren.

```bash
make
```

Het volgende duurt ook weer een kwartier.

```bash
make check
```

```bash
sudo make install
```

Dat was het voor het installeren van Core. Je kunt terug naar de home directory met:

```bash
cd ~
```

