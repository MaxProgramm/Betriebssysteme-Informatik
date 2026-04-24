## Deadlocks
### Was ist ein Deadlock?
Ein Deadlock ist wenn 2+ Prozesse unendlich stuck sind, weil sie auf gegenseitige Ressourcen warten. Dafür müssen 4 Bedingungen gleichzeitig erfüllt sein; die sogenannten Coffman-Bedingungen.

### Coffman-Bedingungen
**Gegenseitiger Ausschluss (mutual exclusion):**  
Eine Ressource kann nur von einem Prozess gleichzeitig verwendet werden (non-sharable resources) 

**Halten und Warten (hold and wait):**  
Ein Prozess "hält" mindestens eine Ressource und wartet um andere Ressourcen zu bekommen. In dieser Zeit gibt er nicht seine Ressourcen frei.

**Keine Verdrängung (No Preemption):**  
Ressourcen können nicht ohne Erlaubnis des Prozesses von einem Prozess entzogen werden. Das Betriebssytem und andere Prozesse dürfen sie nicht wegnehmen, um es einem anderen Prozess zuzuordnen.  

**Zyklisches Warten (circular wait):**  
Es entsteht eine Kette an Prozessen, die alle auf die Ressource von dem nächsten Prozess warten, der letzte dann auf die Ressource vom ersten, wodurch ein Zyklus entsteht.   


---------  

### Wie kann man Deadlocks vermeiden?
Um einen Deadlock zu vermeiden, muss man eine der Coffman Bedingungen lösen bzw. nicht eintreten lassen.  

#### Mutual Exlusion lösen
Während manche Ressourcen von ihrer Natur aus non-sharable sind (wie z.B. ein Drucker), geht diese Methode nicht bei allen. Jedoch können read-only Dateien für alle Prozesse zugänglich machen.  

#### Hold & Wait lösen  
Hierfür gibt es 2 Lösungsansätze:  
1. Das Warten verhindern, indem der Prozess im Vorraus angeben muss, welche Ressourcen er gebbraucht.
2. Das Halten verhindern, indem der Prozess alle von ihm gehaltenen Ressourcen freigibt bevor er eine Anfrage für eine andere Ressource stellen kann.

#### No Preemption lösen  
Hierfür gibt es auch 2 Lösungsansätze:  
1. Der Prozess muss eine Ressource sofort freigeben, wenn er sie nicht mehr braucht.
2. Wenn ein Prozess eine Ressource verlangt die nicht verfügbar ist, muss er all seine Ressourcen freigeben bis alle Ressourcen die er braucht frei werden (*partial allocation* verhindern)

#### Circular Wait lösen  
Jede Ressource bekommt eine ID, Prozesse können nur Ressourcen in dieser Reihenfolge beantragen
