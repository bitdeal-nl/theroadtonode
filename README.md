# Overzicht

{% hint style="info" %}
The Road to Node versie: 2.3.0

Hulp nodig? Check [Telegram](https://t.me/theroadtonode).
{% endhint %}

Op deze site vind je alle benodigdheden om zelf een Bitcoin node op te zetten. Veel plezier!

### Introductie

In het introductie onderdeel vind je informatie over het [doel en de waarom](https://docs.theroadtonode.com/introductie/doel-en-waarom) van deze guide. Ook komen de [benodigdheden](https://docs.theroadtonode.com/introductie/benodigdheden) aan bod.

### Raspberry Pi

De Raspberry Pi is een mini computer waar je de leukste dingen mee kunt. Maar om er gebruik van je maken, moet je hem eerst [aansluiten](https://docs.theroadtonode.com/raspberry-pi/hardware-aansluiten) en er een [besturingssysteem op knallen](https://docs.theroadtonode.com/raspberry-pi/software-flashen). Ook wil je natuurlijk [de beste prestaties](https://docs.theroadtonode.com/raspberry-pi/boot-vanaf-ssd) uit je Pi halen. Als je interesse hebt in het anoniem gebruiken van Bitcoin, dan kan dat met [Tor](https://docs.theroadtonode.com/raspberry-pi/tor).

### Bitcoin Core

Het kloppend hart van deze guide. Allereerst halen we zelf de broncode van Bitcoin Core binnen en [installeren](https://docs.theroadtonode.com/bitcoin-core/installatie) het. Om er gebruik van te maken en de rest van de guide te volgen, heb je [bepaalde instellingen](https://docs.theroadtonode.com/bitcoin-core/configuratie-en-starten) nodig. De instellingen zijn bepalend voor latere apps en Lightning. In deze guide wordt Bitcoin Core als implementatie gebruikt. Uit [onderzoek](https://blog.lopp.net/bitcoin-node-performance-sync-tests/#performance-rankings) blijkt dit de snelste Bitcoin implementatie te zijn. Daarnaast wordt hier het meest aan ontwikkeld, bevat het de nieuwste features, heeft het [grootste aandeel](https://bitnodes.io/nodes/) in de markt en is het de meest volwassen implementatie van het protocol.

### Bitcoin Core extensies

Het "extensies" hoofdstuk met betrekking op Bitcoin Core, heeft voornamelijk te maken met tools die rechtstreeks leunen op Core. Neem bijvoorbeeld [Specter](https://docs.theroadtonode.com/bitcoin-core-extensies/specter), een gebruiksvriendelijke manier om een multisig contructie op te zetten. Je kunt met deze guide ook een Electrum server opzetten aan de hand van tools zoals [Electrum Personal Server](https://docs.theroadtonode.com/bitcoin-core-extensies/electrum-personal-server) of [Electrum X](https://docs.theroadtonode.com/bitcoin-core-extensies/electrum-x). Of misschien wil je je eigen blockchain explorer? Dat kan met [BTC RPC Explorer](https://docs.theroadtonode.com/bitcoin-core-extensies/btc-rpx-explorer).

### Lightning

Bitcoin zijn tweede laag, het Lightning netwerk, moet het schaalbaarheidsprobleem van Bitcoin oplossen. Wil je echt digitaal cowboyen dan zit met Lightning aan het juiste adres. Om Lighting werkend te krijgen, moeten we de programmeertaal [Golang installeren](https://docs.theroadtonode.com/lightning/golang-installatie). Dat heeft te maken met de implementatie van het Lightning protocol genaamd Lighting Network Daemon \(LND\). Hier zit Lightning Labs achter en is wordt goed onderhouden. Lightning Labs is ook de ontwikkelaar van verschillende Lightning extensies zoals pool, faraday en loop. Om dit soort extensies naadloos toe te voegen aan onze node, maken we in deze guide gebruik van LND.

### Lightning extensies

Om makkelijker gebruik te maken van het Lightning netwerk, kun je er allerlei software aan vastklikken. Met een user interface zoals [Ride The Lightning](https://docs.theroadtonode.com/lightning-extensies/ride-the-lightning) maak je op een prettige manier gebruik van LND. Ben je opzoek naar het mooiere broertje van RTL, kun je eens kijken naar [Thunderhub](https://docs.theroadtonode.com/lightning-extensies/thunderhub). Het bied vrijwel dezelfde functionaliteiten maar dan mooier.

### iOS

Het iOS gedeelte van de guide richt zich op apps voor de Apple iPhone en iPad. Zo wordt er uitgelegd hoe je de app [Fully Noded](https://docs.theroadtonode.com/ios/fully-noded) - al dan niet via tor - aan jouw eigen node hangt. Met Fully Noded kun je jouw node beheren en bijvoorbeeld PSBT aanmaken en broadcasten. De app [Zap](https://docs.theroadtonode.com/ios/zap) is de meest gebruiksvriendelijke manier om Lightning transacties te doen via je eigen node.

### Android

Het Android gedeelte van de guide richt zich op apps voor het Android besturingssysteem. Zo wordt er uitgelegd hoe je de app [Zap](https://docs.theroadtonode.com/ios/zap) kunt gebruiken om channels te openen en Lightning transacties te doen - al dan niet via tor - via je eigen node over.

## Contributie leveren

Wil je ook iets toevoegen aan de guide? Dat kan! En graag zelfs. Om het zo gestroomlijnd mogelijk te maken, is er een flow bedacht:

* Maak een issue aan op de [Github](https://github.com/bitdeal-nl/theroadtonode/issues) pagina. Het woord "issue" kun je hier wat breder interpreteren dan alleen "fouten" of "problemen". Een nieuwe feature kun je ook kenbaar maken middels een issue.
* Geef in de Telegram groep aan aan welke issue je werkt, dan kan een maintainer het op je naam zetten. Zo pakt niet iemand anders het per ongeluk op.
* Tak een branch af van `master`. Wil je iets verbeteren, begin je branchnaam dan met `hotfix/` gevolgd door TRTN \(The Road to Node\) en het issuenummer. Wil je een nieuwe feature toevoegen, begin je branchnaam dan met `feature/`. Voorbeelden: `feature/TRTN-022-sparrow-wallet` of `hotfix/TRTN-030-missende-instructie-bitcoin-installatie`.
* Is je feature of hotfix klaar? Maak hem dan kenbaar middels een pull request. Koppel deze meteen aan de eerder aangemaakte issue.

