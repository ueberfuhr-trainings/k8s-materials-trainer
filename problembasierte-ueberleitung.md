# Von Docker nach Kubernetes — problembasierte Überleitung

## Grundidee

**Nicht starten mit:** "Kubernetes ist ein Container-Orchestrierungssystem, das..."

**Stattdessen:** Ein gemeinsames Szenario aufbauen, in dem Docker Compose an seine Grenzen stößt. Die Teilnehmer erarbeiten die Probleme selbst — Kubernetes wird zur logischen Antwort.

---

## Einstiegsszenario

> "Stellt euch vor: Ihr habt unsere Rezepte-App — Backend, Frontend, Datenbank — mit Docker Compose auf einem Server deployed. Läuft wunderbar. Euer Chef ist begeistert und sagt: 'Das geht in Produktion.' Was könnte schiefgehen?"

Kurz sammeln lassen (Zuruf oder Whiteboard), dann gezielt nachbohren:

---

## Problemstellungen zum Durchgehen

### 1. Der Server fällt aus (Self Healing, Clustering)

> "Es ist Freitagabend, euer Server stürzt ab. Docker Compose liegt auf genau dieser einen Maschine. Was passiert?"

**Erwartete Erkenntnis:** Docker Compose kennt nur einen Host. Fällt er aus, ist alles weg. Es gibt keine automatische Wiederherstellung.

**Überleitung:** "Wir brauchen etwas, das Container auf **mehreren Maschinen** verteilen und bei Ausfall automatisch woanders neu starten kann."

---

### 2. Der Online-Shop wird im Fernsehen vorgestellt (Autoscaling)

> "Eure App wird in einer TV-Sendung erwähnt. Plötzlich kommen 50x so viele Anfragen. Wie skaliert ihr mit Docker Compose?"

**Erwartete Erkenntnis:** Man kann `docker compose up --scale backend=5` machen, aber nur auf derselben Maschine. Irgendwann ist die Maschine voll. Und wer verteilt die Anfragen auf die 5 Instanzen?

**Nachfrage:** "Und wer entscheidet, wann runterskaliert wird, wenn der Hype vorbei ist?"

**Überleitung:** "Wir brauchen etwas, das automatisch erkennt, wie viele Instanzen nötig sind, und sie auf mehrere Maschinen verteilt."

---

### 3. Update ohne Downtime (Deployment Rollout)

> "Ihr habt ein Bug-Fix für das Backend. Wie deployed ihr das neue Image, ohne dass Nutzer einen Fehler sehen?"

**Erwartete Erkenntnis:** `docker compose up -d` startet den Container neu — dazwischen gibt es eine Lücke. Kein Rolling Update, kein Canary, kein Rollback.

**Nachfrage:** "Und was macht ihr, wenn das neue Image einen Bug hat und ihr sofort zurück wollt?"

**Überleitung:** "Wir brauchen etwas, das neue Versionen schrittweise ausrollt und bei Problemen automatisch zurückrollt."

---

### 4. Wer startet den Container nach einem Crash neu? (Health Checks, Self Healing)

> "Euer Backend hat einen Memory Leak. Nach 2 Stunden geht dem Container der Speicher aus und er stirbt. Was passiert?"

**Erwartete Erkenntnis:** Docker Compose hat `restart: always`, aber das ist primitiv — es erkennt nicht, ob die App intern hängt (z.B. Deadlock). Es startet nur neu, wenn der Prozess stirbt.

**Nachfrage:** "Was wäre, wenn die App zwar läuft, aber keine Anfragen mehr beantwortet?"

**Überleitung:** "Wir brauchen Health Checks, die erkennen, ob eine App **wirklich** funktioniert — und automatisch reagieren."

---

### 5. Konfiguration und Geheimnisse (Credential Vaults)

> "Euer Backend braucht Datenbankpasswörter. Wo liegen die bei Docker Compose?"

**Erwartete Erkenntnis:** Oft in der `docker-compose.yml` oder in `.env`-Dateien — im Klartext, vielleicht sogar im Git-Repo.

**Nachfrage:** "Und wie regelt ihr, dass Entwickler die Prod-Passwörter nicht sehen?"

**Überleitung:** "Wir brauchen ein System, das Konfiguration und Geheimnisse zentral verwaltet, mit Zugriffsrechten."

---

### 6. Mehrere Teams, eine Infrastruktur (Multi-tenancy)

> "Jetzt gibt es nicht nur eure App, sondern auch Team B mit einem Bestellsystem und Team C mit einem Reporting-Tool. Alle sollen auf derselben Infrastruktur laufen. Wie trennt ihr das?"

**Erwartete Erkenntnis:** Docker Compose hat kein Konzept für Mandantenfähigkeit. Jedes Team braucht seinen eigenen Server oder es gibt Konflikte (Ports, Ressourcen, Sichtbarkeit).

**Überleitung:** "Wir brauchen Namespaces, Ressourcenlimits und Zugriffsrechte — eine Art Mehrmandantenfähigkeit."

---

## Zusammenführung am Whiteboard

Sammle die erkannten Probleme als Liste:

| Problem                          | Docker Compose       | Was wir bräuchten                        |
|----------------------------------|----------------------|------------------------------------------|
| Server fällt aus                 | Kein Failover        | Multi-Node, automatischer Neustart       |
| Mehr Traffic                     | Nur vertikal (1 Host)| Horizontale Skalierung auf viele Hosts   |
| Update ohne Downtime             | Stop → Start         | Rolling Update, Rollback                 |
| App hängt                        | Nur Prozess-Restart  | Health Checks mit Reaktion               |
| Geheimnisse                      | .env im Klartext     | Zentrale, abgesicherte Verwaltung        |
| Mehrere Teams                    | Keine Isolation      | Namespaces, RBAC, Ressourcenlimits       |

> "All das zusammen — das ist Kubernetes. Kein Hexenwerk, sondern Antworten auf reale Probleme, die ihr gerade selbst erkannt habt."

---

## Tipps zur Durchführung

- **Nicht alle 6 Probleme durchgehen, wenn die Gruppe von allein drauf kommt.** Wenn nach Problem 2 jemand sagt "Da bräuchte man ja eine Art Cluster-Manager" — perfekt, aufgreifen und überleiten.
- **Teilnehmer reden lassen.** Die Probleme sollen von ihnen kommen, nicht vom Trainer.
- **Am Whiteboard mitschreiben.** Die Tabelle am Ende entsteht organisch, nicht als fertige Folie.
- **Zeitrahmen:** 15–20 Minuten reichen. Danach in die Architektur-Erklärung überleiten.
