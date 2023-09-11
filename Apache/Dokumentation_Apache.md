# M239

## Apache Webserver

Der Apache Webserver gehört zu den `HTTP`-Diensten mit dem grössten Bekanntheitsgrad. Die Entwicklung begann kurz nach dem "Ur-Webserver", dem CERN httpd.

Nebst der Auslieferung von Daten über `HTTP`, kann Apache auch als `(Reverse-)Proxy` eingesetzt werden. Auch kann es um Module wie `PHP`, `Python`, u.v.m. erweitert werden.

Darunter finden sich auch `mod_lua` und `mod_http2`:

### mod_lua

Dieses Modul erlaubt es dem Server Script, welche in der Programmiersprache `Lua` geschrieben wurde, einzusetzen. Darunter findet sich die Implementierung von `mapping requests` zu Daten, Generierung von dynamischen `Responses` u.v.m.

### mod_http2

Durch die Nutzung dieses Moduls kann Apache mit `HTTP/2` arbeiten. Hierunter fällt das `core engine` von `HTTP/2`.

### Weitere Webserver auf dem Markt

Apache ist zwar der bekannteste Webserver-Dienst, jedoch bei weitem nicht der einzige auf dem Markt. Unter den Konkurrenten finden sich `Nginx`, `LiteSpeed`, `Caddy` und `Tomcat`. <br>

Nichtsdestotrotz fällt der Marktanteil von `Apache` auf 30-40%, gefolgt von `Nginx` mit 20-30%

## Apache installieren

Die Installation wird auf einer Ubuntu-Maschine gemacht:

```bash
# System aktualisieren
sudo apt update && sudo apt upgrade -y

# Paket herunterladen
sudo apt install apache2
```

Nun kann getestet werden, ob die Installation funktioniert hat: