# Algemene dependencies installeren

{% hint style="info" %}
Tijd: 15 minuten
{% endhint %}

Verschillende onderdelen binnen de guide zijn afhankelijk van dezelfde soort tools of programmeertalen. Zo zijn de Lightning Network Daemon en LND Connect afhankelijk van de taal Golang en zijn Ride The Lightning, Lighting Terminal en BTC RPC Explorer allemaal afhankelijk van NodeJS.

Om niet in herhaling te vallen door in ieder onderdeel te benoemen hoe een afhankelijkheid geinstalleerd dient te worden, is deze pagina de waar naar verwezen wordt. Zo kunnen eventuele updates ook centraal verwerkt worden waardoor alle onderdelen die hierop voort bouwen profiteren.

## Golang

Golang \(Go\) is een programmeertaal ontwikkeld door Google. Het is een vrij jonge taal uitgebracht in 2009. De lightning implementatie genaamd LND is hierop gebouwd. Aangezien we LND zelf gaan compilen, hebben we Go nodig.

### Oude Go versie verwijderen \(optioneel\)

Dit deel is alleen nodig als je Go al een keer geïnstalleerd had op je Pi middels `sudo apt install`. De versie die je op die manier hebt geïnstalleerd dient eerst te worden verwijderd.

```bash
sudo apt remove golang-go
```

```bash
sudo apt autoremove golang-go
```

```bash
sudo apt purge golang-go
```

### Nieuwe Go versie installeren

Nadat de oude versie is verwijderd kunnen we de nieuwe installeren. Op moment van schrijven is [v1.15.5](https://golang.org/dl/) het meest recent. Als er een nieuwe versie is die je wil installeren \(of omdat een latere versie van LND dat vereist\), let er dan op dat je een ARMv6 versie van Go download.

```bash
wget https://golang.org/dl/go1.15.5.linux-armv6l.tar.gz
```

```bash
sudo tar -C /usr/local -xzf go1.15.5.linux-armv6l.tar.gz
```

```bash
rm go1.15.5.linux-armv6l.tar.gz
```

### Referencies updaten

Om Go lekker te laten werken moeten we wat paden aanpassen. Daarvoor passen we het profile bestand aan.

```bash
nano ~/.profile
```

Waarschijnlijk zie je al wat staan in het bestand. Dat is goed en moet je laten staan. Plak er het volgende onderaan erin:

```bash
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
export PATH=$PATH:$GOPATH/bin:/usr/local/go/bin
```

Sla het op met `control + X` en `Y`. Nadat het opgeslagen is kun je profile inladen.

```bash
. ~/.profile
```

Om zeker van de te zijn dat alles werkt log je uit met "exit" en start je een nieuwe SSH verbinding. Check de Go versie met:

```bash
go version
```

Als het goed is zie je `go version go1.15.5 linux/arm` verschijnen.

## NodeJS

Net als dat Golang binnengehaald moest worden, gaan we nu NodeJS binnenhalen. We willen de nieuwste versie, dus draaien eerst een scriptje. Zorg allereerst dat je in de home directory zit met `cd ~` commando. Voer daarna het script uit.

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
# Verwachte output: v14.15.1
```

