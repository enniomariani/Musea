# Musea
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Musea-Admin](https://img.shields.io/github/actions/workflow/status/enniomariani/musea-admin/build-test-publish.yml?label=Musea-Admin)](https://github.com/enniomariani/musea-admin/actions)
[![Musea-Player](https://img.shields.io/github/actions/workflow/status/enniomariani/musea-player/build-and-release.yml?label=Musea-Player)](https://github.com/enniomariani/musea-player/actions)
[![Musea-Guide](https://img.shields.io/github/actions/workflow/status/enniomariani/musea-guide/build-test-release.yml?label=Musea-Guide)](https://github.com/enniomariani/musea-guide/actions)

Ein System zur Steuerung und Verwaltung von Multimedia-Installationen in Ausstellungen und Museen.

## Was kann Musea?
Perfekt für Führungen, Dauerausstellungen und interaktive Installationen:

📚 **Zentrale Medienverwaltung** – Mediendateien (Videos und Bilder) zentral verwalten

📱 **Tablet-Steuerung** – Über intuitive Tablet-Anwendungen die Inhalte steuern

🎬 **Synchrone Wiedergabe** – Videos werden synchron auf mehreren Displays abgespielt

💡 **DMX-Lichtsteuerung** – Beleuchtung passt sich automatisch den Medien an

🔧 **Modular erweiterbar** – Flexible Erweiterung der Medien-Player möglich

## Für Museen & Ausstellungsgestalter:innen
**Professioneller Support verfügbar:**
- Installation & Einrichtung
- Schulung Ihres Personals
- Anpassungen für spezifische Anforderungen

**Referenz:** Software läuft im [Naturama Aargau](https://naturama.ch/)

📧 Kontakt: [mail@enniomariani.ch](mailto:mail@enniomariani.ch)

## Für technisches Personal

## 📦 npm Pakete
- **[musea-client](https://github.com/enniomariani/Musea-Client)**  [![npm version](https://img.shields.io/npm/v/musea-client.svg)](https://www.npmjs.com/package/musea-client) - Medien auf den Servern verwalten, synchronisieren, löschen und abspielen
- **[musea-server](https://github.com/enniomariani/Musea-Server)**  [![npm version](https://img.shields.io/npm/v/musea-server.svg)](https://www.npmjs.com/package/musea-server) - Server der Medien-Player: Speichert/löscht Medien, sendet DMX-Signale an angehängte Lampen

## Kompatibilität verschiedener Versionen
Die Versions-Nummern der Apps orientieren sich an der [semantischen Versionierung](https://semver.org/).

Nur Apps mit derselben Major-Version (z.B. 2.X.Y) sind kompatibel.

## Technische Übersicht

🚧 [Technische Dokumentation](docs/dev/README.md)

## Förderung & Danksagung
Dieses Projekt wurde grösstenteils durch das [Naturama Aargau](https://naturama.ch/) finanziert.

<img src="docs/images/naturama-logo.png" alt="logo naturama" width="200">

Herzlichen Dank an [Niklas Dunke](https://www.linkedin.com/in/niklas-dunke-5b9044191/) für das Design und die Benutzererfahrung!

## Lizenz

Dieses Projekt steht unter der [GNU General Public License v3.0](LICENSE).

Das bedeutet: Der Code darf genutzt, verändert und weitergegeben werden, aber abgeleitete Werke müssen ebenfalls unter GPL-3.0 veröffentlicht werden.
