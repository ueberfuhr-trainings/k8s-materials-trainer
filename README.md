# Trainerleitfaden: Kubernetes/OpenShift (2 Tage)

## Zielgruppe & Voraussetzungen

- Entwickler mit Docker-Grundkenntnissen (Container, Images, Dockerfile, Compose)
- Keine Kubernetes-Vorkenntnisse nötig
- Zugang zu einem OpenShift-Cluster (z.B. OpenShift Local / CRC oder gehostetes Lab)

---

## Tagesplan

### Tag 1 — Grundlagen, erste Deployments & Konfiguration

| Zeit          | Dauer  | Typ              | Inhalt                                                                                                                                                                                                                                                                                                                             |
|---------------|--------|------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 09:00 – 09:15 | 15 min | Warm-up          | Vorstellungsrunde, Kursüberblick, Erwartungen sammeln — **[Schulungsunterlagen](https://ueberfuhr-trainings.github.io/k8s-materials/) mitgeben**                                                                                                                                                                                   |
| 09:15 – 09:30 | 15 min | Quiz             | **[Docker Grundlagen-Check](https://ueberfuhr-trainings.github.io/k8s-materials/reflections/docker-grundlagen.html)** — jeder für sich, dann kurz besprechen                                                                                                                                                                       |
| 09:30 – 10:20 | 50 min | Theorie          | **Was ist Kubernetes?** — [Problembasierte Überleitung](problembasierte-ueberleitung.md), [Architektur-Aufbau](architektur-aufbau.md), [Doku](https://ueberfuhr-trainings.github.io/k8s-materials/docs/kubernetes.html)<br>*Trainerhinweis: Recherche durch Lernende vorab — Was kann Kubernetes, was Docker nicht kann? (15 min)* |
| 10:20 – 10:35 | 15 min | Theorie + Praxis | **Was ist OpenShift?** — Unterschiede, SCCs, Routes; **Web Console live erkunden** — [Doku](https://ueberfuhr-trainings.github.io/k8s-materials/docs/openshift.html)                                                                                                                                                               |
| 10:35 – 10:50 | 15 min | Pause            |                                                                                                                                                                                                                                                                                                                                    |
| 10:50 – 11:20 | 30 min | Praxis           | **Erste Schritte in der CLI** — Cluster-Login, erste `oc`-Befehle selbst ausprobieren (`oc get nodes`, `oc get pods -A`, `oc new-project`) — [CLI Cheat Sheet](https://ueberfuhr-trainings.github.io/k8s-materials/docs/cli-cheatsheet.html)                                                                                       |
| 11:20 – 12:00 | 40 min | Theorie + Demo   | **Kubernetes-Ressourcen (Teil 1)** — Pod, Deployment, ReplicaSet, Service — [Doku](https://ueberfuhr-trainings.github.io/k8s-materials/docs/kubernetes-resources.html).<br>*Trainerhinweis: die Schritte aus Übung 1 gleich live vorführen*                                                                                        |
| 12:00 – 13:00 | 60 min | Pause            | Mittagspause                                                                                                                                                                                                                                                                                                                       |
| 13:00 – 13:50 | 50 min | Übung            | **Übung 1: Backend-Deployment** ([K8s](https://ueberfuhr-trainings.github.io/k8s-materials/issues/kubernetes/01-backend-deployment.html), [OpenShift](https://ueberfuhr-trainings.github.io/k8s-materials/issues/openshift/01-backend-deployment.html)) — Deployment, Service, Route (Schritte wurden bereits vorgeführt)          |
| 13:50 – 14:20 | 30 min | Demo             | **[Debugging & Self-Healing](debugging-self-healing.md)** — `oc logs` / `describe` / `exec`, Events lesen; `oc scale`, Pod löschen → Selbstheilung live beobachten                                                                                                                                                                 |
| 14:20 – 14:35 | 15 min | Pause            |                                                                                                                                                                                                                                                                                                                                    |
| 14:35 – 15:15 | 40 min | Übung            | **Übung 2: Frontend-Deployment** ([K8s](https://ueberfuhr-trainings.github.io/k8s-materials/issues/kubernetes/02-frontend-deployment.html), [OpenShift](https://ueberfuhr-trainings.github.io/k8s-materials/issues/openshift/02-frontend-deployment.html)) — Env-Variablen, Debugging                                              |
| 15:15 – 15:30 | 15 min | Theorie          | **Konfiguration** — Umgebungsvariablen, ConfigMaps & Secrets — [Sind Secrets sicher?](https://ueberfuhr-trainings.github.io/k8s-materials/docs/secrets-sicherheit.html)<br/>*Trainerhinweis: 12-Factor App — Config raus aus dem Image, in die Umgebung*                                                                           |
| 15:30 – 15:50 | 20 min | Übung            | **Übung 3: ConfigMaps** ([K8s](https://ueberfuhr-trainings.github.io/k8s-materials/issues/kubernetes/03-configmaps.html), [OpenShift](https://ueberfuhr-trainings.github.io/k8s-materials/issues/openshift/03-configmaps.html)) — Konfiguration auslagern                                                                          |
| 15:50 – 16:20 | 30 min | Retro            | Tagesrückblick, offene Fragen, Ausblick Tag 2                                                                                                                                                                                                                                                                                      |

### Tag 2 — Stateful Workloads, Health Checks & Helm

| Zeit          | Dauer  | Typ       | Inhalt                                                                                                                                                                                                                                                                                                                                                                                                                   |
|---------------|--------|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 09:00 – 09:10 | 10 min | Feedback    | Kurze Feedback-Runde zu Tag 1 (Stimmung, offene Punkte) |
| 09:10 – 09:25 | 15 min | Reflexion   | **Reflexionsfragen zu Übung 3 (ConfigMaps)** gemeinsam besprechen |
| 09:25 – 09:40 | 15 min | Reflexion   | **[Kreuzworträtsel Tag 1](https://ueberfuhr-trainings.github.io/k8s-materials/reflections/crossword-tag1.html)** — spielerische Wiederholung, gemeinsam auflösen |
| 09:40 – 09:55 | 15 min | Theorie     | **PostgreSQL auf Kubernetes** — Best Practices, Operatoren |
| 09:55 – 10:30 | 35 min | Übung       | **Übung 4: PostgreSQL-Datenbank** ([K8s](https://ueberfuhr-trainings.github.io/k8s-materials/issues/kubernetes/04-postgresql.html), [OpenShift](https://ueberfuhr-trainings.github.io/k8s-materials/issues/openshift/04-postgresql.html)) — Secrets, Init-SQL, PGDATA/Volume; anforderungsbasiert, Musterlösung (siehe didaktische Hinweise) |
| 10:30 – 10:45 | 15 min | Pause       |  |
| 10:45 – 11:15 | 30 min | Besprechung | **Übung 4 – Nachbesprechung** — Lösungen vergleichen, den PGDATA/Volume-Fix (OpenShift-Rechte) besprechen, Musterlösung zeigen |
| 11:15 – 11:45 | 30 min | Übung       | **Übung 5: Backend auf PostgreSQL** ([K8s](https://ueberfuhr-trainings.github.io/k8s-materials/issues/kubernetes/05-backend-postgresql.html), [OpenShift](https://ueberfuhr-trainings.github.io/k8s-materials/issues/openshift/05-backend-postgresql.html)) — Service, Service Discovery, DNS |
| 11:45 – 12:15 | 30 min | Übung       | **Übung 6: Persistent Volumes** ([K8s](https://ueberfuhr-trainings.github.io/k8s-materials/issues/kubernetes/06-persistent-volumes.html), [OpenShift](https://ueberfuhr-trainings.github.io/k8s-materials/issues/openshift/06-persistent-volumes.html)) — PVC, StorageClass, Persistenz testen.<br>*Trainerhinweis: entfällt evtl., wenn der Cluster keine Rechte für PVCs / keine StorageClass bietet — dann bleibt das `emptyDir` aus Übung 4.* |
| 12:15 – 13:15 | 60 min | Pause       | Mittagspause |
| 13:15 – 13:30 | 15 min | Theorie     | **Health Checks (Probes)** — Startup, Liveness, Readiness |
| 13:30 – 14:30 | 60 min | Übung       | **Übung 7: Liveness- und Readiness-Probes** ([K8s](https://ueberfuhr-trainings.github.io/k8s-materials/issues/kubernetes/07-probes.html), [OpenShift](https://ueberfuhr-trainings.github.io/k8s-materials/issues/openshift/07-probes.html)) — Health Checks für alle Services |
| 14:30 – 14:45 | 15 min | Pause       |  |
| 14:45 – 15:45 | 60 min | Flex        | **Aus dem Backlog nachrücken** — Helm (Theorie + Übung 8 & 9), danach Jenkins; sonst Puffer für überziehende Übungen |
| 15:45 – 16:15 | 30 min | Retro       | Gesamtrückblick, Feedback, weiterführende Ressourcen |

### Backlog (Tag 2)

Diese Themen sind bewusst **nicht** fest eingeplant. Tag 2 ist so getaktet, dass er ohne Zeitdruck aufgeht — statt Übungen zu kürzen oder zu hetzen, wandern Themen bei Bedarf hierher. Im Nachmittags-Flex (nach dem Mittag) rücken sie in der genannten Reihenfolge nach (oben = höchste Priorität).

| Thema | Aufwand | Empfehlung |
|-------|---------|------------|
| **Helm** (Theorie + **Übung 8: DB-Chart & Helm-Grundlagen** ([K8s](https://ueberfuhr-trainings.github.io/k8s-materials/issues/kubernetes/08-helm-charts.html), [OpenShift](https://ueberfuhr-trainings.github.io/k8s-materials/issues/openshift/08-helm-charts.html)) + **Übung 9: Backend & Frontend** ([K8s](https://ueberfuhr-trainings.github.io/k8s-materials/issues/kubernetes/09-helm-backend-frontend.html), [OpenShift](https://ueberfuhr-trainings.github.io/k8s-materials/issues/openshift/09-helm-backend-frontend.html))) | ~90 min | Paketmanagement & Chart-Struktur, dann Abhängigkeiten. Gut abtrennbar. Im Nachmittags-Flex; bei Zeitnot nur **Übung 8** (DB-Chart + Helm-CLI) machen, Übung 9 als Ausblick. |
| **Helm-Deployment via Jenkins** | ~30 min | CI/CD-Ausblick, für die Grundlagen nicht nötig. Nur bei sehr schneller Gruppe live; sonst als Ausblick zeigen/verweisen. |

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

#### 4. Tag 2 ruhig starten: Feedback → Reflexion → Rätsel
- Erst eine kurze **Feedback-Runde** zu Tag 1 (Stimmung, offene Punkte), dann die **Reflexionsfragen zu Übung 3 (ConfigMaps)** gemeinsam besprechen.
- Zum Aufwärmen dann das **[Kreuzworträtsel Tag 1](https://ueberfuhr-trainings.github.io/k8s-materials/reflections/crossword-tag1.html)** — gemeinsam auflösen, um die Begriffe aus Tag 1 aufzufrischen.

#### 5. OpenShift-Block interaktiv gestalten & CLI-Einstieg abtrennen
- Nicht alle Unterschiede aufzählen, sondern **die Web Console live zeigen** — und die Teilnehmer **selbst darin stöbern lassen**.
- Teilnehmer einladen: "Was fällt euch auf, was es bei purem Kubernetes nicht gibt?"
- SCCs als Sicherheitsfeature greifbar machen: "Versucht mal, einen Container als Root zu starten — was passiert?"
- Der **CLI-Einstieg ist ein eigener Block** direkt nach der Pause: **Erste Schritte in der CLI** — einloggen, `oc get nodes`, `oc get pods -A`, `oc new-project` selbst ausprobieren. Als Referenz das **[CLI Cheat Sheet](https://ueberfuhr-trainings.github.io/k8s-materials/docs/cli-cheatsheet.html)** an die Gruppe geben.

#### 6. Regelmäßige Mikro-Checks einbauen
- Nach jedem Theorie-Block **eine kurze Verständnisfrage** an die Gruppe:
  - "Wenn ich 3 Replicas will und ein Node stirbt — was passiert?"
  - "Worin unterscheidet sich ein Service von einer Route?"
- Nicht als Test, sondern als Denkanstoß — Antworten gemeinsam erarbeiten.

#### 7. Nach dem Mittag direkt in die erste Übung
- Der CLI-Zugang wurde bereits im eigenen CLI-Block (vor dem Mittag) geübt — daher nach der Pause **kein weiterer CLI-Block** mehr nötig.
- Kurz die zuvor besprochene Architektur "anfassbar" machen (`oc get nodes`, `oc get pods -A`) und direkt überleiten: "Jetzt seid ihr dran — Übung 1."

#### 8. "Vorführen, dann selbst machen" — und der Block zwischen zwei Übungen
- Im Theorie-Block **Kubernetes-Ressourcen (Teil 1)** die Schritte aus Übung 1 ([K8s](https://ueberfuhr-trainings.github.io/k8s-materials/issues/kubernetes/01-backend-deployment.html), [OpenShift](https://ueberfuhr-trainings.github.io/k8s-materials/issues/openshift/01-backend-deployment.html)) direkt **live vorführen**. Die Teilnehmer sehen den Weg einmal komplett — danach brauchen sie für Übung 1 spürbar weniger Zeit (ca. **50 min**).
- Zwischen Übung 1 und Übung 2 liegt sonst kein Puffer. Vorschlag für den Zwischenblock: **[Debugging & Self-Healing](debugging-self-healing.md)** live — `oc logs`/`describe`/`exec`, Events lesen, dann `oc scale` hochskalieren und einen Pod löschen, um die Selbstheilung zu beobachten. Das bereitet den Debugging-Teil von Übung 2 vor und bringt nach der ersten Übung wieder Energie rein. Detaillierter Ablauf mit allen Befehlen: **[debugging-self-healing.md](debugging-self-healing.md)**.
  - *Alternativen, falls es besser passt:* gemeinsame Besprechung der Übung-1-Ergebnisse, ein Mikro-Quiz oder ein kurzer Rolling-Update-Demo.
- Das Thema **Konfiguration (12-Factor App, Umgebungsvariablen)** und die zugehörige **Übung 3 (ConfigMaps)** bilden den Abschluss von **Tag 1** (nach Übung 2). Bleibt am Ende von Tag 1 keine Zeit, lassen sie sich als Puffer auf den Beginn von Tag 2 (vor PostgreSQL) verschieben.

#### 9. Übung 4 (PostgreSQL): anforderungsbasiert + Musterlösung
- Übung 4 gibt bewusst **kein YAML** vor — Secrets und ConfigMaps sollen die Lernenden selbst erschließen. Plane dafür realistisch **~45 min** ein.
- Eine vollständige **[Musterlösung](https://ueberfuhr-trainings.github.io/k8s-materials/solutions/uebung4-postgresql.html)** (YAML + `oc`/`kubectl`-Befehle, inkl. Zufallspasswort) liegt bereit. Sie ist bewusst **nicht** aus den Übungen oder der Startseite verlinkt — du kannst sie den Lernenden bei Bedarf bereitstellen (zur Kontrolle oder wenn jemand feststeckt).

---

### Übungen begleiten

- **Nicht zu früh helfen.** Die Reflection-Fragen am Ende jeder Übung sind absichtlich da — sie fördern eigenständiges Nachdenken.
- **Fehler zulassen.** Fehlerbilder (falsches/nicht existentes Image, abstürzender Container) werden bewusst im Demo-Block **[Debugging & Self-Healing](debugging-self-healing.md)** gezeigt — Übung 1 selbst läuft als Happy Path. Fehler in den Übungen sind Lernmomente, kein Problem.
- **Pair Programming anbieten** — bei heterogenen Gruppen können stärkere und schwächere Teilnehmer zusammenarbeiten.
- **Gemeinsame Besprechung** nach jeder Übung (5–10 min): "Was war überraschend? Was hat nicht funktioniert?" — besonders die Reflection-Fragen gemeinsam durchgehen.

### Zeitpuffer

- Tag 2 ist bewusst mit Puffer getaktet (Kernprogramm bis ~14:30, danach Flex für Helm; Ende ~16:15), damit Übungen überziehen dürfen, ohne dass etwas gehetzt werden muss. Was nicht sicher reinpasst, steht im **Backlog** (Abschnitt am Ende des Tagesplans) statt im festen Plan.
- **Läuft der Tag rund**, füllt der Nachmittags-Flex die Backlog-Themen in ihrer Reihenfolge (zuerst Helm inkl. Übung 8, dann Jenkins).
- **Läuft der Tag knapp**, lieber die nächste Theorie kürzen als eine laufende Übung abbrechen — der Puffer am Ende fängt Überziehen auf.
- Weitere spontane Vertiefungen bei schnellen Gruppen: Skalierung live zeigen, Rolling Update simulieren, Pod löschen und Selbstheilung beobachten.

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
