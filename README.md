# docker

Per creare un container eseguire i seguenti comandi di esempio:

```bash
docker build -t dotnet-dev .
```

Per avviare il container usare il seguente comando:

```bash
docker run -it --rm -v ${PWD}:/app -p 5000:5000 -p 5001:5001 dotnet-dev
```
