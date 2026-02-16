# Hydra Server – Docker Stack

Dieses Repository enthält den Docker-Stack für den Betrieb des Hydra Servers.

- 🌐 **Website:** [hydra-karte.de](https://hydra-karte.de)
- 📖 **Docs:** [docs.hydra-karte.de](https://docs.hydra-karte.de)
- 🐳 **Docker Hub:** [hydrakarte/hydra-server](https://hub.docker.com/r/hydrakarte/hydra-server)


## Voraussetzungen
- AMD64 / x86-64 basierter Host
- Docker
- Docker Compose


## Installation

### 1. Dateien bereitstellen

Du hast zwei Möglichkeiten:

**Variante A: Repository klonen**
```bash
git clone https://github.com/hydra-karte/Hydra-Server.git
cd hydra-server
```

**Variante B: Dateien manuell erstellen**

Erstelle in einem leeren Verzeichnis die folgenden Dateien:

* `docker-compose.yml`
* `config.env`

und kopiere den jeweiligen Inhalt hinein.

### 2. Datenbank-Zugangsdaten anpassen

Öffne die Datei `config.env` und passe die Datenbank-Zugangsdaten an.

```env
DATABASE_USER=hydra
DATABASE_PASS=
DATABASE_NAME=hydra
```

Erzeuge ein **zufälliges, sicheres Passwort**, z. B.:

```bash
openssl rand -base64 32
```

und trage es bei `DATABASE_PASS` ein.

---

### 3. Auth-Secret für Session-Authentifizierung erzeugen

Der Server benötigt ein Secret für die Session-Authentifizierung.

```env
AUTH_SECRET=
```

Erzeuge auch hier ein sicheres Secret, z. B.:

```bash
openssl rand -base64 48
```

**Wichtig:**
Dieses Secret sollte **nachträglich nicht geändert** werden, da sonst bestehende Sessions ungültig werden.


### 4. Instanz-Daten konfigurieren

Diese Werte werden in der Weboberfläche angezeigt und für externe Links genutzt.

```env
INSTANCE_NAME=My Hydra Server
INSTANCE_URL=https://example.com
```

* `INSTANCE_NAME`
  Anzeigename der Instanz (z. B. Organisation oder Standort)

* `INSTANCE_URL`
  Öffentliche URL, unter der der Server erreichbar ist
  (inkl. `https://`)

### 5. SMTP / Mail-Einstellungen vornehmen

Der Server kann E-Mails versenden (z. B. Provisionierungen).

```env
MAIL_ENABLED=true
SMTP_SECURE=true
SMTP_HOST=smtp.example.com
SMTP_PORT=587
SMTP_USER=user@example.com
SMTP_PASS=yourpassword
SMTP_FROM=from@example.com
```

**Hinweise:**

* `MAIL_ENABLED=false` deaktiviert den Mailversand vollständig
* `SMTP_SECURE=true` nutzt TLS (empfohlen)
* `SMTP_FROM` ist die Absender-Adresse


## Stack starten

Starte den Stack im Verzeichnis mit der `docker-compose.yml`:

```bash
docker compose up -d
```

Der Server wartet automatisch, bis die Datenbank bereit ist.


## Ports & Reverse Proxy

Standardmäßig werden **keine Ports direkt veröffentlicht**.

```yaml
#ports:
# - "8080:8080"
```

**Empfohlen:**
Betrieb hinter einem Reverse Proxy (z. B. Nginx, Apache, Traefik, Caddy) mit TLS.


## Datenpersistenz

Die Datenbank wird in einem Docker Volume gespeichert:

```yaml
volumes:
  db_data:
```

Ein Neustart oder Update des Stacks löscht **keine Daten**.

## Update

Um auf die neueste Version zu aktualisieren:

```bash
docker compose pull
docker compose up -d
```

## Hinweise

* Änderungen an der `config.env` werden beim **nächsten Start** übernommen
* Einige Konfigurationswerte können zusätzlich über die Weboberfläche geändert werden
* Docker-Environment-Variablen haben beim Start Vorrang

## Lizenz

Hydra Server ist unter der PolyForm Strict License 1.0.0 lizenziert.
Kommerzielle Nutzungslizenzen sind auf Anfrage erhältlich.

Hydra Server is licensed under the PolyForm Strict License 1.0.0.
Commercial licensing options are available upon request.