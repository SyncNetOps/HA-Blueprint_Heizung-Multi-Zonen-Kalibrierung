# 🌡️ SNO-Heizung: Multi-Zonen Kalibrierung (Offset)

Ein hochoptimierter Home Assistant Blueprint, der die Temperaturabweichung zwischen externen Raumsensoren und Heizkörper-Thermostaten dynamisch, zuverlässig und herstellerunabhängig ausgleicht.

## ❓ Das Problem

Heizkörper-Thermostate messen die Temperatur direkt an der Heizung. Dort staut sich die Wärme, und es ist meistens viel wärmer als im restlichen Raum. Die Folge: Das Thermostat regelt zu früh ab, der Raum bleibt kalt.

## 💡 Die Lösung

Dieser Blueprint liest die echte Raumtemperatur von einem externen Sensor (z. B. auf dem Sofa oder am Schreibtisch) aus, berechnet die exakte Temperaturdifferenz zum Heizkörper und trägt diese Differenz vollautomatisch als Offset (Kalibrierung) in das Thermostat ein.

## ✨ Features & Vorteile

* **🌍 Multi-Zone in einer Instanz:** Verwalte bis zu 7 Heizzonen (Räume) zentral in einer einzigen Automatisierung. Das hält deinen Home Assistant aufgeräumt.
* **🔋 Extremer Batterieschutz:** Ein einstellbares Mindest-Delta (Toleranz) sorgt dafür, dass nicht bei jeder 0,1°C Schwankung ein Funkbefehl gesendet wird.
* **🛡️ Anti-Aufschaukeln (Cooldown):** Eine konfigurierbare Zeitsperre verhindert Endlos-Schleifen und das "Aufschaukeln" der Offset-Werte nach einer Anpassung.
* **🛠️ Absolut Fehlertolerant:** Ausfallsichere Templates und hybride Trigger fangen Fehler ab, falls ein Sensor beim HA-Neustart (Boot) kurzzeitig nicht verfügbar ist oder eine Zone leer gelassen wird. Kein Spam im Core-Log!
* **🔓 Herstellerunabhängig:** Funktioniert mit jedem Thermostat (Homematic, Bosch, Aqara, Tuya, Fritz!DECT etc.), das eine Kalibrierungs-Entität (`number` oder `input_number`) bereitstellt.

## 📋 Voraussetzungen

Für jede Heizzone benötigst du in Home Assistant:
1. Einen **externen Temperatursensor** (`sensor` mit `device_class: temperature`).
2. Das **Heizkörperthermostat** (`climate` Entität), das die aktuelle Temperatur am Heizkörper misst.
3. Die **Kalibrierungs-Entität / Offset-Entität** deines Thermostats (`number` oder `input_number` Entität).

## 🚀 Installation

Es gibt verschiedene Möglichkeiten, diesen Blueprint in deinem Home Assistant zu installieren.

### Methode 1: 1-Klick Installation (Empfohlen)

Klicke einfach auf den folgenden Button, um den Blueprint direkt in deinen Home Assistant zu importieren:

[![Import Blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FSyncNetOps%2FHA-Blueprint_Heizung-Multi-Zonen-Kalibrierung%2Fblob%2Fmain%2FSNO-thermostat_calibration.yaml)

### Methode 2: Import über URL

1. Öffne deinen Home Assistant.
2. Gehe zu **Einstellungen > Automatisierungen & Szenen > Blueprints**.
3. Klicke unten rechts auf **Blueprint importieren**.
4. Füge die folgende URL ein und klicke auf **Vorschau** und dann auf **Blueprint importieren**:  
   `https://github.com/SyncNetOps/HA-Blueprint_Heizung-Multi-Zonen-Kalibrierung/blob/main/SNO-thermostat_calibration.yaml`

### Methode 3: Manuelle Installation

1. Lade die Datei `SNO-thermostat_calibration.yaml` aus diesem Repository herunter.
2. Kopiere die Datei in deiner Home Assistant Instanz in den Ordner:  
   `/config/blueprints/automation/SyncNetOps/` *(Erstelle den Ordner, falls er nicht existiert).*
3. Gehe in Home Assistant zu **Entwicklerwerkzeuge > YAML** und lade die Vorlagen neu (oder starte HA neu).

## 📖 Dokumentation & Hilfe

Wir haben ausführliche Dokumentationen für Benutzer und Entwickler bereitgestellt:

* **🌍 [Projekt-Website & FAQ (Hilfeseite)](https://sno.mb222.de/mzk-faq/)** – Antworten auf die häufigsten Fragen und Praxis-Tipps.
* **📘 Benutzerhandbuch (Repository)** – Schritt-für-Schritt Erklärung aller Felder und Einstellungen des Blueprints sowie eine Liste gängiger Thermostate und deren Offset-Bezeichnungen.
* **🛠️ Technische Dokumentation (Repository)** – Für Entwickler: Erklärungen zur Architektur, Fehlerbehebung (Hotfix v1.0.1) und Hinweise zur Weiterentwicklung.

## 🏷️ Versionshistorie

**Aktuelle Version: 1.0.1 (Hotfix)**
* Behebt einen Timeout-Fehler beim Speichern in neueren Home Assistant Versionen (Jinja-Parsing Optimierungen).
* Datentyp-Sicherheit (`float` Casts) bei Cooldown und Mindest-Delta hinzugefügt.
* Schema-Validierung für Texteingaben verbessert.

*Version 1.0.0:* Initiales Release mit 7 Zonen und Fehlertoleranz.

---
**Entwickelt von [SyncNetOps](https://github.com/SyncNetOps) | Open Source für ein smartes Zuhause.**