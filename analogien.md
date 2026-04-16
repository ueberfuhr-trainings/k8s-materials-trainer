# Durchgängige Analogie: Das Restaurant

## Grundidee

Statt für jedes Kubernetes-Konzept eine eigene Analogie zu erfinden, nutzen wir **ein einziges, durchgängiges Bild**: ein Restaurant. Die Teilnehmer können jedes neue Konzept an der bekannten Analogie einordnen. Das Bild wächst mit dem Kurs mit.

> **Das Restaurant = der Kubernetes-Cluster**

---

## Teil 1: Cluster-Architektur (Aufbau & Struktur)

*Verwenden beim Theorie-Block "Was ist Kubernetes?" am Vormittag von Tag 1.*

### Überblick

| Kubernetes-Konzept     | Restaurant-Analogie                                    |
|------------------------|--------------------------------------------------------|
| **Cluster**            | Das gesamte Restaurant (Gebäude, Küche, Personal)      |
| **Control Plane**      | Die Restaurantleitung (Büro, nicht in der Küche)       |
| **API Server**         | Die Tür zum Büro — alle Anfragen gehen hierüber        |
| **Scheduler**          | Der Restaurantleiter, der Bestellungen an freie Köche verteilt |
| **Controller Manager** | Der Manager, der durchzählt: "Stehen wirklich 3 Portionen bereit?" |
| **etcd**               | Das Bestellbuch — hier steht alles drin, was bereitstehen soll |
| **Worker Node**        | Ein Küchenarbeitsplatz mit Herd, Schneidebrett etc.    |
| **Pod**                | Ein Tablett mit einem fertigen Gericht (oder Menü)     |
| **Container**          | Eine einzelne Komponente auf dem Tablett (Hauptgang, Beilage) |
| **Deployment**         | Das Rezept im Kochbuch inkl. "Davon immer 3 Portionen bereithalten" |
| **ReplicaSet**         | Der Küchenchef, der zählt: "3 Tabletts bereit? Nein? Nachmachen!" |
| **ConfigMap**          | Die Tageskarte an der Wand — für alle sichtbar, leicht änderbar |
| **Secret**             | Das Geheimrezept im Tresor — nur der Küchenchef hat den Schlüssel |
| **Volume**             | Der Vorratsschrank — bleibt stehen, auch wenn der Koch Feierabend macht |
| **Namespace / Project**| Separate Abteilungen: "Küche Italienisch", "Küche Asiatisch" |

### Im Kurs verwenden

#### Einstieg: Das Restaurant vorstellen

> "Stellt euch ein großes Restaurant vor. Es gibt eine **Restaurantleitung** — die plant, organisiert, weiß was auf der Karte steht. Und es gibt **Küchenarbeitsplätze** — da wird tatsächlich gekocht. Die Leitung kocht nicht selbst, sondern sorgt dafür, dass alles läuft."

- **Control Plane** = Restaurantleitung (plant, überwacht, reagiert)
- **Worker Nodes** = Küchenarbeitsplätze (hier wird gekocht)
- **API Server** = Die Tür zum Büro der Leitung — alle Anfragen gehen hierüber
- **Scheduler** = Der Restaurantleiter, der entscheidet: "Koch A hat noch Kapazität, der macht die nächste Bestellung"
- **Controller Manager** = Der Manager, der ständig durchzählt: "Auf der Karte stehen 3 Gerichte als Tagesempfehlung — stehen wirklich 3 bereit?"
- **etcd** = Das Bestellbuch — hier steht alles drin, was bestellt wurde und was bereitstehen soll

#### Pods und Container

> "Ein **Pod** ist wie ein Tablett, das aus der Küche kommt. Darauf steht normalerweise ein Gericht — das ist der **Container**. Manchmal stehen zwei Sachen zusammen auf dem Tablett — Hauptgericht und Soße in separaten Schüsseln. Die gehören zusammen und werden immer gemeinsam serviert. Das sind Multi-Container-Pods."

- Warum nicht einfach alles in eine Schüssel? → Weil man die Soße austauschen kann, ohne das Hauptgericht neu zu kochen (Sidecar-Pattern).

#### Deployments und ReplicaSets

> "Das **Deployment** ist das Rezept im Kochbuch. Da steht nicht nur drin, *wie* man das Gericht zubereitet, sondern auch: 'Davon immer 3 Portionen bereithalten.' Das **ReplicaSet** ist der Küchenchef, der ständig durchzählt: Stehen 3 Tabletts in der Durchreiche? Eins wurde abgeholt? Sofort nachmachen!"

- Wenn das Rezept geändert wird (neues Image-Tag) → Das ist ein Rolling Update: Die alten Gerichte werden nach und nach durch neue ersetzt, nicht alle auf einmal weggeworfen.

#### ConfigMaps und Secrets

> "Die **ConfigMap** ist die Tageskarte, die im Restaurant an der Wand hängt. Jeder kann sie lesen. Wenn sich etwas ändert — 'Heute kein Fisch' — wird die Karte einfach ausgetauscht. Das **Secret** ist das Geheimrezept der Großmutter, das im Tresor liegt. Nur der Küchenchef darf es lesen."

- **Warum nicht alles ins Rezept (Image) schreiben?** → Weil die gleiche Pasta in der Filiale Hamburg anders gewürzt wird als in München. Das Rezept ist gleich, die Tageskarte ist anders.

#### Volumes

> "Der **Volume** ist der Vorratsschrank. Wenn der Koch nach Hause geht (Container stoppt), bleibt der Vorratsschrank stehen. Am nächsten Tag ist alles noch da. Ohne Vorratsschrank wäre nach Feierabend alles weg — jeder Tag fängt bei Null an."

#### Namespaces

> "Das Restaurant hat verschiedene **Abteilungen**: Italienische Küche, Asiatische Küche, Desserts. Jede Abteilung hat eigene Köche, eigene Rezepte, eigenen Bereich. Sie teilen sich die Infrastruktur (Strom, Wasser, Gebäude), aber sie kommen sich nicht in die Quere."

---

## Teil 2: Laufzeitverhalten (Requests, Routing, Health Checks)

*Verwenden bei den Theorie-Blöcken zu Services/Routes und Health Checks — verteilt über Tag 1 und Tag 2.*

### Überblick

| Kubernetes-Konzept     | Restaurant-Analogie                                    |
|------------------------|--------------------------------------------------------|
| **Service**            | Die Durchreiche zur Küche — der Kellner ruft "Einmal Pasta!", egal welcher Koch es macht |
| **ClusterIP**          | Durchreiche nur innerhalb der Küche (intern)           |
| **NodePort / LoadBalancer** | Durchreiche durchs Fenster nach draußen           |
| **Route / Ingress**    | Die Eingangstür mit dem Schild und der Speisekarte     |
| **DNS-Auflösung**      | Der Kellner kennt den Namen der Durchreiche, nicht den Koch dahinter |
| **Startup Probe**      | "Ist der Koch angekommen und hat seine Station eingerichtet?" |
| **Liveness Probe**     | "Kocht der Koch noch, oder schläft er am Herd?"        |
| **Readiness Probe**    | "Hat der Koch eine Portion fertig, die rausgegeben werden kann?" |

### Im Kurs verwenden

#### Der Weg einer Bestellung (Request-Flow)

> "Ein Gast betritt das Restaurant durch die **Eingangstür** (Route). Er wird zum Tisch geführt und gibt beim Kellner seine Bestellung auf. Der Kellner geht zur **Durchreiche** (Service) und ruft: 'Einmal Pasta!' Er weiß nicht und muss nicht wissen, welcher Koch sie zubereitet. Hauptsache, durch die Durchreiche kommt eine Pasta."

Die Durchreiche als zentrales Bild für Service Discovery:

- Die Durchreiche hat einen **festen Namen** → DNS-Auflösung. Der Kellner sagt immer "Pasta-Durchreiche", egal welcher Koch dahintersteht.
- Wenn **Koch A krank ist**, macht es Koch B. Die Durchreiche bleibt dieselbe.
- Wenn **3 Köche** gleichzeitig Pasta machen können, verteilt die Durchreiche die Bestellungen → Load Balancing.

#### Verschiedene Arten von Durchreichen

> "Es gibt verschiedene Durchreichen im Restaurant:
> - **ClusterIP** = Die interne Durchreiche zwischen den Küchenabteilungen. Die Patisserie bestellt bei der Hauptküche 'Einmal Karamellsoße' — das sieht kein Gast.
> - **NodePort** = Ein Fenster in der Außenwand. Jemand auf der Straße kann direkt bestellen, aber er muss die Fensternummer kennen.
> - **LoadBalancer** = Ein eleganter Empfangstresen vor dem Restaurant, der Gäste gleichmäßig auf mehrere Eingänge verteilt."

#### Route / Ingress

> "Die **Route** ist die Eingangstür des Restaurants mit dem Namensschild: 'Ristorante Kubernetes, Eingang hier.' Gäste von der Straße (Internet) finden das Restaurant über diese Tür. Drinnen werden sie dann zur richtigen Durchreiche geleitet."

- Mehrere Routen = mehrere Eingänge: "pasta.restaurant.de" führt zur Pasta-Küche, "sushi.restaurant.de" zur Sushi-Abteilung.

#### Health Checks: Der Qualitätsprüfer

> "Im Restaurant gibt es einen **Qualitätsprüfer**, der regelmäßig durch die Küche geht und jeden Koch einzeln kontrolliert."

Die drei Prüfungen — mit konkretem Ablauf:

> **Startup Probe** — Die Morgenkontrolle:
> "Der Koch kommt morgens rein. Der Prüfer schaut: 'Hast du deine Station eingerichtet? Herd an, Zutaten da, Schürze um?' Erst wenn alles steht, wird der Koch für die anderen Prüfungen freigegeben. Vorher bekommt er keine Bestellungen."

> **Liveness Probe** — Die Lebenszeichenkontrolle:
> "Alle paar Minuten schaut der Prüfer vorbei: 'Kochst du noch, oder schläfst du am Herd?' Wenn der Koch **nicht mehr reagiert** — Augen zu, Kopf auf der Arbeitsplatte — wird er nach Hause geschickt und ein frischer Koch geholt (Container-Restart). Es geht nicht darum, ob das Essen gut schmeckt, sondern ob der Koch überhaupt noch arbeitsfähig ist."

> **Readiness Probe** — Die Bereitschaftskontrolle:
> "Der Prüfer schaut in die Durchreiche: 'Hast du gerade eine fertige Portion, die raus kann?' Wenn der Koch sagt 'Moment, Soße ist noch nicht fertig' — dann leitet der Kellner die nächste Bestellung an einen anderen Koch weiter. Der Koch wird **nicht ausgetauscht**, er bekommt nur **keine neuen Bestellungen**, bis er wieder bereit ist."

Zusammenspiel verdeutlichen:

> "Stellt euch vor, Koch B kommt gerade aus der Pause zurück (**Startup Probe**: Station noch nicht bereit). Koch A hat 5 Portionen gleichzeitig am Laufen und sagt 'Ich kann gerade nichts Neues annehmen' (**Readiness Probe**: nicht ready). Koch C steht mit leerem Blick am Herd und rührt seit 10 Minuten in einem leeren Topf (**Liveness Probe**: nicht alive → wird ersetzt). Der Kellner schickt Bestellungen nur an Köche, die sowohl da als auch bereit sind."

---

## Tipps zur Nutzung

- **Beim ersten Mal** die Analogie explizit einführen: "Wir stellen uns den Cluster als Restaurant vor. Das Bild begleitet uns die nächsten zwei Tage."
- **Teil 1 am Vormittag** von Tag 1 einführen, **Teil 2 verteilt** einstreuen: Services bei den Ressourcen, Health Checks am Nachmittag von Tag 2.
- **Zurückverweisen**, wenn ein neues Konzept dazukommt: "Erinnert ihr euch an die Durchreiche? Jetzt hängen wir noch ein Schild an die Eingangstür — das ist die Route."
- **Teilnehmer einbinden**: "Was wäre in unserem Restaurant-Bild der PersistentVolumeClaim?" → Der Koch sagt der Leitung: "Ich brauche einen Kühlschrank mit mindestens 200 Litern."
- **Grenzen der Analogie benennen**: Wenn sie nicht mehr passt, ehrlich sagen. "An der Stelle hinkt das Bild — ein ReplicaSet ist technisch etwas anderes als ein Küchenchef, aber das Prinzip passt."
- **Nicht erzwingen.** Wenn die Gruppe die Konzepte direkt versteht — nicht krampfhaft drauf beharren.
