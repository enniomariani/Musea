# Netzwerkprotokoll
Das Protokoll besteht aus einem Array aus Strings oder Uint8Arrays (für Dateidaten).
Alle Segmente (Elemente) des Arrays werden in ein Uint8Array umgewandelt und gesendet.
Basiert auf dem [darunterliegenden Protokoll](network-protocol-base.md).

## Nachrichtenformat
```
[kategorie, aktion, ...parameter]
```

---

## Nachrichten an den Server

### `media.put`
**Richtung:** Client → Server

Speichert ein Medium auf dem Server.

**Anfrage:**
- `mediaType` (string): Medientyp (Dateiendung) - `jpeg`, `mp4` oder `png`
- `data` (Uint8Array): Binäre Daten des Mediums

**Antwort:**
- `id` (string): Eindeutige ID des gespeicherten Mediums (integer 0-X)

**Beispiel:**
```javascript
// Client sendet:
['media', 'put', 'mp4', videoData]

// Server antwortet:
['media', 'put', '42']
```

---

### `media.delete`
**Richtung:** Client → Server

Löscht Medium mit der angegebenen ID auf dem Server.

**Anfrage:**
- `id` (string): Medium-ID (0-X, eindeutig auf jedem Server)

**Antwort:** Keine

**Beispiel:**
```javascript
['media', 'delete', '42']
```

---

### `media.control.play`
**Richtung:** Client → Server

Spielt Video oder Bild auf dem Server ab.

**Anfrage:**
- `id` (string): Medium-ID (0-X)

**Antwort:** Keine

**Beispiel:**
```javascript
['media', 'control', 'play', '5']
```

---

### `media.control.stop`
**Richtung:** Client → Server

Stoppt das laufende Video oder blendet das Bild aus.

**Anfrage:** Keine Parameter

**Antwort:** Keine

**Beispiel:**
```javascript
['media', 'control', 'stop']
```

---

### `media.control.pause`
**Richtung:** Client → Server

Pausiert das laufende Video.
Kein Einfluss auf Bilder.

**Anfrage:** Keine Parameter

**Antwort:** Keine

**Beispiel:**
```javascript
['media', 'control', 'pause']
```

---

### `media.control.seek`
**Richtung:** Client → Server

Springt zu einer bestimmten Position im laufenden Video.
Kein Einfluss auf Bilder.

**Anfrage:**
- `position` (string): Position in Sekunden (0-X)

**Antwort:** Keine

**Beispiel:**
```javascript
['media', 'control', 'seek', '30']
```

---

### `media.control.sync`
**Richtung:** Client → Server

Synchronisiert mehrere laufende Videos. Wenn ein Video an Position 30 ist, der sync-call aber 32 vorgibt, wird das Video schneller abgespielt, bis es "aufholt".
Wenn der erhaltene Wert kleiner ist als die aktuelle Position, wird das Video langsamer abgespielt.
Ist der Unterschied zu gross springt der Player direkt zur neuen Position.
Hat keinen Einfluss auf Bilder.

**Anfrage:**
- `position` (string): Position in Sekunden

**Antwort:** Keine

**Beispiel:**
```javascript
['media', 'control', 'sync', '45']
```

---

### `media.control.forward`
**Richtung:** Client → Server

Spult ein Video vorwärts.
Kein Einfluss auf Bilder.

**Anfrage:** Keine Parameter

**Antwort:** Keine

**Beispiel:**
```javascript
['media', 'control', 'forward']
```

---

### `media.control.rewind`
**Richtung:** Client → Server

Spult ein Video rückwärts.
Kein Einfluss auf Bilder.

**Anfrage:** Keine Parameter

**Antwort:** Keine

**Beispiel:**
```javascript
['media', 'control', 'rewind']
```

---

### `contents.put`
**Richtung:** Client → Server

Überschreibt die auf dem Server vorhandene contents-Datei mit der übertragenen.

**Anfrage:**
- `data` (string): JSON-Daten der Contents-Datei

**Antwort:** Keine

**Beispiel:**
```javascript
['contents', 'put', '{data...}']
```

---

### `contents.get`
**Richtung:** Client → Server

Lädt die contents-Datei vom Server herunter.
Die contents-Datei existiert nur auf dem Player mit der Rolle "Controller".

**Anfrage:** Keine Parameter

**Antwort:**
- `data` (string): JSON-Daten der contents-Datei oder `{}` wenn keine existiert

**Beispiel:**
```javascript
// Client sendet:
['contents', 'get']

// Server antwortet:
['contents', 'put', '{data...}']
```

---

### `system.volume.mute`
**Richtung:** Client → Server

Schaltet den Ton aus.

**Anfrage:** Keine Parameter

**Antwort:** Keine

**Beispiel:**
```javascript
['system', 'volume', 'mute']
```

---

### `system.volume.unmute`
**Richtung:** Client → Server

Schaltet den Ton an.

**Anfrage:** Keine Parameter

**Antwort:** Keine

**Beispiel:**
```javascript
['system', 'volume', 'unmute']
```

---

### `system.volume.set`
**Richtung:** Client → Server

Setzt die Lautstärke.

**Anfrage:**
- `amount` (string): Lautstärke zwischen 0.0 und 1.0

**Antwort:** Keine

**Beispiel:**
```javascript
['system', 'volume', 'set', '0.7']
```

---

### `light.preset`
**Richtung:** Client → Server

Sendet vordefinierte Kanal-Stärken an die DMX-Lampen.

**Anfrage:**
- `preset` (string): Preset-Wert - `0`, `1` oder `2`

**Antwort:** Keine

**Beispiel:**
```javascript
['light', 'preset', '1']
```

---

### `network.ping`
**Richtung:** Client → Server

Überprüfung ob die Server-App erreichbar ist. Nicht mit ICMP-Ping verwechseln.

**Anfrage:** Keine Parameter

**Antwort:**
- Erwartet ein `pong`-Signal als Antwort

**Beispiel:**
```javascript
// Client sendet:
['network', 'ping']

// Server antwortet:
['network', 'pong']
```

---

### `network.register`
**Richtung:** Client → Server

Registriert einen Client beim Server. Ist die Voraussetzung dafür, dass der Server Medien, Contents- und Systembefehle akzeptiert.

**Anfrage:**
- `appType` (string): App-Typ - `admin` (Guide- oder Admin-App) oder `user` (z.B. ein Touchscreen, der die MS steuert)

**Antwort:**
- `connectionResult` (string): `accepted`, `rejected` oder `accepted_block` (siehe Server-Antworten weiter unten)

**Beispiel:**
```javascript
// Client sendet:
['network', 'register', 'admin']

// Server antwortet:
['network', 'registration', 'accepted']
```

---

### `network.isRegistrationPossible`
**Richtung:** Client → Server

Prüft, ob eine Registrierung mit der Rolle "admin" möglich ist.

**Anfrage:** Keine Parameter

**Antwort:**
- `result` (string): `yes` wenn keine andere Admin-App verbunden ist, `no` wenn bereits ein Client der Rolle "Admin" verbunden ist

**Beispiel:**
```javascript
// Client sendet:
['network', 'isRegistrationPossible']

// Server antwortet:
['network', 'isRegistrationPossible', 'yes']
```

---

### `network.disconnect`
**Richtung:** Client → Server

Sendet dem Server die Information, dass die Verbindung getrennt wird.

**Anfrage:** Keine Parameter

**Antwort:** Keine

**Beispiel:**
```javascript
['network', 'disconnect']
```

---

## Nachrichten an den Client

### `network.registration`
**Richtung:** Server → Client

Bestätigung der Registrierung des Clients.

**Antwort:**
- `connectionResult` (string):
    - `accepted` - Registrierung erfolgreich
    - `rejected` - Registrierung abgelehnt
    - `accepted_block` - App mit der Rolle "user" konnte sich registrieren, es ist aber auch eine App mit der Rolle "admin" verbunden.
Die App mit der Rolle "user" wird blockiert, bis die "admin"-App sich abmeldet.

**Beispiel:**
```javascript
['network', 'registration', 'accepted_block']
```

---

### `system.block`
**Richtung:** Server → Client

Sendet eine Nachricht, dass der Client, der als "user" verbunden ist, blockiert werden soll.
Blockiert heisst hier: dass die App keine Nachrichten mehr an den Server sendet.

**Anfrage:** Keine Parameter

**Antwort:** Keine

**Beispiel:**
```javascript
['system', 'block']
```

---

### `system.unblock`
**Richtung:** Server → Client

Sendet eine Nachricht, dass der Client, der als "user" verbunden ist, wieder freigegeben werden soll.

**Anfrage:** Keine Parameter

**Antwort:** Keine

**Beispiel:**
```javascript
['system', 'unblock']
```

---

### `network.registration`
**Richtung:** Server → Client

Antwort auf eine Registrierungsanfrage. Bei `accepted` wurde der Client erfolgreich registriert.
Momentan kann nur eine App pro Rolle "admin" und "user" gleichzeitig registriert sein.

**Antwort:**
- `connectionResult` (string): `accepted` oder `rejected`

**Beispiel:**
```javascript
['network', 'registration', 'accepted']
```

---

### `network.isRegistrationPossible`
**Richtung:** Server → Client

Antwort auf die Anfrage, ob eine Registrierung mit der Rolle "admin" möglich ist.

**Antwort:**
- `result` (string): `yes` oder `no`

**Beispiel:**
```javascript
['network', 'isRegistrationPossible', 'yes']
```

---

### `contents.put`
**Richtung:** Server → Client

Antwort, wenn der Client die contents-Datei vom Server angefragt hat.

**Antwort:**
- `data` (string): JSON-Daten der Contents-Datei oder `{}` wenn keine existiert

**Beispiel:**
```javascript
['contents', 'put', '{"items": [...]}']
```

---

### `media.put`
**Richtung:** Server → Client

Antwort, wenn ein Client ein Medium auf dem Server speichert.

**Antwort:**
- `id` (string): eindeutige ID des lokal gespeicherten Mediums (0 - X)

**Beispiel:**
```javascript
['media', 'put', '42']
```

---