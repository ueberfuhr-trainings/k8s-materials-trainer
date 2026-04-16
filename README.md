# Trainerleitfaden: Kubernetes/OpenShift (2 Tage)

## Zielgruppe & Voraussetzungen

- Entwickler mit Docker-Grundkenntnissen (Container, Images, Dockerfile, Compose)
- Keine Kubernetes-Vorkenntnisse nötig
- Zugang zu einem OpenShift-Cluster (z.B. OpenShift Local / CRC oder gehostetes Lab)

---

## Tagesplan

### Tag 1 — Grundlagen & erste Deployments

| Zeit          | Dauer  | Typ     | Inhalt                                                                                                                                                           |
|---------------|--------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 09:00 – 09:15 | 15 min | Warm-up | Vorstellungsrunde, Kursüberblick, Erwartungen sammeln                                                                                                            |
| 09:15 – 09:30 | 15 min | Quiz    | **Docker Grundlagen-Check** — jeder für sich, dann kurz besprechen                                                                                               |
| 09:30 – 10:45 | 75 min | Theorie | **Was ist Kubernetes?** — [Problembasierte Überleitung](problembasierte-ueberleitung.md), [Architektur-Aufbau](architektur-aufbau.md), [Analogien](analogien.md) |
| 10:45 – 11:00 | 15 min | Pause   |                                                                                                                                                                  |
| 11:00 – 11:45 | 45 min | Theorie | **Was ist OpenShift?** — Unterschiede, SCCs, Routes, Web Console                                                                                                 |
| 11:45 – 12:15 | 30 min | Theorie | **Kubernetes-Ressourcen (Teil 1)** — Pod, Deployment, ReplicaSet, Service                                                                                        |
| 12:15 – 13:15 | 60 min | Pause   | Mittagspause                                                                                                                                                     |
| 13:15 – 13:30 | 15 min | Demo    | **CLI Cheat Sheet** — Cluster-Zugang zeigen, `oc`-Befehle live                                                                                                   |
| 13:30 – 14:30 | 60 min | Übung   | **Übung 1: Backend-Deployment** — Deployment, Service, Route                                                                                                     |
| 14:30 – 14:45 | 15 min | Pause   |                                                                                                                                                                  |
| 14:45 – 15:00 | 15 min | Theorie | **Konfiguration** — ConfigMaps, Secrets, Environment Variables                                                                                                   |
| 15:00 – 15:45 | 45 min | Übung   | **Übung 2: Frontend-Deployment** — Env-Variablen, Debugging                                                                                                      |
| 15:45 – 16:30 | 45 min | Retro   | Tagesrückblick, offene Fragen, Ausblick Tag 2                                                                                                                    |

### Tag 2 — ConfigMaps, Stateful Workloads, Health Checks & Helm

| Zeit          | Dauer  | Typ      | Inhalt                                                              |
|---------------|--------|----------|---------------------------------------------------------------------|
| 09:00 – 09:30 | 30 min | Warm-up  | Feedback- & Wiederholungsrunde Tag 1, Reflexionsfragen Übung 2 besprechen, offene Fragen klären |
| 09:30 – 09:50 | 20 min | Übung    | **Übung 3: ConfigMaps** — Konfiguration auslagern                   |
| 09:50 – 10:05 | 15 min | Theorie  | **PostgreSQL auf Kubernetes** — Best Practices, Operatoren          |
| 10:05 – 10:35 | 30 min | Übung    | **Übung 4: PostgreSQL-Datenbank** — Secrets, Init-SQL, Volumes      |
| 10:35 – 10:50 | 15 min | Pause    |                                                                     |
| 10:50 – 11:20 | 30 min | Übung    | **Übung 5: Backend auf PostgreSQL** — Service Discovery, DNS        |
| 11:20 – 11:35 | 15 min | Theorie  | **Health Checks (Probes)** — Startup, Liveness, Readiness           |
| 11:35 – 12:05 | 30 min | Übung    | **Übung 6: Probes** — Health Checks für alle Services               |
| 12:05 – 13:05 | 60 min | Pause    | Mittagspause                                                        |
| 13:05 – 13:25 | 20 min | Theorie  | **Helm** — Paketmanagement, Chart-Struktur                          |
| 13:25 – 14:10 | 45 min | Übung    | **Helm-Übung** — Bestehendes Setup als Chart                        |
| 14:10 – 14:25 | 15 min | Pause    |                                                                     |
| 14:25 – 14:55 | 30 min | Optional | **Helm-Deployment via Jenkins** — CI/CD-Automatisierung             |
| 14:55 – 15:45 | 50 min | Retro    | Gesamtrückblick, Feedback, weiterführende Ressourcen                |

---

## Didaktische Hinweise

### Die Theorie-Blöcke am Anfang lebendig gestalten

Der erste Tag hat einen großen Theorie-Anteil, bevor die Teilnehmer selbst loslegen. Das ist die kritischste Phase — hier entscheidet sich, ob die Gruppe abschaltet oder mitdenkt. Folgende Ideen helfen:

#### 1. Docker-Quiz als Eisbrecher nutzen
- Jeder Teilnehmer löst das Quiz **selbst** (15 min).
- Danach kurz in der Gruppe besprechen: "Welche Fragen fandet ihr knifflig?"
- Zeigt dem Trainer, wo die Gruppe steht und schafft eine gemeinsame Wissensbasis.

#### 2. Von Docker nach Kubernetes überleiten — nicht "erklären"
- Detaillierte Problemstellungen und Gesprächsführung: **[problembasierte-ueberleitung.md](problembasierte-ueberleitung.md)**

#### 3. Architektur schrittweise aufbauen
- Fragenbasierter Aufbau der Cluster-Architektur an der Tafel: **[architektur-aufbau.md](architektur-aufbau.md)**

#### 4. Analogien und Vergleiche nutzen
- Durchgängiges Analogie-Beispiel (Restaurant-Küche) für alle Kubernetes-Konzepte: **[analogien.md](analogien.md)**

#### 5. OpenShift-Block interaktiv gestalten
- Nicht alle Unterschiede aufzählen, sondern **die Web Console live zeigen**.
- Teilnehmer einladen: "Was fällt euch auf, was es bei purem Kubernetes nicht gibt?"
- SCCs als Sicherheitsfeature greifbar machen: "Versucht mal, einen Container als Root zu starten — was passiert?"

#### 6. Regelmäßige Mikro-Checks einbauen
- Nach jedem Theorie-Block **eine kurze Verständnisfrage** an die Gruppe:
  - "Wenn ich 3 Replicas will und ein Node stirbt — was passiert?"
  - "Worin unterscheidet sich ein Service von einer Route?"
- Nicht als Test, sondern als Denkanstoß — Antworten gemeinsam erarbeiten.

#### 7. CLI-Demo als Brücke zur ersten Übung
- Die CLI-Demo nach der Mittagspause holt die Gruppe zurück ins Tun.
- **Live am Cluster** zeigen: einloggen, `oc get nodes`, `oc get pods -A`, `oc new-project`.
- Dabei die zuvor besprochene Architektur "anfassbar" machen.
- Direkt überleiten: "Jetzt seid ihr dran — Übung 1."

---

### Übungen begleiten

- **Nicht zu früh helfen.** Die Reflection-Fragen am Ende jeder Übung sind absichtlich da — sie fördern eigenständiges Nachdenken.
- **Fehler zulassen.** Übung 1 provoziert bewusst einen Pod-Fehler (falsches Image-Tag). Das ist ein Lernmoment, kein Problem.
- **Pair Programming anbieten** — bei heterogenen Gruppen können stärkere und schwächere Teilnehmer zusammenarbeiten.
- **Gemeinsame Besprechung** nach jeder Übung (5–10 min): "Was war überraschend? Was hat nicht funktioniert?" — besonders die Reflection-Fragen gemeinsam durchgehen.

### Zeitpuffer

- Die Zeiten sind knapp kalkuliert. Wenn eine Übung länger dauert, lieber die nächste Theorie kürzen als die Übung abbrechen.
- Der Helm-Block und der optionale Jenkins-Slot am Nachmittag von Tag 2 sind bewusst als **Puffer** eingeplant. Wenn die Übungen länger dauern, kann Helm als Ausblick in 15 min abgehandelt und Jenkins entfallen.
- Bei schnellen Gruppen: Helm-Deployment via Jenkins live durchführen oder zusätzliche Szenarien (Skalierung live zeigen, Rolling Update simulieren, Pod löschen und Selbstheilung beobachten).

---

## Checkliste vor dem Kurs

- [ ] OpenShift-Cluster verfügbar und getestet
- [ ] Pro Teilnehmer ein eigenes Projekt/Namespace angelegt
- [ ] `oc` CLI auf allen Rechnern installiert
- [ ] Docker Hub erreichbar (für Image-Pull)
- [ ] Zugangsdaten / Login-Tokens vorbereitet
- [ ] Docker Grundlagen-Quiz im Browser aufrufbar
- [ ] Schulungsunterlagen (GitHub Pages) erreichbar
- [ ] Beamer/Screen-Sharing für Live-Demos bereit
