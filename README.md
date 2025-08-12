# docker

Per creare un container eseguire i seguenti comandi di esempio:

```bash
docker build -t dotnet-dev .
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

