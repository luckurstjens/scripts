Om een Windows-computer via SSH een commando naar een Linux NAS te laten sturen om de NAS uit te schakelen wanneer de Windows-pc wordt uitgezet, moet je een script of een batchbestand maken. Hier is een algemene methode om dit te bereiken:

**Stap 1: Installeer OpenSSH op Windows:**

Zorg ervoor dat de Windows-computer OpenSSH-client geïnstalleerd heeft. Je kunt dit doen via de Windows-instellingen.

**Stap 2: Genereer SSH-sleutels:**

Genereer SSH-sleutels op de Windows-computer met het volgende commando in PowerShell:

```powershell
ssh-keygen
```

Volg de instructies om de sleutels te genereren. Hierdoor wordt een openbare sleutel (id_rsa.pub) gegenereerd die je zult gebruiken om verbinding te maken met de Linux NAS zonder een wachtwoord.

**Stap 3: Kopieer de openbare sleutel naar de Linux NAS:**

Kopieer de inhoud van het gegenereerde `id_rsa.pub`-bestand op de Windows-computer en voeg het toe aan het `~/.ssh/authorized_keys`-bestand op de Linux NAS. Dit kun je doen via SSH of door het bestand handmatig te kopiëren en plakken.

```powershell
cat ~/.ssh/id_rsa.pub | ssh root@nas "mkdir ~/.ssh; cat >> ~/.ssh/authorized_keys"
```

**Stap 4: Test SSH-verbinding:**

Test of je zonder wachtwoord naar de Linux NAS kunt verbinden vanaf de Windows-computer met het volgende commando:

```bash
ssh root@nas
```

Hierbij moet je "root" vervangen door de daadwerkelijke gebruikersaccount op de Linux NAS en "nas" door het IP-adres of de hostname van de NAS.

**Stap 5: Maak een batchbestand op Windows:**

Maak een batchbestand (bijvoorbeeld `shutdown_nas.bat`) op de Windows-computer met de volgende inhoud:

```batch
@echo off
ssh root@nas "shutdown -h now"
shutdown /s /f /t 0
```

Vervang "root" door de daadwerkelijke root op de Linux NAS.

**Stap 6: Voer het batchbestand uit bij het afsluiten:**

Om dit batchbestand uit te voeren wanneer je de Windows-computer uitzet, kun je een groepsbeleid instellen. Hier is hoe:

- Druk op Win + R om het Uitvoeren-venster te openen.
- Typ "gpedit.msc" en druk op Enter.
- Ga naar "Computerconfiguratie" > "Windows-instellingen" > "Scripts (opstart/afsluiten)".
- Selecteer "Aan de slag" bij "Afsluiten".
- Klik op "Toevoegen" en selecteer het batchbestand dat je hebt gemaakt.

Nu zal het batchbestand worden uitgevoerd wanneer je de Windows-computer afsluit, en het zal de SSH-opdracht verzenden om de Linux NAS uit te schakelen voordat de Windows-pc wordt afgesloten.
