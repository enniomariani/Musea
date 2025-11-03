# Basis-Übertragungsprotokoll
Die Segmente der Nachricht werden in ein Uint8Array umgewandelt und über WebSocket versendet.

## Nachrichtenstruktur

Jede Nachricht besteht aus einem **Header** gefolgt von den **Segmenten**.

**Beispiel**
``['media', 'put', 'mp4', videoData]``
- 4 Segmente, jedes Array-Element ist ein Segment

---

## Header-Format

Der Header besteht aus **minimal 8 Bytes**:

### Bytes 1 und 2: Chunk-Anzahl
- **Größe:** 2 Bytes
- **Inhalt:** Anzahl der Chunks, in die die Nachricht unterteilt ist
- **Verwendung:** Wenn eine Nachricht größer als 2 MB ist (z.B. wenn grosse Video-Dateien übertragen werden), wird sie in verschiedene Chunks unterteilt, die nacheinander gesendet werden
- ⚠️ **Wichtig:** Die gesamte Nachricht wird in Chunks unterteilt - unabhängig von den einzelnen Segmenten im darüberliegenden Protokoll.

### Byte 3: Anzahl Segmente
- **Größe:** 1 Byte
- **Inhalt:** Anzahl der Segmente der Nachricht als unsigned Integer.

### Bytes 4+: Beschreibung der Segmente
Für jedes Segment folgen **5 Bytes** mit folgender Struktur:

#### Byte 1: Datentyp
- `0` = Text in UTF-8
- `1` = Binäre Daten (Bilder, Videos, andere Dateien)

#### Bytes 2-5: Startposition
- **Größe:** 4 Bytes (32-bit unsigned Integer)
- **Inhalt:** Position des Start-Bytes des Segments in der Nachricht

**Beispiel Header-Struktur:**
```
[Chunk-Anzahl (2 Bytes)][Teil-Anzahl (1 Byte)][Segment 1 Info (5 Bytes)][Segment 2 Info (5 Bytes)]...
```

---

## Segmente
Momentan maximal 4.

### Segment 1 und 2
- **Format:** Immer UTF-8 Strings oder leer
- **Verwendung:** Kategorie und Aktion des Befehls

### Segment 3
- **Format:** UTF-8 String oder leer
- **Verwendung:** Parameter (z.B. bei `network.registration.admin`)
- **Hinweis:** Auch JSON-Dateien werden als Strings übertragen

### Segment 4
- **Format:** Binäre Daten oder leer
- **Verwendung:** Bis jetzt ausschliesslich Dateien (Bilder oder Videos) als binäre Daten

---

## Beispiel

Eine Nachricht zum Hochladen eines Videos (`media.put`) sieht wie folgt aus:

**Annahme:**
- Segment 1 "media" startet bei Byte 23
- Segment 2 "put" startet bei Byte 28
- Segment 3 "mp4" startet bei Byte 31
- Segment 4 (Video-Daten) startet bei Byte 34

### Header (23 Bytes):
```
Byte 0-1:   0x01 0x00           → 1 Chunk (Annahme, dass die Video-Datei nicht so gross sind)
Byte 2:     0x04                → 4 Segmente

Byte 3:     0x00                → Segment 1: UTF-8 Text
Byte 4-7:   0x17 0x00 0x00 0x00 → Start-Position: Byte 23 (Little Endian)

Byte 8:     0x00                → Segment 2: UTF-8 Text
Byte 9-12:  0x1C 0x00 0x00 0x00 → Start-Position: Byte 28 (Little Endian)

Byte 13:    0x00                → Segment 3: UTF-8 Text
Byte 14-17: 0x1F 0x00 0x00 0x00 → Start-Position: Byte 31 (Little Endian)

Byte 18:    0x01                → Segment 4: Binäre Daten
Byte 19-22: 0x22 0x00 0x00 0x00 → Start-Position: Byte 34 (Little Endian)
```

### Segmente (ab Byte 23):
```
Byte 23-27: "media"              → Segment 1 (5 Bytes UTF-8)
Byte 28-30: "put"                → Segment 2 (3 Bytes UTF-8)
Byte 31-33: "mp4"                → Segment 3 (3 Bytes UTF-8)
Byte 34+:   [Video-Daten]        → Segment 4 (variable Größe, binär)
```

### Vollständige Nachricht visualisiert:
```
[Header 23 Bytes][media][put][mp4][Video-Daten...]
```

---