# Zwischenblock: Debugging & Self-Healing (30 min)

## Wann & warum

Dieser Block liegt an Tag 1 **zwischen Übung 1 und Übung 2**. Er hat zwei Aufgaben:

1. **Fehlerbilder und Debugging-Werkzeuge einführen** — Übung 1 läuft bewusst als „Happy Path" (die Teilnehmer deployen gleich das korrekte Image). Was passiert, wenn etwas *nicht* funktioniert, zeigen wir hier kontrolliert an zwei absichtlich kaputten Deployments.
2. **Self-Healing erlebbar machen** — das Konzept „Desired State vs. Actual State" aus dem Architektur-Block jetzt live am Cluster zeigen.

**Kein Frontalvortrag.** Alles live am Cluster, die Gruppe rät bei jedem Schritt mit und trifft Vorhersagen.

> **Voraussetzung:** Cluster-Zugang, ein Projekt/Namespace ist angelegt. Die Demos sind unabhängig vom Ergebnis aus Übung 1 — sie legen eigene (kurzlebige) Deployments an und räumen sie wieder ab.

---

## Teil 1 — Fehler lesen: `get` → `describe` → `logs` (ca. 15 min)

Roter Faden: **Vom Groben zum Feinen.** Zwei typische Fehlerklassen, zwei Werkzeuge.

Kurz die Status-Werte einordnen, die gleich auftauchen:

| Status              | Bedeutung                                                        |
|---------------------|------------------------------------------------------------------|
| `Running`           | Läuft — aber heißt noch nicht „gesund" (siehe Probes, Tag 2)     |
| `Pending`           | Noch keinem Node zugewiesen (kein Platz? Ressourcen? Scheduler?) |
| `ContainerCreating` | Image wird gezogen, Volumes werden gemountet                     |
| `ImagePullBackOff`  | Image nicht gefunden (falscher Name/Tag) oder kein Zugriff       |
| `CrashLoopBackOff`  | Container startet, stirbt, startet, stirbt … Klassiker           |
| `Error`             | Container ist mit Fehlercode beendet                             |

### Demo A — Fehler beim Image-Pull (nicht existentes Image)

> **Ansage:** „Ich deploye jetzt ein Image, das es gar nicht gibt. Was erwartet ihr?"

```bash
oc create deployment demo-fail --image=ralfueberfuhr/recipes-backend:gibt-es-nicht
oc get pods
```

Der Pod hängt in `ErrImagePull` / `ImagePullBackOff`. **Woher wissen wir, warum?**

```bash
oc describe pod <pod-name>
```

- Ganz nach unten zu **Events** scrollen — dort steht im Klartext, dass das Image/der Tag nicht gefunden wurde.
- Kernaussage: `describe` zeigt den kompletten Lebenslauf des Pods inkl. der Entscheidungen von Scheduler und kubelet.

> **Merksatz für die Gruppe:** „Bei jedem Problem zuerst die **Events** aus `describe` lesen — ein Großteil der Fehler steht da wörtlich drin."

Aufräumen:

```bash
oc delete deployment demo-fail
```

### Demo B — Container startet, stirbt dann (falsches Image mit `latest`)

> **Ansage:** „Jetzt das *richtige* Image — aber mit dem Tag `latest` statt `latest-dev`. Das erwartet eine externe PostgreSQL-Datenbank, die es hier nicht gibt."

```bash
oc create deployment demo-db --image=ralfueberfuhr/recipes-backend:latest
oc get pods
```

Diesmal wird das Image **erfolgreich gezogen**, der Container startet — und läuft dann in einen Fehler (`Error` / `CrashLoopBackOff`).

> **Frage:** „Der Pull hat geklappt. `describe`/Events sagen wenig über die *Anwendung* selbst. Wo schauen wir jetzt?"

```bash
oc logs <pod-name>
oc logs <pod-name> --previous   # Log des bereits abgestürzten Vorgänger-Containers
```

- In den Logs steht die Fehlermeldung der Anwendung — sie kommt nicht an die Datenbank.
- **Unterschied klarmachen:** `describe`/Events = die Sicht von Kubernetes auf den Pod (Pull, Scheduling, Restarts). `logs` = die Sicht *in die Anwendung hinein*.
- `--previous` betonen: Bei `CrashLoopBackOff` ist der aktuelle Container evtl. schon wieder weg — das interessante Log steckt im *vorherigen*.

Aufräumen:

```bash
oc delete deployment demo-db
```

> **Optional — in den Container schauen:** Bei laufenden Pods lohnt sich `oc exec`. Das ist die Brücke zu Übung 2 (Umgebungsvariablen):
> ```bash
> oc exec <pod-name> -- env          # kommt die erwartete Variable wirklich an?
> oc exec -it <pod-name> -- /bin/sh  # interaktive Shell im Container
> ```

> **Optional (Web Console):** Dieselben Informationen parallel in der OpenShift Web Console zeigen (Pod → Tabs *Logs*, *Terminal*, *Events*). Wer lieber klickt, findet dort alles wieder.

---

## Teil 2 — Self-Healing: „Kubernetes heilt sich selbst" (ca. 15 min)

Ziel: **Desired State vs. Actual State** aus dem Architektur-Block *live erlebbar* machen. Am besten am funktionierenden Backend aus Übung 1 (oder einem frischen `latest-dev`-Deployment).

### Schritt 1 — Skalieren

> **Frage:** „Ich will plötzlich 3 Instanzen statt 1. Muss ich 2 neue Pods von Hand starten?"

```bash
oc get pods
oc scale deployment recipes-backend --replicas=3
oc get pods -w          # -w = watch: beim Hochfahren live zuschauen
```

- Betonen: Wir sagen **was** wir wollen (`--replicas=3`), nicht **wie**. Der Controller erledigt den Rest.

### Schritt 2 — Der Aha-Moment: Pod löschen

> **Frage (vorher raten lassen!):** „Ich lösche jetzt einen der 3 Pods. Was passiert?"

Die Gruppe soll **vor** dem Befehl eine Vorhersage treffen. Dann:

```bash
# In einem zweiten Terminal mitlaufen lassen:
oc get pods -w

# Im ersten Terminal:
oc delete pod <pod-name>
```

- Im Watch-Fenster sieht man: Der Pod verschwindet (`Terminating`) — und **sofort startet ein neuer** (`ContainerCreating` → `Running`).
- **Das ist der Moment.** Kurz sacken lassen.

> **Auflösen:** „Der gewünschte Zustand ist ‚3 Pods'. Ihr habt einen gelöscht → tatsächlicher Zustand ‚2'. Der ReplicaSet-Controller bemerkt die Differenz und startet einen neuen. Niemand hat das von Hand gemacht — genau das ist **Self-Healing**."

### Schritt 3 — Gegenprobe (optional, wenn Zeit)

> **Frage:** „Und wenn ich das Deployment auf 0 skaliere und dann wieder hochfahre?"

```bash
oc scale deployment recipes-backend --replicas=0    # alle Pods verschwinden
oc scale deployment recipes-backend --replicas=2    # kommen zurück
```

- Zeigt: Der **Desired State im Deployment** ist die Wahrheit — nicht die einzelnen Pods.

---

## Abschluss & Überleitung

Kurz zusammenfassen — am besten die Gruppe die Werkzeuge selbst aufzählen lassen:

- **Fehler lesen:** `get` (Status) → `describe` (Events: Pull/Scheduling/Restarts) → `logs` (`--previous`, Sicht in die Anwendung).
- **Self-Healing:** Das Deployment hält den Desired State, egal was mit einzelnen Pods passiert.

> **Überleitung zu Übung 2:** „In Übung 2 baut ihr ein Frontend, das über eine Umgebungsvariable mit dem Backend verbunden wird — und dort ist ein Konfigurationsfehler eingebaut. Ihr habt jetzt das Handwerkszeug, um ihn zu finden."

---

## Häufige Stolpersteine

- **`-w` / `get pods -w` nicht vergessen zu beenden** (`Ctrl+C`), sonst blockiert das Terminal.
- **Zweites Terminal / geteilter Bildschirm** vorbereiten, damit man Watch und Befehle nebeneinander sieht — der Self-Healing-Effekt lebt davon.
- **Pod-Namen ändern sich** nach jedem Neustart. Immer vorher `oc get pods` und den aktuellen Namen kopieren.
- **Demo-Deployments wieder abräumen** (`oc delete deployment demo-fail demo-db`), damit sie nicht mit dem echten Backend verwechselt werden.
- **`gibt-es-nicht`-Tag** ist bewusst gewählt — jeder erfundene, nicht existierende Tag erzeugt denselben `ImagePullBackOff`.

## Zeitpuffer

- Läuft die Zeit knapp: **Teil 2 (Self-Healing) hat Vorrang** — das ist der Aha-Moment. In Teil 1 reicht dann Demo A.
- Ist noch Luft: einen **Rolling Update** zeigen (`oc set image deployment/recipes-backend recipes-backend=ralfueberfuhr/recipes-backend:latest-dev`) und im Watch beobachten, wie neue Pods hoch- und alte runterfahren.
