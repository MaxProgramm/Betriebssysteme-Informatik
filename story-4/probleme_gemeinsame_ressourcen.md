# Probleme bei der Nutzung gemeinsamer Ressourcen

## 1. Ausgangslage und Notwendigkeit gemeinsamer Ressourcen

Die gemeinsame Nutzung von Ressourcen (Resource Sharing) ist eine der Kernaufgaben eines modernen Betriebssystems.
Die Nutzung gemeinsamer Ressourcen ist notwendig, um, Datenaustausch und die **effiziente Nutzung begrenzter Mittel** zu ermöglichen.

**Zusammenarbeit von Prozessen**:

- In den meisten Systemen müssen verschiedene Akteure Informationen austauschen können
- Eine gemeinsame Ressource dient hier als **Schnittstelle**:
  - Erzeuger kann Informationen ablegen
  - Verbraucher kann Informationen entnehmen

**Synchronisierung**:

- Prozesse sollten gleichzeitig laufen um Zeit zu sparen
  -> damit alle Prozesse auf dem gleichen Stand sind müssen sie gemeinsame Ressourcen nutzen

**Effizienz und Redundanz**: Das Nutzen gemeinsamer Ressourcen verhindert redundante Informationen und ist effizienter, da Prozesse gleichzeitig laufen können

## 2. Probleme bei der Nutzung gemeinsamer Ressourcen (kritischer Abschnitt)

### Race Condition

**Definition**: Ein Zustand, bei dem das Endergebnis einer Operation davon abhängt, in welcher zeitlichen Reihenfolge (Timing) die beteiligten Threads ausgeführt werden.

**Beispiel**: Zwei Threads erhöhen gleichzeitig eine Zählervariable. Wenn beide gleichzeitig den alten Wert lesen, bevor ein Thread einen neuen schreibt, geht ein Inkrement verloren.

### Erzeuger-Verbraucher-Problem

Dieses Problem tritt auf, wenn zwei Prozesse (Threads) einen gemeinsamen Zwischenspeicher nutzen.

**Das Erzeuger-Verbraucher-Problem:**

1. Ein Erzeuger produziert Daten oder Objekte und legt sie in einem Zwischenspeicher ab.
2. Ein Verbraucher entnimmt diese Objekte aus dem Speicher.
3. **Problematik**: Wenn der Speicher voll ist, kann der Erzeuger nichts ablegen; ist er leer, kann der Verbraucher nichts entnehmen. Ohne Absprache verschwenden die Threads Zeit durch aktives Warten (Busy Waiting), indem sie ständig prüfen, ob der Speicher bereit ist.

**Koordinationsmangel**: Ein Erzeuger könnte versuchen, Daten in einen bereits vollen Speicher zu schreiben, oder ein Verbraucher versucht, Daten aus einem leeren Speicher zu lesen.

**Ineffizienz durch Aktives Warten (Busy Waiting)**: Ständige Überprüfung durch einen Thread ob eine Ressource frei ist -> verschwendet Rechenzeit

### Leser-Schreiber-Probleme

Dieses Problem tritt auf wenn mehrere Prozesse gleichzeitig auf eine Ressource zugreifen wollen. Die Methoden im Code, die zum Zugriff auf diese gemeinsamen Ressourcen benötigt werden, bilden den sogennanten **kritischen Abschnitt** (also einen bestimmten Teilbereich des Codes).
Der kritische Abschnitt darf nur von einem Prozess gleichzeitig ausgeführt werden, um Dateninkonsistenzen zu verhindern.

**Das Leser-Schreiber-Problem:**

- Hier greifen zwei Arten (Leser und Schreiber) von Threads auf Daten (z. B. eine Datenbank) zu.
- Regeln: Beliebig viele Leser dürfen gleichzeitig zugreifen, da sie Daten nicht verändern. Ein Schreiber benötigt jedoch exklusiven Zugriff; während er schreibt, darf niemand sonst lesen oder schreiben, um Inkonsistenzen zu vermeiden.
- Es müssen Strategien entwickelt werden, damit z. B. Schreiber nicht durch einen stetigen Strom an Lesern „verhungern“.

Für den kritischen Abschnitt gelten 3 Bedingungen:

1. **Wechselseitiger Ausschluss (Mutual Exclusion)**: Wenn ein Prozess seinen kritischen Abschnitt ausführt, darf kein anderer Prozess im kritischen Abschnitt derselben Ressource sein. Nur ein Prozess zur gleichen Zeit.
2. **Fortschritt (Progress)**: Wenn kein Prozess im kritischen Abschnitt ist und ein Prozess hinein möchte, darf die Entscheidung nicht verzögert werden. Nur Prozesse, die am kritischen Abschnitt teilnehmen, dürfen entscheiden, wer als Nächstes eintritt.
3. **Begrenztes Warten (Bounded Waiting)**: Ein Prozess darf nicht unendlich lange auf Eintritt in den kritischen Abschnitt warten müssen. Es muss eine Garantie geben, dass jeder Prozess nach einer endlichen Anzahl von Zugriffen anderer Prozesse eintreten darf (Vermeidung von Verhungern/Starvation).

## 3. Deadlocks als Folge von Synchronisationsproblemen

**Das Problem der Speisenden Philosophen:**

- Fünf Philosophen sitzen an einem Tisch; jeder benötigt zwei Gabeln zum Essen, hat aber nur eine Gabel links und eine rechts von sich.
- Wenn jeder gleichzeitig die linke Gabel aufnimmt, wartet jeder ewig darauf, dass die rechte Gabel frei wird. Es entsteht ein zyklisches Warten.

Ein Deadlock ist ein Zustand, in dem eine Gruppe von Threads sich gegenseitig blockiert, weil jeder auf eine Ressource wartet, die ein anderer aus der Gruppe besetzt hält (-> Problem der speisenden Philosophen).

Damit ein Deadlock entstehen kann, müssen die **vier Coffman-Bedingungen** erfüllt sein:

1. **Wechselseitiger Ausschluss (Mutual Exclusion)**: Mindestens eine Ressource wird nicht-teilbar genutzt. Nur ein Prozess kann die Ressource exklusiv verwenden.
2. **Halten und Warten (Hold & Wait)**: Prozesse, die bereits Ressourcen benutzen, fordern weitere Ressourcen an, die von anderen Prozessen genutzt werden.
3. **Ununterbrechbarkeit**: Ressourcen können einem Thread nicht entzogen werden. Sie werden nur frei, wenn der Thread sie abgibt.
4. **Zyklisches Warten**: Es entscheht eine geschlossene Kette von Prozessen ($P_0, P_1, ..., P_n$), die jeweils auf eine Ressource des nächsten Threads warten: $P_0$ wartet auf $P_1$, dieser auf $P_2$ und $P_n$ schließlich auf $P_0$.

**Vermeiden von Deadlocks**: Um Deadlocks zu verhindern, muss mindestens eine der Coffman-Bedingungen gezielt außer Kraft gesetzt werden.

## 4. Warum ist wechselseitiger Ausschluss notwendig?

- Der kritische Abschnitt darf nur von einem Prozess gleichzeitig ausgeführt werden, um Dateninkonsistenzen zu verhindern.
- **Wechselseitiger Ausschluss (Mutual Exclusion)**: Wenn ein Prozess seinen kritischen Abschnitt ausführt, darf kein anderer Prozess im kritischen Abschnitt derselben Ressource sein. Nur ein Prozess zur gleichen Zeit.
- Regeln: Beliebig viele Leser dürfen gleichzeitig zugreifen, da sie Daten nicht verändern. Ein Schreiber benötigt jedoch exklusiven Zugriff; während er schreibt, darf niemand sonst lesen oder schreiben, um Inkonsistenzen zu vermeiden.
