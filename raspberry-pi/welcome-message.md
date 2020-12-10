Om bij het inloggen over SSH direct een overzicht te krijgen van de status van het systeem en 4 belangrijke services van je node kan je een shellscript toevoegen wat wordt uitgevoerd wanneer je inlogt. Deze handleiding is een bewerking van [Bonus Guide: System Overview](https://stadicus.github.io/RaspiBolt/raspibolt_61_system-overview.html) waarbij Electrs is vervangen door Electrum Personal Server. Daarnaast gaan we voor de versies uit van de laatste getagde versie in Github en niet de latest release omdat we die in deze gebruiken.



```bash
sudo apt install jq net-tools
cd /tmp/
wget https://raw.githubusercontent.com/Stadicus/RaspiBolt/master/resources/20-raspibolt-welcome
```
