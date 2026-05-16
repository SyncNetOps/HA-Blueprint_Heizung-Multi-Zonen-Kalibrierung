# **Bedienungsanleitung: SNO- Heizung \- Multi-Zonen Kalibrierung**

Herzlichen Glückwunsch zur Installation des **SNO- Multi-Zonen Kalibrierungs-Blueprints**. Diese Anleitung führt Sie durch alle Einstellungen und hilft Ihnen, die richtigen Entitäten für Ihre Smart-Home-Geräte zu finden.

## **1\. Globale Logik & Parameter (Sektion 1\)**

Diese Einstellungen gelten übergreifend für **alle** konfigurierten Zonen.

* **Cooldown-Sperre (Minuten):**  
  * *Bedeutung:* Wie lange soll nach einer Änderung des Offsets gewartet werden, bevor eine erneute Änderung erlaubt ist?  
  * *Zweck:* Ein Raum braucht Zeit, um warm zu werden. Wenn das Thermostat korrigiert wird, dauert es, bis der externe Sensor das merkt. Die Sperre verhindert, dass die Automatisierung in dieser Zeit "übersteuert". Empfehlung: 10 \- 20 Minuten.  
* **Mindest-Abweichung (°C):**  
  * *Bedeutung:* Die Toleranzgrenze. Ab welcher Differenz zwischen aktuellem und neuem Offset soll überhaupt ein Funkbefehl gesendet werden?  
  * *Zweck:* Schont die Batterien der Thermostate massiv. Standard sind 0.5 °C.

## **2\. Zonen-Konfiguration (Zone 1 bis 7\)**

Sie müssen nur Zone 1 zwingend ausfüllen. Die Zonen 2 bis 7 sind optional.

* **Bezeichnung der Zone:** Frei wählbarer Text (z.B. "Wohnzimmer"). Dient nur Ihrer Übersicht.  
* **Externer Temperatursensor (sensor):** Der Sensor im Raum (z.B. an der Wand), der die *echte* Temperatur misst.  
* **Thermostat (climate):** Die Haupt-Entität Ihres Heizkörperthermostats.  
* **Kalibrierungs-Entität (number):** Die spezielle Entität, die den Offset im Thermostat steuert. *(Siehe Liste unten).*

## **🔍 Hersteller-Liste: So heißen die Kalibrierungs-Entitäten (Offsets)**

Die Kalibrierungs-Entität ist meist eine number-Entität. Wie sie genau heißt, hängt von Ihrem Thermostat und dem genutzten System (Zigbee2MQTT, ZHA, direkte Integration) ab. Hier eine ausführliche Übersicht:

### **🟢 Zigbee-Geräte (via Zigbee2MQTT / ZHA)**

* **Aqara (E1, etc.):**  
  * number.\[Gerätename\]\_local\_temperature\_calibration  
* **Tuya / Moes / Sonoff (Zigbee TRV):**  
  * number.\[Gerätename\]\_local\_temperature\_calibration oder number.\[Gerätename\]\_local\_temp\_calib  
* **Eurotronic Spirit Zigbee:**  
  * number.\[Gerätename\]\_local\_temperature\_calibration  
* **Danfoss Ally / Hive:**  
  * Oft über number.\[Gerätename\]\_external\_measured\_room\_sensor (Achtung: Hier wird teils die gemessene Temperatur direkt übertragen, nicht der Offset. Unterstützt das TRV nur Offset, heißt es local\_temperature\_calibration).  
* **Bosch Radiator Thermostat II (Zigbee):**  
  * number.\[Gerätename\]\_local\_temperature\_calibration

### **🔵 WLAN / Hersteller-Integrationen**

* **Bosch Smart Home (Native Integration):**  
  * number.\[Gerätename\]\_room\_temperature\_offset  
* **Homematic IP (Local / Cloud):**  
  * number.\[Gerätename\]\_temperature\_offset oder number.\[Gerätename\]\_offset  
* **Shelly TRV:**  
  * number.\[Gerätename\]\_temperature\_offset  
* **Tado°:**  
  * Tado° verwendet in der nativen HA-Integration standardmäßig den Offset. Die Entität heißt meist: number.\[Gerätename\]\_offset.  
* **Fritz\!DECT (AVM):**  
  * number.\[Gerätename\]\_offset

*Tipp zur Suche in Home Assistant:* Gehen Sie auf Einstellungen \-\> Geräte & Dienste \-\> Entitäten. Suchen Sie nach dem Namen Ihres Thermostats und filtern Sie rechts nach der Domäne number. Die gesuchte Entität enthält meist Worte wie "calibration", "offset" oder "kalibrierung".