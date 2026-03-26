# Probleme bei der Nutzung gemeinsamer Ressourcen

## Race Condition

**Definition**: Ein Zustand, bei dem das Endergebnis einer Operation davon abhängt, in welcher zeitlichen Reihenfolge (Timing) die beteiligten Threads ausgeführt werden.

**Beispiel**: Zwei Threads erhöhen gleichzeitig eine Zählervariable. Wenn beide gleichzeitig den alten Wert lesen, bevor ein Thread einen neuen schreibt, geht ein Inkrement verloren.

## Erzeuger-Verbraucher-Problem

Dieses Problem tritt auf, wenn zwei Prozesse (Threads) einen gemeinsamen Zwischenspeicher nutzen.

**Koordinationsmangel**: Ein Erzeuger könnte versuchen, Daten in einen bereits vollen Speicher zu schreiben, oder ein Verbraucher versucht, Daten aus einem leeren Speicher zu lesen.

**Ineffizienz**:

- **Aktives Warten / Busy Waiting**: Ständige Überprüfung durch einen Thread ob eine Ressource frei ist -> verschwendet Rechenzeit

## Leser-Schreiber-Probleme

Dieses Problem tritt auf wenn mehrere Prozesse gleichzeitig auf eine Ressource zugreifen wollen. Die Methoden im Code, die zum Zugriff auf diese gemeinsamen Ressourcen benötigt werden, bilden den sogennanten **kritischen Abschnitt**.
Der kritsiche Abschnitt darf nur von einem Prozess gleichzeitig ausgeführt werden, um Dateninkonsistenzen zu verhindern.

Für den kritischen Abschnitt gelten 3 Bedingungen:

1. **Wechselseitiger Ausschluss (Mutual Exclusion)**: Wenn ein Prozess seinen kritischen Abschnitt ausführt, darf kein anderer Prozess im kritischen Abschnitt derselben Ressource sein. Nur ein Prozess zur gleichen Zeit.
2. **Fortschritt (Progress)**: Wenn kein Prozess im kritischen Abschnitt ist und ein Prozess hinein möchte, darf die Entscheidung nicht verzögert werden. Nur Prozesse, die am kritischen Abschnitt teilnehmen, dürfen entscheiden, wer als Nächstes eintritt.
3. **Begrenztes Warten (Bounded Waiting)**: Ein Prozess darf nicht unendlich lange auf Eintritt in den kritischen Abschnitt warten müssen. Es muss eine Garantie geben, dass jeder Prozess nach einer endlichen Anzahl von Zugriffen anderer Prozesse eintreten darf (Vermeidung von Verhungern/Starvation).

## Deadlocks

Ein Deadlock ist ein Zustand, in dem eine Gruppe von Threads sich gegenseitig blockiert, weil jeder auf eine Ressource wartet, die ein anderer aus der Gruppe besetzt hält (-> Problem der speisenden Philosophen).

Damit ein Deadlock entstehen kann, müssen die **vier Coffman-Bedingungen** erfüllt sein:

1. **Wechselseitiger Ausschluss (Mutual Exclusion)**: Mindestens eine Ressource wird nicht-teilbar genutzt. Nur ein Prozess kann die Ressource exklusiv verwenden.
2. **Halten und Warten (Hold & Wait)**: Prozesse, die bereits Ressourcen benutzen, fordern weitere Ressourcen an, die von anderen Prozessen genutzt werden.
3. **Ununterbrechbarkeit**: Ressourcen können einem Thread nicht entzogen werden. Sie werden nur frei, wenn der Thread sie abgibt.
4. **Zyklisches Warten**: Es entscheht eine geschlossene Kette von Prozessen ($P_0, P_1, ..., P_n$), die jeweils auf eine Ressource des nächsten Threads warten: $P_0$ wartet auf $P_1$, dieser auf $P_2$ und $P_n$ schließlich auf $P_0$.
