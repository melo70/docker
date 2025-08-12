# docker

Per creare un container eseguire i seguenti comandi di esempio:

```bash
docker build -t dotnet-dev .
```
Avendo preparati diversi dockerfile per ognuna delle versioni di dotnet si potrebbe procedere in questo modo:

```bash
docker build -f Dockerfile.dotnet.sdk.6.0 -t dotnet-dev6 .
docker build -f Dockerfile.dotnet.sdk.7.0 -t dotnet-dev7 .
docker build -f Dockerfile.dotnet.sdk.8.0 -t dotnet-dev8 .
docker build -f Dockerfile.dotnet.sdk.8.0 -t dotnet-dev8 .
```

Per avviare il container usare il seguente comando:

```bash
docker run -it --rm -v ${PWD}:/app -p 5000:5000 -p 5001:5001 dotnet-dev
```
Per installare il repostitory sotto /etc/apt usare i seguenti comandi:

```bash
wget https://packages.microsoft.com/config/debian/12/packages-microsoft-prod.deb -O packages-microsoft-prod.deb

dpkg -i packages-microsoft-prod.deb 

apt update

apt search dotnet-sdk
```

ed installare la versione di dotnet interessata

```bash
apt search dotnet-sdk
```
Ora supponendo di aver tutte le versioni disponibili di dotnet-sdk all'interno del container, per creare applicazioni che si basano su framework differenti si dovrebbero usare i seguenti comandi:


# COMANDI PER GESTIRE MULTIPLE VERSIONI .NET ===

1. Verificare tutte le versioni installate
```
dotnet --list-sdks
```


# 2. Creare applicazioni con versioni specifiche:

# Creare app con .NET 6.0
dotnet new console -n MiaApp60 -f net6.0

# Creare app con .NET 7.0  
dotnet new console -n MiaApp70 -f net7.0

# Creare app con .NET 8.0
dotnet new console -n MiaApp80 -f net8.0

# 3. Usare global.json per forzare una versione specifica in una directory
echo '{
  "sdk": {
    "version": "6.0.425"
  }
}' > global.json

# 4. Verificare quale versione verrà usata
dotnet --version

# === COMANDI DOCKER BASE ===

# Costruire l'immagine Docker
docker build -t dotnet-dev-debian .

# Eseguire il container in modalità interattiva
docker run -it --rm -p 5000:5000 -p 5001:5001 dotnet-dev

# Eseguire il container con un volume persistente per i progetti
docker run -it --rm -v C:\percorso\ai\tuoi\progetti:/app -p 5000:5000 -p 5001:5001 dotnet-dev

# Eseguire il container in background
docker run -d --name dotnet-container -v ${PWD}:/app -p 5000:5000 -p 5001:5001 dotnet-dev

# Entrare nel container se è in esecuzione in background
docker exec -it dotnet-container bash

# Fermare e rimuovere il container
docker stop dotnet-container
docker rm dotnet-container
