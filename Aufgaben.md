## Aufgabe 1: Entity eines Adders deklarieren

```vhdl
ENTITY adder IS
    PORT (  x, y : IN  BIT;
            z, c : OUT BIT);
END ENTITY adder;
-- erstellt die Schnittstelle nach außen
```

## Aufgabe 2: Architecture eines Adders deklarieren

```vhdl
ARCHITECTURE data_flow OF adder IS
BEGIN
    z <= ((NOT x) AND y) OR (x AND (NOT y));
    c <= x AND y;
END ARCHITECTURE data_flow;
-- das sind nebenläufige Anweisungen
```

```vhdl
-- Alternativ mit SIGNAL
ARCHITECTURE data_flow OF adder IS
    SIGNAL a, b : BIT;
BEGIN
    a <= (NOT x) AND y;
    b <= x AND (NOT y);   -- vorher fälschlich identisch mit a
    z <= a OR b;          -- Summe = a ODER b (vorher AND)
    c <= x AND y;
END ARCHITECTURE data_flow;


-- Frage: Name der Architecture ist hier data_flow ...
--        und mit "OF adder" wird die Architecture mit der Entity adder verbunden?
--        --> ja. data_flow ist der Architecture-Name, "OF adder" bindet sie an die Entity.
--            (Name oben und bei END muss gleich sein.)


-- Frage: Sind alle Signalzuweisungen hier nebenläufig?
--        --> ja, alle Signalzuweisungen im Architecture-Body sind nebenläufig (concurrent).

```
```vhdl
-- Und wie sieht es mit WHEN ELSE aus?
ARCHITECTURE data_flow OF adder IS
BEGIN
    z <= '1' WHEN x = '1' AND y = '0' ELSE
         '1' WHEN x = '0' AND y = '1' ELSE
         '0';
    c <= '1' WHEN x = '1' AND y = '1' ELSE   -- vorher: x = '1' AND x = '1'
         '0';
END ARCHITECTURE data_flow;
-- Frage: Ist das hier auch nebenläufig?
--        --> ja. Bedingte Signalzuweisungen (WHEN ELSE) außerhalb eines Process
--            sind ebenfalls nebenläufig. Reihenfolge spielt keine Rolle.
```

## Aufgabe 3 : erstelle einen or Baustein

```vhdl
ENTITY or2 IS
    PORT (  a, b : IN  BIT;
            r : OUT BIT);
END or2 adder;
```
```vhdl
ARCHITECTURE data_flow OF or2 IS
BEGIN
	r <= a OR b;
END ARCHITECTURE data_flow
```

## Aufgabe 4: erstelle eine Volladdierer

```vhdl
ENTITY adder3 IS
    PORT (  a, b, c : IN  BIT;
            z, co : OUT BIT);
END ENTITY adder;

--  mit PORTMAP :
ARCHITECTURE  structure OF adder3 IS
	-- COMPONENT am besten aus Entity Kopieren und Entity in COMPONENT unebenen
	COMPONENT adder IS
    		PORT (  x, y : IN  BIT;
            		z, c : OUT BIT);
	END COMPONENT adder;

	COMPONENT or2 IS
    		PORT (  a, b : IN  BIT;
            		r : OUT BIT);
	COMPONENT or2 adder;

	
	SIGNAL t_sum, t_c1,t_c2: BIT;
BEGIN
	G1: adder PORT MAP(a,b,t_sum, t_c1);
	G2: adder PORT MAP(t_sum, c, z, t_c2)
	G3: or2   PORT MAP(t_c2, t_c1, co)
END  ARCHITECTURE structure;
```
```vhdl
-- direkt mit Instantiierung (VHDL 93)

ARCHITECTURE  structure OF adder3 IS
	SIGNAL t_sum, t_c1,t_c2: BIT;
BEGIN
	G1: ENTITY work.adder(data_flow) PORT MAP (a,b, t_sum, t_c1);
	G2: ENTITY work.adder(data_flow) PORT MAP (t_sum, c, z, t_c2);
	G3: ENTITY work.or2(data_flow)   PORT MAP (t_c2, t_c1, co);
END  ARCHITECTURE structure;
```

## Aufgabe 5: erstelle eine architecture eines adders (sequenziell)
```vhdl
ENTITY adder IS
    PORT (  x, y : IN  BIT;
            z, c : OUT BIT);
END ENTITY adder;
```
```vhdl
ARCHITECTURE algorithm OF adder IS
BEGIN
    PROCESS (x, y) IS
    BEGIN
        IF ((x = '0' AND y = '1') OR (x = '1' AND y = '0')) THEN
            z <= '1'; c <= '0';
        ELSIF (x = '1' AND y = '1') THEN
            z <= '0'; c <= '1';
        ELSE
            z <= '0'; c <= '0';
        END IF;
    END PROCESS;
END ARCHITECTURE algorithm;
```

## Aufgabe 6: 4-aus-16-Decoder mit Schleife
```vhdl
ENTITY decoder IS
    PORT (  in_array  : IN  BIT_VECTOR(3 DOWNTO 0);
            out_array : OUT BIT_VECTOR(15 DOWNTO 0));
END ENTITY decoder;
```
```vhdl
ARCHITECTURE algorithm OF decoder IS
BEGIN
    PROCESS (in_array)
        VARIABLE sum : INTEGER RANGE 0 TO 15;
    BEGIN
        -- Eingang in eine Ganzzahl umrechnen
        sum := 0;
        FOR i IN 0 TO 3 LOOP
            IF in_array(i) = '1' THEN
                sum := sum + 2**i;   -- Bit i hat die Wertigkeit 2^i
            END IF;
        END LOOP;

        -- alle Ausgänge auf 0 setzen
        FOR i IN 0 TO 15 LOOP
            out_array(i) <= '0';
        END LOOP;

        -- den passenden Ausgang aktivieren
        out_array(sum) <= '1';
    END PROCESS;
END ARCHITECTURE algorithm;
```
## Aufgabe 7: Delay-Modelle

### Aufgabe 7.1 Unit-Delay-Modell für eine ARCHITECTURE
```vhdl
LIBRARY ieee;
USE ieee.std_logic_1164.ALL;

ENTITY and2 IS
    PORT(x,y:   IN STD_LOGIC;
        z:      OUT STD_LOGIC);
END ENTITY and2;
```
```vhdl
ARCHITECTURE const_delay OF and2 IS
BEGIN  
    PROCESS(x,y) IS
        CONSTANT delay: TIME := 1 ns;
    BEGIN
        z <= (x AND y) AFTER delay;
    END PROCESS;
END ARCHITECTURE const_delay;
 ```

 ### Aufgabe 7.2 Unit-Delay-Modell für alle ENTITY´s (GENERIC)

 ```vhdl
LIBRARY ieee;
USE ieee.std_logic_1164.ALL;

ENTITY and2 IS
    GENERIC(delay : TIME := 1 ns);
    PORT(x,y:   IN STD_LOGIC;
        z:      OUT STD_LOGIC);
END ENTITY and2;
```
```vhdl
ARCHITECTURE generic_delay OF and2 IS
BEGIN  
    PROCESS(x,y) IS
    BEGIN
        z <= (x AND y) AFTER delay;
    END PROCESS;
END ARCHITECTURE generic_delay;
 ```

### Aufgabe 7.3 Unit-Delay-Modell für GENERICMAP (GENERIC)

 ```vhdl
LIBRARY ieee;
USE ieee.std_logic_1164.ALL;

ENTITY and3 IS
    PORT(a,b,c:   IN STD_LOGIC;
         o:      OUT STD_LOGIC);
END ENTITY and3;
```
```vhdl
ARCHITECTURE struct OF and3 IS
    COMPONENT and2 IS
        GENERIC(delay : TIME);
        PORT(x,y:   IN STD_LOGIC;
            z:      OUT STD_LOGIC);
    END COMPONENT and2;

    SIGNAL t: STD_LOGIC;
BEGIN  
    G1: and2 GENERIC MAP(2 ns) PORT MAP (a,b,t);
    G2: and2 GENERIC MAP(2 ns) PORT MAP (t,c,o);
END ARCHITECTURE struct;
 ```

 ### Aufgabe 7.4 Lastabhänige Verzögerung

 ```vhdl
ENTITY and2 IS
    GENERIC(tlh_factor, thl_factor : TIME := 0 ns;
            load_value             : REAL := 1.0);
    PORT(x,y: IN BIT;
         z: OUT BIT);
END ENTITY and2;
```
```vhdl
ARCHITECTURE load_delay OF and2 IS
BEGIN
    PROCESS(x,y) IS
        VARIABLE and_new, and_old : BIT := '0';
        CONSTANT base_delay       : TIME := 500 ps;
    BEGIN
        and_new := x AND y;
        IF and_new = '1' AND and_old = '0' THEN          -- steigende Flanke (0 -> 1)
            z <= and_new AFTER (base_delay + tlh_factor * load_value);
        ELSIF and_new = '0' AND and_old = '1' THEN       -- fallende Flanke (1 -> 0)
            z <= and_new AFTER (base_delay + thl_factor * load_value);
        END IF;
        and_old := and_new;   -- aktuellen Zustand fuer den naechsten Aufruf merken
    END PROCESS;
END ARCHITECTURE load_delay;
 ```

 ## Aufgabe 8: Erselle ein D-FlipFlo

 ```VHDL
LIBRARY ieee;
USE ieee.std_logic1164.ALL;

ENTITY dff IS
    PORT(clk, rest, d: IN  STD_LOGIC;
         q    : OUT STD_LOGIC
    );
END ENTITY dff;
```
```vhdl
ARCHITECTURE behaviour OF dff IS
BEGIN
    PROCESS(clk) IS -- NUR clk in der Sensitivity-List!
    BEGIN
        -- Der Prozess reagiert NUR auf die Taktflanke
        IF rising_edge(clk) THEN 
            -- Im Moment der Taktflanke wird zuerst 'rest' geprüft
            IF (rest = '1') THEN
                q <= '1'; -- Synchroner Ausgangszustand (Set)
            ELSE
                q <= d;   -- Normaler D-FF-Betrieb (Wert von d übernehmen)
            END IF;       
        END IF;
    END PROCESS;
END ARCHITECTURE behaviour;
```
```vhdl
ARCHITECTURE delays OF dff IS
BEGIN
    PROCESS(clk) IS
        CONSTANT thl : TIME := 2 ns; -- Verzögerung High-to-Low
        CONSTANT tlh : TIME := 1 ns; -- Verzögerung Low-to-High
        
        -- Wir brauchen zwei Variablen: Eine für den alten Zustand, eine für die Kombination
        VARIABLE state_old : STD_LOGIC := '0'; 
        VARIABLE change    : STD_LOGIC_VECTOR(1 DOWNTOW 0);
    BEGIN
        IF rising_edge(clk) THEN
            -- Wir kombinieren: [Alter Zustand] & [Neuer Wunschzustand d]
            change := state_old & d;
            
            CASE change IS
                WHEN "00" | "11" => 
                    NULL; -- Keine Änderung, tu nichts
                    
                WHEN "10" => 
                    state_old := d;       -- Aktualisiere den internen Zustand
                    q <= d AFTER thl;     -- Schalte verzögert um 2 ns auf '0'
                    
                WHEN "01" => 
                    state_old := d;       -- Aktualisiere den internen Zustand
                    q <= d AFTER tlh;     -- Schalte verzögert um 1 ns auf '1'
                    
                WHEN OTHERS => 
                    NULL; -- Wichtig bei STD_LOGIC für undefinierte Zustände (U, X, Z)
            END CASE;
        END IF;
    END PROCESS;
END ARCHITECTURE delays;
```

# Aufgabe 9: Erstelle einen INT TO STD_LOGIC_VECTOR mit einer Function

### 9.1. Der Algorithmus (Schritt für Schritt)

1. **Eingabe:** Eine Ganzzahl (`value`) und die gewünschte Bitbreite (`width`) des Zielvektors.
2. **Initialisierung:**
    * Lege eine Arbeitskopie der Zahl an (Variable `v`).
    * Erstelle einen leeren Zielvektor (`result`) mit der Länge `width - 1 DOWNTO 0`.
3. **Schleife:** Durchlaufe den Zielvektor von **rechts nach links** (vom niederwertigsten Bit an Position 0 bis zum höchsten Bit).
    * **Schritt A (Rest bestimmen):** Prüfe den Rest der Division durch 2. Wenn die Zahl ungerade ist (Rest = 1), setze das aktuelle Bit auf '1'. Wenn sie gerade ist (Rest = 0), setze das Bit auf '0'.
    * **Schritt B (Zahl verkleinern):** Führe eine Ganzzahldivision durch 2 durch (`v / 2`). Die Nachkommastellen fallen bei Ganzzahlen automatisch weg.
4. **Ausgabe:** Wenn alle Bit-Positionen abgearbeitet sind, gib den fertigen Vektor zurück.

---

### 9.2. Hinweis zur Bitbreite (width)

Mathematisch reichen für die Zahl 5 zwar 3 Bit (5 dezimal = 101 binär), aber in der Digitaltechnik werden Vektoren meist an feste Busbreiten (z. B. 4, 8, 16 Bit) angepasst. Die Funktion füllt fehlende Stellen links einfach automatisch mit Nullen ('0') auf:
* Bei `width = 3` -> "101" (Mindestbreite)
* Bei `width = 4` -> "0101"
* Bei `width = 8` -> "00000101"

---

### 9.3. Beispiel: Umwandlung von value = 10 mit width = 4

Der Zielvektor hat die Indizes 3 DOWNTO 0. Die Schleife läuft rückwärts von Index 0 bis 3:

* **1. Durchlauf (Index 0):**
    * Aktueller Wert: `v = 10` (gerade)
    * Rechnung: 10 MOD 2 = 0 -> **Bit an Position 0 wird '0'**
    * Nächster Wert: `v = 10 / 2 = 5`

* **2. Durchlauf (Index 1):**
    * Aktueller Wert: `v = 5` (ungerade)
    * Rechnung: 5 MOD 2 = 1 -> **Bit an Position 1 wird '1'**
    * Nächster Wert: `v = 5 / 2 = 2`

* **3. Durchlauf (Index 2):**
    * Aktueller Wert: `v = 2` (gerade)
    * Rechnung: 2 MOD 2 = 0 -> **Bit an Position 2 wird '0'**
    * Nächster Wert: `v = 2 / 2 = 1`

* **4. Durchlauf (Index 3):**
    * Aktueller Wert: `v = 1` (ungerade)
    * Rechnung: 1 MOD 2 = 1 -> **Bit an Position 3 wird '1'**
    * Nächster Wert: `v = 1 / 2 = 0`

**Endergebnis für die 10:** "1010"

### 9.4 VHDL-Code
```VHDL
LIBRARY ieee;
USE ieee.std_logic_1164.ALL;

ENTITY conv IS
    PORT (
        x : IN INTEGER;
        z : OUT STD_LOGIC_VECTOR(7 DOWNTO 0)
    );
END ENTITY conv;
```

```VHDL
ARCHITECTURE behavioral OF conv IS
    -- Funktion einheitlich benannt und auf DOWNTO umgestellt
    FUNCTION to_std_logic_vector (value, width : INTEGER) RETURN STD_LOGIC_VECTOR IS
        VARIABLE v      : INTEGER := value;
        VARIABLE result : STD_LOGIC_VECTOR(width - 1 DOWNTO 0); -- Jetzt passend zu OUT-Port z
    BEGIN
        -- Da result nun DOWNTO ist, fängt 'RANGE automatisch beim LSB (0) an zu zählen
        FOR i IN result'RANGE LOOP
            IF v MOD 2 = 1 THEN
                result(i) := '1';
            ELSE
                result(i) := '0';
            END IF;
            
            IF v >= 0 THEN
                v := v / 2; -- Semikolon ergänzt
            ELSE 
                v := (v - 1) / 2; -- crashes if v = -2**31
            END IF; -- Semikolon ergänzt
        END LOOP;
        
        RETURN result;
    END FUNCTION to_std_logic_vector; -- Name korrigiert

BEGIN
    PROCESS (x) IS
    BEGIN
        -- Name korrigiert und Semikolon ergänzt
        z <= to_std_logic_vector(x, 8); 
    END PROCESS;
END ARCHITECTURE behavioral;
```

# Aufgabe 10: Prozedur-Aufrufe und Parameter-Assoziation

Dieses Beispiel zeigt die Definition einer Prozedur innerhalb eines Prozesses sowie die verschiedenen legalen und illegalen Arten, Parameter zu übergeben (Positional, Default und Named Association).

```vhdl
-- Deklaration von Signalen außerhalb des Prozesses
SIGNAL a, b : BIT;
SIGNAL ab   : BIT_VECTOR(1 DOWNTO 0);

PROCESS IS
    -- Definition der Prozedur im deklarativen Teil des Prozesses
    PROCEDURE assign (
        ip    : BIT_VECTOR;          -- Erforderlicher Parameter (kein Default)
        pause : TIME := 10 ns        -- Optionaler Parameter mit Default-Wert
    ) IS
    BEGIN
        a <= ip(0);
        b <= ip(1);
        WAIT FOR pause;
    END PROCEDURE assign;

BEGIN
    -- 1. Positional Association (Zuweisung nach Reihenfolge)
    assign(ab, 5 ns); 

    -- 2. Default Value (Nutzt den Standardwert pause = 10 ns)
    assign(ab); 

    -- 3. ILLEGAL / Geht nicht! (Der Parameter 'ip' hat keinen Default-Wert)
    -- assign(); 

    -- 4. Named Association (Zuweisung explizit über den Namen, Reihenfolge egal)
    assign(pause => 3 ns, ip => ab); 

END PROCESS;
```

# Aufgabe 11: Überladen von Operatoren (Operator Overloading)

Dieses Beispiel zeigt, wie man bestehende VHDL-Standardoperatoren (wie `and` oder `or`) für eigene, benutzerdefinierte Datentypen verfügbar macht und die Logik effizient über eine Nachschlagetabelle (Look-up-Table) löst.

```vhdl
-- Beispiel 1: Deklarations-Beispiele für das Überladen des "or"-Operators
FUNCTION "or" (a, b: BIT) RETURN BIT;
FUNCTION "or" (a, b: BOOLEAN) RETURN BOOLEAN;
FUNCTION "or" (a, b: BIT_VECTOR) RETURN BIT_VECTOR;

-- Beispiel 2: Überladen des "and"-Operators für einen 3-wertigen Typ via Look-up-Table
TYPE tri IS ('0', '1', 'Z');

FUNCTION "and" (left, right: tri) RETURN tri IS
    -- Definition eines 2D-Arrays, dessen Indizes selbst vom Typ 'tri' sind
    TYPE tri_array IS ARRAY (tri, tri) OF tri;
    
    -- Die Wahrheitstabelle (Look-up-Table) für alle 9 Kombinationen
    CONSTANT and_table: tri_array := ( 
        ('0', '0', '0'),  -- wenn left = '0' (für right = '0', '1', 'Z')
        ('0', '1', '1'),  -- wenn left = '1' (für right = '0', '1', 'Z')
        ('0', '1', '1')   -- wenn left = 'Z' (für right = '0', '1', 'Z')
    );
BEGIN
    -- Das Ergebnis wird direkt aus der Tabelle an den Kreuzungspunkten gelesen
    RETURN and_table(left, right);
END FUNCTION "and";