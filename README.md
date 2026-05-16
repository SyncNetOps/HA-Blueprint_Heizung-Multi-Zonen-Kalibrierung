# 🌡️ SNO-Heizung: Multi-Zonen Kalibrierung (Offset)

Ein hochoptimierter Home Assistant Blueprint, der die Temperaturabweichung zwischen externen Raumsensoren und Heizkörper-Thermostaten dynamisch, zuverlässig und herstellerunabhängig ausgleicht.

## ❓ Das Problem

Heizkörper-Thermostate messen die Temperatur direkt an der Heizung. Dort staut sich die Wärme, und es ist meistens viel wärmer als im restlichen Raum. Die Folge: Das Thermostat regelt zu früh ab, der Raum bleibt kalt.

## 💡 Die Lösung

Dieser Blueprint liest die echte Raumtemperatur von einem externen Sensor (z. B. auf dem Sofa oder am Schreibtisch) aus, berechnet die exakte Temperaturdifferenz zum Heizkörper und trägt diese Differenz vollautomatisch als Offset (Kalibrierung) in das Thermostat ein.

## ✨ Features & Vorteile

* 🌍 **Multi-Zone in einer Instanz:** Verwalte bis zu **7 Heizzonen** (Räume) zentral in einer einzigen Automatisierung. Das hält deinen Home Assistant aufgeräumt.

* 🔋 **Extremer Batterieschutz:** Ein einstellbares *Mindest-Delta* (Toleranz) sorgt dafür, dass nicht bei jeder 0,1°C Schwankung ein Funkbefehl gesendet wird.

* 🛡️ **Anti-Aufschaukeln (Cooldown):** Eine konfigurierbare Zeitsperre verhindert Endlos-Schleifen und das "Aufschaukeln" der Offset-Werte nach einer Anpassung.

* 🛠️ **Absolut Fehlertolerant:** Ausfallsichere Templates und hybride Trigger fangen Fehler ab, falls ein Sensor beim HA-Neustart (Boot) kurzzeitig nicht verfügbar ist oder eine Zone leer gelassen wird. Kein Spam im Core-Log!

* 🔓 **Herstellerunabhängig:** Funktioniert mit jedem Thermostat (Homematic, Bosch, Aqara, Tuya, Fritz!DECT etc.), das eine Kalibrierungs-Entität (`number` oder `input_number`) bereitstellt.

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

2. Gehe zu **Einstellungen** > **Automatisierungen & Szenen** > **Blueprints**.

3. Klicke unten rechts auf **Blueprint importieren**.

4. Füge die folgende URL ein und klicke auf *Vorschau* und dann auf *Blueprint importieren*:

   ```text
   [https://github.com/SyncNetOps/HA-Blueprint_Heizung-Multi-Zonen-Kalibrierung/blob/main/SNO-thermostat_calibration.yaml](https://github.com/SyncNetOps/HA-Blueprint_Heizung-Multi-Zonen-Kalibrierung/blob/main/SNO-thermostat_calibration.yaml)