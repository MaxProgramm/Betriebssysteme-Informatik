# Betriebsmittelzuteilungsgraphen

sind Graphen, die visualisieren, welche Threads welche Ressourcen belegen bzw. angefordert haben. Dabei können Verklemmungen(Deadlocks) im Falle einer zyklischen Konstellation leicht erkannt werden.

## Bestandteile


| Thread | Ressource | Belegung | Anforderung |
| ------ | --------- | -------- | ----------- |
| ![](Bilder/B-Thread.png) <br> Runde Ecken/Kreis| ![](Bilder/B-Ressource.png) <br> Rechteck| ![](Bilder/B-Belegung.png) | ![](Bilder/B-Anforderung.png) <br> Auch als gestrichelter Pfeil möglich|
<br>  
<br>  
<br>  

![fail](Bilder/B-Beispiel.png)
<br>  
<br>  
<br>  
Je nach Ablauf kann dabei eine zyklische Wartesituation, wie im obigen Beispiel zu sehen, entstehen, die auf einen Deadlock visualisiert.
Beispiel mit 3 Abläufen, 3 Betriebsmitteln und folgendem Aufbau der Abläufe:
<br>  

![](Bilder/B-situation.png)
<br>  
<br>  
| Ausführung mit Deadlock | Ausführung mit Deadlock |
| ----------------------- | ----------------------- |
| ![](Bilder/B-with.png) | ![](Bilder/B-without.png) |


