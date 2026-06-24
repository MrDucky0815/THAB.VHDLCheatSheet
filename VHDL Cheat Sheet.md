# VHDL Cheat Sheet

## 0. Inhaltsverzeichnis
1. [Entity-Deklaration](#1-entity-deklaration)
2. [Architecture-Deklaration](#2-architecture-deklaration)
    1. [Signalzuweisung](#211-bedingte-signalzuweisung-when-else)
    2. [Selektive Signalzuweisung](#212-selektive-signalzuweisung-with-select)
    3. [Entity-Instanziierung als Komponente](#22-entity-instanziierung-als-komponente)
    4. [Direkte Entity-Instanziierung](#23-direkte-entity-instanziierung)
3. [Process-Anweisung](#3-process-anweisung)
    1. [Wait-Anweisungen](#31-wait-anweisungen)
    2. [Sequentielle Anweisungen](#32-sequentielle-anweisungen)
4. [Operatoren](#4-operatoren)
    1. [Relationale Operatoren](#41-relationale-operatoren)
    2. [Logische Operatoren](#42-logische-operatoren)
    3. [Shift-Operatoren](#43-shift-operatoren)
5. [Logische Werte mit STD_LOGIC](#5-logische-werte-mit-std_logic)
    1. [Bus-Resolution-Funktionen](#51-bus-resolution-funktionen)
6. [Numerische Werte mit NUMERIC_STD](#6-numerische-werte-mit-numeric_std)
7. [Attribute](#7-attribute)
    1. [Signal-Attribute](#71-signal-attribute)
    2. [Array-Attribute](#72-array-attribute)
    3. [Typ-Attribute](#73-typ-attribute)
8. [Bibliotheken und Packages](#8-bibliotheken-und-packages)
    1. [Verwenden von Bibliotheken und Packages](#81-verwenden-von-bibliotheken-und-packages)
    2. [Standardbibliotheken](#82-standardbibliotheken)
    3. [Definieren von Bibliotheken und Packages](#83-definieren-von-bibliotheken-und-packages)
9. [Funktionen und Prozeduren](#9-funktionen-und-prozeduren)
    1. [Funktionen](#91-funktionen)
    2. [Prozeduren](#92-prozeduren)
    3. [Übergabemechanismen (By Value / By Reference)](#93-übergabemechanismen-by-value--by-reference)
10. [Konfiguration](#10-konfiguration)
    1. [Entity-Konfiguration (Typ 1)](#101-entity-konfiguration-typ-1)
    2. [Instanz-Konfiguration (Typ 2)](#102-instanz-konfiguration-typ-2)
11. [Datentypen (Objektklassen)](#11-datentypen-objektklassen)
    1. [Kategorien von Datentypen](#111-kategorien-von-datentypen)
    2. [Aufzählungstypen](#111-aufzählungstypen)
    3. [Numerische Typen](#111-numerische-typen)
        1. [Integer- und Real-Typen](#1111-integer--und-real-typen)
        2. [Physikalische Typen](#1112-physikalische-typen)
12. [Testbenches (Nur für die Simulation)](#12-testbenches-nur-für-die-simulation)
    1. [Testbench-Struktur](#121-testbench-struktur)
    2. [TEXTIO-Package](#122-textio-package)
13. [Assertion-Anweisungen (Nur für die Simulation)](#13-assertion-anweisungen-nur-für-die-simulation)
14. [Modellierung von Verzögerungen (Nur für die Simulation)](#14-modellierung-von-verzögerungen-nur-für-die-simulation)
    1. [Transport-Verzögerung](#141-transport-verzögerung)
    2. [Inertiale Verzögerung](#142-inertiale-verzögerung)


## 1. Entity-Deklaration

Eine Entity ist ein Modul, das die Schnittstelle einer digitalen Schaltung beschreibt.

```vhdl
ENTITY entity_name IS
    GENERIC ( generic_name : type := standardwert; );
    PORT ( a, b : <port_richtung> <port_typ>;
              c : <port_richtung> <port_typ>);
END ENTITY entity_name; 
```

* Gültige Port-Richtungen: `IN`, `OUT`, `INOUT`, `BUFFER`
* Grundlegende gültige Port-Typen: `BOOLEAN`, `BIT`, `INTEGER`, `REAL`, `CHARACTER` oder `BIT_VECTOR (1 DOWNTO 0)`
* `GENERIC` wird verwendet, um Parameter an die Entity zu übergeben. Es ist optional.
* Ports werden zur Kommunikation mit der Außenwelt verwendet.


## 2. Architecture-Deklaration

Eine Architecture ist ein Modul, das das Verhalten einer digitalen Schaltung beschreibt.

```vhdl
ARCHITECTURE architecture_name OF entity_name IS
    SIGNAL signal_name : type;
BEGIN
    -- VHDL-Code hier
END ARCHITECTURE architecture_name;
```
* Interne Signale werden zur Kommunikation zwischen verschiedenen Teilen der Architecture verwendet.

### 2.1. Signalzuweisung

```vhdl
signal_name <= wert;
```
* `<=` ist der Signalzuweisungsoperator.
* Dies ist eine nebenläufige (concurrent) Anweisung und wird ausgeführt, sobald sich eines der Signale auf der rechten Seite ändert.
* Dies gilt auch für bedingte Signalzuweisungen.

#### 2.1.1. Bedingte Signalzuweisung (WHEN ELSE)

```vhdl
signal_name <= wert1 WHEN bedingung ELSE
               wert2 WHEN bedingung ELSE
                 ...
               anderer_wert;
```
* `WHEN` wird verwendet, um einem Signal abhängig von einer Bedingung einen Wert zuzuweisen.
* `ELSE` ist optional. Ist es nicht vorhanden, behält das Signal seinen vorherigen Wert.

#### 2.1.2. Selektive Signalzuweisung (WITH SELECT)

```vhdl
WITH selektor SELECT
    signal_name <= wert1 WHEN sel_wert1,
                   wert2 WHEN sel_wert2,
                     ...
                   anderer_wert WHEN OTHERS;
```

* `SELECT` wird verwendet, um einem Signal abhängig vom Wert eines Selektors einen Wert zuzuweisen.
* `OTHERS` fängt alle übrigen Fälle ab. Es ist immer erforderlich.

### 2.2. Entity-Instanziierung als Komponente

Eine Komponente wird verwendet, um einen „Sockel" zu instanziieren. Ein Sockel ist ein Platzhalter für eine Entity, die in einer Architecture verwendet wird.

```vhdl
ARCHITECTURE architecture_name OF entity_name IS
    -- am besten direkt die Entity kopieren, ENTITY in COMPONENT umbenennen
    COMPONENT component_name
        PORT (  a, b: <port_typ>;
                c   : <port_typ>);
    END COMPONENT component_name;
BEGIN
    instance_name : component_name PORT MAP (a, b, c);
END ARCHITECTURE architecture_name;
```

* `PORT MAP` wird verwendet, um die Ports der Entity mit den Signalen in der Architecture zu verbinden.
* Die Reihenfolge der Ports in der Entity und in der Architecture muss übereinstimmen.
* Ports können mit dem Schlüsselwort `open` unverbunden gelassen werden. (Eingangs-Ports benötigen dann einen Standardwert)

* `GENERIC MAP` wird verwendet, um Parameter an die Entity zu übergeben. Es ist optional.

### 2.3. Direkte Entity-Instanziierung

```vhdl
ARCHITECTURE architecture_name OF entity_name IS
    -- ggf. interne Signale zum Verdrahten
    SIGNAL a, b, c : <port_typ>;
BEGIN
    instance_name : ENTITY library_name.sub_entity(sub_architecture)
        PORT MAP (a, b, c);
END ARCHITECTURE architecture_name;
```

* `library_name` ist der Name der Bibliothek, in der die Entity definiert ist. (wird er nicht angegeben, wird die Standardbibliothek „`work`" verwendet)

## 3. Process-Anweisung

Ein Process wird in einer Architecture verwendet, um das Verhalten einer digitalen Schaltung zu beschreiben.
Ein Process ist ein sequentieller Codeblock, der ausgeführt wird, sobald sich eines der Signale in der Sensitivitätsliste ändert.

```vhdl
ARCHITECTURE architecture_name OF entity_name IS
BEGIN
    PROCESS (sensitivitätsliste) IS
        VARIABLE variable_name : type;
    BEGIN
        -- VHDL-Code hier
    END PROCESS;
END ARCHITECTURE architecture_name;
```
* Die Sensitivitätsliste ist eine Liste von Signalen, auf die der Process reagiert. Ändert sich eines dieser Signale, wird der Process ausgeführt.
* Variablen werden verwendet, um innerhalb eines Process temporäre Werte zu speichern.
* Um einer Variablen einen Wert zuzuweisen, wird `:=` statt `<=` verwendet.
* Signale nehmen ihre neuen Werte erst an, nachdem der Process abgeschlossen ist.

### 3.1. Wait-Anweisungen

Wait-Anweisungen werden verwendet, um die Ausführung eines Process anzuhalten, bis eine bestimmte Bedingung erfüllt ist.
Sie ersetzen die Sensitivitätsliste in einem Process.

* `WAIT;` – wartet unbegrenzt.
* `WAIT ON signal_name;` – wartet, bis sich signal_name ändert.
* `WAIT FOR zeit;` – wartet eine bestimmte Zeitspanne.
* `WAIT UNTIL bedingung;` – wartet, bis eine Bedingung erfüllt ist.

`ON`, `FOR` und `UNTIL` können kombiniert werden:

```vhdl
WAIT ON signal_name  UNTIL bedingung FOR zeit; 
-- Der Process läuft, wenn die Bedingung erfüllt ist und sich signal_name ändert oder die Zeit abgelaufen ist.
```

Eine `WAIT ON`-Anweisung am Ende eines Process ist äquivalent zu einer Sensitivitätsliste.


#### 3.2. Sequentielle Anweisungen

* `IF`-Anweisung:
    ```vhdl
    IF bedingung THEN
        -- VHDL-Code hier
    ELSIF bedingung THEN
        -- VHDL-Code hier
    ELSE
        -- VHDL-Code hier
    END IF;
    ```

* `CASE`-Anweisung:
    ```vhdl
    CASE ausdruck IS
        WHEN wert1 =>
            -- VHDL-Code hier
        WHEN wert2 =>
            -- VHDL-Code hier
        ...
        WHEN OTHERS =>
            -- VHDL-Code hier
    END CASE;
    ```
    * `OTHERS` fängt alle übrigen Fälle ab. Es ist immer erforderlich. (kann durch `WHEN OTHERS => NULL;` ersetzt werden, wenn keine Anweisung durchgeführt werden soll)

* `WHILE`-Anweisung:
    ```vhdl
    WHILE bedingung LOOP
        -- VHDL-Code hier
    END LOOP;
    ```

* `FOR`-Anweisung:
    ```vhdl
    FOR i IN bereich LOOP
        -- VHDL-Code hier
    END LOOP;
    ```

    * Die Laufvariable `i` kann innerhalb der Schleife verwendet werden.
    * `bereich` kann die Länge eines Signals (`signal'RANGE`), ein Bereich (`0 TO 7`) oder eine Liste von Werten (`0, 1, 2, 3`) sein.

* `LOOP`-`EXIT`-Anweisung:
    ```vhdl
    LOOP
        -- VHDL-Code hier
        EXIT WHEN bedingung;
    END LOOP;
    ```

    * `EXIT` wird verwendet, um die Schleife zu verlassen, wenn eine Bedingung erfüllt ist.

* `NEXT`-Anweisung:
    ```vhdl
        -- ohne Label (so wie in deinem Cheat Sheet, völlig okay):
        FOR i IN bereich LOOP
            NEXT WHEN bedingung;
        END LOOP;
        -- bei verschachtelten schleifen siehe eins weiter unten
    ```

    * `NEXT` wird verwendet, um den Rest der Schleife zu überspringen und mit der nächsten Iteration fortzufahren.

* Beschriften (Labeln) einer Schleife:
    ```vhdl
    label_name: 
    LOOP
        -- VHDL-Code hier
        NEXT label_name WHEN bedingung;
    END LOOP label_name;
    ```

    * Ein Label kann verwendet werden, um eine Schleife aus einer verschachtelten Schleife heraus zu verlassen.
    * `NEXT`, `EXIT` und `RETURN` können mit einem Label sowie mit einer bedingten Anweisung verwendet werden.

## 4. Operatoren

### 4.1. Relationale Operatoren

`=`, `/=` (ungleich), `<`, `<=`, `>`, `>=`

### 4.2. Logische Operatoren

`AND`, `OR`, `NAND`, `NOR`, `XOR`, `XNOR`, `NOT` (definiert für die Typen BOOLEAN und BIT)

* `NOT` hat die höchste Priorität; alle anderen Operatoren werden von links nach rechts ausgewertet.
* daher am besten Klammern verwenden

### 4.3. Shift-Operatoren

`SLL`, `SRL`, `SLA`, `SRA`, `ROL`, `ROR`

* Logisches Schieben (shift logical) -> leeren und mit 0 auffüllen
* Arithmetisches Schieben (shift arithmetic) -> leeren und mit dem Vorzeichenbit auffüllen
* Logisches Rotieren (rotate logical) -> mit den Bits von der anderen Seite auffüllen


## 5. Logische Werte mit STD_LOGIC

Das Package `IEEE.STD_LOGIC_1164` definiert die Datentypen `STD_ULOGIC` und `STD_ULOGIC_VECTOR`.
Sie werden verwendet, um digitale Signale mit zusätzlichen Zuständen darzustellen.

* `U` – uninitialisiert (uninitialized)
* `X` – unbekannt (unknown)
* `0` – logisch 0
* `1` – logisch 1
* `Z` – hochohmig (high impedance)
* `W` – schwach unbekannt (weak unknown)
* `L` – schwach logisch 0
* `H` – schwach logisch 1
* `-` – don't care

Der Compiler erzeugt Fehler, wenn ein Signal von mehreren Quellen mit widersprüchlichen Werten getrieben wird.

### 5.1. Bus-Resolution-Funktionen

Die Bus-Resolution-Funktionen werden verwendet, um mehrere Quellen aufzulösen, die ein Signal treiben.

Die Verwendung der Datentypen `STD_LOGIC` und `STD_LOGIC_VECTOR` führt die Bus-Resolution-Funktionen ein.

```text
Das Ergebnis ist der Wert, der im Schema am weitesten oben steht:
        U
        │  
        X
    ┌───┼───┐
    0   -   1
    └───┬───┘
        W
    ┌───┴───┐
    L       H
    └───┬───┘
        Z
```


## 6. Numerische Werte mit NUMERIC_STD

`IEEE.NUMERIC_STD` und `IEEE.NUMERIC_BIT` ermöglichen numerische Operationen auf Vektor-Typen.
Dafür sind die Datentypen `SIGNED` und `UNSIGNED` definiert.
`NUMERIC_STD` verwendet den Datentyp `STD_LOGIC_VECTOR`.
`NUMERIC_BIT` verwendet den Datentyp `BIT_VECTOR`.

Zum Konvertieren von und nach `STD_LOGIC_VECTOR` werden folgende Funktionen verwendet:
* `unsigned()` 
* `signed()`
* `std_logic_vector()`


## 7. Attribute

Attribute liefern Informationen über Signale, Arrays, Typen oder Werte. Welche Objekte zulässig sind, hängt von der Kategorie ab.

### 7.1. Signal-Attribute

**Nur für Signale** – sie beziehen sich auf die Simulations-Historie (Events/Transactions). Auf Variablen oder Konstanten angewendet gibt es einen Compile-Fehler.

* `signal'EVENT` – liefert TRUE, wenn sich das Signal im aktuellen Simulationszyklus geändert hat.
* `signal'ACTIVE` – liefert TRUE, wenn das Signal aktuell getrieben wird.
* `signal'STABLE(t)` – liefert TRUE, wenn für t Zeiteinheiten kein Event aufgetreten ist. (Event = Änderung des Signalwerts)
* `signal'QUIET(t)` – liefert TRUE, wenn für t Zeiteinheiten keine Transaction aufgetreten ist. (Transaction = Aktualisierung des Signalwerts)
* `signal'DELAYED(t)` – liefert den Wert des Signals, verzögert um t Zeiteinheiten.
* `signal'TRANSACTION` – BIT, das bei jeder Transaction des Signals kippt.
* `signal'LAST_EVENT` – liefert die Zeit seit dem letzten Event des Signals.
* `signal'LAST_ACTIVE` – liefert die Zeit seit der letzten Transaction des Signals.
* `signal'LAST_VALUE` – liefert den Wert des Signals vor dem letzten Event.

`RISING_EDGE(signal)` und `FALLING_EDGE(signal)` sind vom Package `IEEE.STD_LOGIC_1164` vordefinierte Attribute.
Sie liefern TRUE, wenn das Signal im aktuellen Simulationszyklus eine steigende bzw. fallende Flanke hat.

```VHDL
-- D-FlipFlop-Beispiel (LIBRARY ieee; USE ieee.std_logic_1164.ALL; nicht vergessen!)
PROCESS(reset, clk) IS
BEGIN
    IF reset = '0' THEN
        q <= '0';
    ELSIF clk = '1' AND clk'EVENT THEN   -- steigende Taktflanke
        q <= d;
    END IF;
END PROCESS;
```

### 7.2. Array-Attribute

Gelten für **alles, was ein (begrenztes) Array ist** – also auch für Variablen, Konstanten und Array-Typen, nicht nur für Signale. (`array` steht für ein beliebiges Array-Objekt oder einen Array-Typ.)

* `array'LENGTH` – liefert die Anzahl der Elemente.
* `array'RANGE` – liefert den Indexbereich. (z. B. `0 TO 7`) (für Schleifen verwendet)
* `array'REVERSE_RANGE` – liefert den umgekehrten Indexbereich. (z. B. `7 DOWNTO 0`) (für Schleifen verwendet)
* `array'LEFT` / `array'RIGHT` – linke / rechte Indexgrenze.
* `array'HIGH` / `array'LOW` – höchster / niedrigster Index.
* `array'ASCENDING` – TRUE, wenn der Indexbereich aufsteigend ist (`TO`).

Typische Array-Typen sind **Vektoren** wie `STD_LOGIC_VECTOR`, `BIT_VECTOR` oder `UNSIGNED`/`SIGNED` – die Attribute funktionieren also direkt darauf (egal ob Signal, Variable oder Konstante):

```vhdl
SIGNAL data : STD_LOGIC_VECTOR(7 DOWNTO 0);
-- data'LENGTH        =  8
-- data'LEFT          =  7          -- erster Index
-- data'RIGHT         =  0          -- letzter Index
-- data'HIGH          =  7          -- größter Index
-- data'LOW           =  0          -- kleinster Index
-- data'RANGE         =  7 DOWNTO 0
-- data'REVERSE_RANGE =  0 TO 7
-- data'ASCENDING     =  FALSE      -- weil DOWNTO (bei TO wäre es TRUE)

-- Damit kann man index-unabhängig über den ganzen Vektor laufen:
FOR i IN data'RANGE LOOP
    data(i) <= '0';
END LOOP;
```

### 7.3. Typ-Attribute

Anwendbar auf einen **Typ-/Subtyp-Namen**, unabhängig von Signal/Variable/Konstante.

* `type'VAL(n)` – liefert den Wert an Position n (z. B. bei Aufzählungstypen).
* `type'POS(w)` – liefert die Position des Werts w.
* `type'SUCC(w)` / `type'PRED(w)` – Nachfolger / Vorgänger von w.
* `type'IMAGE(w)` – liefert den Wert w als String.
* `type'VALUE(s)` – wandelt den String s zurück in einen Typwert.

## 8. Bibliotheken und Packages
### 8.1. Verwenden von Bibliotheken und Packages
```vhdl
LIBRARY library_name;
USE library_name.package_name.ALL;
```

Der Bibliotheksname wird zur Build-Zeit oder vom Hersteller festgelegt.
Der Standard-Bibliotheksname für eigene Packages ist `work`.

### 8.2. Standardbibliotheken
* `IEEE` – Standardbibliothek für VHDL.
    * `IEEE.STD_LOGIC_1164` – Standard-Package für den Datentyp STD_LOGIC.
    * `IEEE.NUMERIC_STD` – Standard-Package für numerische Datentypen.
    * `IEEE.NUMERIC_BIT` – Standard-Package für bitweise Operationen.

### 8.3. Definieren von Bibliotheken und Packages
```vhdl
PACKAGE package_name IS
    CONSTANT constant_name : type := wert;
    TYPE type_name IS (wert1, wert2, ...);
    SUBTYPE subtype_name IS type_name RANGE wert1 TO wert2;

    FUNCTION function_name (argument1 : type; argument2 : type) RETURN type;
    PROCEDURE procedure_name (argument1 : type; argument2 : type);
END PACKAGE package_name;

PACKAGE BODY package_name IS
    FUNCTION function_name (argument1 : type; argument2 : type) RETURN type IS
    BEGIN
        -- VHDL-Code hier
        RETURN wert;
    END FUNCTION function_name;

    PROCEDURE procedure_name (argument1 : type; argument2 : type) IS
    BEGIN
        -- VHDL-Code hier
    END PROCEDURE procedure_name;
END PACKAGE BODY package_name;
```


## 9. Funktionen und Prozeduren

Funktionen und Prozeduren werden verwendet, um wiederverwendbaren Code zu kapseln.

* In ihnen dürfen nur Variablen verwendet werden.
* Sie können sowohl im Architecture-Body als auch im Process-Body verwendet werden.
* Funktionen und Prozeduren können im Kopf einer Architecture oder in einem Package definiert werden.
* Funktionen und Prozeduren können überladen werden.
* Argumente können per Wert (by value) oder per Referenz (by reference) übergeben werden.

```vhdl	
procedure_name (argument1, argument2, ...);
procedure_name (argument1 => wert1, argument2 => wert2, ...);
```

### 9.1. Funktionen

* Eine Funktion kann die übergebenen Parameter nicht verändern und gibt nur einen Wert zurück.
* Variablen werden bei jedem Aufruf der Funktion neu initialisiert.

```vhdl
FUNCTION function_name (argument1 : type; argument2 : type) RETURN type IS
    VARIABLE variable_name : type;
BEGIN
    -- Sequentieller VHDL-Code hier
    RETURN wert;
END FUNCTION function_name;
```

**Beispiel:** wandelt einen Wert generisch in einen `STD_LOGIC_VECTOR` um. Die Breite wird als `NATURAL` (`width`) übergeben und bestimmt die Größe des Rückgabewerts.

```vhdl
FUNCTION to_slv (wert : NATURAL; width : NATURAL) RETURN STD_LOGIC_VECTOR IS
    VARIABLE ergebnis : STD_LOGIC_VECTOR(width-1 DOWNTO 0);
BEGIN
    ergebnis := STD_LOGIC_VECTOR(TO_UNSIGNED(wert, width));
    RETURN ergebnis;   -- z. B. to_slv(5, 8) -> "00000101"
END FUNCTION to_slv;
```

### 9.2. Prozeduren

* Eine Prozedur kann die übergebenen Parameter verändern und gibt keinen Wert zurück.
* Eine Prozedur kann eine `WAIT`-Anweisung enthalten.
* Eine Prozedur kann ein Signal im Architecture-Body verändern.
* Variablen werden bei jedem Aufruf der Prozedur neu initialisiert.

```vhdl
PROCEDURE procedure_name (SIGNAL argument1 : port_richtung port_typ;
argument2 : type := standardwert) IS
    VARIABLE variable_name : type;
BEGIN
    -- Sequentieller VHDL-Code hier
END PROCEDURE procedure_name;
```
### 9.3. Übergabemechanismen (By Value / By Reference)

Der Mechanismus wird implizit über die Objektklasse und die Richtung des Arguments bestimmt.

```vhdl
-- 1. By Value (Kopie des Wertes, Standard bei IN und Funktionen)
-- Parameter können innerhalb der Funktion NIEMALS abgeändert werden!
FUNCTION add (a : INTEGER; b : INTEGER) RETURN INTEGER;

-- 2. By Value in einer Prozedur (Reines Einlesen von Werten via IN)
PROCEDURE pruefe_werte (a : IN INTEGER; b : IN INTEGER);

-- 3. By Reference (Direkter Zugriff auf Original-Variable via :=)
PROCEDURE inkrement (VARIABLE counter : INOUT INTEGER);

-- 4. By Reference (Direkter Zugriff auf Original-Signal via <=)
PROCEDURE set_pin (SIGNAL hw_pin : OUT STD_LOGIC);
```

## 10. Konfiguration

Eine Konfiguration wird verwendet, um die Architecture einer Entity oder die Architecture einer Instanz festzulegen.

### 10.1. Entity-Konfiguration (Typ 1)

Legt die Architecture einer Entity fest.

```vhdl
CONFIGURATION configuration_name OF entity_using_config IS
    FOR architecture_to_use
    END FOR;
END CONFIGURATION configuration_name;
```

entity_using_config verwendet architecture_to_use.

### 10.2. Instanz-Konfiguration (Typ 2)

Legt für Instanzen innerhalb einer Architecture fest, welche Entity und welche Architecture verwendet werden.

```vhdl
CONFIGURATION configuration_name OF entity_using_config IS
    FOR architecture_using_config
        FOR instance_name : component_name USE ENTITY library_name.entity_name(architecture_name);
    END FOR;
END CONFIGURATION configuration_name;
```

instance_name in architecture_using_config verwendet entity_name mit architecture_name.

Dies kann auch im Kopf der Architecture erfolgen:

```vhdl
ARCHITECTURE architecture_name OF entity_name IS
    FOR instance_name : component_name USE ENTITY library_name.entity_name(architecture_name);
BEGIN
    -- VHDL-Code hier
END ARCHITECTURE architecture_name;
```


## 11. Datentypen (Objektklassen)

* `CONSTANT` : fester Wert (in Architecture, Process, Function, Procedure oder Package)
* `SIGNAL`   : Kommunikation zwischen verschiedenen Teilen der Architecture verwendet (Deklaration in der Architecture)
* `VARIABLE` : lokales Speichern von Werten (nur im Process)
* `FILE`/ `Dateien`: lesen / schreiben von Daten

### 11.1. Kategorien von Datentypen

* Skalare Typen (Scalar Types)
    * Aufzählungstypen (Enumeration Types)
    * Numerische Typen
        * Integer-Typen
        * Real-Typen
        * Physikalische Typen
* Zusammengesetzte Typen (Composite Types)
    * Array-Typen
    * Record-Typen
* File-Typen

### 11.2. Aufzählungstypen

```vhdl
TYPE type_name IS (wert1, wert2, ...); -- "0", "1" geht aber auch "S1", "S2"
SUBTYPE subtype_name IS type_name RANGE wert1 TO wert2;
```
### 11.3. Numerische Typen

#### 11.3.1. Integer- und Real-Typen
`INTEGER` (Bereich: -2^31 bis 2^31-1)
    
```vhdl
SUBTYPE NATURAL IS INTEGER RANGE 0 TO INTEGER'HIGH;
SUBTYPE POSITIVE IS INTEGER RANGE 1 TO INTEGER'HIGH;
```

`REAL` (Bereich: -2^63 bis 2^63-1)

```vhdl
SUBTYPE REAL IS INTEGER RANGE -2**63 TO 2**63-1;
```

#### 11.3.2. Physikalische Typen

```vhdl
TYPE time IS RANGE von TO bis
UNITS 
    fs;
    ps = 1000 fs;
    ns = 1000 ps;
    ...
END UNITS;
```

### 11.4. Arrays

```vhdl
-- Array-Typen definieren 
TYPE fixed_range IS ARRAY (bereich) OF element_typ;            -- feste Größe, z. B. (0 TO 7)
TYPE x_by_y      IS ARRAY (bereich1, bereich2) OF element_typ; -- mehrdimensional
TYPE variable    IS ARRAY (positive RANGE <>) OF element_typ;  -- offene Größe (<>), erst bei Deklaration festgelegt

-- Array-Objekte deklarieren 
VARIABLE array_name : variable(1 TO 10);     -- Index 1..10 (aufsteigend)
VARIABLE array_name : variable(10 DOWNTO 1); -- Index 10..1 (absteigend)

-- Auf Elemente und Teilbereiche zugreifen 
signal_name(i)          -- einzelnes Element an Index i
signal_name(3)          -- Bit/Element an Position 3
signal_name(7 DOWNTO 4) -- Teilbereich (Slice), hier 4 Bits
signal_name(0 TO 3)     -- Slice in aufsteigender Richtung

TYPE led_matrix IS ARRAY (0 TO 3, 0 TO 3) OF BIT;   -- definiert einen neuen Typ namens led_matrix
SIGNAL display : led_matrix;                         -- erzeugt ein konkretes Objekt dieses Typs
```

### 11.4. Records

```vhdl
TYPE record_name IS RECORD
    feld1 : typ1;
    feld2 : typ2;
    ...
END RECORD;

VARIABLE record_name : record_name;

record_name.feld1 := wert;
```


## 12. Testbenches (Nur für die Simulation)

Eine Testbench wird verwendet, um die Funktionalität einer digitalen Schaltung zu testen.

### 12.1. Testbench-Struktur

```vhdl
ENTITY testbench_name IS
END ENTITY testbench_name;

ARCHITECTURE testbench_name OF testbench_name IS
    -- Komponenten-Deklaration
    COMPONENT dut IS
        PORT ( ... );
    END COMPONENT dut;

    -- Signal-Deklaration

BEGIN
    -- Komponenten-Instanziierung
    dut_inst : dut PORT MAP ( ... );

    -- Stimulus-Process
    PROCESS
    BEGIN
        -- VHDL-Code hier
    END PROCESS;

    -- Prüf-Process (Check Process)
    PROCESS
    BEGIN
        -- VHDL-Code hier
    END PROCESS;

END ARCHITECTURE testbench_name;
```


#### 12.2. TEXTIO-Package

Das Package `std.textio` wird verwendet, um Textdateien zu lesen und zu schreiben.

```vhdl
USE STD.TEXTIO.ALL;
```
Eine Datei öffnen:

```vhdl 
FILE file_name : TEXT OPEN READ_MODE IS "file_name.txt";
```

Beispiel: Aus einer Datei lesen:

```vhdl
VARIABLE line : LINE;
VARIABLE sample_char : CHARACTER;
VARIABLE sample_bit : BIT;
VARIABLE sample_time : TIME;

WHILE NOT ENDFILE(file_name) LOOP
    READLINE(file_name, line);
        READ(line, sample_char);
        READ(line, sample_bit);
        READ(line, sample_time);
END LOOP;
```

Beispiel: In eine Datei schreiben:

```vhdl
VARIABLE line : LINE;
VARIABLE sample_char : CHARACTER := 'A';
VARIABLE sample_bit : BIT := '1';
VARIABLE sample_time : TIME := 10 ns;

WRITELINE(file_name, line);
    WRITE(line, sample_char);
    WRITE(line, sample_bit);
    WRITE(line, sample_time);
```

Eine Datei schließen:

```vhdl
CLOSE file_name;
```


## 13. Assertion-Anweisungen (Nur für die Simulation)

Werden verwendet, um die Korrektheit des Designs während der Simulation zu prüfen.
Assertions werden im Architecture-Body platziert.

```vhdl
ASSERT bedingung REPORT "Fehlermeldung" SEVERITY schweregrad;
```

* `bedingung` ist die zu prüfende Bedingung.
* `REPORT` ist die Meldung, die angezeigt wird, wenn die Bedingung falsch ist.
* `SEVERITY` ist der Schweregrad der Fehlermeldung.
    * `NOTE` – Informationsmeldung
    * `WARNING` – Warnmeldung
    * `ERROR` – Fehlermeldung
    * `FAILURE` – fatale Fehlermeldung


## 14. Modellierung von Verzögerungen (Nur für die Simulation)
### 14.1. Transport-Verzögerung

Die Transport-Verzögerung wird verwendet, um ein Signal um eine bestimmte Zeitspanne zu verzögern.

```vhdl
x <= TRANSPORT logischer_ausdruck AFTER 10 ns;
-- verzögert das Signal um 10 ns 
```

### 14.2. Inertiale Verzögerung

Die inertiale Verzögerung wird verwendet, um ein Signal um eine bestimmte Zeitspanne zu verzögern. Zusätzlich muss das Signal für die Dauer der Verzögerung stabil bleiben.

```vhdl
x <= logischer_ausdruck AFTER 10 ns;
-- verzögert das Signal um 10 ns, wenn der logische_ausdruck für die gesamte Dauer wahr ist.

-- Variante der inertialen Verzögerung:

x <= REJECT reject_zeit INERTIAL logischer_ausdruck AFTER 10 ns;
-- verzögert das Signal um 10 ns und verwirft / ignoriert Änderungen, die kürzer als reject_zeit sind.
```