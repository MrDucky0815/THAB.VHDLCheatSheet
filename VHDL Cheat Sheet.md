# VHDL Cheat Sheet

## 0. Inhaltsverzeichnis

1. [Datentypen](#1-datentypen)
    1. [Kategorien von Datentypen](#11-kategorien-von-datentypen)
    2. [Aufzählungstypen](#12-aufzählungstypen)
    3. [Numerische Typen](#13-numerische-typen)
        1. [Integer- und Real-Typen](#131-integer--und-real-typen)
        2. [Physikalische Typen](#132-physikalische-typen)
    4. [Arrays](#14-arrays)
    5. [Records](#15-records)
2. [STD_LOGIC – Logikwerte](#2-std_logic--logikwerte)
    1. [Bus-Resolution-Funktionen](#21-bus-resolution-funktionen)
3. [NUMERIC_STD – Arithmetik](#3-numeric_std--arithmetik)
4. [Typkonvertierung](#4-typkonvertierung)
    1. [Casts](#41-casts)
    2. [Konvertierungsfunktionen](#42-konvertierungsfunktionen)
5. [Operatoren](#5-operatoren)
    1. [Relationale Operatoren](#51-relationale-operatoren)
    2. [Logische Operatoren](#52-logische-operatoren)
    3. [Shift-Operatoren](#53-shift-operatoren)
    4. [Arithmetik-Operatoren](#54-arithmetik-operatoren)
6. [Attribute](#6-attribute)
    1. [Signal-Attribute](#61-signal-attribute)
    2. [Array-Attribute](#62-array-attribute)
    3. [Typ-Attribute](#63-typ-attribute)
7. [Entity – Schnittstelle](#7-entity--schnittstelle)
8. [Architecture – Implementierung](#8-architecture--implementierung)
    1. [Signalzuweisung](#81-signalzuweisung)
        1. [Bedingt (WHEN ELSE)](#811-bedingt-when-else)
        2. [Selektiv (WITH SELECT)](#812-selektiv-with-select)
    2. [Komponenten-Instanziierung](#82-komponenten-instanziierung)
    3. [Direkte Instanziierung](#83-direkte-instanziierung)
    4. [GENERATE (Strukturen replizieren)](#84-generate-strukturen-replizieren)
9. [Process – sequentieller Ablauf](#9-process--sequentieller-ablauf)
    1. [Wait-Anweisungen](#91-wait-anweisungen)
    2. [Sequentielle Anweisungen](#92-sequentielle-anweisungen)
10. [Unterprogramme (Funktionen & Prozeduren)](#10-unterprogramme-funktionen--prozeduren)
    1. [Funktionen](#101-funktionen)
    2. [Prozeduren](#102-prozeduren)
    3. [Übergabemechanismen (By Value / By Reference)](#103-übergabemechanismen-by-value--by-reference)
11. [Libraries & Packages](#11-libraries--packages)
    1. [Verwenden von Bibliotheken und Packages](#111-verwenden-von-bibliotheken-und-packages)
    2. [Standardbibliotheken](#112-standardbibliotheken)
    3. [Definieren von Bibliotheken und Packages](#113-definieren-von-bibliotheken-und-packages)
12. [Konfiguration](#12-konfiguration)
    1. [Entity-Konfiguration (Typ 1)](#121-entity-konfiguration-typ-1)
    2. [Instanz-Konfiguration (Typ 2)](#122-instanz-konfiguration-typ-2)
    3. [Beispiel: Entity-/Architecture-Wahl](#123-beispiel-entity-architecture-wahl)
    4. [Beispiel: Porttyp-Konvertierung](#124-beispiel-porttyp-konvertierung)
    5. [Verteilung auf Dateien](#125-verteilung-auf-dateien)
13. [Sichtbarkeit (Geltungsbereiche)](#13-sichtbarkeit-geltungsbereiche)
14. [Synthese und Optimierung](#14-synthese-und-optimierung)
    1. [Synthetisierbare Datentypen](#141-synthetisierbare-datentypen)
    2. [Synthetisierbare Operatoren](#142-synthetisierbare-operatoren)
    3. [Nicht erlaubte Anweisungen](#143-nicht-erlaubte-anweisungen)
    4. [Kombinatorische Logik](#144-kombinatorische-logik)
    5. [Getaktete Logik (Speicher, Flip-Flop)](#145-getaktete-logik-speicher-flip-flop)
    6. [Initialisierung und Reset](#146-initialisierung-und-reset)
    7. [Optimierung](#147-optimierung)
15. [Testbenches (nur Simulation)](#15-testbenches-nur-simulation)
    1. [Testbench-Struktur](#151-testbench-struktur)
    2. [TEXTIO-Package](#152-textio-package)
16. [Assertions](#16-assertions)
    1. [VHDL-Assertions](#161-vhdl-assertions)
    2. [PSL-Assertions](#162-psl-assertions)
17. [Verzögerungsmodelle (nur Simulation)](#17-verzögerungsmodelle-nur-simulation)
    1. [Transport-Verzögerung](#171-transport-verzögerung)
    2. [Inertiale Verzögerung](#172-inertiale-verzögerung)

**[Zusätzliche Informationen](#zusätzliche-informationen)**

- [Nebenläufige Anweisungen (concurrent statements)](#nebenläufige-anweisungen-concurrent-statements)
- [Sequenzielle Anweisungen (sequential statements)](#sequenzielle-anweisungen-sequential-statements)
- [Operationen und Operatoren (Übersicht)](#operationen-und-operatoren-übersicht)
- [Ereignisgesteuerte Simulation (virtuelle Verzögerung Δt)](#ereignisgesteuerte-simulation-virtuelle-verzögerung-δt)
- [Regeln für Prozesse](#regeln-für-prozesse)
- [Regeln für Variablen](#regeln-für-variablen)
- [Unterteilung digitaler Systeme](#unterteilung-digitaler-systeme)
- [Betriebsbedingungen (Flip-Flop)](#betriebsbedingungen-flip-flop)
- [Zustandsautomaten (FSM)](#zustandsautomaten-fsm)
- [Digital Clock Manager (DCM)](#digital-clock-manager-dcm)
    - [Clock Skew (Problem)](#clock-skew-problem)
    - [Clock-Skew-Elimination](#clock-skew-elimination)
    - [Mehrere Taktdomänen (Problem)](#mehrere-taktdomänen-problem)
    - [Lösung 1 Steuerung der Phase (DLL)](#lösung-1-steuerung-der-phase-dll)
    - [Lösung 2 Zwei Flip-Flops](#lösung-2-zwei-flip-flops)
    - [Lösung 3 FIFO](#lösung-3-fifo)
- [Verifikation](#verifikation)
    - [Verifikationstechniken](#verifikationstechniken)
    - [Simulator-Typen](#simulator-typen)
    - [Statische Laufzeitanalyse (STA)](#statische-laufzeitanalyse-sta)


## 1. Datentypen

* `CONSTANT` : fester Wert (in Architecture, Process, Function, Procedure oder Package)
* `SIGNAL`   : Kommunikation zwischen verschiedenen Teilen der Architecture verwendet (Deklaration in der Architecture)
* `VARIABLE` : lokales Speichern von Werten (nur im Process)
* `FILE`/ `Dateien`: lesen / schreiben von Daten

### 1.1. Kategorien von Datentypen

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

### 1.2. Aufzählungstypen

```vhdl
TYPE type_name IS (wert1, wert2, ...); -- "0", "1" geht aber auch "S1", "S2" oder "rot", "grün"
SUBTYPE subtype_name IS type_name RANGE wert1 TO wert2;
```

`BIT` ist ein vordefinierter Aufzählungstyp, die einfache zweiwertige Logik im Gegensatz zum neunwertigen `STD_LOGIC` aus Kapitel 2, dazu passt der Array-Typ `BIT_VECTOR`.

```vhdl
-- so ist BIT vordefiniert, ein Aufzählungstyp mit zwei Werten
TYPE BIT IS ('0', '1');
```

Weitere vordefinierte Aufzählungstypen sind `BOOLEAN` (`false`, `true`) und `CHARACTER`.

### 1.3. Numerische Typen

#### 1.3.1. Integer- und Real-Typen
`INTEGER` – Ganzzahl (Bereich: -2^31 bis 2^31-1)
    
```vhdl
-- Vordefinierte Subtypen
SUBTYPE NATURAL  IS INTEGER RANGE 0 TO INTEGER'HIGH;
SUBTYPE POSITIVE IS INTEGER RANGE 1 TO INTEGER'HIGH;

-- Eigener Subtyp von INTEGER (bleibt kompatibel):
SUBTYPE byte IS INTEGER RANGE -128 TO 127;    -- oder RANGE 0 TO 255

-- Eigener, NEUER Integer-Typ (eigenständig, nur per Konvertierung mischbar):
TYPE coord IS RANGE -100 TO 100;

```

`REAL` – Gleitkomma (Bereich: -2^63 bis 2^63-1)

```vhdl
SUBTYPE REAL IS INTEGER RANGE -2**63 TO 2**63-1;
```

#### 1.3.2. Physikalische Typen

```vhdl
-- TIME ist vordefiniert (in STD.STANDARD):
TYPE time IS RANGE von TO bis
UNITS
    fs;              -- Basiseinheit
    ps = 1000 fs;
    ns = 1000 ps;
    ...
END UNITS;

-- Eigener physikalischer Typ (Kapazität):
TYPE capacitance IS RANGE 0 TO 1E9
UNITS
    fF;              -- Basiseinheit (Femtofarad)
    pF = 1000 fF;    -- Pikofarad
    nF = 1000 pF;    -- Nanofarad
    uF = 1000 nF;    -- Mikrofarad (µF)
END UNITS capacitance;
```

### 1.4. Arrays

```vhdl
-- Array-Typen definieren 
TYPE fixed_range IS ARRAY (bereich) OF element_typ;            -- BESCHRÄNKT: feste Größe, z. B. (0 TO 7)
TYPE x_by_y      IS ARRAY (bereich1, bereich2) OF element_typ; -- mehrdimensional
TYPE variable    IS ARRAY (positive RANGE <>) OF element_typ;  -- UNBESCHRÄNKT: offene Größe (<>)

-- Array-Objekte deklarieren: bei UNBESCHRÄNKTEM Typ wird die Größe HIER festgelegt
VARIABLE array_name : variable(1 TO 10);     -- Index 1..10 (aufsteigend)
VARIABLE array_name : variable(10 DOWNTO 1); -- Index 10..1 (absteigend)
VARIABLE array_name : variable;              -- ❌ Fehler: unbeschränkt braucht einen Bereich!

-- Auf Elemente und Teilbereiche zugreifen 
signal_name(i)          -- einzelnes Element an Index i
signal_name(3)          -- Bit/Element an Position 3
signal_name(7 DOWNTO 4) -- Teilbereich (Slice), hier 4 Bits
signal_name(0 TO 3)     -- Slice in aufsteigender Richtung

-- Ganzen Vektor setzen (Aggregat) – OTHERS = alle übrigen Indizes
sig <= (OTHERS => '0');             -- alle Bits 0 (typischer Reset)
sig <= (OTHERS => '1');             -- alle Bits 1
sig <= (7 => '1', OTHERS => '0');   -- Bit 7 = '1', Rest = '0'
sig <= ('1', '0', OTHERS => '0');   -- Position 0 = '1', 1 = '0', Rest = '0'

TYPE led_matrix IS ARRAY (0 TO 3, 0 TO 3) OF BIT;   -- definiert einen neuen Typ namens led_matrix
SIGNAL display : led_matrix;                         -- erzeugt ein konkretes Objekt dieses Typs
```

### 1.5. Records
Vergleichbar mit einem struct in C: nur Datenfelder, keine Methoden/Vererbung

```vhdl
TYPE record_name IS RECORD
    feld1 : typ1;
    feld2 : typ2;
    ...
END RECORD;

VARIABLE record_name : record_name;

record_name.feld1 := wert;     -- Feldzugriff per Punkt
```


## 2. STD_LOGIC – Logikwerte

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

### 2.1. Bus-Resolution-Funktionen

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


## 3. NUMERIC_STD – Arithmetik

`IEEE.NUMERIC_STD` und `IEEE.NUMERIC_BIT` ermöglichen numerische Operationen auf Vektor-Typen.
Dafür sind die Datentypen `SIGNED` und `UNSIGNED` definiert.
`NUMERIC_STD` baut auf `STD_LOGIC_VECTOR` auf, `NUMERIC_BIT` auf `BIT_VECTOR`.

Die alten Synopsys-Packages `std_logic_arith`, `std_logic_signed` und `std_logic_unsigned` sollten nicht mehr verwendet werden, sie sind kein IEEE-Standard.

Ein `STD_LOGIC_VECTOR` selbst kann **nicht** rechnen (kein `+`, `-`, …) – er ist nur eine Bitfolge. Für Arithmetik geht man über `UNSIGNED` / `SIGNED`.

**Cast zwischen Vektor-Typen** (gleiche Bitbreite, nur Umdeutung der Bits):

| von → nach | Funktion |
|---|---|
| `STD_LOGIC_VECTOR` → `UNSIGNED` | `unsigned(slv)` |
| `STD_LOGIC_VECTOR` → `SIGNED` | `signed(slv)` |
| `UNSIGNED` / `SIGNED` → `STD_LOGIC_VECTOR` | `std_logic_vector(x)` |

**Umrechnung von/nach `INTEGER`** (Bitbreite n beim Erzeugen angeben):

| von → nach | Funktion |
|---|---|
| `UNSIGNED` / `SIGNED` → `INTEGER` | `to_integer(x)` |
| `INTEGER` → `UNSIGNED` (n Bit) | `to_unsigned(wert, n)` |
| `INTEGER` → `SIGNED` (n Bit) | `to_signed(wert, n)` |

`STD_LOGIC_VECTOR` hat **keine** direkte Integer-Umwandlung – immer erst nach `UNSIGNED`/`SIGNED` casten.

**Beispiel:**
```vhdl
LIBRARY ieee;
USE ieee.std_logic_1164.ALL;
USE ieee.numeric_std.ALL;
-- ... in der Architecture deklariert:
SIGNAL slv : STD_LOGIC_VECTOR(7 DOWNTO 0);
SIGNAL u   : UNSIGNED(7 DOWNTO 0);
SIGNAL s   : SIGNED(7 DOWNTO 0);
SIGNAL i   : INTEGER;

-- INTEGER -> Vektor (Breite 8 angeben!)
u   <= to_unsigned(5, 8);                    -- 5 als 8-Bit UNSIGNED
s   <= to_signed(-3, 8);                     -- -3 als 8-Bit SIGNED (Zweierkomplement)
slv <= std_logic_vector(to_unsigned(5, 8));  -- dasselbe als STD_LOGIC_VECTOR

-- Vektor -> INTEGER
i <= to_integer(unsigned(slv));              -- erst casten, dann to_integer

-- Rechnen
u   <= u + 1;                                -- direkt auf UNSIGNED
slv <= std_logic_vector(unsigned(slv) + 1);  -- auf STD_LOGIC_VECTOR via Cast
```


## 4. Typkonvertierung

Zwei Mechanismen: **Casts** wandeln zwischen *verwandten* Typen (gleicher Basistyp – nur umgedeutet bzw. gerundet), **Konvertierungsfunktionen** zwischen *verschiedenen Darstellungen*.

**Faustregel:** gleicher Basistyp → Cast `typ(wert)`; andere Darstellung → Funktion `to_xxx(...)`.

### 4.1. Casts

| von → nach | Cast |
|---|---|
| `REAL` → `INTEGER` | `INTEGER(r)` – rundet zur nächsten ganzen Zahl |
| `INTEGER` → `REAL` | `REAL(i)` |
| `STD_LOGIC_VECTOR` → `UNSIGNED` / `SIGNED` | `unsigned(slv)` / `signed(slv)` |
| `UNSIGNED` / `SIGNED` → `STD_LOGIC_VECTOR` | `std_logic_vector(x)` |

### 4.2. Konvertierungsfunktionen

Aus `ieee.numeric_std` (Zahlentyp ↔ `INTEGER`):

| von → nach | Funktion |
|---|---|
| `UNSIGNED` / `SIGNED` → `INTEGER` | `to_integer(x)` |
| `INTEGER` → `UNSIGNED` (n Bit) | `to_unsigned(i, n)` |
| `INTEGER` → `SIGNED` (n Bit) | `to_signed(i, n)` |

Aus `ieee.std_logic_1164` (`BIT` ↔ `STD_LOGIC`):

| von → nach | Funktion |
|---|---|
| `BIT` → `STD_LOGIC` | `to_stdulogic(b)` |
| `STD_LOGIC` → `BIT` | `to_bit(s)` |
| `BIT_VECTOR` → `STD_LOGIC_VECTOR` | `to_stdlogicvector(bv)` |
| `STD_LOGIC_VECTOR` → `BIT_VECTOR` | `to_bitvector(slv)` |

**Metawerte bei `to_integer`:** `'0'`/`'L'` → `0`, `'1'`/`'H'` → `1`. Alles andere (`'U'`, `'X'`, `'W'`, `'Z'`, `'-'`) ist keine gültige Zahl → Ergebnis **`0`** + Simulations-**Warnung** (`metavalue detected, returning 0`). Mit `to_01(v, '0')` kann man Metawerte vorab durch einen definierten Wert ersetzen.


## 5. Operatoren

### 5.1. Relationale Operatoren

`=`, `/=` (ungleich), `<`, `<=`, `>`, `>=`

### 5.2. Logische Operatoren

`AND`, `OR`, `NAND`, `NOR`, `XOR`, `XNOR`, `NOT` (vordefiniert für `BOOLEAN` und `BIT`, über `std_logic_1164` zusätzlich für `STD_ULOGIC`/`STD_LOGIC` und deren Vektoren wie `STD_LOGIC_VECTOR`)

* `NOT` hat die höchste Priorität; alle anderen Operatoren werden von links nach rechts ausgewertet.
* daher am besten Klammern verwenden

### 5.3. Shift-Operatoren

Sechs Operatoren der Form `vektor OP anzahl`, z. B. `vektor SRL 2` (um 2 Stellen schieben). Die **Anzahl ist ein `INTEGER`** (darf negativ sein → dreht die Richtung um); kommt sie aus einem Vektor, erst mit `to_integer(...)` wandeln.

![Shift- und Rotate-Operatoren: Pfeil = Schieberichtung, grün = kommt rein, rot = fällt raus](images/shift-operatoren.svg)

`SRA` zieht das Vorzeichenbit nach → entspricht **Division durch 2** im Zweierkomplement.

Gilt für `BIT_VECTOR` (ab VHDL-2008 auch `STD_LOGIC_VECTOR`). In `numeric_std` alternativ `shift_left` / `shift_right` / `rotate_left` / `rotate_right`.

### 5.4. Arithmetik-Operatoren

| Operator | Bedeutung | Operanden |
|---|---|---|
| `+` `-` | Addition, Subtraktion | numerische Typen (Integer, Real), Vektoren über `numeric_std` |
| `*` `/` | Multiplikation, Division | Integer, Real, `numeric_std`-Typen |
| `**` | Exponentiation | Basis numerisch, **Exponent muss `INTEGER`** sein |
| `ABS` | Betrag (absoluter Wert) | numerischer Typ |
| `&` | Konkatenation (Arrays aneinanderhängen) | Array- oder Element-Typ |
| `MOD` `REM` | Rest der Integer-Division | nur `INTEGER` |

`**` kennt keine gebrochenen Exponenten, also kein „hoch 1/2“ und keine Wurzeln. Bei der Synthese sind `/`, `**`, `MOD`, `REM` nur mit Zweierpotenzen erlaubt (siehe [Synthese und Optimierung](#14-synthese-und-optimierung)).

**REM und MOD** liefern beide den Rest einer Integer-Division und unterscheiden sich nur im Vorzeichen.

* `a REM b` hat das **Vorzeichen von `a`**, also `a = (a/b)·b + (a REM b)`
* `a MOD b` hat das **Vorzeichen von `b`**, also `a = b·N + (a MOD b)`
* Der Betrag ist immer kleiner als `|b|`. Bei **gleichem Vorzeichen** von `a` und `b` sind beide gleich, erst bei unterschiedlichem Vorzeichen weichen sie ab.

| a | b | a REM b | a MOD b |
|---|---|---|---|
| 3 | 7 | 3 | 3 |
| -3 | 7 | -3 | 4 |
| -1 | 7 | -1 | 6 |

So entstehen die negativen Zeilen mit `b = 7`

* `-3 REM 7` → `-3 / 7 = 0` (zur Null abgeschnitten), also `-3 = 0·7 + (-3)`, Rest **`-3`** (Vorzeichen von `a`)
* `-3 MOD 7` → mit `N = -1` gilt `-3 = 7·(-1) + 4`, Rest **`4`** (Vorzeichen von `b`)
* `-1 REM 7` → `-1 = 0·7 + (-1)`, Rest **`-1`**
* `-1 MOD 7` → mit `N = -1` gilt `-1 = 7·(-1) + 6`, Rest **`6`**


## 6. Attribute

Attribute liefern Informationen über Signale, Arrays, Typen oder Werte. Welche Objekte zulässig sind, hängt von der Kategorie ab.

### 6.1. Signal-Attribute

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

### 6.2. Array-Attribute

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

### 6.3. Typ-Attribute

Anwendbar auf einen **Typ-/Subtyp-Namen**, unabhängig von Signal/Variable/Konstante.

* `type'VAL(n)` – liefert den Wert an Position n (z. B. bei Aufzählungstypen).
* `type'POS(w)` – liefert die Position des Werts w.
* `type'SUCC(w)` / `type'PRED(w)` – Nachfolger / Vorgänger von w.
* `type'IMAGE(w)` – liefert den Wert w als String.
* `type'VALUE(s)` – wandelt den String s zurück in einen Typwert.

## 7. Entity – Schnittstelle

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


## 8. Architecture – Implementierung

Eine Architecture ist ein Modul, das das Verhalten einer digitalen Schaltung beschreibt.

```vhdl
ARCHITECTURE architecture_name OF entity_name IS
    SIGNAL signal_name : type;
BEGIN
    -- VHDL-Code hier
END ARCHITECTURE architecture_name;
```
* Interne Signale werden zur Kommunikation zwischen verschiedenen Teilen der Architecture verwendet.

### 8.1. Signalzuweisung

```vhdl
signal_name <= wert;
```
* `<=` ist der Signalzuweisungsoperator.
* Dies ist eine nebenläufige (concurrent) Anweisung und wird ausgeführt, sobald sich eines der Signale auf der rechten Seite ändert.
* Dies gilt auch für bedingte Signalzuweisungen.

#### 8.1.1. Bedingt (WHEN ELSE)

```vhdl
signal_name <= wert1 WHEN bedingung ELSE
               wert2 WHEN bedingung ELSE
                 ...
               anderer_wert;
```
* `WHEN` wird verwendet, um einem Signal abhängig von einer Bedingung einen Wert zuzuweisen.
* `ELSE` ist optional. Ist es nicht vorhanden, behält das Signal seinen vorherigen Wert.

#### 8.1.2. Selektiv (WITH SELECT)

```vhdl
WITH selektor SELECT
    signal_name <= wert1 WHEN sel_wert1,
                   wert2 WHEN sel_wert2,
                     ...
                   anderer_wert WHEN OTHERS;
```

* `SELECT` wird verwendet, um einem Signal abhängig vom Wert eines Selektors einen Wert zuzuweisen.
* `OTHERS` fängt alle übrigen Fälle ab. Es ist immer erforderlich.

### 8.2. Komponenten-Instanziierung

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
* Positionelle Zuordnung `PORT MAP (a, b, c)`, dann muss die Reihenfolge mit der Port-Reihenfolge der Entity übereinstimmen.
* Benannte Zuordnung mit `=>`, also `PORT MAP (a => sig_a, b => sig_b, c => sig_c)`, dann ist die Reihenfolge egal. Die Zuordnung nutzt `=>`, nicht `<=`.
* Nicht benötigte Ports: `open` lässt einen Port unverbunden — **Ausgänge** immer, **Eingänge** nur mit Standardwert in der Entity (`i : IN BIT := '0'`).
* Konstanten direkt übergeben statt eines Signals: `PORT MAP (x, y, '0', a, open)`.

* `GENERIC MAP` wird verwendet, um Parameter an die Entity zu übergeben. Es ist optional.

### 8.3. Direkte Instanziierung

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

### 8.4. GENERATE (Strukturen replizieren)

`FOR … GENERATE` erzeugt mehrere gleichartige **nebenläufige** Anweisungen (meist Instanzen) – z. B. n gleiche Bausteine in einer Kette. Steht im Architecture-Body (nicht im Process) und braucht zwingend ein **Label**.

```vhdl
G1: FOR i IN 0 TO n-1 GENERATE
    fi : ENTITY work.fadd
        PORT MAP (a(i), b(i), carry(i), sum(i), carry(i+1));
END GENERATE G1;
```

Bedingt erzeugen mit `IF … GENERATE` (z. B. Sonderfall am Rand):

```vhdl
G2: IF n > 1 GENERATE
    -- wird nur erzeugt, wenn die Bedingung wahr ist
END GENERATE G2;
```

* `FOR … GENERATE` ist **strukturell/nebenläufig** (Hardware vervielfältigen) – nicht zu verwechseln mit `FOR … LOOP` (sequentiell, im Process).
* Das Label (`G1:`) ist **Pflicht**.


## 9. Process – sequentieller Ablauf

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

### 9.1. Wait-Anweisungen

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


### 9.2. Sequentielle Anweisungen

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
        -- ohne Label:
        FOR i IN bereich LOOP
            NEXT WHEN bedingung;
        END LOOP;
        -- bei verschachtelten Schleifen siehe eins weiter unten
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

## 10. Unterprogramme (Funktionen & Prozeduren)

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

### 10.1. Funktionen

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

### 10.2. Prozeduren

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
### 10.3. Übergabemechanismen (By Value / By Reference)

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

## 11. Libraries & Packages
### 11.1. Verwenden von Bibliotheken und Packages
```vhdl
LIBRARY library_name;
USE library_name.package_name.ALL;
```

Der Bibliotheksname wird zur Build-Zeit oder vom Hersteller festgelegt.
Der Standard-Bibliotheksname für eigene Packages ist `work`.

* `LIBRARY` und `USE` gelten nur für die **nächste** Entwurfseinheit, nicht für die ganze Datei.
* Hängt eine Entwurfseinheit A von einer Einheit B ab, muss B vor A analysiert werden (Analysereihenfolge).

### 11.2. Standardbibliotheken
* `work` – die aktuelle Bibliothek, in die eigene Entwurfseinheiten analysiert werden.
* `std` – immer sichtbar, mit den Packages `standard` (BIT, BOOLEAN, CHARACTER, INTEGER, Operatoren) und `textio` (Dateien lesen und schreiben).
* `IEEE` – Standardbibliothek für VHDL.
    * `IEEE.STD_LOGIC_1164` – Standard-Package für den Datentyp STD_LOGIC.
    * `IEEE.NUMERIC_STD` – Standard-Package für numerische Datentypen.
    * `IEEE.NUMERIC_BIT` – Standard-Package für bitweise Operationen.

### 11.3. Definieren von Bibliotheken und Packages
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


## 12. Konfiguration

Eine Entity kann **mehrere Architectures** haben. Eine *Configuration* legt fest, **welche** davon benutzt wird – und in strukturellen Designs, **welche Entity/Architecture** jede Component-Instanz (jeder „Sockel“) bekommt. Ohne Configuration nimmt der Simulator standardmäßig die **zuletzt analysierte** Architecture.

### 12.1. Entity-Konfiguration (Typ 1)

Wählt **eine Architecture für eine Entity** aus.

```vhdl
CONFIGURATION cfg_name OF entity_name IS
    FOR architecture_name      -- diese Architecture soll genommen werden
    END FOR;
END CONFIGURATION cfg_name;
```

Simuliert wird dann über den Configuration-Namen (z. B. `vsim cfg_name`), nicht über die Entity.

### 12.2. Instanz-Konfiguration (Typ 2)

Bindet **Component-Instanzen** an eine konkrete Entity + Architecture – nützlich, wenn eine Architecture `COMPONENT`s (Platzhalter) instanziiert und du festlegen willst, welcher echte Baustein in welchen Sockel kommt.

```vhdl
CONFIGURATION cfg_name OF top_entity IS
    FOR top_architecture                                      -- in dieser Architecture ...
        FOR inst_label_1 : comp_name_1 USE ENTITY work.entity_name(arch_name_1); 
        FOR inst_label_2 : comp_name_2 USE ENTITY work.entity_name(arch_name_2); 
        END FOR;                                              -- ... Instanz inst_label binden
    END FOR;
END CONFIGURATION cfg_name;
```

Die innere Bindung kann statt einer einzelnen Instanz auch Gruppen ansprechen — das sind **Alternativen** (je eine pro Bindung), jede mit eigenem `END FOR;`:

```vhdl
FOR inst_label : comp_name USE ENTITY work.entity_name(arch_name);   END FOR;  -- eine Instanz
FOR ALL        : comp_name USE ENTITY work.entity_name(arch_name);   END FOR;  -- alle Instanzen
FOR OTHERS     : comp_name USE ENTITY work.entity_name(arch_name);   END FOR;  -- alle übrigen
FOR ALL        : comp_name USE CONFIGURATION work.sub_cfg;           END FOR;  -- an eine Configuration
```

**Alternative – Configuration Specification:** Dieselbe Bindung kann auch **direkt im Deklarationsteil der Architecture** stehen (nach der `COMPONENT`-Deklaration, vor `BEGIN`). Dann ist sie fest eingebaut, ohne eigene Configuration-Unit:

```vhdl
ARCHITECTURE top_architecture OF top_entity IS
    COMPONENT comp_name PORT ( ... ); END COMPONENT;
    FOR inst_label : comp_name USE ENTITY work.entity_name(arch_name);   -- Bindung direkt hier
BEGIN
    inst_label : comp_name PORT MAP ( ... );
END ARCHITECTURE top_architecture;
```

**Faustregel:** separate `CONFIGURATION` = **austauschbar** (mehrere Varianten, Auswahl beim Simulieren); Spec in der Architecture = **fest eingebaut**, dafür kompakt.

### 12.3. Beispiel: Entity-/Architecture-Wahl

```vhdl
-- === Schritt 1: eine Entity, zwei Architectures ===
ENTITY and2 IS
    PORT (a, b : IN  BIT;
          y    : OUT BIT);
END ENTITY and2;

ARCHITECTURE dataflow OF and2 IS        -- ideal, ohne Verzögerung
BEGIN
    y <= a AND b;
END ARCHITECTURE dataflow;

ARCHITECTURE delayed OF and2 IS         -- mit Gatterlaufzeit
BEGIN
    y <= a AND b AFTER 2 ns;
END ARCHITECTURE delayed;


-- === Schritt 2: Typ 1 - Architecture einer Entity festlegen ===
CONFIGURATION and2_fast OF and2 IS
    FOR dataflow
    END FOR;
END CONFIGURATION and2_fast;

CONFIGURATION and2_real OF and2 IS
    FOR delayed
    END FOR;
END CONFIGURATION and2_real;


-- === Schritt 3: Top-Modul nutzt and2 als COMPONENT (Sockel) ===
ENTITY and3 IS
    PORT (a, b, c : IN  BIT;
          y       : OUT BIT);
END ENTITY and3;

ARCHITECTURE structure OF and3 IS
    COMPONENT and2
        PORT (a, b : IN BIT; y : OUT BIT);
    END COMPONENT;
    SIGNAL t : BIT;
BEGIN
    u1 : and2 PORT MAP (a, b, t);
    u2 : and2 PORT MAP (t, c, y);
END ARCHITECTURE structure;


-- === Schritt 4: Typ 2 - Instanzen an Entity+Architecture binden ===
CONFIGURATION and3_cfg OF and3 IS
    FOR structure
        FOR u1 : and2 USE ENTITY work.and2(dataflow);
        END FOR;
        FOR u2 : and2 USE ENTITY work.and2(delayed);
        END FOR;
    END FOR;
END CONFIGURATION and3_cfg;
```

### 12.4. Beispiel: Porttyp-Konvertierung

Beim Binden kann die `PORT MAP` der Configuration auch **Typen umwandeln** – z. B. wenn der Component mit `BIT` deklariert ist, die echte Entity aber `STD_ULOGIC` nutzt:

```vhdl
LIBRARY ieee;
USE ieee.std_logic_1164.ALL;          -- liefert to_stdulogic / to_bit

-- Die echte Entity arbeitet mit STD_ULOGIC (Component "and2" aus 12.3 nutzt BIT):
ENTITY and2_sl IS
    PORT (p, q : IN  STD_ULOGIC;
          r    : OUT STD_ULOGIC);
END ENTITY and2_sl;

ARCHITECTURE rtl OF and2_sl IS
BEGIN
    r <= p AND q;
END ARCHITECTURE rtl;

-- Configuration bindet and2 (BIT) an and2_sl (STD_ULOGIC) und konvertiert die Porttypen:
CONFIGURATION and3_conv OF and3 IS
    FOR structure
        FOR ALL : and2 USE ENTITY work.and2_sl(rtl)
            PORT MAP (p => to_stdulogic(a),    -- IN:  Component-BIT     -> Entity-STD_ULOGIC
                      q => to_stdulogic(b),
                      to_bit(r) => y);         -- OUT: Entity-STD_ULOGIC -> Component-BIT
        END FOR;
    END FOR;
END CONFIGURATION and3_conv;
```

* **IN-Port:** Konvertierung steht beim *actual* (Component-Seite): `formal => conv(actual)`.
* **OUT-Port:** Konvertierung steht beim *formal* (Entity-Seite): `conv(formal) => actual`.


### 12.5. Verteilung auf Dateien

Wie man Configurations im Projekt organisiert:

1. **Pro Entity eine Configuration** — eine Änderung muss an vielen Stellen gemacht werden.
2. **Alle Configurations in einer separaten Datei** — Änderungen bleiben auf diese Datei beschränkt.
3. **Eine Configuration für die ganze Hierarchie** — nur eine zu pflegen, dafür kann die Syntax unübersichtlich werden.


## 13. Sichtbarkeit (Geltungsbereiche)

Hierarchie der Geltungsbereiche (außen → innen):
`PACKAGE → ENTITY → ARCHITECTURE → PROCESS → FUNCTION/PROCEDURE`

Eine Deklaration ist im **eigenen** Bereich sichtbar **und in allen darin geschachtelten** (tieferen) Bereichen.

Bei **gleichem Namen** in geschachtelten Bereichen:

* Ein Name meint die Deklaration der **gleichen Ebene** (die innerste verdeckt die äußere).
* **Tiefer** liegende Deklarationen sind außen **nicht** sichtbar.
* **Höher** liegende gleichnamige Deklarationen erreicht man qualifiziert: `<bereich>.<name>` — `<bereich>` = Package-/Entity-/Architecture-Name bzw. Process-Label.

```vhdl
ARCHITECTURE behavioral OF xxx IS
    CONSTANT x : INTEGER := 2;          -- x der Architecture
BEGIN
    p1 : PROCESS (i1) IS
        VARIABLE x : INTEGER := 3;      -- x des Process (verdeckt das obere x)
    BEGIN
        o1 <= behavioral.x * i1;        -- 2  (Architecture-x über den Namen)
        o2 <= p1.x * i1;                -- 3  (Process-x; "x" allein reicht hier auch)
    END PROCESS p1;
END ARCHITECTURE behavioral;
```

So erreicht man auch `entity_name.x` (Entity-Bereich) oder `package_name.x` (Package) für höher liegende Deklarationen.


## 14. Synthese und Optimierung

Die Logiksynthese macht aus der RTL-Beschreibung eine Netzliste (Graphentransformation, Boolesche Optimierung mit Karnaugh oder Quine-McCluskey, Technology-Mapping in eine Bibliothek). Ziel ist synchrone, getaktete Logik aus Registern und kombinatorischen Blöcken. Latches nur bewusst einsetzen.

### 14.1. Synthetisierbare Datentypen

* `BIT`, `BIT_VECTOR`, `STD_[U]LOGIC`, `STD_[U]LOGIC_VECTOR` → direkt umgesetzt
* `INTEGER` → unsigned bzw. signed (Zweierkomplement), Breite und Vorzeichen über `RANGE`
* Aufzählungstypen → durchnummeriert
* physikalische Typen, Dateien, Gleitkomma (`REAL`) → nicht synthetisierbar
* empfohlen → `STD_[U]LOGIC`, `STD_[U]LOGIC_VECTOR`

### 14.2. Synthetisierbare Operatoren

* logisch → `AND`, `OR`, `NOR`, `XOR`, `XNOR`, `NOT`
* Vergleich → `=`, `/=`, `<`, `<=`, `>`, `>=`
* Verkettung → `&`
* Schieben/Rotieren → `SLL`, `SRL`, `SLA`, `SRA`, `ROL`, `ROR`
* `ABS`, `+`, `-`, `*`
* `/`, `REM`, `MOD`, `**` → **nur mit Zweierpotenz (2 hoch n), sonst nicht synthetisierbar**

### 14.3. Nicht erlaubte Anweisungen

* `WAIT FOR ... ns`
* `... AFTER ... ns`
* `LOOP` mit variablen Grenzen (feste Grenzen sind ok)
* `REPORT`, `ASSERT`

### 14.4. Kombinatorische Logik

Nebenläufige Zuweisung (`d <= a OR b OR c;`) oder Prozess mit `IF`, `CASE`, `LOOP` (feste Grenzen). Drei Regeln.

* **Sensitivitätsliste vollständig** (jedes gelesene Signal)
* **jede Zuweisung vollständig** (in `IF`/`CASE` unter allen Bedingungen), sonst Latch
* **keine Rückführung** (kein gelesenes Signal später zuweisen)

```vhdl
PROCESS (a, b, c, d) IS                  -- vollständige Sensitivitätsliste
    VARIABLE v : STD_ULOGIC_VECTOR(3 DOWNTO 0);
BEGIN
    v(3) := a; v(2) := b; v(1) := c; v(0) := d;
    f <= 0;                              -- vollständige Zuweisung (Default)
    FOR i IN 0 TO 3 LOOP
        IF v(i) = '1' THEN
            f <= i;
            EXIT;
        END IF;
    END LOOP;
END PROCESS;                             -- keine Rückführung
```

### 14.5. Getaktete Logik (Speicher, Flip-Flop)

In einem getakteten Prozess (innerhalb `IF RISING_EDGE(clk)`) entscheidet die Art der Zuweisung, ob ein Flip-Flop entsteht.

* Eine **Signalzuweisung** in einem getakteten Prozess wird **immer** zu einem **Speicher** (Flip-Flop).
* Eine **Variable** wird nur dann zu einem Flip-Flop, wenn sie im Takt **zuerst gelesen und dann geschrieben** wird, denn dann trägt sie den Wert aus dem vorherigen Takt und braucht Speicher.
* Wird die Variable **zuerst geschrieben und dann gelesen** (in jedem Takt frisch), ist sie nur eine Verbindung (Kombinatorik), kein Flip-Flop.

```vhdl
PROCESS (clk) IS
    VARIABLE v : STD_ULOGIC;
BEGIN
    IF RISING_EDGE(clk) THEN
        v := '1';                  -- erst schreiben, dann lesen -> nur temporär, kein FF
        FOR i IN 0 TO 7 LOOP
            v := v AND input(i);
        END LOOP;
        reg <= v;                  -- Signalzuweisung -> reg wird ein Flip-Flop
    END IF;
END PROCESS;
```

### 14.6. Initialisierung und Reset

* ASIC → explizite Init (`:= '0'`) vermeiden, lieber über Reset (sonst kann Simulation vor/nach Synthese abweichen)
* FPGA (AMD/Xilinx) → explizite Init (`SIGNAL q : STD_ULOGIC := '0';`) möglich
* Reset spart oft Ressourcen, wenn nötig synchronen Reset verwenden
* ein **synchroner Reset** kann aber verhindern, dass kompakte Primitive wie `SRL16` (Schieberegister in einer LUT) genutzt werden, das kostet Fläche (Beispiel in den Aufgaben)

### 14.7. Optimierung

Zwei große Ziele, die meist im Konflikt stehen, **Geschwindigkeit** und **Fläche**. Mehr Geschwindigkeit kostet in der Regel mehr Fläche und umgekehrt.

Die Geschwindigkeit hat drei Metriken

* **Betriebsfrequenz** (frequency, in Hz)
* **Latenz** (Eingangs-Ausgangs-Verzögerung, in Taktzyklen)
* **Durchsatz** (throughput, in Bit pro Taktzyklus)

Maßnahmen je nach Ziel

* **Hoher Durchsatz** → Pipelining, also Register zwischen die Rechenstufen, das kostet aber Fläche
* **Geringe Latenz** → Parallelismus, logische Vereinfachung, Pipelineregister wieder entfernen
* **Hohe Betriebsfrequenz** → Register einfügen (kritischen Pfad verkürzen), parallele Strukturen, logische Vereinfachung, kritischen Pfad balancieren (Flip-Flops verschieben)
* **Geringe Fläche** → Ressourcen mehrfach nutzen, iterativ statt entrollt rechnen, Register sparen

Die maximale Frequenz wird vom längsten Pfad zwischen zwei Registern bestimmt.

`f_max = 1 / (t_clk-q + t_comb + t_load + t_setup - t_slew)`

Die Terme im Nenner sind

* `t_clk-q` Verzögerung im sendenden Flip-Flop, von der Taktflanke bis der Ausgang Q gültig ist (Steilheit der Flanke)
* `t_comb` Laufzeit durch die Kombinatorik zwischen den beiden Registern
* `t_load` zusätzliche Verzögerung durch Last und Leitungen (Verdrahtung und Fanout)
* `t_setup` Setup-Zeit des empfangenden Flip-Flops, d muss vor der Taktflanke stabil sein
* `t_slew` Taktversatz zwischen den Registern ([Clock Skew](#clock-skew-problem)), wird abgezogen, weil ein später ankommender Zieltakt mehr Zeit für den Pfad lässt

Kurz, je kürzer der längste Pfad zwischen zwei Registern, desto höher die mögliche Frequenz. Genau da setzen die Maßnahmen oben an.

Ausführliche Beispiele mit Blockschaltbild und Code (Mittelung iterativ, als Pipeline und latenzarm, FIR-Filter mit und ohne eingefügte Register, paralleler Multiplizierer, Prioritätsencoder, Pfad-Balancierung, serieller Multiplizierer, Resource Sharing) stehen in den Aufgaben.


## 15. Testbenches (nur Simulation)

Eine Testbench wird verwendet, um die Funktionalität einer digitalen Schaltung zu testen.

### 15.1. Testbench-Struktur

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


### 15.2. TEXTIO-Package

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


## 16. Assertions

Eine Assertion ist eine Behauptung, also ein Monitor, der ein bestimmtes Verhalten überprüft und meldet, sobald es verletzt wird. Das verbessert die Beobachtbarkeit und dient als Selbsttest bei späterer Wiederverwendung. Es gibt das einfache VHDL-`ASSERT` als Momentaufnahme und PSL für zeitliche Eigenschaften über mehrere Takte.

### 16.1. VHDL-Assertions

Prüfen einen Ausdruck genau in diesem Moment, also pro Delta-Zyklus, und das nur in der Simulation. Sie stehen im Architecture-Body oder in einem Prozess.

```vhdl
ASSERT bedingung REPORT "Fehlermeldung" SEVERITY schweregrad;
```

* `bedingung` ist die zu prüfende Bedingung, bei `FALSE` wird gemeldet.
* `REPORT` ist die Meldung, die angezeigt wird.
* `SEVERITY` ist der Schweregrad der Fehlermeldung.
    * `NOTE` Informationsmeldung
    * `WARNING` Warnmeldung
    * `ERROR` Fehlermeldung
    * `FAILURE` fatale Fehlermeldung

Ein vollständiges Beispiel mit Setup, Hold und Pulsbreite steht im Anhang unter [Betriebsbedingungen (Flip-Flop)](#betriebsbedingungen-flip-flop).

### 16.2. PSL-Assertions

PSL (Property Specification Language) beschreibt Eigenschaften, die über mehrere Takte gelten sollen, anders als das VHDL-`ASSERT` als Momentaufnahme. Eingebettet wird PSL als Kommentar `-- psl ...`, damit Werkzeuge ohne PSL-Unterstützung die Zeile überlesen. Die ausführlichen Beispiele stehen in [Aufgabe 18 (PRÜFUNG)](Aufgaben.md#aufgabe-18-psl-assertions-prüfung).

**Aufbau einer PSL-Anweisung**

<pre><code><span style="color:#7f8c8d">-- psl </span><span style="color:#c0392b">mutex</span>: <span style="color:#8e44ad">ASSERT</span> <span style="color:#1e8449">ALWAYS NOT (rd AND wr)</span> <span style="color:#d35400">@RISING_EDGE(clk)</span>;</code></pre>

* <span style="color:#c0392b">**mutex** ist der Name der Assertion, frei wählbar.</span>
* <span style="color:#8e44ad">**ASSERT** ist die Verifikationsanweisung, sie prüft die Eigenschaft.</span>
* <span style="color:#1e8449">**ALWAYS NOT (rd AND wr)** ist die zu prüfende Eigenschaft.</span>
* <span style="color:#d35400">**@RISING_EDGE(clk)** ist die Taktangabe, sie bestimmt, wann geprüft wird.</span>

Bedeutung, die Signale `rd` und `wr` dürfen bei einer steigenden Flanke nie beide `'1'` sein.

**PSL-Schichten**

* Boolesche Schicht, der eigentliche Ausdruck in der HDL, etwa `NOT (rd AND wr)`.
* Zeitliche Schicht, wann der Ausdruck gelten muss, etwa `ALWAYS` und `@RISING_EDGE(clk)`.
* Modellierungsschicht, Zusatzcode nur für die Verifikation.
* Verifikationsschicht, was mit der Eigenschaft geschieht, also `ASSERT`, `ASSUME` oder `RESTRICT`.


## 17. Verzögerungsmodelle (nur Simulation)
### 17.1. Transport-Verzögerung

Die Transport-Verzögerung wird verwendet, um ein Signal um eine bestimmte Zeitspanne zu verzögern.

```vhdl
x <= TRANSPORT logischer_ausdruck AFTER 10 ns;
-- verzögert das Signal um 10 ns 
```

### 17.2. Inertiale Verzögerung

Die inertiale Verzögerung wird verwendet, um ein Signal um eine bestimmte Zeitspanne zu verzögern. Zusätzlich muss das Signal für die Dauer der Verzögerung stabil bleiben.

```vhdl
x <= logischer_ausdruck AFTER 10 ns;
-- verzögert das Signal um 10 ns, wenn der logische_ausdruck für die gesamte Dauer wahr ist.

-- Variante der inertialen Verzögerung:

x <= REJECT reject_zeit INERTIAL logischer_ausdruck AFTER 10 ns;
-- verzögert das Signal um 10 ns und verwirft / ignoriert Änderungen, die kürzer als reject_zeit sind.
```

Ein durchgerechnetes Beispiel mit Wellenformen für beide Modelle steht in [Aufgabe 3 der alten Prüfung](AltePruefung.md#aufgabe-3), inertial verschluckt kurze Pulse, transport gibt sie weiter.


## Zusätzliche Informationen

### Nebenläufige Anweisungen (concurrent statements)

Stehen direkt im Architecture-Body und laufen alle gleichzeitig.

* nebenläufige Signalzuweisung (`<=`)
* Komponenteninstantiierung (`COMPONENT`)
* `PROCESS`
* `ASSERT`
* Prozeduraufrufe
* `GENERATE`
* `BLOCK`

### Sequenzielle Anweisungen (sequential statements)

Stehen nur in einem Prozess oder Unterprogramm und laufen der Reihe nach.

* sequenzielle Signalzuweisung (`<=`)
* Variablenzuweisung (`:=`)
* `WAIT` (ersetzt die Sensitivitätsliste)
* `IF`, `CASE`
* `FOR LOOP`, `WHILE LOOP`, `LOOP`, `EXIT`, `NEXT`
* Prozeduraufruf
* `ASSERT`

### Operationen und Operatoren (Übersicht)

Alle Operator-Kategorien mit zulässigen Operanden- und Ergebnistypen. Details und Beispiele in [Operatoren](#5-operatoren), zu `REM`/`MOD` siehe [Arithmetik-Operatoren](#54-arithmetik-operatoren).

| Operationstyp | Operation | Operandentyp | Ergebnistyp |
|---|---|---|---|
| Logisch | `NOT AND NAND OR NOR XOR XNOR` | BIT, BOOLEAN, STD_(U)LOGIC, Array davon | BIT, BOOLEAN, STD_(U)LOGIC, Array davon |
| Vergleich | `= /= < <= > >=` | beide vom gleichen Typ | BOOLEAN |
| Addition | `+ -` | numerischer Typ, BIT, BIT_VECTOR | numerischer Typ, BIT, BIT_VECTOR |
| Konkatenation | `&` | Array-Typ, Element-Typ | Array-Typ |
| Vorzeichen | `+ - ABS` | numerischer Typ | numerischer Typ |
| Exponentiation | `**` | Basis numerisch, Exponent `INTEGER` | numerischer Typ |
| Multiplikation | `* /` | INTEGER, REAL, BIT, BIT_VECTOR | INTEGER, REAL, BIT, BIT_VECTOR |
| Modulo | `MOD` | INTEGER | INTEGER |
| Rest | `REM` | INTEGER | INTEGER |
| physikalische Typen | `* /` | physikalischer und numerischer Typ | physikalischer Typ oder INTEGER |

`XNOR` gibt es ab VHDL-93.

### Ereignisgesteuerte Simulation (virtuelle Verzögerung Δt)

* Hardware arbeitet **parallel/nebenläufig**, der Rechner aber **sequentiell/seriell**.
* Trick → **virtuelle Verzögerungszeit Δt**. Ein Signal nimmt seinen neuen Wert nicht sofort an, sondern erst Δt später. Δt ist unendlich klein und ändert die simulierte Zeit nicht.
* Pro Simulationszeitpunkt werden so über mehrere **Delta-Zyklen** Ereignisse abgearbeitet. Die simulierte Zeit bleibt tₙ, nur die Rechenschritte steigen.

![Ereignisgesteuerte Simulation, pro Zeitpunkt läuft die Schleife Berechnung und Signalveränderung mehrfach (Δt) bevor die Simulationszeit weitergeht](images/sim-delta.svg)

**Beispiel (Delta-Zyklen)**

```vhdl
ARCHITECTURE data_flow OF example IS
    SIGNAL int : BIT;
BEGIN
    int  <= a NAND b;
    out1 <= int XOR b;
END ARCHITECTURE data_flow;
```

| Signal | t=0 ns | t=1 ns | t=1 ns + Δt | t=1 ns + 2Δt |
|---|---|---|---|---|
| `a` | 1 | 1 | 1 | |
| `b` | 1 | 0 | 0 | |
| `int` | 0 | 0 | 1 | |
| `out1` | 1 | 1 | 0 | 1 |

Die Rechenzeit hängt von der Anzahl der Δt-Schritte ab, die simulierte Zeit bleibt tₙ.

### Regeln für Prozesse

* Ein Prozess ist eine **nebenläufige** Anweisung in der Architecture und enthält selbst **sequenzielle** Anweisungen.
* Beim Start des Simulators wird jeder Prozess einmal gestartet.
* Ein Prozess endet nie. Er wird abgearbeitet oder wartet (zeitweilig angehalten).
* Ein Prozess braucht entweder eine **Sensitivitätsliste** oder eine oder mehrere **`WAIT`**-Anweisungen, nicht beides.
* Signalzuweisungen im Prozess sind sequenziell und übernehmen den Wert erst nach Δt, also am `END PROCESS` (bei Sensitivitätsliste) oder am `WAIT`.
* Im Prozess kann kein Signal deklariert werden. Signale aus Entity oder Architecture lassen sich lesen und verändern.

### Regeln für Variablen

* Variablen werden im Deklarationsteil eines Prozesses deklariert und sind nur dort gültig (Ausnahme shared variable in VHDL-93).
* Zuweisung mit `:=`.
* Variablen nehmen den neuen Wert **sofort** an (anders als Signale, die erst nach Δt).
* Initialisierung beim ersten Durchlauf des Prozesses.
* Wartet ein Prozess, behält er die Variablenwerte.

### Unterteilung digitaler Systeme

Digitale Systeme unterteilt man in kombinatorische und sequenzielle, sequenzielle weiter in synchrone und asynchrone.

![Unterteilung digitaler Systeme in kombinatorisch und sequenziell, sequenziell weiter in synchron (FSM) und asynchron](images/unterteilung-systeme.svg)

### Betriebsbedingungen (Flip-Flop)

Ein Flip-Flop übernimmt den Eingang d nur dann sicher, wenn d rund um die Taktflanke stabil bleibt.

![Setup, Hold und Verzögerung am D-Flip-Flop, im roten Fenster darf sich d nicht ändern](images/ff-betriebsbedingungen.svg)

* `t_p` Verzögerungszeit des Flip-Flops, von der Taktflanke bis der Ausgang reagiert
* `t_w` Pulsbreitenbedingung, minimale Impulsbreite zum Beispiel für das Taktsignal
* `t_h` Hold-Bedingung, das Eingangssignal muss nach der Taktflanke stabil bleiben
* `t_su` Setup-Bedingung, das Eingangssignal muss schon vor der Taktflanke stabil anliegen

Im roten Fenster „keine Änderung" von `t_su` vor bis `t_h` nach der Flanke darf sich d nicht ändern, sonst kann der Flip-Flop metastabil werden.

Prüfen lassen sich diese Bedingungen mit [VHDL-Assertions](#161-vhdl-assertions) im Architecture-Body. Die Attribute `'STABLE`, `'DELAYED` und `RISING_EDGE` liefern dafür die Zeitbezüge.

```vhdl
ARCHITECTURE constraints OF dff IS
    CONSTANT tsu : TIME := 2 ns;
    CONSTANT th  : TIME := 2 ns;
    CONSTANT tw  : TIME := 3 ns;
BEGIN
    PROCESS (clk) IS
        CONSTANT delay : TIME := 3 ns;
    BEGIN
        IF RISING_EDGE(clk) THEN
            q <= d AFTER delay;
        END IF;
    END PROCESS;

    -- Setup, bei steigender Flanke muss d schon tsu lang stabil sein
    ASSERT NOT (RISING_EDGE(clk) AND NOT d'STABLE(tsu)) REPORT "setup time violation";
    -- Hold, th nach der Flanke (clk um th verzögert) muss d noch stabil sein
    ASSERT NOT (RISING_EDGE(clk'DELAYED(th)) AND NOT d'STABLE(th)) REPORT "hold time violation";
    -- Pulsbreite, bei fallender Flanke muss clk vorher tw lang '1' gewesen sein
    ASSERT NOT (FALLING_EDGE(clk) AND NOT (clk'DELAYED(tw) = '1')) REPORT "pulse width violation";
END ARCHITECTURE constraints;
```

### Zustandsautomaten (FSM)

Aufbau (Moore und Mealy) → Eingang → Kombinatorik (nächster Zustand) → Zustandsregister → `state` → Kombinatorik (Ausgang) → Ausgang.

* **Moore** → Ausgang hängt nur vom Zustand ab. Der Ausgang ändert sich nur, wenn sich das Zustandsregister ändert.
* **Mealy** → Ausgang hängt vom Zustand und vom Eingang ab. Der Ausgang ändert sich bei Änderung des Zustandsregisters und des Eingangs.

![Moore-Automat: Eingang in die Kombinatorik, Zustandsregister, Ausgangs-Kombinatorik nur aus dem Zustand](images/moore.svg)

![Mealy-Automat: wie Moore, aber der Eingang geht zusätzlich direkt in die Ausgangs-Kombinatorik](images/mealy.svg)

**Beschreibung in VHDL**

* Ein Prozess
* **Zwei Prozesse** (1. Zustandsübergang und Zustandsregister, 2. Ausgang) → bevorzugt
* Drei Prozesse (1. Zustandsübergang, 2. Zustandsregister, 3. Ausgang)

Ein vollständiges Code-Beispiel in dieser Zwei-Prozess-Schreibweise (Modulo-4-Zähler als Moore- und Mealy-Automat) steht in den Aufgaben.

**Encoding**

* Gray (nur 1 Bit Wechsel)
* Binär (automatisch)
* One-Hot (`0001`, `0010`, `0100`, `1000`, ...)

**Realisierung**

* LUT (Look-Up-Table)
* BRAM

### Digital Clock Manager (DCM)

Der Digital Clock Manager ist ein Hardware-Block im FPGA, der aus einem Eingangstakt saubere, verschobene oder vervielfachte Takte erzeugt. Bestandteile sind

* **Delay-Locked-Loop** zur Clock-Skew-Elimination
* **Digital Frequency Synthesizer** für mehrere Takte aus einem Eingangstakt
* **Phase Shift** zum Beispiel für DDR-RAM-Schaltungen
* **Status Logic** mit Information über den Zustand des DCM

#### Clock Skew (Problem)

Derselbe Takt erreicht über Puffer und Leitungen verschiedene Punkte zu leicht unterschiedlichen Zeiten. Diese Differenz heißt Clock Skew. Im Bild kommen die Flanken an A, B und C um Δb und Δc versetzt an, dadurch kann ein Register seine Daten zu früh oder zu spät übernehmen.

![Clock Skew, derselbe Takt erreicht A, B und C zeitversetzt](images/clock-skew.svg)

#### Clock-Skew-Elimination

Der DCM gleicht mit seiner Delay-Locked-Loop die Laufzeit aus und verschiebt den Takt so vor, dass die Flanken an allen Punkten wieder gleichzeitig ankommen. Das Ergebnis ist eine ideale Taktausrichtung, A, B und C liegen übereinander. Dadurch bleibt mehr von der Taktperiode für die eigentliche Logik übrig und Timing-Fehler durch Skew verschwinden.

![Clock-Skew-Elimination durch den DCM, alle Flanken liegen übereinander](images/clock-skew-elim.svg)

#### Mehrere Taktdomänen (Problem)

Wechselt ein Signal von einer Taktdomäne in eine andere, hier von slow_clk nach fast_clk, kann es genau auf der Flanke des Zieltakts wechseln. Das abtastende Flip-Flop wird dann metastabil, sein Ausgang ist für eine unbestimmte Zeit weder sicher 0 noch 1. Im Bild trifft die Datenänderung Di genau auf die fast_clk-Flanke.

![Mehrere Taktdomänen, Di wechselt auf der fast_clk-Flanke und wird metastabil](images/taktdomaenen.svg)

#### Lösung 1 Steuerung der Phase (DLL)

Mit einer DLL wird der schnelle Takt fest aus dem langsamen abgeleitet, zum Beispiel fast_clk = 2·slow_clk mit Phase 0°. Beide Takte haben dann eine feste, bekannte Phasenbeziehung und die Datenänderung liegt nie auf der Abtastflanke. So entsteht keine Metastabilität. Das funktioniert aber nur, wenn die Takte voneinander abgeleitet und nicht wirklich asynchron sind.

![Lösung 1, die DLL erzeugt fast_clk = 2·slow_clk mit fester Phase, Di bleibt stabil](images/phase-dll.svg)

#### Lösung 2 Zwei Flip-Flops

Zwei hintereinander geschaltete Flip-Flops im Zieltakt bilden einen Synchronisierer. Das erste Flip-Flop darf metastabil werden, hat aber fast einen ganzen Takt Zeit sich zu stabilisieren, bevor das zweite Flip-Flop abtastet.

![Lösung 2, zwei Flip-Flops als Synchronisierer im Zieltakt](images/sync-2ff.svg)

* Das erste Flip-Flop kann metastabil werden.
* Mit hoher Wahrscheinlichkeit ist das Signal stabil, bevor das zweite Flip-Flop es übernimmt, die nachfolgende Kombinatorik sieht dann keinen metastabilen Zustand.
* Theoretisch kann Metastabilität unendlich dauern, praktisch wird der Zustand in endlicher Zeit stabil.
* Man kann nicht vorhersagen, ob der Übergang im aktuellen oder erst im nächsten Takt erfolgt.

Anschaulich passiert in FF1 Folgendes. FF1 tastet das asynchrone Signal ab, und trifft der Wechsel genau die Taktflanke, wird FF1 metastabil. Das heißt nicht, dass der Ausgang einfach 1 statt 0 ist, sondern der Ausgang hängt für kurze Zeit zwischen 0 und 1, ein Zwischenpegel der vielleicht schwingt. Dieser Zustand klingt von selbst ab und FF1 kippt nach kurzer Zeit auf einen sauberen Wert, also eindeutig 0 oder 1. Erst danach tastet FF2 ab und gibt nur noch diesen sauberen Wert an die Logik weiter.

Das eignet sich nur für einzelne Steuersignale, nicht für ganze Busse.

#### Lösung 3 FIFO

Für ganze Busse zwischen asynchronen Taktdomänen nimmt man ein asynchrones FIFO. Auf der einen Seite wird mit clk1 geschrieben, auf der anderen mit clk2 gelesen, die Statussignale full und empty zeigen den Füllstand. Die Schreib- und Lesezeiger werden intern über Zwei-Flip-Flop-Synchronisierer aus Lösung 2 in die jeweils andere Domäne übertragen, damit auch die Zeiger sicher ankommen.

![Lösung 3, ein asynchrones FIFO überträgt Busse zwischen zwei Taktdomänen](images/fifo.svg)

### Verifikation

Verifikation stellt sicher, dass die Implementierung zum geplanten Konzept passt, und deckt Entwurfsfehler früh auf, bevor die teure Realisierung beginnt. Das Hauptproblem ist die Komplexität, denn ein spät entdeckter Fehler verursacht hohe Kosten.

#### Verifikationstechniken

* Simulation, oft hardwarebeschleunigt, legt Stimuli an ein Modell an und prüft die Ausgänge.
* FPGA-Prototyping, die Schaltung läuft als echte Hardware auf einem FPGA.
* Emulation, die Schaltung wird auf einen spezialisierten Emulator abgebildet.
* Formale Verifikation, Eigenschaften werden bewiesen statt simuliert, durch Modellprüfung (Model Checking) gegen ein Modell oder durch Beweistechniken (Theorembeweis) über die Schaltung.
* Assertion Based Verification (ABV), Assertions laufen in Simulation oder formaler Verifikation mit und erkennen Fehler direkt an der Quelle, siehe [Assertions](#16-assertions).

#### Simulator-Typen

* Übersetzend (compiled), die Schaltung wird in ein Programm übersetzt, die Gatter werden nach Ebene sortiert und der Reihe nach ausgewertet. Es gibt keine Ereignisliste, dafür aber auch unnötige Auswertungen.
* Ereignisgesteuert, arbeitet eine sortierte Ereignisliste ab und ermittelt alle Signalwerte zu allen Zeitpunkten. Verarbeitet auch asynchrone Schaltungen, die Rechenzeit hängt von der Zahl der Ereignisse ab. Näheres unter [Ereignisgesteuerte Simulation](#ereignisgesteuerte-simulation-virtuelle-verzögerung-δt).
* Zyklenbasiert, ermittelt die Signalwerte nur an den Registern zur Taktflanke und trennt Funktion vom Zeitverhalten. Setzt eine synchrone Schaltung voraus und ist 10 bis 100 Mal schneller als die ereignisgesteuerte Simulation.

Die ereignisgesteuerte Simulation zeigt jede Aktivität samt Zeitverhalten und Glitches, die zyklenbasierte nur die Werte genau an den Taktflanken.

![Vergleich ereignisgesteuerte und zyklenbasierte Simulation](images/sim-event-cycle.svg)

#### Statische Laufzeitanalyse (STA)

Die STA bestimmt die höchstmögliche Taktfrequenz, indem sie die Verzögerung auf dem längsten Pfad von Register zu Register berechnet, inklusive Setup- und Hold-Zeit, und das ganz ohne Simulation auf Gatterebene. Sie funktioniert auch bei sehr großen Schaltungen mit über einer Million Gattern.

Betrachtet wird der Pfad von einem Flip-Flop über die kombinatorische Logik bis zum nächsten Flip-Flop, beide am selben Takt. Genau diese Strecke durchläuft die längste Verzögerung `t_pl` und die kürzeste `t_pk`.

Die Verzögerungszeiten der Gatter und Signale hängen ab von der internen Gatterverzögerung, der Signalsteilheit am Eingang, den Ausgangskapazitäten (eine große Last bedeutet eine lange Verzögerung) sowie von Prozess, Spannung und Temperatur (PVT).

Dazu wird der kombinatorische Block als gewichteter Graph dargestellt (Knoten sind Gatter und Ein- bzw. Ausgänge, Kanten sind Signale mit ihrer Verzögerung) und darin der längste Pfad gesucht, mit einem Algorithmus für den längsten Pfad ähnlich Dijkstra über die Rekursion `D(N) = max über alle Vorgänger v von ( D(v) + d(v->N) ) + d(N)`. Probleme bereiten asynchrone Schaltungen und falsche Pfade, also Kombinationen, die real nie auftreten.

Im Beispiel steht über jedem Knoten die Ankunftszeit `D(N)`, an der Kante das Kantengewicht und unter dem Knoten die Gatterlaufzeit. Der längste Pfad `b → G1 → G2 → G4 → f` ist magenta hervorgehoben und ergibt die Ankunftszeit 5.50.

![Längster Pfad im gewichteten Graphen, Ankunftszeit am Ausgang f ist 5.50](images/sta-graph.svg)

Mit der Taktperiode `T`, der Verzögerung auf dem längsten Pfad `t_pl`, der auf dem kürzesten Pfad `t_pk` und der Flip-Flop-Verzögerung `t_p` gelten zwei Zeitbedingungen. Im roten Fenster „keine Änderung" aus `t_su` und `t_h` darf sich D nicht ändern, der Ausgang Q reagiert erst `t_p` nach der Taktflanke.

![Taktperiode T mit Setup, Hold und Ausgangsverzögerung am D-Flip-Flop](images/ff-timing-sta.svg)

* Setup-Bedingung `(t_p + t_pl) <= (T - t_su)`
* Hold-Bedingung `(t_p + t_pk) >= t_h`