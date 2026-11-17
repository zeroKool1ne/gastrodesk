# n8n für GastroDesk

n8n läuft lokal in einem Container und orchestriert den Ablauf: Mail kommt
rein, wird klassifiziert, landet im Reservierungsbuch oder geht zur Freigabe.

## Starten

```
cd n8n
docker compose up -d
```

Dann `http://localhost:5678` im Browser öffnen. Beim ersten Start legt n8n ein
Besitzerkonto an — Mail und Passwort sind rein lokal, kein Cloud-Konto nötig.

## Stoppen

```
docker compose down
```

Workflows und Zugangsdaten liegen im Docker-Volume `n8n_data` und überleben das.
Erst `docker compose down -v` löscht sie.

## Warum ein benanntes Volume und kein Ordner

n8n läuft im Container als Benutzer `node` (UID 1000). Ein Bind-Mount auf einen
Ordner des Macs führt je nach Setup zu Rechteproblemen beim ersten Start. Das
benannte Volume umgeht das. Nachteil: Man kann nicht einfach hineinschauen.
Dafür gibt es den Workflow-Export (siehe unten).

## Versionspinnung — offen

Das Image steht auf `:latest`. Für ein Portfolio-Repo ist das zu wackelig,
weil ein Prüfer in sechs Monaten eine andere Version zieht als die, gegen die
hier entwickelt wurde. Sobald n8n einmal läuft:

```
docker image inspect n8nio/n8n:latest --format '{{index .RepoDigests 0}}'
```

und die gemeldete Version dann in der Compose-Datei festnageln.

## Workflows ins Repo

n8n speichert Workflows in seiner internen Datenbank, nicht als Datei. Damit
sie im Repository nachvollziehbar sind, werden sie in der n8n-Oberfläche
exportiert (Workflow-Menü → Download) und als JSON unter `n8n/workflows/`
abgelegt.
