## Scheduling Strategien 

### **FCFS**

Beim FCFS ("First Come, First Serve") werden Prozesse in der Reihenfolge ihres Eintreffens in der Warteschlange abgearbeitet.

Ablauf:
- **Ankunft**: Prozesse gelangen ins System und werden in der Reihenfolge ihres Eintreffens in eine Warteschlange eingereiht.
- **Ausführung**: Die CPU nimmt den ersten Prozess von der Warteschlange, führt ihn vollständig aus und entfernt ihn anschließend.
- **Wiederholung**: Die CPU nimmt den nächsten Prozess in der Warteschlange und wiederholt den Ausführungsvorgang.

FCFS ist ein ***non-preemptive*** Algorithmus. Das bedeutet, das einem Prozess solange die Nutzung der CPU ermöglicht ist, bis er ihn nicht mehr benötogt.

#### Vorteile
* Einfachste Logik und Implementierung
* Strikt nach Ankunft (FIFO), keine Bevorzugung
* Jeder Prozess kommt garantiert an die Reihe; kein Verhungern (Starvation)
* Batch-geeignet: Gut für Stapelverarbeitung ohne Zeitdruck


#### Nachteile
* Konvoi-Effekt (convoy effect): Kurze Prozesse werden durch lange Prozesse blockiert
* Hohe Wartezeit: Durchschnittlich schlechte Performance
* Nicht präemptiv: Keine Unterbrechung laufender Prozesse
* Ungeeignet für Time-Sharing/Echtzeit


**Beispiel 1: Prozesse mit gleicher Ankunftszeit**

**Abkürzungen:**
* **AT (Arrival Time):** Ankunftszeit
* **BT (Burst Time):** Bedienzeit oder CPU-Zeit
* **TAT (Turn Around Time):** Durchlaufzeit
* **WT (Waiting Time):** Wartezeit


| Prozess | AT | BT | CT | TAT | WT |
| --- | --- | --- | --- | --- | --- |
| **P1** | 0 | 5 | 5 | $5 - 0 = 5$ | $5 - 5 = 0$ |
| **P2** | 0 | 3 | 8 | $8 - 0 = 8$ | $8 - 3 = 5$ |
| **P3** | 0 | 8 | 16 | $16 - 0 = 16$ | $16 - 8 = 8$ |

**Ausführung:**
* **P1** beginnt zuerst und läuft für **5 Zeiteinheiten** (von 0 bis 5).
* **P2** beginnt als Nächstes und läuft für **3 Zeiteinheiten** (von 5 bis 8).
* **P3** läuft zuletzt und wird für **8 Zeiteinheiten** ausgeführt (von 8 bis 16).


**Beispiel 2: Prozesse mit unterschiedlichen Ankunftszeiten**

| Prozess | AT | BT | CT | TAT | WT |
| --- | --- | --- | --- | --- | --- |
| **P2** | 0 | 3 | 3 | $3 - 0 = 3$ | $3 - 3 = 0$ |
| **P1** | 2 | 5 | 8 | $8 - 2 = 6$ | $6 - 5 = 1$ |
| **P3** | 4 | 4 | 12 | $12 - 4 = 8$ | $8 - 4 = 4$ |

**Ausführung:**
* **P2** beginnt zuerst und läuft für **3 Zeiteinheiten** (von 0 bis 3).
* **P1** beginnt als Nächstes und läuft für **5 Zeiteinheiten** (von 3 bis 8).
* **P3** läuft zuletzt und wird für **4 Zeiteinheiten** ausgeführt (von 8 bis 12).

---

### **Round Robin**
In der Round Robin Scheduling Strategie rotiert das System (OS) durch jeden anstehenden Prozess und gibt ihnen allen die gleiche Anzahl an Zeit, genannt *Quantum Time*, zur Verfügung. Dabei wird nicht auf Priorität geachtet. 

#### Beispiel 1: Prozesse mit gleichzeitigem Ankommen
| Process | Burst Time | Arrival Time |
|---------|------------|--------------|
| P1 | 4 ms | 0 ms |
| P2 | 5 ms | 0 ms |
| P3 | 3 ms | 0 ms |

**Step-by-Step:**
1. Time 0-2 (P1): P1 läuft für 2 ms (Gesamte Zeit Übrig: 2 ms)
2. Time 2-4 (P2): P2 läuft für 2 ms (Gesamte Zeit Übrig: 3 ms)
3. Time 4-6 (P3): P3 läuft für 2 ms (Gesamte Zeit Übrig: 1 ms)
4. Time 6-8 (P1): P1 beendet seine letzten 2 ms
5. Time 8-10 (P2): P2 läuft für nochmal 2 ms (Gesamte Zeit Übrig: 1 ms)
6. Time 10-11 (P3): P3 beendet seine letze 1 ms
7. Time 11-12 (P2): P2 beendet seine letzte 1 ms

#### Beispiel 2: Prozesse mit unterschiedlichem Ankommen
| Process | Burst Time (BT) | Arrival Time (AT) |
|---------|------------|--------------|
| P1 | 5 ms | 0 ms |
| P2 | 2 ms | 4 ms |
| P3 | 4 ms | 5 ms |

**Step-by-Step:**
1. Time 0-2 (P1 führt aus):
    - P1 fängt Ausführung an als es bei 0 ms ankommt
    -  Läuft für 2 ms; restliche BT = 5 - 2 = 3 ms
    - Ready Queue: P1

2. Time 2-4 (P1 führt nochmal aus):
    - P1 führt weiter aus, da noch kein anderer Prozess angekommen ist
    - Läuft für 2 ms; restliche BT = 3 - 2 = 1 ms
    - P2 kommt bei 4 ms an
    - Ready Queue: P2, P1

3. Time 4-6 (P2 führt aus):
    - P2 fängt Ausführung an als es bei 4 ms ankommt
    - Läuft für 2 ms; restliche BT = 2 - 2 = 0 ms
    - P2 ist fertig
    - P3 kommt bei 5 ms an
    - Ready Queue: P1, P3

4. Time 6-7 (P1 führt aus):
    - P1 fängt Ausführung an
    - Läuft für 1 ms; restliche BT = 1 - 1 = 0 ms
    - Ready Queue: P3

5. Time 7-9 (P3 führt aus):
    - P3 fängt ausführung an
    - Restliche BT = 4 - 2 = 2 ms
    - Ready Queue: P3
  
6. Time 9-11 (P3 führt nochmal aus):
    - P3 fährt fort und läuft für 2 ms und beedet seine Ausführung
    - Restliche BT = 2 - 2 = 0 ms
    - Ready Queue: 

#### Vorteile: 
- Jeder Prozess kriegt die gleiche Menge an CPU
- Der Algorithmus ist einfach zu implementieren und verstehen
- Kann mehrere Prozesse gleichzeitig bewältigen ohne große Verzögerungen

#### Nachteile:
- Großer Overhead wenn der Quantum (Quantum Time) zu klein ist
- Fühlt sich langsam an wenn der Quantum zu groß ist
  
---------------

### **Shortest-Job-First (SJF)**
Wie der Name schon sagt wird der kürzester Prozess, also der Prozess mit der wenigsten BT, ausgeführt.  

Dabei ist das erste Problem schon, dass das System nicht die BT's der einzelnen Prozesse weiß, weswegen es nur in spezifischen Situationen verwendet werden kann wo die BT bekannt ist.  

Um zu vermeiden, dass immer mehr kürzere Prozess kommen und die längeren ignoriert werden, muss man ageing implementieren. 

#### Vorteile:
- Besser als FCFS, weil es die durchschnittliche Wartezeit verkürzt
- Gut in Situationen wo die BT's bekannt sind

#### Nachteile:
- Kann zu starving führen (siehe ageing)
- BT zu berechnen ist schwer

---

### **Priority Scheduling**

Beim Priority Scheduling weist das Betriebssystem Prozessen basierend auf ihrer  **Priorität** (ermittelt durch Ressourcenbedarf oder Zeitfaktoren) der CPU zu.

**Regel**: Höchste Priorität wird zuerst bedient; bei mehreren Prozessen mit gleicher Priorität gilt First-Come, First-Served.


#### **Non-preemptive:**

Ein laufender Prozess behält die CPU bis zum Abschluss, selbst wenn ein wichtigerer Prozess eintrifft.

**Beispiel:**

*Hinweis: Eine niedrigere Zahl steht für eine höhere Priorität.*

| Prozess | AT | BT | Priorität | CT | TAT | WT |
| --- | --- | --- | --- | --- | --- | --- |
| **P1** | 0 | 4 | 2 | 4 | $4 - 0 = 4$ | $4 - 4 = 0$ |
| **P2** | 1 | 2 | 1 | 6 | $6 - 1 = 5$ | $5 - 2 = 3$ |
| **P3** | 2 | 6 | 3 | 12 | $12 - 2 = 10$ | $10 - 6 = 4$ |

*Ausführung:*
* **Zeitpunkt 0:** Nur **P1** ist angekommen. **P1** beginnt mit der Ausführung, da es der einzige verfügbare Prozess ist. Da es ein nicht-präemptiver Ansatz ist, läuft er bis $t = 4$ durch.
* **Zeitpunkt 4:** **P1** beendet die Ausführung. Sowohl **P2** als auch **P3** sind mittlerweile angekommen. Da **P2** die höchste Priorität hat (Priorität 1), wird es als Nächstes ausgewählt.
* **Zeitpunkt 6:** **P2** beendet die Ausführung. Der einzige verbleibende Prozess ist **P3**, welcher nun mit der Ausführung beginnt.
* **Zeitpunkt 12:** **P3** beendet die Ausführung.



#### **Preemptive:**

Ein laufender Prozess kann durch einen höherpriorisierten Prozess unterbrochen werden.

**Beispiel - gleiche Ankunftszeit**

*Hinweis: Eine höhere Zahl steht hier für eine höhere Priorität.*

| Prozess | AT | BT | Priority | CT | TAT | WT |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **P3** | 0 | 7 | 3 | 13 | 13 | 6 |
| **P1** | 0 | 4 | 2 | 17 | 17 | 13 |
| **P2** | 0 | 6 | 1 | 6 | 6 | 0 |

**Ausführung:**
* **Zeitpunkt 0:** Alle Prozesse kommen gleichzeitig an. **P3** hat die höchste Priorität (Priorität 3) und beginnt daher mit der Ausführung.
* **Zeitpunkt 6:** **P3** beendet die Ausführung. Unter den verbleibenden Prozessen hat **P1** (Priorität 2) eine höhere Priorität als **P2**, daher beginnt **P1** mit der Ausführung.
* **Zeitpunkt 13:** **P1** beendet die Ausführung. Der einzige verbleibende Prozess ist **P2** (Priorität 1), welcher nun mit der Ausführung beginnt.
* **Zeitpunkt 17:** **P2** beendet die Ausführung. Alle Prozesse sind nun abgeschlossen.



**Beispiel - unterschiedliche Ankunftszeit**


| Prozess | AT | BT | Priorität | CT | TAT | WT |
| --- | --- | --- | --- | --- | --- | --- |
| **P1** | 0 | 6 | 2 | 10 | 10 | 4 |
| **P2** | 1 | 4 | 3 | 5 | 4 | 0 |
| **P3** | 2 | 5 | 1 | 15 | 13 | 8 |

**Ausführung:**
* **Zeitpunkt 0:** Nur **P1** ist angekommen und beginnt mit der Ausführung.
* **Zeitpunkt 1:** **P2** kommt mit einer höheren Priorität (Priorität 3) als **P1** an. **P1** wird unterbrochen (preempted) und **P2** beginnt mit der Ausführung.
* **Zeitpunkt 5:** **P2** beendet die Ausführung. Sowohl **P1** als auch **P3** sind verfügbar. **P1** hat die höhere Priorität (Priorität 2), also beginnt es mit der Ausführung.
* **Zeitpunkt 10:** **P1** beendet die Ausführung. **P3** nimmt die Ausführung auf, um seine verbleibende Zeit zu beenden.
* **Zeitpunkt 15:** **P3** beendet die Ausführung. Alle Prozesse sind nun abgeschlossen.




#### Vorteile
* **Priorisierung:** Wichtige Aufgaben (z. B. Systemprozesse) werden bevorzugt behandelt.
* **Flexibilität:** Erlaubt es, Prozesse nach Bedeutung statt nur nach Zeit zu ordnen.
* **Echtzeit-Fähigkeit:** Gut geeignet für Systeme, in denen bestimmte Fristen kritisch sind.



#### Nachteile
* **Starvation (Verhungern):** Prozesse mit niedriger Priorität kommen eventuell nie dran, wenn ständig neue wichtige Prozesse eintreffen.
* **Aging-Notwendigkeit:** Erfordert oft Zusatzmechanismen (wie Aging), um Starvation zu verhindern.
* **Komplexität:** Schwieriger zu implementieren und zu verwalten als FCFS.
* **Overhead:** Das ständige Vergleichen der Prioritäten kostet CPU-Ressourcen.


---

### Vergleich der Scheduling-Strategien

| Kriterium | FCFS | Round Robin (RR) | Shortest Job First (SJF) | Priority Scheduling |
| --- | --- | --- | --- | --- |
| **Preemptive?** | Nein | **Ja** |  Nein | Beides möglich |
| **Kriterium** | Ankunftszeit | Time Quantum | Burst Time (BT) | Priorität |
| **Wartezeit** | **Hoch** | Mittelmäßig | **Minimal** (Optimal) | Variabel |
| **Starvation?** | Nein | Nein | **Ja** (lange Prozesse) | **Ja** (niedrige Priorität) |
| **Komplexität** | Sehr niedrig | Mittel | Hoch (BT-Schätzung) | Mittel (Priority-Schätzung) |
| **Vorteil** | Einfachste Logik | Fair für alle User | Schnelles Erledigen vieler Prozesse | Wichtigtes zuerst |
| **Nachteil** | Kurze Jobs warten lange | Wahl der richtigen Quantum Time | BT oft unbekannt | Benötigt Aging |
