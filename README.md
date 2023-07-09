# mach4
Hier ist eine Sammlung von MACH4 Dokumentation 

Einführung

Das Booten eines Mikro-Kernel/Server-Systems ist etwas komplizierter als das Booten eines Makro-Kernelsystems, da mehr Dateien vorhanden sein müssen und korrekt funktionieren müssen, bevor das System Eingaben des Benutzers akzeptiert.

Die CMU-Umgebung hat auch zu einer gewissen **Komplexität** des üblichen BSD-Unix-Setups beigetragen

1 - In Mach 3.0 werden folgende Teile für einen erfolgreichen Start benötigt: 

- den 16-Sektor-Boot-Code (in der Regel vom Hersteller des Rechners geliefert), den Mikrokernel
- eine Auslagerungsdatei(paging file)
- einen Server, den der Kernel aufruft
- eine Emulationsbibliothek (zumindest für unsere Server)
- ein Benutzerprogramm zum Starten.

2-   Die anderen Komplikationen sind die Tatsache, dass wir normalerweise ein Super-Root und ein lokales Root haben, um das CMU-RFS-Dateisystem zu unterstützen, und die Tatsache, dass man beim Hochfahren als Einzelbenutzer keinen Root-Zugang hat, sondern mit der Benutzerkennung "opr" läuft.

3-  Außerdem haben wir auf CMU-Rechnern eine /etc/rc, die darauf besteht, dass /vmunix ein symbolischer Link zu dem Unix-Server ist, auf dem Sie laufen, bevor der Bootvorgang zum Multi-User abgeschlossen wird.
