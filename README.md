# Hydra Server – Docker Stack

Dieses Repository enthält den Docker-Stack für den Betrieb des **Hydra Servers**.

* 🌐 **Website:** [https://hydra-karte.de](https://hydra-karte.de)
* 📖 **Docs:** [https://docs.hydra-karte.de](https://docs.hydra-karte.de)
* 🐳 **Docker Hub:** [https://hub.docker.com/r/hydrakarte/hydra-server](https://hub.docker.com/r/hydrakarte/hydra-server)

---

## Voraussetzungen

* AMD64 / x86-64 basierter Host
* Docker
* Docker Compose (`docker compose`)

---

# Installation

## 1. Dateien bereitstellen

Du hast zwei Möglichkeiten:

### Variante A – Repository klonen

```bash
git clone https://github.com/hydra-karte/Hydra-Server.git
cd Hydra-Server
```

### Variante B – Dateien manuell erstellen

Erstelle in einem leeren Verzeichnis die folgenden Dateien:

* `docker-compose.yml`
* `config.env`

und kopiere den jeweiligen Inhalt hinein.

---

# Betriebsarten

Es stehen zwei Compose-Varianten zur Verfügung:

## 🔹 Variante A – Direktbetrieb (ohne Reverse Proxy)

* Zugriff über:
  `http://[IP-Adresse]:3000`
* Port 3000 wird direkt veröffentlicht
* Geeignet für:

  * interne Netzwerke
  * Testumgebungen
  * einfache Installationen

## 🔹 Variante B – Betrieb mit Reverse Proxy (z. B. Traefik)

* Keine direkten Ports werden veröffentlicht
* Zugriff erfolgt über eine Domain (z. B. `https://example.com`)
* Empfohlen für:

  * Produktivbetrieb
  * öffentliche Erreichbarkeit
  * HTTPS/TLS

---

# Konfiguration (`config.env`)

## 2. Datenbank-Zugangsdaten anpassen

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

⚠️ Der Server startet nicht korrekt, wenn `DATABASE_PASS` leer bleibt.

---

## 3. Auth-Secret für Session-Authentifizierung erzeugen

```env
AUTH_SECRET=
```

Erzeuge ein sicheres Secret, z. B.:

```bash
openssl rand -base64 48
```

**Wichtig:**
Dieses Secret darf nachträglich nicht geändert werden, da sonst bestehende Sessions ungültig werden.

⚠️ `AUTH_SECRET` darf nicht leer sein.

---

## 4. Instanz-Daten konfigurieren

```env
INSTANCE_URL=https://example.com
```

### `INSTANCE_URL`

Öffentliche URL, unter der der Server erreichbar ist.
Diese Angabe ist zwingend erforderlich und muss mit `http://` oder `https://` beginnen.
Es darf kein Pfad enthalten sein (z. B. korrekt: `https://example.com`).

---

### Optionale Anzeigenamen

Diese Werte können auch später über die Weboberfläche konfiguriert werden:

```env
# INTERNAL_INSTANCENAME=Hydranten SG Beispielhausen
# CLOUD_INSTANCENAME=SG Beispielhausen
```

* `INTERNAL_INSTANCENAME`
  Anzeigename der Instanz (Organisation/Standort)

* `CLOUD_INSTANCENAME`
  Öffentlicher Anzeigename in der Hydra Cloud

---

## 5. SMTP / Mail-Einstellungen

Der Server kann E-Mails versenden (z. B. Provisionierungen).
Dafür wird ein SMTP-Server benötigt.

```env
MAIL_ENABLED=true
SMTP_SECURE=true
SMTP_HOST=smtp.example.com
SMTP_PORT=587
SMTP_USER=user@example.com
SMTP_PASS=yourpassword
SMTP_FROM=from@example.com
```

### Hinweise

* `MAIL_ENABLED=false` deaktiviert den Mailversand
* `SMTP_SECURE=true` aktiviert TLS (empfohlen)
* `SMTP_FROM` ist die Absender-Adresse

---

# Stack starten

Im Verzeichnis mit der `docker-compose.yml`:

```bash
docker compose up -d
```

Der Server wartet automatisch, bis die Datenbank vollständig bereit ist.

---

# Datenpersistenz

Die Datenbank wird in einem Docker Volume gespeichert:

```yaml
volumes:
  db_data:
```

Ein Neustart oder Update des Stacks löscht **keine Daten**.

---

# Update

### Wenn Watchtower aktiviert ist

Updates erfolgen automatisch.

### Manuelles Update

```bash
docker compose pull
docker compose up -d
```

---

# Hinweise

* Änderungen an der `config.env` werden beim nächsten Container-Neustart übernommen
* Einige Einstellungen können zusätzlich über die Weboberfläche geändert werden
* Docker-Environment-Variablen haben beim Start Vorrang

---

# Lizenz

Hydra Server ist unter der **PolyForm Strict License 1.0.0** lizenziert.
Kommerzielle Nutzungslizenzen sind auf Anfrage erhältlich.

Hydra Server is licensed under the PolyForm Strict License 1.0.0.
Commercial licensing options are available upon request.