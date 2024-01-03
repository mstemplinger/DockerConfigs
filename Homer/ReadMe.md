
Als erstes verbinden wir uns zum PI:
Dazu starten wir eine CMD und geben folgenden Befehl ein. 

(Dabei ist die IP entsprechend anzupassen.)

```
ssh pi@192.168.2.2
```

Nachdem man sich erfolgreich am PI angemeldet hat, wechselt man im home Verzeichnis in das Unterverzeichnis vom Homer:

```
cd Homer
```

Im Anschluss, holt man den aktuellen Stand vom GitHub ab:

```
sudo git pull https://github.com/mstemplinger/Homer.git main
```



Auf dem Windows Rechner kann man mittels dem Git Desktop arbeiten.

Änderungen können lokal vorgenommen werden.
Updates dann mittels Push ins Git laden.
Mit dem Befehl unter 3. wieder auf den Pi updaten.

Homer evtl. neu starten.
