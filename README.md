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
| 09:00 – 09:15 | 15 min | Warm-up | Vorstellungsrunde, Kursüberblick, Erwartungen sammeln — **[Schulungsunterlagen](https://ueberfuhr-trainings.github.io/k8s-materials/) mitgeben**                 |
| 09:15 – 09:30 | 15 min | Quiz    | **[Docker Grundlagen-Check](https://ueberfuhr-trainings.github.io/k8s-materials/reflections/docker-grundlagen.html)** — jeder für sich, dann kurz besprechen     |
| 09:30 – 10:45 | 75 min | Theorie | **Was ist Kubernetes?** — [Problembasierte Überleitung](problembasierte-ueberleitung.md), [Architektur-Aufbau](architektur-aufbau.md), [Doku](https://ueberfuhr-trainings.github.io/k8s-materials/docs/kubernetes.html) |
| 10:45 – 11:00 | 15 min | Pause   |                                                                                                                                                                  |
| 11:00 – 12:00 | 60 min | Theorie + Praxis | **Was ist OpenShift?** — Unterschiede, SCCs, Routes; **Web Console live erkunden** und Zugriff per `oc`-CLI ausprobieren ([CLI Cheat Sheet](https://ueberfuhr-trainings.github.io/k8s-materials/docs/cli-cheatsheet.html)) |
| 12:00 – 13:00 | 60 min | Theorie + Demo | **Kubernetes-Ressourcen (Teil 1)** — Pod, Deployment, ReplicaSet, Service — [Doku](https://ueberfuhr-trainings.github.io/k8s-materials/docs/kubernetes-resources.html). *Trainerhinweis: die Schritte aus Übung 1 gleich live vorführen — dadurch 60 min statt 30* |
| 13:00 – 14:00 | 60 min | Pause   | Mittagspause                                                                                                                                                     |
| 14:00 – 14:30 | 30 min | Übung   | **Übung 1: Backend-Deployment** ([K8s](https://ueberfuhr-trainings.github.io/k8s-materials/issues/kubernetes/01-backend-deployment.html), [OpenShift](https://ueberfuhr-trainings.github.io/k8s-materials/issues/openshift/01-backend-deployment.html)) — Deployment, Service, Route (Schritte wurden bereits vorgeführt) |
| 14:30 – 14:45 | 15 min | Pause   |                                                                                                                                                                  |
| 14:45 – 15:15 | 30 min | Demo    | **[Debugging & Self-Healing](debugging-self-healing.md)** — `oc logs` / `describe` / `exec`, Events lesen; `oc scale`, Pod löschen → Selbstheilung live beobachten |
| 15:15 – 16:00 | 45 min | Übung   | **Übung 2: Frontend-Deployment** ([K8s](https://ueberfuhr-trainings.github.io/k8s-materials/issues/kubernetes/02-frontend-deployment.html), [OpenShift](https://ueberfuhr-trainings.github.io/k8s-materials/issues/openshift/02-frontend-deployment.html)) — Env-Variablen, Debugging |
| 16:00 – 16:30 | 30 min | Retro   | Tagesrückblick, offene Fragen, Ausblick Tag 2                                                                                                                    |

### Tag 2 — ConfigMaps, Stateful Workloads, Health Checks & Helm

| Zeit          | Dauer  | Typ      | Inhalt                                                              |
|---------------|--------|----------|---------------------------------------------------------------------|
| 09:00 – 09:15 | 15 min | Reflexion | **[Kreuzworträtsel Tag 1](https://ueberfuhr-trainings.github.io/k8s-materials/reflections/crossword-tag1.html)** — spielerische Wiederholung, gemeinsam auflösen |
| 09:15 – 09:30 | 15 min | Warm-up  | *(Optional)* [Analogien](analogien.md) (Restaurant-Küche) als roten Faden einführen; offene Fragen Tag 1 & Reflexionsfragen Übung 2 besprechen |
| 09:30 – 09:45 | 15 min | Theorie  | **Konfiguration** — Umgebungsvariablen, ConfigMaps & Secrets. *Trainerhinweis: 12-Factor App — Config raus aus dem Image, in die Umgebung* |
| 09:45 – 10:05 | 20 min | Übung    | **Übung 3: ConfigMaps** ([K8s](https://ueberfuhr-trainings.github.io/k8s-materials/issues/kubernetes/03-configmaps.html), [OpenShift](https://ueberfuhr-trainings.github.io/k8s-materials/issues/openshift/03-configmaps.html)) — Konfiguration auslagern |
| 10:05 – 10:20 | 15 min | Theorie  | **PostgreSQL auf Kubernetes** — Best Practices, Operatoren          |
| 10:20 – 10:50 | 30 min | Übung    | **Übung 4: PostgreSQL-Datenbank** ([K8s](https://ueberfuhr-trainings.github.io/k8s-materials/issues/kubernetes/04-postgresql.html), [OpenShift](https://ueberfuhr-trainings.github.io/k8s-materials/issues/openshift/04-postgresql.html)) — Secrets, Init-SQL, Volumes |
| 10:50 – 11:05 | 15 min | Pause    |                                                                     |
| 11:05 – 11:35 | 30 min | Übung    | **Übung 5: Backend auf PostgreSQL** ([K8s](https://ueberfuhr-trainings.github.io/k8s-materials/issues/kubernetes/05-backend-postgresql.html), [OpenShift](https://ueberfuhr-trainings.github.io/k8s-materials/issues/openshift/05-backend-postgresql.html)) — Service Discovery, DNS |
| 11:35 – 11:50 | 15 min | Theorie  | **Health Checks (Probes)** — Startup, Liveness, Readiness           |
| 11:50 – 12:20 | 30 min | Übung    | **Übung 6: Probes** ([K8s](https://ueberfuhr-trainings.github.io/k8s-materials/issues/kubernetes/07-probes.html), [OpenShift](https://ueberfuhr-trainings.github.io/k8s-materials/issues/openshift/07-probes.html)) — Health Checks für alle Services |
| 12:20 – 13:20 | 60 min | Pause    | Mittagspause                                                        |
| 13:20 – 13:40 | 20 min | Theorie  | **Helm** — Paketmanagement, Chart-Struktur                          |
| 13:40 – 14:25 | 45 min | Übung    | **Helm-Übung** ([K8s](https://ueberfuhr-trainings.github.io/k8s-materials/issues/kubernetes/08-helm-charts.html), [OpenShift](https://ueberfuhr-trainings.github.io/k8s-materials/issues/openshift/08-helm-charts.html)) — Bestehendes Setup als Chart |
| 14:25 – 14:40 | 15 min | Pause    |                                                                     |
| 14:40 – 15:10 | 30 min | Optional | **Helm-Deployment via Jenkins** — CI/CD-Automatisierung             |
| 15:10 – 16:00 | 50 min | Retro    | Gesamtrückblick, Feedback, weiterführende Ressourcen                |

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

#### 4. Analogien erst zu Beginn von Tag 2 (optional)
- Das durchgängige Analogie-Beispiel (Restaurant-Küche) **nicht** am ersten Tag verbrennen, sondern — wenn überhaupt — zu Beginn von Tag 2 als roten Faden einführen: **[analogien.md](analogien.md)**
- Tag 2 startet stattdessen mit einer spielerischen Reflexion: dem **[Kreuzworträtsel Tag 1](https://ueberfuhr-trainings.github.io/k8s-materials/reflections/crossword-tag1.html)**. Gemeinsam auflösen, das Lösungswort schafft die Brücke zu den Analogien.

#### 5. OpenShift-Block interaktiv gestalten (60 min, Theorie + Praxis)
- Nicht alle Unterschiede aufzählen, sondern **die Web Console live zeigen** — und die Teilnehmer **selbst darin stöbern lassen**.
- Direkt den CLI-Zugang mit `oc` mitnehmen: einloggen, `oc get nodes`, `oc get pods -A`, `oc new-project`. Als Referenz das **[CLI Cheat Sheet](https://ueberfuhr-trainings.github.io/k8s-materials/docs/cli-cheatsheet.html)** an die Gruppe geben und Befehle ausprobieren lassen.
- Teilnehmer einladen: "Was fällt euch auf, was es bei purem Kubernetes nicht gibt?"
- SCCs als Sicherheitsfeature greifbar machen: "Versucht mal, einen Container als Root zu starten — was passiert?"
- So werden aus reiner Theorie locker 45–60 min, in denen die Gruppe schon aktiv am Cluster ist. Ein separater CLI-Cheat-Sheet-Block nach dem Mittag entfällt dadurch.

#### 6. Regelmäßige Mikro-Checks einbauen
- Nach jedem Theorie-Block **eine kurze Verständnisfrage** an die Gruppe:
  - "Wenn ich 3 Replicas will und ein Node stirbt — was passiert?"
  - "Worin unterscheidet sich ein Service von einer Route?"
- Nicht als Test, sondern als Denkanstoß — Antworten gemeinsam erarbeiten.

#### 7. Nach dem Mittag direkt in die erste Übung
- Der CLI-Zugang wurde bereits im OpenShift-Block (vor dem Mittag) angefasst — daher nach der Pause **kein separater CLI-Demo-Block** mehr nötig.
- Kurz die zuvor besprochene Architektur "anfassbar" machen (`oc get nodes`, `oc get pods -A`) und direkt überleiten: "Jetzt seid ihr dran — Übung 1."

#### 8. "Vorführen, dann selbst machen" — und der Block zwischen zwei Übungen
- Im Theorie-Block **Kubernetes-Ressourcen (Teil 1)** die Schritte aus Übung 1 ([K8s](https://ueberfuhr-trainings.github.io/k8s-materials/issues/kubernetes/01-backend-deployment.html), [OpenShift](https://ueberfuhr-trainings.github.io/k8s-materials/issues/openshift/01-backend-deployment.html)) direkt **live vorführen**. Die Teilnehmer sehen den Weg einmal komplett — danach brauchen sie für Übung 1 nur noch **30 min** statt 60.
- Zwischen Übung 1 und Übung 2 liegt sonst kein Puffer. Vorschlag für den Zwischenblock: **[Debugging & Self-Healing](debugging-self-healing.md)** live — `oc logs`/`describe`/`exec`, Events lesen, dann `oc scale` hochskalieren und einen Pod löschen, um die Selbstheilung zu beobachten. Das bereitet den Debugging-Teil von Übung 2 vor und bringt nach der ersten Übung wieder Energie rein. Detaillierter Ablauf mit allen Befehlen: **[debugging-self-healing.md](debugging-self-healing.md)**.
  - *Alternativen, falls es besser passt:* gemeinsame Besprechung der Übung-1-Ergebnisse, ein Mikro-Quiz oder ein kurzer Rolling-Update-Demo.
- Das Thema **Konfiguration (12-Factor App, Umgebungsvariablen)** ist bewusst auf **Tag 2** gewandert — direkt vor Übung 3 (ConfigMaps), wo es seine eigene Übung hat und inhaltlich hingehört.

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
- [ ] Kreuzworträtsel Tag 1 (Reflexion zu Beginn Tag 2) im Browser aufrufbar
- [ ] CLI Cheat Sheet für die Teilnehmer griffbereit (Link)
- [ ] Schulungsunterlagen (GitHub Pages) erreichbar
- [ ] Beamer/Screen-Sharing für Live-Demos bereit
