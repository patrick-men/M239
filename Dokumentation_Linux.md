# M239

## Was ist Linux

Linux ist ein monolithischer OS-Kernel (simples OS, in welchem Files, Memory, Devices und Prozesse vom Kernel gemanaged werden). <br>
Dieser Kernel ist offen, und es gibt entsprechende verschiedenste Distributionen, welche auf demselben Kernel aufbauen. 

Eine Distribution ist eine Verarbeitung des Kernels: Dieser wird mit eigens gewählten Programmen zu einem Paket zusammengeführt, wodurch sich in der Nutzung unterschiedende Distributionen ergeben. Auch dank der Open-Source-Philosophie gibt es heutzutage zig-tausende Distros; darunter finden sich die gängigen Distros wie Ubuntu, Arch, Red Hat - aber auch weniger bekannte, darunter Manjaro, NixOS, EndeavourOS. <br>
Ein nennenswerter Unterschied für den Endnutzer zeigt sich in der Nutzung unter anderem bei der Paketverwaltung; diese wird je nach Distro anders gehandhabt. So nutzen Debian-basiert Distros das apt (Advanced package tool), wohingegen Arch-basierte Distros oftmals pacman oder yay. 

## Konsole

Linux, sowie auch andere *nix OS, basieren die primäre Schnittstelle zum Endbenutzer per Kommandozeile. Das OS selbst kann vollständig per Kommandozeile verwaltet werden - viele der auf den OS verfügbaren Programme und Tools sind entweder gänzlich oder zusätzlich per cli (command-line interface) nutzbar.

### Befehle

Bei der Arbeit mit dem cli werden Befehle genutzt. Diese beinhalten zumeist folgende Elemente:

```bash
<prompt>: <pwd> <Befehl> <Kurzoption> <Langoption> <Argument>

#Beispiel:
vmadmin@bmLP1: ~$ cp -a --verbose test.txt test.txt.backup
```

Dort wo man die Struktur des Befehls nicht kennt, gibts es im cli selbst zwei Methoden um die vom Programm gewünschten Angaben kennenzulernen:

- man <Befehl>
    - Hier finden sich die "manpages", welche eine Sammlung an Handbüchern zu einer Grosszahl Befehlen ist
- <Befehl> -h, <Befehl> --help
    - Viele cli-Tools beinhalten das Argument "-h" oder "--help". Der Output zeigt Informationen zum Programm sowie zu der Befehlsstruktur sowie -optionen

### Tastenkürzel

Je nach gewähltem Konsolenemulator sind die verfügbaren Tastenkürzel anders. Die folgenden gelten jedoch für nahezu alle:

- CTRL + C
    - Laufenden Befehl abbrechen
- CTRL + A/E
    - Cursor zum Anfang/Ende verschieben
- CTRL + K
    - Vom Cursor bis zum Ende löschen
- CTRL + L
    - Bildschirm leeren, äquivalent zu `clear`
- CTRL + D
    - End Of File senden; Verlassen der Session
- Pfeil hoch/runter
    - Durch die Befehls-History navigieren
    - Die History lässt sich auch mit dem Befehl `history` anzeigen

## Dateisystem

Im Dateisystem finden sich nebst der eigentlichen Daten auch Angaben darüber, wie sie gehandhabt werden sollen; Zugriffsrechte, Schreib- und Lesegeschwindigkeiten sowie max. Datei- und Volumengrösse sind einige der Angaben. <br>
Als solches stellt es eine wichtige Grundlage für die Funktionalität dar, was ein wichtiger Entscheidungsfaktor bei der OS-Suche darstellt.

Grundsätzlich sieht das Root-Directory wie folgt aus:

```Markdown
/
├── bin > Essenzielle Binaries
├── boot > Boot-Files
├── dev > Gerätefunktionen
├── etc > Konfigurationsdateien
├── home > Userhomes
├── lib > Gemeinsame Bibliotheken
├── media > Wechselmedien
├── mnt > Temporäre Mountpoints
├── opt > Optionale Software
├── proc > Prozessinformationen
├── root > Userhome des Administrators
├── run > Laufzeitdaten
├── sbin > System-Binaries
├── srv > Dienstspeicher
├── sys > Kernel-Schnittstelle
├── tmp > Temporäre Dateien
├── usr > Benutzerbezogene Daten
└── var > Variable Daten

```

## Dateisystem navigieren

|Befehl|Funktion|
|---|---|
|pwd| print working directory > Gibt aktuelles Verzeichnis aus|
|ls| list > Zeit alle Dateien im angegebenen (default: aktuelles) Verzeichnis an|
|cd| change directory > Wechselt Verzeichnisse, Navigation|

`ls` hat verschiedenste Optionen, welche mehr Informationen zu den Dateien angeben können; darunter finden sich Berechtigungen, Dateigrössen u.v.m. <br>
Wenn dem Befel `cd` kein Argument angehängt wird, dann wird das aktuelle Verzeichnis auf das Home-Verzeichnis des Users gewechselt. Für eine gezielte Nagivation muss man als Argument das gewünschte (Sub-)Directory angeben.

## Berechtigungen

Wie bereits genannt, kann man mit `ls` und den entsprechenden Optionen die Berechtigungen anzeigen lassen. Diese folgen immer dieser Struktur:

```Markdown
<File Type Indicator><Besitzer-Berechtigungen>-<Gruppen-Berechtigungen>-<Sonstige Berechtigungen>
```

Beispiel-Output von `ls -l`:

```bash
-rw-r--r--r 1 user group 12345 Aug 21 10:00 test.txt
```
Hier erkennt man folgendes:

- Der Besitzer hat Lese- und Schreibberechtigungen, alle anderen haben lediglich Leseberechtigung.
- '1' ist die Anzahl an hard links zu diesem File
- 'user' ist der Besitzer des Files
- 'group' ist die mit dem File assoziierte Gruppe
- '12345' ist die File-Grösse
- 'Aug 21 10:00' ist das Datum der letzten File-Bearbeitung
- 'test.txt' ist der File-Name

### Berechtigungs-String

- File Type Indicator (1 Zeichen)
    - '-': Reguläre Datei
    - 'd': Directory
    - 'l': Symbolic Link
    - 'c': Character device file
    - 'b': Block device file
    - 'p': Named pipe 
    - 's': Socket
- Besitzer-Berechtigungen (3 Zeichen)
    - 'r': Leseberechtigung
    - 'w': Schreibberechtigung
    - 'x': Ausführberechtigung
    - '-': Keine Berechtigung
- Gruppen-Berechtigungen (3 Zeichen)
    - 'r': Leseberechtigung
    - 'w': Schreibberechtigung
    - 'x': Ausführberechtigung
    - '-': Keine Berechtigung
- Sonstige Berechtigungen (3 Zeichen)
    - 'r': Leseberechtigung
    - 'w': Schreibberechtigung
    - 'x': Ausführberechtigung
    - '-': Keine Berechtigung

Berechtigungen können auch wie folgt dargestellt werden:

|rwx|oktaler Wert|
|--|--|
|001|1|
|010|2|
|011|3|
|100|4|
|101|5|
|110|6|
|111|7|

## Ein- und Ausgabeumlenkung

Die Ein- und Ausgabetypen sind:

- STDIN, Standard Input
    - Durch diesen bekommen Programme die Eingabe
- STDOUT, Standard Output
    - Durch diesen Kanal wird die normale Ausgabe geschrieben
- STDERR, Standard Error
    - Durch diesen Kanal werden die Fehlermeldungen geschrieben

Diese können wie folgt umgelenkt werden:

```bash
# Umlenkung von STDOUT, hier in file hinein
cmd > file

# Umlenkung von STDERR, hier in das Device /dev/null
cmd 2>/dev/null

# Umlenkung von STDIN, cmd bekommt als Eingabedaten den Inhalt von file
cmd < file
```
## Pipes

Eine weiter Form der Umlenkung wird mit den Pipes angegangen. Mit dieser wird der Output eines Befehls direkt in einen anderen Befehl gegeben.

So beispielsweise:

```bash
ls | grep -i "test.txt"
```

Hier wird mit ls der Inhalt des aktuellen Directories ausgegeben, und direkt mit "grep" durchsucht.