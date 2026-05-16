# **🛠️ Technische Dokumentation & Entwickler-Hinweise**

**Projekt:** SNO- Heizung: Multi-Zonen Kalibrierung (Offset)

**Aktuelle Version:** 1.0.1 (SyncNetOps)

**Typ:** Home Assistant Blueprint (Automation)

## **📝 Changelog / Versionshistorie**

### **v1.0.1 (Hotfix) \- *Aktuelle Version***

* **FIX (Kritisch): Timeout beim Speichern behoben.** Home Assistant brach den Dry-Run bei der Einrichtung ab. Ursache war ein Jinja-Parsing-Fehler bei mehrzeiligen Template-Blöcken (\>). Behoben durch Whitespace-Control (\>-) und strikte | float Casts bei der Weiterverarbeitung von new\_offset.  
* **FIX: String-Multiplikations-Fehler abgewehrt.** cooldown\_minutes wird nun strikt als float gecastet, um zu verhindern, dass die UI den Wert als String übergibt und das Template den String 60-mal aneinanderhängt ("10" \* 60).  
* **FIX: Schema-Validierung UI.** Den Eingabefeldern zX\_name wurde der explizite Selektor selector: text: {} hinzugefügt, um Kompatibilität mit neueren HA-Core Versionen zu gewährleisten.  
* **FEATURE:** Die Kalibrierungs-Entität (Offset) akzeptiert nun neben number auch die Domain input\_number (wichtig für generische Helfer-Entitäten und bestimmte MQTT-Integrationen).

### **v1.0.0 (Initial Release)**

* Grundgerüst für bis zu 7 asynchrone Zonen.  
* Implementierung der Mindest-Abweichung (Batterieschutz).  
* Cooldown-Check und hybrides Trigger-Design integriert.

## **⚙️ Architektur & Funktionsweise**

### **1\. Hybrides Trigger-Design**

Um das Limit statischer Blueprints zu umgehen (dynamische UI-Listen sind nicht möglich), nutzt der Blueprint 7 statische Zonen, von denen Zone 2 bis 7 optional sind.

* **Zone 1 (Primär):** Löst sofort bei state Änderungen aus.  
* **Zone 2-7 (Sekundär):** Werden durch einen time\_pattern Trigger alle 3 Minuten asynchron abgearbeitet.  
* **Warum kein State-Trigger für alle?** Wenn eine optionale Zone leer bleibt, würde ein state-Trigger mit einer leeren entity\_id beim Neuladen der Automatisierungen in Home Assistant einen fatalen Error im Core-Log werfen. Das time\_pattern umgeht dies elegant.

### **2\. Dynamisches Array & Kapselung**

Alle Variablen werden in das Array zones gepusht. Die repeat \-\> for\_each Aktion iteriert darüber.

* **Condition-Gates:** Bevor eine Zone berechnet wird, prüfen drei template Conditions:  
  1. Ist die Zone überhaupt konfiguriert? (\!= '')  
  2. Sind die Entitäten "online" und liefern numerische Werte? (Schützt vor unavailable beim Boot-Vorgang).  
  3. Ist der Cooldown abgelaufen?

### **3\. Technische Fallstricke & Best Practices (Lessons Learned aus v1.0.1)**

#### **Whitespace Control in Jinja2**

In YAML und Jinja2 erzeugen mehrzeilige if/else Blöcke mit dem \> Operator versteckte Zeilenumbrüche (\\n).

\# FALSCH (Führt zu " \\n 5.0 \\n "):  
new\_offset: \>  
  {% if raw\_offset \> max %}  
    {{ max }}  
  ...

Wird dieser Wert später subtrahiert (new\_offset \- current\_offset), stürzt der HA Parser ab.

**Lösung:** Strikte Nutzung von \>- und {%- ... \-%} um Whitespaces restlos zu strippen, gefolgt von einem sauberen | float Cast in nachfolgenden Conditions.

#### **Datentyp-Sicherheit bei Input-Variablen**

Home Assistant Blueprints übergeben UI-Eingaben nicht immer typsicher. Ein Number-Slider kann im Template plötzlich als String ("10") ankommen.

{{ cooldown\_minutes \* 60 }} ergibt bei einem String nicht 600, sondern hängt die "10" sechzig Mal aneinander, was den Arbeitsspeicher fluten kann.

**Lösung:** Immer explizit casten: {{ cooldown\_minutes | float \* 60 }}.

#### **Hardware-Limit-Erkennung**

Das Template liest die Attribute min und max der Offset-Entität aus. Fehlen diese Attribute (z.B. bei unsauber programmierten Custom Components), springt der Fallback ein (default(-5.0, true)), damit die Berechnung nicht fehlschlägt.

## **🚀 Hinweise zur Weiterentwicklung**

Wenn du den Blueprint künftig erweitern möchtest, hier einige Ansätze:

1. **Fenster-Offen-Erkennung (Blocker)**  
   * **Umsetzung:** Ein optionaler binary\_sensor Selektor (Geräteklasse window oder door) pro Zone.  
   * **Logik:** In der repeat-Schleife eine weitere Bedingung hinzufügen: {{ states(repeat.item.window) \!= 'on' }}. Nur wenn das Fenster zu ist, wird kalibriert.  
2. **Zonenspezifische Toleranzen**  
   * **Umsetzung:** Das globale min\_delta könnte pro Zone überschrieben werden (z. B. Bad braucht genauere Werte als der Flur).  
   * **Logik:** Neues Input-Feld in den Sektionen, das in der Variablen-Zuweisung via Fallback (default(global\_min\_delta)) eingebunden wird.  
3. **Benachrichtigungen (Notify)**  
   * **Umsetzung:** Bei extremen Abweichungen (z. B. Delta \> 3°C über mehr als 30 Minuten) könnte eine Nachricht verschickt werden (Könnte auf einen Defekt oder ein offenes Fenster hindeuten).