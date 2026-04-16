# Cluster-Architektur schrittweise aufbauen

## Grundidee

Die Cluster-Architektur **nicht als fertiges Diagramm** zeigen. Stattdessen Frage für Frage an der Tafel entwickeln. Jede Komponente wird zur Antwort auf ein konkretes Problem.

**Voraussetzung:** Die problembasierte Überleitung ist erfolgt. Die Teilnehmer wissen, *warum* Kubernetes existiert. Jetzt klären wir *wie*.

---

## Ablauf: Fragen und Antworten

### Schritt 1 — Wo laufen die Container?

> **Frage:** "Wir haben gerade gesagt, wir brauchen mehrere Maschinen. Was muss auf jeder Maschine laufen, damit Container darauf starten können?"

An die Tafel zeichnen: **2–3 Kästchen nebeneinander** (die Maschinen).

**Antwort erarbeiten:** Jede Maschine braucht eine Container-Runtime (wie Docker) und einen Agenten, der Befehle entgegennimmt.

An die Tafel schreiben: **Worker Node** mit **Container Runtime** und **kubelet** darin.

> **Nachfrage:** "Der kubelet ist wie ein Vorarbeiter auf der Baustelle — er bekommt gesagt 'Starte diesen Container' und macht es. Aber wer sagt ihm das?"

---

### Schritt 2 — Wer gibt die Befehle?

> **Frage:** "Wir haben jetzt 3 Worker Nodes. Wenn ich sage 'Starte mein Backend' — an wen geht dieser Befehl?"

An die Tafel: Ein **neues Kästchen oben** (die Steuerungszentrale).

**Antwort erarbeiten:** Es braucht eine zentrale Anlaufstelle, die Befehle entgegennimmt.

An die Tafel schreiben: **API Server** in der Steuerungszentrale.

> **Zusammenfassung:** "Der API Server ist das Eingangstor. Alles — ob `kubectl`, die Web Console oder interne Komponenten — spricht mit dem API Server."

---

### Schritt 3 — Wer entscheidet, auf welchem Node?

> **Frage:** "Ich sage dem API Server: 'Starte 3 Instanzen meines Backends.' Woher weiß das System, auf welchen der 3 Nodes die jeweils laufen sollen?"

**Antwort erarbeiten:** Jemand muss schauen, welcher Node Kapazität hat, und die Container verteilen.

An die Tafel schreiben: **Scheduler** in der Steuerungszentrale.

> **Vertiefung:** "Der Scheduler berücksichtigt: Wie viel CPU/RAM ist frei? Gibt es Regeln, dass bestimmte Pods nicht zusammen laufen dürfen? Braucht der Pod eine GPU?"

---

### Schritt 4 — Wer merkt, wenn etwas kaputt geht?

> **Frage:** "Ich habe gesagt, ich will 3 Instanzen. Eine stirbt. Wer bemerkt das und startet eine neue?"

**Antwort erarbeiten:** Es braucht jemanden, der ständig prüft: "Ist der aktuelle Zustand = der gewünschte Zustand?" Wenn nein → handeln.

An die Tafel schreiben: **Controller Manager** in der Steuerungszentrale.

> **Schlüsselkonzept (aufschreiben!):**
> - **Desired State** = "Ich will 3 Instanzen"
> - **Actual State** = "Es laufen gerade 2"
> - **Controller** = "Differenz erkannt → 1 neue starten"
>
> "Das ist das Herzstück von Kubernetes: **deklarativ, nicht imperativ.** Ihr sagt nicht 'Starte einen Container', sondern 'Ich will, dass 3 laufen.' Kubernetes kümmert sich ums Wie."

---

### Schritt 5 — Wo wird der gewünschte Zustand gespeichert?

> **Frage:** "Der Controller Manager vergleicht gewünschten und tatsächlichen Zustand. Aber wo ist der gewünschte Zustand eigentlich gespeichert? Was passiert, wenn die Steuerungszentrale selbst neustartet?"

**Antwort erarbeiten:** Es braucht einen persistenten Speicher für den gesamten Cluster-Zustand.

An die Tafel schreiben: **etcd** in der Steuerungszentrale.

> **Erklärung:** "etcd ist eine verteilte Key-Value-Datenbank. Hier steht alles drin: Welche Deployments gibt es, welche Pods sollen laufen, welche ConfigMaps existieren. Es ist das Gedächtnis des Clusters."

---

### Schritt 6 — Wie finden sich Pods untereinander?

> **Frage:** "Unser Frontend muss das Backend erreichen. Aber Pods bekommen zufällige IP-Adressen und können jederzeit neu gestartet werden. Wie findet das Frontend das Backend?"

An die Tafel: **Verbindungslinien** zwischen den Pods auf den Worker Nodes.

**Antwort erarbeiten:** Es braucht eine stabile Adresse, die auf die jeweils aktuellen Pods zeigt.

An die Tafel schreiben: **kube-proxy** auf jedem Worker Node.

> **Erklärung:** "Kubernetes hat Services — stabile Adressen mit DNS-Namen. Der kube-proxy auf jedem Node sorgt dafür, dass Anfragen an den Service bei den richtigen Pods ankommen."

---

### Schritt 7 — Das Gesamtbild benennen

Jetzt die Bereiche einrahmen und beschriften:

- Die obere Box: **Control Plane** (Steuerungszentrale)
  - API Server, Scheduler, Controller Manager, etcd
- Die unteren Boxen: **Worker Nodes** (Arbeiter)
  - kubelet, kube-proxy, Container Runtime, Pods
- Das Ganze zusammen: **Cluster**

> "Das ist die Kubernetes-Architektur. Ihr habt sie gerade selbst hergeleitet."

---

## Tafelbild-Skizze

```
┌─────────────────── Control Plane ───────────────────┐
│                                                      │
│   ┌────────────┐  ┌───────────┐  ┌──────────────┐  │
│   │ API Server │  │ Scheduler │  │  Controller   │  │
│   │            │  │           │  │  Manager      │  │
│   └────────────┘  └───────────┘  └──────────────┘  │
│                    ┌──────┐                          │
│                    │ etcd │                          │
│                    └──────┘                          │
└──────────────────────────────────────────────────────┘
        │                    │                  │
┌───────┴──────┐  ┌──────────┴─────┐  ┌────────┴──────┐
│  Worker Node │  │  Worker Node   │  │  Worker Node  │
│              │  │                │  │               │
│ ┌──────────┐ │  │ ┌──────────┐  │  │ ┌──────────┐  │
│ │ kubelet  │ │  │ │ kubelet  │  │  │ │ kubelet  │  │
│ ├──────────┤ │  │ ├──────────┤  │  │ ├──────────┤  │
│ │kube-proxy│ │  │ │kube-proxy│  │  │ │kube-proxy│  │
│ ├──────────┤ │  │ ├──────────┤  │  │ ├──────────┤  │
│ │ Pod  Pod │ │  │ │ Pod  Pod │  │  │ │ Pod      │  │
│ └──────────┘ │  │ └──────────┘  │  │ └──────────┘  │
└──────────────┘  └────────────────┘  └───────────────┘
```

---

## Tipps

- **Langsam zeichnen.** Jeder Schritt dauert 2–3 Minuten. Das Gesamtbild entsteht in ca. 20 Minuten.
- **Farben nutzen:** Control Plane in einer Farbe, Worker Nodes in einer anderen.
- **Die Teilnehmer raten lassen**, bevor du die Komponentennamen hinschreibst.
- **Desired State vs. Actual State** ist das wichtigste Konzept — hier ruhig 2 Minuten extra investieren.
- **Nicht alles erklären.** NetworkPolicy, RBAC, Ingress Controller etc. kommen später. Hier geht es um die Grundstruktur.
