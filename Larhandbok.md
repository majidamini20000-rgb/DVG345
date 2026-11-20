# DVG345 – lärhandbok

## Så använder du materialet

Det här är en förklarande lärhandbok till dina 18 föreläsningar. Läs i kapitelordning när du lär dig från grunden. Om du redan kan ett område, försök med övningarna först och läs sedan det du inte kunde förklara. Målet är att själv kunna följa, skriva och testa kod.

Varje kapitel innehåller mål, stegvisa förklaringar, CAD-exempel och övningar. Facit finns samlat längre bak i PDF:en och dolt efter övningarna i webbversionen. Försök först utan facit. En förklaring du känner igen är inte samma sak som en uppgift du kan lösa själv.

Kod som kallas utdrag förutsätter de typer och imports som anges i texten eller i kursprojektet. De kompletta övningsexemplen i kodmappen är fristående Java 17-exempel. Klasser med samma namn i olika kapitel kan visa olika utvecklingssteg och ska inte klistras in tillsammans som ett enda program.

## Kursens röda tråd

| Steg | Fråga du lär dig besvara | Kapitel |
|---|---|---|
| Giltiga byggstenar | Vad är ett objekt och vilka löften håller det? | 1–3 |
| Samarbeten | Hur når en händelse rätt objekt och sedan vyn? | 4–5 |
| Större strukturer | Hur hanteras del, helhet, identitet och utökning? | 6–8 |
| Kontrollerad redigering | Hur ångras något och vad ska ske vid fel? | 9–11 |
| Användbar applikation | Hur kopplas GUI och beständig lagring in? | 12–13 |
| Helhet | Hur visar och prövar jag hela samarbetet? | 14 |

## Ett arbetssätt för varje pass

1. Återberätta det du lärde dig sist utan att läsa, cirka 5 minuter.
2. Läs en eller två förklaringar och följ kodens värden för hand, cirka 20 minuter.
3. Stäng materialet och lös en övning, cirka 20 minuter.
4. Jämför med facit: skriv varför skillnaden uppstod, inte bara rätt svar.
5. Gör om den svaga uppgiften nästa dag, efter tre dagar och efter en vecka.

Tiderna är ett förslag. Dela ett kapitel på flera pass när det behövs. Använd en enkel skala: 0 = nytt, 1 = känner igen, 2 = kan förklara, 3 = kan använda och testa. I webbversionen kan du markera avklarade kapitel; lagringen sker lokalt i webbläsaren när den stöder det.

## Rekommenderad passplan

| Pass | Läsning | Något du ska producera |
|---|---|---|
| 1 | Kapitel 1 | Referensbild och förklaring av identitet |
| 2 | Kapitel 2 | Validerad Point2/Circle och tre kontraktstester |
| 3 | Kapitel 3 | Förklaring av statisk typ och dynamiskt metodval |
| 4 | Kapitel 4 | Lambda-exempel och bild av delade listelement |
| 5–6 | Kapitel 5 | Sekvens för musgest, cancel och notifiering |
| 7 | Kapitel 6 | Nästlat träd och rotation kring gemensam pivot |
| 8–9 | Kapitel 7 | Gruppering/uppdelning och wildcard-resonemang |
| 10 | Kapitel 8 | Ett designbeslut motiverat med SOLID |
| 11–12 | Kapitel 9 | Båda stackarna genom en hel redigeringssekvens |
| 13 | Kapitel 10 | Feltest som kontrollerar oförändrat tillstånd |
| 14 | Kapitel 11 | Jämförelse mellan invers och sparat tillstånd |
| 15–16 | Kapitel 12 | Körbart litet Swing-GUI |
| 17 | Kapitel 13 | JSON-träd och plan för säkert öppna/spara |
| 18 | Kapitel 14 | Slutövningen och egna diagram |

## Skillnader mellan kursens utvecklingssteg

- I F2 kan Figure ritas och ge centrum. Senare tillkommer move, rotate och parts. Läs alltid det kontrakt som gäller för exemplet.
- I F6 flyttar MoveTool direkt under dragningen. I F15 visas bara preview tills släppet skapar ett kommando. De två varianterna får inte blandas.
- I F13 används inverser för flyttning och rotation. F14:s Memento är ett alternativt teoriresonemang och är uttryckligen inte ett krav i labb 4 eller projektet.
- List.copyOf skyddar liststruktur, inte muterbara figurers geometri. Detta återkommer i grupper, generics och historik.
- Repaint begär senare målning. En modellnotifiering, en repaint-begäran och ett paintComponent-anrop är olika saker.
- F17 nämner F19 och F20. De ingick inte i dina filer och har inte behandlats som tillgängligt kursmaterial.

## Underlag och läsanvisningar

Handbokens underlag är DVG345-F1.pdf till DVG345-F18.pdf. Hänvisningar som F10 s. 5 avser den femte PDF-sidan räknat från 1, även om en bild saknar tryckt sidnummer. Förklaringar, nya exempel, övningar och passplan är bearbetat studiematerial. Det är inga nya inlämningskrav och ingen prognos om tentans innehåll. Canvas, föreläsningarnas ZIP-exempel och kursens givna projekt/tester ingick inte som filer.


# Kapitel 1. Objekt, referenser och ansvar

**Bygger på:** F1, PDF-sidor 3–7 och 15–23; F4, sidor 2–13. Läs denna del före konstruktorer och polymorfism.

**Efter delen ska du kunna:** rita vilka variabler som pekar på vilka objekt, skilja objektets identitet från dess värden och förklara varför privata fält bara är en del av inkapslingen.

## Vad försöker objektorientering lösa?

I ett CAD-program finns figurer, ett dokument, en markering och verktyg. Programmet blir lättare att förstå om dessa begrepp får tydliga ansvar. En cirkel känner sin geometri. Dokumentet håller reda på vilka figurer som ingår och i vilken ordning de ritas. Markeringen håller reda på vilka befintliga figurer användaren har valt. Ett verktyg tolkar användarens musgest.

En **klass** beskriver en sorts objekt. Ett **objekt** är en bestämd instans som skapas under körning. Två cirklar hör till samma klass men kan vara två olika objekt.

Tre frågor beskriver ett objekt:

| Fråga | Begrepp | Exempel för en cirkel |
|---|---|---|
| Vad vet objektet just nu? | Tillstånd | Centrum och radie |
| Vad kan klienten be objektet göra? | Beteende | Rita sig, svara på centrum, senare flytta sig |
| Vilket enskilt objekt är detta? | Identitet | Just den cirkel som användaren har markerat |

Med **klient** menas kod som använder objektet. Ett flyttverktyg är klient till en figur när det anropar `figure.move(...)`.

## Variabeln är inte objektet

En variabel av referenstyp innehåller en referens till ett objekt, eller `null`. Att tilldela en referens till en annan variabel skapar inte en kopia av objektet.

```java
Point2 origin = new Point2(0, 0);
Circle a = new Circle(origin, 5);
Circle b = a;
Circle c = new Circle(origin, 5);
```

Följ instruktionerna en i taget:

1. `new Point2(0, 0)` skapar ett punktobjekt. `origin` hänvisar till det.
2. Första `new Circle(...)` skapar cirkel A. `a` hänvisar till A.
3. `b = a` kopierar referensvärdet. Både `a` och `b` hänvisar nu till A.
4. Andra `new Circle(...)` skapar cirkel C. `c` hänvisar till C.

```text
a ──┐
    ├──> cirkel A: center ──┐   radius = 5
b ──┘                      │
                           ├──> punkt P: (0, 0)
c ─────> cirkel C: center ──┘   radius = 5
```

Det finns två cirkelobjekt men tre variabler som refererar till cirklar. Att de båda cirklarna använder samma oföränderliga punktvärde gör dem inte till samma cirkel.

När två referenser når samma objekt kallas det **alias**. Om en senare version av Circle har `move`, syns en flytt via `b` också när vi frågar via `a`: båda vägarna når samma cirkel.

`b = null` bryter bara kopplingen från `b`. Cirkeln finns fortfarande tillgänglig genom `a`. Att sätta `a = null` också gör inte automatiskt att cirkeln försvinner ur dokumentet; dokumentet kan ha en egen referens. Om inget som programmet kan nå längre kan nå objektet kan det bli föremål för skräpinsamling. Exakt när minnet återtas bestämmer inte dessa tilldelningar.

**Kopplingen till CAD:** avmarkera betyder att ta bort referenser ur markeringen. Radera betyder att ta bort figuren ur dokumentets struktur. Det är två olika operationer, även om figuren slutar vara markerad efter båda.

## Identitet och värdelikhet

För referenstyper frågar `==` om två referenser pekar på samma objekt eller båda är `null`. `equals` anropar en metod som klassens kontrakt definierar. För kursens Point2-record jämför `equals` komponenternas värden.

```java
Point2 p = new Point2(2, 3);
Point2 q = new Point2(2, 3);
Point2 r = p;

System.out.println(p == q);      // false
System.out.println(p.equals(q)); // true
System.out.println(p == r);      // true
```

`p` och `q` beskriver samma position men är två objekt. `p` och `r` är två vägar till samma objekt.

Det är värdet vi vill kontrollera när vi testar en beräknad mittpunkt. Det är identiteten vi behöver kontrollera när en gruppering ska bevara just de ursprungliga figurerna. Regeln är därför inte att `equals` alltid är bättre än `==`. Börja med att fråga vad testet ska avgöra.

En vanlig klass får inte automatiskt samma värdelikhet som en record. Om klassen inte åsidosätter `Object.equals`, jämför dess ärvda implementation identitet.

## Java skickar alltid argument som värden

När vi lämnar en referens som argument kopierar Java referensvärdet till parametern. Parametern blir en ny variabel som från början pekar på samma objekt.

```java
static void replace(Point2 point) {
    point = new Point2(9, 9);
}

Point2 p = new Point2(2, 3);
replace(p);
System.out.println(p); // Point2[x=2.0, y=3.0]
```

Inne i `replace` byter bara den lokala parametern mål. Den anropande variabeln `p` ändras inte. Detta är **pass by value**, även för referenstyper.

Om parametern i stället når ett muterbart objekt kan metoden ändra det gemensamma objektet. I en senare labbversion kan därför `static void nudge(Figure f) { f.move(1, 0); }` flytta figuren som anroparen också når. Skillnaden är mellan att tilldela parametervariabeln en ny referens och att anropa en ändrande metod på objektet.

## Abstraktion och inkapsling

**Abstraktion** beskriver vad klienten behöver kunna göra. Klienten behöver kunna fråga efter en linjes centrum; klienten behöver inte veta om det lagras eller räknas ut.

**Representation** är hur klassen faktiskt lagrar och beräknar sitt tillstånd. En linje kan ha två ändpunkter. Dess centrum är då ett **härlett attribut**: en egenskap som beräknas från annan information. Ett extra centrumfält skulle behöva uppdateras korrekt vid varje förändring. Genom att beräkna mittpunkten undviker klassen två versioner av samma information.

**Inkapsling** betyder att klassen kontrollerar hur representationen kan användas och förändras. Privata fält hjälper, men en metod som lämnar ut en intern muterbar lista kan ändå ge klienten en väg runt klassens regler. Granska därför både fältens åtkomst och vad publika metoder lämnar ut.

| Begrepp | Konkret betydelse |
|---|---|
| Fält/instansvariabel | Hör till objektet och finns kvar mellan metodanrop |
| Parameter | Tar emot ett argument för det aktuella anropet |
| Lokal variabel | Används inne i ett visst kodblock eller metodanrop |
| `this.center` | Fältet `center` i det aktuella objektet |
| `static` medlem | Hör till klassen snarare än till varje enskild instans |

## Ansvar, kohesion och koppling

**Kohesion** handlar om hur väl en klass uppgifter hör ihop. Cirkelns radievalidering och cirkelns centrum hör till cirkelgeometrin. Att bestämma vilken dialogruta som ska visa ett fel hör till gränssnittet.

**Koppling** beskriver vilka andra delar en klass måste känna till. Om Circle måste importera en viss GUI-dialog för att avvisa en radie blir geometrin beroende av hur ett fönster ser ut. Det försvårar både återanvändning och tester utan GUI.

En tydlig uppdelning är: Circle avvisar ogiltig radie med ett undantag; gränssnittet väljer hur felet presenteras. Detta förbereder principen SRP, som senare förklaras mer utförligt i F12.

## Övningar

**Övning 1A – spåra referenser, grundnivå.** Utgå från att `move` finns i den senare Circle-versionen. Kör koden för hand. Hur många cirkelobjekt skapas? Vilka centrum har de cirklar som nås genom `a` respektive `c` efter flytten? Vad gör sista raden med cirkeln?

```java
Point2 p = new Point2(1, 2);
Circle a = new Circle(p, 3);
Circle b = a;
Circle c = new Circle(p, 3);
b.move(4, -1);
b = null;
```

**Övning 1B – identitet, grundnivå.** Förutsäg resultaten och motivera varje rad:

```java
Point2 p = new Point2(3, 4);
Point2 q = new Point2(3, 4);
Point2 r = q;
System.out.println(p == q);
System.out.println(p.equals(q));
System.out.println(q == r);
q = new Point2(0, 0);
System.out.println(r.equals(new Point2(3, 4)));
```

**Övning 1C – anropsförståelse, mellannivå.** Varför ändrar denna metod inte anroparens variabel? Skriv om metoden och anropet så att `p` efteråt hänvisar till punkten `(9,9)`.

```java
static void replace(Point2 value) {
    value = new Point2(9, 9);
}
Point2 p = new Point2(2, 3);
replace(p);
```

**Övning 1D – ansvar, mellannivå.** Du ser en Circle-klass som räknar area, validerar radie och öppnar en Swing-dialog när radien är fel. Vilket ansvar bör flyttas, till vilken del och varför? Förklara även varför avmarkera och radera behöver skiljas åt.

# Kapitel 2. Giltiga objekt, Point2 och kontraktstester

**Bygger på:** F2, PDF-sidor 15–16; F3, sidor 2–21; F4, sidor 3–12 och 14–24.

**Efter delen ska du kunna:** konstruera ett giltigt objekt, beskriva dess invariant, skilja `final` från oföränderlighet och välja tester för normala fall, gränser och fel.

## En invariant gäller hela objektet

En **invariant** är ett villkor som ett färdigkonstruerat objekt ska uppfylla och som dess tillåtna operationer ska bevara. För en cirkel i kursens modell gäller bland annat:

- Centrum finns, alltså är referensen inte `null`.
- Centrumets koordinater är ändliga.
- Radien är positiv och ändlig.

Ett ändligt `double` är varken NaN eller positiv/negativ oändlighet. `radius > 0` är inte ensam en fullständig radiekontroll: positiv oändlighet är större än noll. Därför behövs också `Double.isFinite(radius)`.

Konstruktorn etablerar invarianten. Privata fält begränsar vägarna som kan ändra den. Varje senare ändrande metod måste kontrollera att nästa tillstånd fortfarande är giltigt. En konstruktor som gör rätt kompenserar inte för en setter som accepterar ogiltiga värden.

## Point2 är ett värde

Här är en liten egen undervisningsversion i Java 17. Den är till för resonemangen och ersätter inte kursens givna API.

```java
import java.util.Objects;

public record Point2(double x, double y) {
    public Point2 {
        if (!Double.isFinite(x) || !Double.isFinite(y)) {
            throw new IllegalArgumentException("Koordinater måste vara ändliga");
        }
    }

    public Point2 midpoint(Point2 other) {
        Objects.requireNonNull(other, "other");
        return new Point2(x / 2.0 + other.x / 2.0,
                          y / 2.0 + other.y / 2.0);
    }
}
```

Recordens komponenter `x` och `y` ger privata final-fält och accessorerna `x()` och `y()`. Den får också konstruktor, `equals`, `hashCode` och `toString` utifrån komponenterna. Den kompakta konstruktorn ovan lägger till validering innan komponentvärdena lagras.

`midpoint` ändrar ingen punkt. Den beräknar och returnerar ett nytt värde. För `(2,4)` och `(8,10)` blir det `(5,7)`. Division före addition minskar risken att summan av mycket stora ändliga koordinater först blir oändlig. Exemplet är inte ett löfte om exakt aritmetik för alla flyttalsvärden; beräknade `double`-värden kan behöva tolerans i tester.

En figur kan flytta sig trots att punkter är oföränderliga. Figuren byter sin egen centrumreferens från ett gammalt punktvärde till ett nytt. Den ändrar inte koordinaterna i det gamla Point2-objektet. Två figurer kan därför säkert dela ett Point2-värde utan att den enas flytt muterar den andras punkt.

## `final`, immutable och records

`final` beskriver olika begränsningar beroende på var ordet används:

| Användning | Begränsning |
|---|---|
| `final` variabel/fält | Får inte tilldelas på nytt efter sin tillåtna initialisering |
| `final` metod | Kan inte åsidosättas i en subklass |
| `final` klass | Kan inte ha subklasser |

**Immutable** betyder att objektets observerbara tillstånd inte förändras efter konstruktion. Detta är en egenskap hos objektets konstruktion och API, inte en effekt som automatiskt följer av ett enda nyckelord.

```java
final java.util.List<String> names = new java.util.ArrayList<>();
names.add("cirkel"); // tillåtet: listobjektet ändras
// names = new java.util.ArrayList<>(); // inte tillåtet
```

Records ger inte automatiskt djup oföränderlighet:

```java
record Tags(java.util.List<String> values) {}

var input = new java.util.ArrayList<String>();
var tags = new Tags(input);
input.add("ny");
System.out.println(tags.values()); // [ny]
```

Recorden behåller samma listreferens, men listobjektet kan ändras. En undervisningsversion med `values = List.copyOf(values)` i den kompakta konstruktorn skyddar listans struktur. Om elementen själva är muterbara krävs dessutom eftertanke om dem. Point2 är enklare eftersom dess komponenter är primitiva `double`-värden.

## Läs en konstruktor rad för rad

```java
import java.util.Objects;

public final class Circle {
    private final Point2 center;
    private final double radius;

    public Circle(Point2 center, double radius) {
        this.center = Objects.requireNonNull(center, "center");
        if (!Double.isFinite(radius) || radius <= 0) {
            throw new IllegalArgumentException("Radie måste vara positiv och ändlig");
        }
        this.radius = radius;
    }

    public Point2 getCenter() { return center; }
    public double getRadius() { return radius; }
}
```

Detta är en avsiktligt oföränderlig version för del 2. Senare behöver en flyttbar Circle andra fältdeklarationer eller annan representation; försök inte tilldela ett nytt centrum till detta final-fält.

`this.center` är fältet och `center` är parametern. `Objects.requireNonNull` returnerar samma referens om den finns och kastar annars `NullPointerException`. När ett giltigt Point2 redan garanterar ändliga koordinater behöver Circle inte läsa och kontrollera dem en gång till.

Om radien är ogiltig kastar konstruktorn ett undantag. Anroparens `new Circle(...)` ger då ingen normal retur med en referens till en färdig cirkel. Det betyder inte att `new` returnerar `null`.

Ordningen är betydelsefull för testet: om både centrum är `null` och radien är negativ kommer denna implementation först till null-kontrollen. Testa därför en felorsak i taget när du vill verifiera en viss regel.

## Förvillkor, eftervillkor och felkontrakt

Ett metodnamn och en parameterlista beskriver inte hela löftet.

| Del av kontraktet | Circle-exempel |
|---|---|
| Förvillkor | Centrum finns; radien är positiv och ändlig |
| Eftervillkor vid lyckad konstruktion | Objektet har de angivna giltiga geometrivärdena |
| Felkontrakt | Null eller ogiltiga mått avvisas med avsedda undantag |
| Frågas sidoeffekter | `getCenter()` ändrar inte geometrin |

Ett test av returvärdet kontrollerar inte automatiskt frånvaron av sidoeffekter. Fråga därför efter centrum flera gånger och kontrollera även det tillstånd som ska vara kvar.

## Konstruktordelegering och initialiseringsordning

Överlagrade konstruktorer har samma klassnamn men olika parameterlistor. `this(...)` låter en konstruktor återanvända en annan konstruktor i samma klass, så att validering inte behöver dupliceras.

```java
final class Radius {
    private final double value;

    Radius() { this(10); }

    Radius(double value) {
        if (!Double.isFinite(value) || value <= 0) {
            throw new IllegalArgumentException("Radie");
        }
        this.value = value;
    }
}
```

`super(...)` väljer basklassens konstruktor. Det skapar inte ett separat basklassobjekt vid sidan av subklassobjektet. Objektet är en instans av subklassen och innehåller även den ärvda delens tillstånd.

I den vanliga Java 17-kedjan får instansfälten först standardvärden. Basklassens fältinitialiserare och konstruktorkropp körs före subklassens fältinitialiserare och konstruktorkropp. Vid `this(...)` måste du dessutom följa delegationen till den konstruktor som fortsätter kedjan mot basklassen.

Undvik anrop till metoder som subklasser kan åsidosätta från en konstruktor. Det dynamiska metodvalet kan nå subklassens implementation innan subklassens fält har fått sina uttryckliga initialvärden. Se också övning 3D.

## Tester från kontraktet

Utgå från vad en klient kan observera. Kursen använder JUnit 5; följ projektets givna paket och beroenden. Ett exempel på testmetod:

```java
@Test
void midpointReturnsNewValueWithoutChangingInputs() {
    Point2 p = new Point2(2, 4);
    Point2 q = new Point2(8, 10);

    Point2 result = p.midpoint(q);

    assertEquals(new Point2(5, 7), result);
    assertEquals(new Point2(2, 4), p);
    assertEquals(new Point2(8, 10), q);
}
```

Lägg till `import org.junit.jupiter.api.Test;` och `import static org.junit.jupiter.api.Assertions.*;` i en testklass. I `assertEquals` ska **förväntat** värde stå först och **faktiskt** värde sedan. Då blir felmeddelandena lättare att tolka.

```java
assertThrows(IllegalArgumentException.class,
             () -> new Circle(new Point2(0, 0), -1));
assertEquals(0.15, new Point2(0.1, 0)
    .midpoint(new Point2(0.2, 0)).x(), 1e-12);
```

`()` och pilen beskriver en handling som JUnit själv får köra inne i `assertThrows`. Om konstruktionen inte kastar avsett undantag misslyckas testet. Toleransen i det andra testet tillåter ett litet flyttalsfel; välj tolerans efter beräkning och kontrakt.

## Övningar

**Övning 2A – validering, grundnivå.** Med Circle-konstruktorn ovan: vilka indata godtas? För avvisade fall, ange undantagstyp. Använd ett giltigt Point2 utom i sista raden.

| Fall | Radie/centrum |
|---|---|
| a | Radie 5 |
| b | Radie 0 |
| c | Radie -2 |
| d | Radie `Double.NaN` |
| e | Radie `Double.POSITIVE_INFINITY` |
| f | Centrum `null`, radie 5 |

**Övning 2B – skydda ett värde, mellannivå.** Varför är `record Tags(List<String> values) {}` inte tillräcklig om en Tags ska behålla samma taggar? Skriv en version som tar en strukturell kopia och förklara vad den kopian skyddar.

**Övning 2C – skriv ett kontraktstest, mellannivå.** Skriv två separata JUnit-testmetoder: en som kontrollerar mittpunkten mellan `(2,4)` och `(8,10)` samt oförändrade argument, och en som kontrollerar att Point2 avvisar en NaN-koordinat. Förklara vilket fel respektive test kan upptäcka.

**Övning 2D – skapa ett säkert ändrings-API, fördjupning.** Anta att en annan, muterbar Circle-version har ett icke-finalt `radius`-fält. Skriv `setRadius(double radius)` så att en avvisad ändring lämnar det gamla värdet kvar. Beskriv hur du skulle testa det med `getRadius()`.

# Kapitel 3. Interface, arv och dynamisk bindning

**Bygger på:** F2, PDF-sidor 2–14 och 17–20; F3, sidor 11–19; F5, sidor 2–18.

**Efter delen ska du kunna:** välja en referenstyp efter klientens behov, skilja överlagring från override och avgöra när koddelning via en basklass faktiskt passar geometrin.

## Ett interface anger ett löfte

F2 börjar med ett litet ritkontrakt:

```java
public interface Drawable {
    void draw(DrawingContext context);
}

public interface Figure extends Drawable {
    Point2 getCenter();
}
```

En Drawable lovar att kunna ritas. En Figure lovar dessutom att kunna svara på sitt centrum. `extends` mellan interface innebär att det mer specifika kontraktet inkluderar det andra.

En klass som `implements Figure` måste ge de konkreta implementationer som inte redan är tillgängliga genom giltigt arv eller standardmetoder. Interfacets abstrakta instansmetoder är publika; implementerande metoder måste därför också vara `public`.

`new Figure()` fungerar inte direkt: ett interface är inget färdigt recept på ett konkret objekt. Däremot kan en Figure-variabel hänvisa till ett objekt av en klass som implementerar Figure.

I senare labbsteg utökas kontraktet:

```java
public interface Movable {
    void move(double dx, double dy);
}

public interface Figure extends Drawable, Movable {
    Point2 getCenter();
}
```

Det är en utveckling av modellen. Om en gammal konkret figurklass ännu saknar `move` blir den ofullständig i förhållande till det utökade kontraktet. Läs alltså alltid vilket labbsteg exemplet gäller.

## Två typer måste hållas isär

```java
Line line = new Line(new Point2(0, 0), new Point2(10, 0));
Figure f = line;
Movable m = f;
Object o = f;
```

Det finns fortfarande ett enda Line-objekt. Inga objekt kopieras eller omvandlas när vi skapar de nya referenserna.

| Referens | Deklarerad typ | Klienten får bland annat anropa |
|---|---|---|
| `line` | Line | Line-API och ärvda tillgängliga metoder |
| `f` | Figure | `draw`, `move`, `getCenter` |
| `m` | Movable | `move`, men inte `getCenter` genom denna typ |
| `o` | Object | Exempelvis `toString`, men inte `move` genom denna typ |

Kompilatorn använder uttryckets deklarerade/statiska typ för att avgöra om ett anrop är tillåtet. Ett objekt blir därför inte mindre kapabelt av att en referens har en liten typ; klienten har bara ett mindre kontrakt att använda genom just den referensen.

Detta ger en praktisk designregel: välj ett kontrakt som räcker för klientens uppgift. En renderingsmetod behöver ofta bara Drawable. Ett flyttverktyg behöver Movable. En klient som både behöver centrum och flyttning kan behöva Figure.

## Polymorfism: samma anrop, olika arbete

```java
static void nudge(Movable target) {
    target.move(1, 0);
}
```

När vi lämnar en Circle väljs cirkelns implementation av `move`; när vi lämnar en Line väljs linjens implementation. Circle kan behöva byta ett centrumvärde. Line måste flytta båda ändpunkterna. Klientmetoden behöver inte känna till skillnaden i representation.

Detta kallas **dynamisk bindning** för de aktuella instansmetoderna: när en tillåten signatur väl är vald avgör objektets klass vilken override-implementation som körs. Skriv därför inte automatiskt en kedja med `instanceof Circle`, `instanceof Line` och så vidare för varje gemensam operation. Om beteendet redan finns i kontraktet kan objektet utföra det själv.

När en figur ritas är samarbetet på samma sätt: klienten anropar `figure.draw(context)`, figurens implementation väljs och figuren anropar ritporten med sin geometri. Ritningen ska lämna figurens tillstånd oförändrat. DrawingContext är gränsen mot själva rittekniken.

## Överlagring och override är olika mekanismer

**Överlagring** innebär samma metodnamn men olika parameterlistor. Kompilatorn väljer vilken signatur som gäller utifrån argumentuttryckens typer. Bara olika returtyp räcker inte för att skapa en ny överlagring.

**Override**, att åsidosätta en instansmetod, innebär en ny implementation av en ärvd metod med motsvarande signatur. `@Override` låter kompilatorn kontrollera att detta faktiskt sker. Ett felskrivet namn eller en fel parameterlista ska inte tyst bli en annan metod.

Följ detta exempel i två steg:

```java
class Reporter {
    String describe(Object value) { return "objekt"; }
    String describe(Movable value) { return "flyttbar"; }
}

class FigureReporter extends Reporter {
    @Override
    String describe(Movable value) { return "flyttbar figur"; }
}

Reporter reporter = new FigureReporter();
Object object = line;
Movable movable = line;
System.out.println(reporter.describe(object));
System.out.println(reporter.describe(movable));
```

Första anropet har ett argument med typen Object. Kompilatorn väljer `describe(Object)`. Det finns ingen ny override av just den signaturen i FigureReporter, så resultatet är `objekt`.

Andra anropets argument har typen Movable. Kompilatorn väljer `describe(Movable)`. Mottagarobjektet är en FigureReporter, så dess override körs: `flyttbar figur`.

Objektet som argumentet pekar på är samma Line i båda fallen. Skillnaden börjar med argumentuttryckets statiska typ. Mottagarobjektets klass avgör därefter implementationen av den valda instansmetoden.

## Abstrakt klass: koddelning med antaganden

En abstrakt klass kan ha fält, konstruktorer och både konkreta och abstrakta metoder. Den går inte att instansiera direkt. En konkret subklass behöver fullgöra de återstående abstrakta löftena.

En möjlig AbstractFigure kan lagra centrum och implementera `getCenter` och `move`. Men en sådan implementation bygger på ett antagande: att centrumfältet räcker för att beskriva den del av geometrin som ska flyttas. Det kan passa en viss cirkelrepresentation. Det räcker inte för en linje som ritas från två oförändrade ändpunktsfält. Att bara ändra ett extra centrumfält skulle då göra att frågan efter centrum och den ritade geometrin motsäger varandra.

Återanvänd därför kod först när klasserna delar rätt beteendemässiga och representativa antaganden. Att flera klasser har en metod med samma namn räcker inte som motivering för en gemensam basklass.

En klass kan i Java ärva från en klass men implementera flera interface. Interface hjälper alltså olika klasshierarkier att uppfylla gemensamma förmågor utan krav på samma lagrade representation.

## Har-en, är-en och åtkomst

En Circle **har** ett centrum som är en Point2. Det gör inte cirkeln till en punkt. Att skriva `Circle extends Point2` skulle dessutom inte fungera för kursens record, eftersom records är finala. Den mer grundläggande modellfrågan är vilka klientlöften arvet skulle påstå: ska en cirkel verkligen kunna användas överallt där en punkt förväntas?

`private` håller representationen inom klassen. `public` anger den offentliga ytan. `protected` kan användas för avsedda utökningspunkter; i Java ger det också paketåtkomst, så det betyder inte enbart ”subklasser”. En subklass ska inte behöva godtycklig direkt åtkomst till alla fält för att kunna fungera.

En cast skapar inte en ny figur och byter inte objektets klass. Den försöker behandla en befintlig referens som en mer specifik typ. Om objektet inte har den typen kan casten misslyckas med `ClassCastException`. En typkontroll kan vara motiverad för en verkligt specialiserad operation, men återkommande kontroller för samma gemensamma förmåga kan vara en signal om ett saknat eller dåligt använt kontrakt.

## Övningar

**Övning 3A – vad kompilerar, grundnivå?** Anta den senare Figure-versionen som utökar Drawable och Movable samt en giltig `DrawingContext context`. För varje rad, ange om den kompilerar och varför.

```java
Figure f = new Circle(new Point2(0, 0), 5);
Movable m = f;
Drawable d = f;
m.move(2, 0);       // a
m.getCenter();     // b
d.draw(context);   // c
d.move(2, 0);      // d
f.getCenter();     // e
```

Här avses kursens senare flyttbara Circle, inte den avsiktligt oföränderliga undervisningsversionen i del 2.

**Övning 3B – välj metod, mellannivå.** Utgå från Reporter/FigureReporter ovan. Vilka två strängar skrivs ut? Varför gör inte Lines klass att första anropet väljer `describe(Movable)`?

**Övning 3C – skriv en klient, mellannivå.** Skriv en metod `moveRight` som tar en Movable och flyttar den 10 enheter åt höger. Beskriv varför metoden kan användas för både Line och Circle utan typkontroll. Vilken parametertyp av de två, Drawable eller Movable, passar en metod som enbart ritar?

**Övning 3D – konstruktorfälla, fördjupning.** Förutsäg utskriften och förklara initialiseringsordningen:

```java
class Base {
    Base() { System.out.println(value()); }
    int value() { return -1; }
}
class Child extends Base {
    private int n = 7;
    @Override int value() { return n; }
}
// Kör: new Child();
```

# Kapitel 4. Samlingar och lambda: lagra objekt och skicka beteenden

**Bygger på:** F7, PDF-sidor 2–18. För lyssnare fortsätter vi i F8, sidor 3–9. **Förkunskap:** referenser, interface och metoder. **Mål:** kunna följa vad en lista innehåller och när ett sparat beteende faktiskt körs.

## En lista håller referenser i en bestämd ordning

Ett ritprogram behöver komma ihåg flera figurer. I stället för separata variabler för varje cirkel använder vi en samling:

```java
List<Figure> figures = new ArrayList<>();
figures.add(new Circle(new Point2(20, 20), 5));
figures.add(new Line(new Point2(0, 0), new Point2(10, 0)));
```

`List<Figure>` är variabelns typ. Den anger vilket samlingskontrakt klienten får använda och att elementen ska kunna behandlas som `Figure`. `ArrayList` är implementationen vi väljer. En cirkel och en linje kan finnas i samma lista därför att båda uppfyller Figure-kontraktet. Generics-parametern `<Figure>` betyder inte att objekten blir nya objekt av klassen Figure.

Ordningen är betydelsefull. Om ritytan ritar från första till sista elementet hamnar den sist ritade figuren över tidigare figurer där de överlappar. Därför kan ordningen i listan också vara dokumentets ritordning. En samling är alltså inte bara en behållare; dess kontrakt kan bära en del av programmets betydelse. Se F7, sidor 2–3.

```java
for (Figure figure : figures) {
    figure.move(10, 0);
}
```

I varje varv innehåller den lokala variabeln `figure` en referens till ett element. Loopen kopierar inte figurerna. När `move` ändrar objektet är det samma figur som dokumentet redan känner till. Om två olika listor innehåller samma figurreferens ser båda den ändrade geometrin. F7, sida 4.

## Listans struktur och elementens tillstånd är två olika saker

```java
Circle circle = new Circle(new Point2(0, 0), 2);
List<Figure> original = new ArrayList<>();
original.add(circle);
List<Figure> copy = List.copyOf(original);

original.add(new Circle(new Point2(10, 0), 2));
copy.get(0).move(3, 0);
```

Efteråt har `original` två element medan `copy` fortfarande har ett. Liststrukturerna är skilda. Båda når däremot samma första cirkel, som nu har centrum `(3, 0)`. `copy.add(...)` är inte tillåtet, men det innebär inte att `copy.get(0).move(...)` är förbjudet. Skyddet gäller vilka referenser listan innehåller och deras ordning, inte varje refererat objekts inre tillstånd.

Det här kallas ofta en ytlig kopia: vi får en skyddad liststruktur med delade element. Den är användbar när dokumentet ska kunna lämna ut sina figurer utan att klienten kan lägga in eller ta bort listposter direkt. Den är däremot otillräcklig som en stabil historisk ögonblicksbild av muterbara figurer. F7, sida 5; kopplingen till historik behandlas i F14.

## Ett funktionellt interface beskriver en form på ett beteende

Vanliga variabler kan hålla data som tal eller figurreferenser. En variabel kan också referera till ett objekt vars uppgift är att utföra en handling. Ett funktionellt interface har en enda abstrakt metod som bestämmer handlingens form.

| Typ | Abstrakt metod | Argument | Resultat | Typisk fråga |
|---|---|---|---|---|
| `Runnable` | `run()` | Inga | Inget | Vad ska göras senare? |
| `Predicate<T>` | `test(T value)` | Ett värde | `boolean` | Uppfyller värdet ett villkor? |
| `Consumer<T>` | `accept(T value)` | Ett värde | Inget | Vad ska göras med värdet? |
| JUnits `Executable` | `execute()` | Inga | Inget; får kasta | Vilken handling ska testas? |

Tabellen beskriver den förenklade kärnan i föreläsningarnas exempel. De inbyggda typerna importeras från Java respektive JUnit. Definiera inte egna kopior av dem i projektet. F7, sidor 6–11 och 16.

En namngiven klass kan implementera Runnable:

```java
final class PrintChange implements Runnable {
    @Override
    public void run() {
        System.out.println("Ändrat");
    }
}

Runnable action = new PrintChange();
action.run();
```

En lambda beskriver samma sorts implementation mer kortfattat:

```java
Runnable action = () -> System.out.println("Ändrat");
```

Läs `()` som ”inga argument”, pilen som ”utför detta” och uttrycket efter pilen som metodkroppen. Tilldelningen skriver inte ut texten. Det sker när någon anropar `action.run()`. Den skillnaden är grunden för både händelselyssnare och tester.

```java
Predicate<Integer> odd = n -> n % 2 != 0;
boolean a = odd.test(3); // true
boolean b = odd.test(4); // false

Consumer<Figure> moveRight = f -> f.move(10, 0);
figures.forEach(moveRight);
```

`forEach` lämnar varje figurreferens som argument till beteendet. Det motsvarar en vanlig loop som anropar `move(10, 0)` på varje figur. En lambda gör inte flyttningen mer atomär: om den tredje figuren kastar ett undantag kan tidigare figurer redan ha flyttats. Behövs ”allt eller inget” måste det skyddet planeras separat.

## Metodreferenser och fångade lokala värden

När lambdan bara lämnar vidare till en befintlig metod kan en metodreferens bli tydlig:

```java
Runnable first = () -> panel.repaint();
Runnable second = panel::repaint;
```

Båda beskriver en handling utan argument. Ingen av raderna utför målningen direkt. Det behövs fortfarande ett senare `run()`; `repaint` begär dessutom att Swing målar senare.

Signaturen måste passa. `figures.forEach(Movable::move)` ger inte automatiskt `dx` och `dy` till `move`. `forEach` har bara en figur att lämna vidare. Därför är `f -> f.move(10, 0)` rätt form i detta exempel. F7, sidor 13–14.

```java
double dx = 10;
Consumer<Figure> action = f -> f.move(dx, 0);
// dx = 20; // skulle göra fångsten ogiltig
```

Den lokala variabel som lambdan fångar måste vara `final` eller effektivt final: den får inte tilldelas på nytt efter sin initialisering. Det betyder inte att alla objekt som en fångad referens når blir oföränderliga. En fångad lista kan exempelvis fortfarande ha ändringsbara element. F7, sida 15.

## Genomgånget exempel: varför assertThrows tar en lambda

```java
assertThrows(IllegalArgumentException.class,
    () -> new Circle(new Point2(0, 0), -1));
```

Förloppet är:

1. Testet skapar en beskrivning av handlingen genom lambdan.
2. `assertThrows` tar emot den handlingen.
3. JUnit anropar handlingens `execute()` i sin egen kontroll.
4. Circle-konstruktorn körs och avvisar radien.
5. JUnit jämför undantagets typ med den förväntade typen.

Om konstruktionen körs före `assertThrows` får JUnit inte möjlighet att fånga den som den handling som ska testas. Poängen är alltså inte att göra undantaget ofarligt, utan att placera körningen inne i testets kontroll. F7, sidor 16–17.

## Vanliga misstag

- Att säga att en foreach-loop skapar kopior av figurerna. Den ger en referens åt gången.
- Att säga att `List.copyOf` fryser figurernas geometri. Det är liststrukturen som skyddas.
- Att förvänta sig ett metodanrop när en lambda registreras. Det måste finnas någon som senare anropar beteendet.
- Att använda metodreferens utan att kontrollera argument och returvärde.

## Övningar

**SL1 – Följ referenserna.** En lista `a` innehåller en cirkel `c`. `b = List.copyOf(a)`. Sedan läggs en linje till i `a`, och `b.get(0).move(5, 0)` körs. Hur många element finns i varje lista? Har `c` flyttats? Motivera med en referensbild.

**SL2 – Välj beteendets typ.** Skriv lämplig funktionell typ och en lambda för: a) skriva ”Sparat” utan argument, b) testa om ett heltal är positivt, c) flytta en mottagen figur `(0, 7)`. Visa också anropet som faktiskt kör beteendet.

**SL3 – Förutsäg utskriften.** Vad skrivs ut och i vilken ordning?

```java
System.out.println("A");
Runnable r = () -> System.out.println("B");
System.out.println("C");
r.run();
r.run();
```

**SL4 – Skriv testhandlingen.** Skriv ett JUnit-test som förväntar `IllegalArgumentException` för en cirkel med radie noll. Förklara varför konstruktionen måste ligga inuti lambdan. Skriv därefter en rad som flyttar alla figurer 4 enheter i x-led med `forEach`.

# Kapitel 5. Händelser, verktyg och Observer: från musgest till modell

**Bygger på:** F6, sidor 2–27; F8, sidor 2–24; jämförelse med F15, sidor 2–9. **Mål:** kunna följa en hel gest och placera ansvar, tillfälligt tillstånd och notifiering på rätt ställe.

## Samma musrörelse kan betyda olika saker

Ett tryck följt av dragning kan skapa en cirkel, flytta en figur eller göra något annat beroende på valt verktyg. Panelen behöver inte ha alla dessa regler i en stor villkorssats. Den kan lämna händelser vidare till ett objekt som uppfyller `Tool`:

```java
public interface Tool {
    void pressed(Point2 point, boolean extend);
    void dragged(Point2 point);
    void released(Point2 point);
    void cancel();
    void drawPreview(DrawingContext context);
}
```

Panelen tar emot Swings `MouseEvent` och översätter dess uppgifter till ett domännära `Point2` och en flagga. Verktyget behöver därmed inte läsa tekniska musevent direkt. `extend` anger om markeringen ska utökas. Panelens anrop kan vara `tool.pressed(point(event), event.isShiftDown())`. Objektets klass avgör om CircleTool eller MoveTool utför metoden. F6, sidor 3–6.

Här används **State**: det aktiva verktygsobjektet representerar det läge som styr hur nästa händelse tolkas. Det finns också tillstånd *inne i* ett verktyg: exempelvis om en cirkelgest har startat och vilket centrum den har. Dessa två nivåer är lätta att blanda ihop. Panelen kan ha CircleTool valt samtidigt som det verktyget väntar i vila utan aktiv gest.

## En cirkelgest behöver minnas mellan metodanrop

`pressed`, `dragged` och `released` sker vid olika tillfällen. Därför behöver centrum och aktuell slutpunkt vara fält i verktygsobjektet. Lokala variabler i `pressed` skulle försvinna när det anropet är klart.

| Händelse | Verktygets tillstånd | Dokumentet |
|---|---|---|
| `pressed((10,20), false)` | centrum och slutpunkt sätts till `(10,20)` | Oförändrat |
| `dragged((13,24))` | slutpunkten blir `(13,24)` | Oförändrat |
| `drawPreview(...)` | läser centrum och slutpunkt | Oförändrat |
| `released((13,24))` | beräknar radie, skapar, rensar gesten | Får en ny cirkel med radie 5 |

Radien är avståndet mellan punkterna: skillnaderna är 3 och 4, så avståndet är 5. Point2 kan använda `Math.hypot(dx, dy)` för beräkningen. `distance` returnerar ett tal och ändrar inga punkter. `translated` returnerar i stället ett nytt punktvärde. Gemensam avståndsberäkning hör hemma i Point2 så att både verktyg och träfftest kan återanvända den. F6, sidor 10–16; F8, sidor 16–17.

En förhandsvisning ritas via `drawPreview` men förs inte in i dokumentets figurlista. Den är ett tillfälligt förslag. Vid `released` ska den slutliga musupp-punkten användas: den behöver inte vara exakt samma som senaste dragpunkten. Nollradie ger ingen cirkel i det här verktygsflödet. `cancel` sätter centrum och slutpunkt till `null`, så en gammal gest inte senare kan skapa en figur.

Vid verktygsbyte ska panelen först anropa det **gamla** verktygets `cancel`, därefter spara det nya verktyget. Om referensen byts först riskerar du att rensa fel verktyg. F6, sidor 7 och 15.

## Flyttverktyget har två olika versioner i kursen

I **labb 2**, som F6 visar, ändras modellen under dragning:

```java
target.move(point.x() - last.x(), point.y() - last.y());
document.changed();
last = point;
```

Delta beräknas från **föregående** muspunkt. Start `(20,20)`, drag `(23,24)` och släpp `(25,25)` ger först `(3,4)` och sedan `(2,1)`: totalt `(5,5)`. `released` gör det sista delsteget och avslutar sedan. Escape avslutar gesten men flyttningen som redan skett behålls. F6, sidor 20–24.

I **labb 4**, som F15 visar, ändrar dragning bara tillfälligt preview-tillstånd. Vid släpp beräknas hela förskjutningen från startpunkten och ett kommando skickas till History. Escape före släpp lämnar geometrin oförändrad och skapar inget kommando. Efter släpp rensas gesten även om exekveringen får fel. F15, sidor 2–5.

| Fråga | Labb 2, F6 | Labb 4, F15 |
|---|---|---|
| Ändras modellen vid `dragged`? | Ja | Nej, bara preview |
| Vilken delta används? | Från senaste punkt | Total delta från start vid släpp |
| Vad gör Escape före släpp? | Stoppar; utförd flytt behålls | Stoppar; modellen är fortfarande oförändrad |
| Hur många historiksteg? | Denna version använder direkt flyttning | En färdig gest ger ett kommando |

Flyttverktyget behöver också välja rätt figur. Eftersom sista elementet ritas överst söker träfftestet bakifrån genom figurlistan. Det första träffade objektet blir målet. Utan Shift töms den tidigare markeringen först och den träffade figuren markeras. Med Shift behålls övrig markering och den träffade figurens markering växlas med `toggle`; därefter sätts `target` till `null` så att ingen flyttgest startar. Detta är en uttalad regel i det givna verktyget, inte en allmän regel som Java bestämmer. F6, sidor 18–19.

## Modellen meddelar via ett litet lyssnarkontrakt

En figur kan flyttas utan att känna DrawingPanel. Ändå behöver panelen få veta att bilden behöver uppdateras. I **Observer** registrerar intresserade mottagare ett beteende hos modellen:

```java
// I panelens konstruktor:
document.addListener(this::repaint);
```

Dokumentet sparar en Runnable-referens. Det anropar inte `run` när lyssnaren läggs till. Vid ändring körs de registrerade handlingarna:

```java
public void changed() {
    for (Runnable listener : List.copyOf(listeners)) {
        listener.run();
    }
}
```

Kedjan i labb 2 blir `move` lyckas → `document.changed()` → lyssnarens `run()` → panelens `repaint()` → Swing målar senare. Dokumentet behöver känna Runnable-kontraktet, men inte panelens konkreta klass. Två paneler kan registrera var sin lyssnare och läsa samma dokument. F8, sidor 3–11.

Kopian av lyssnarlistan gör att just den genomgången har en stabil liststruktur även om registreringen ändras under ett lyssnaranrop. Det är samma skillnad mellan liststruktur och element som tidigare: lyssnarobjekten själva kopieras inte.

**State**, **Strategy** och **Observer** besvarar olika frågor. State handlar här om hur nästa händelse tolkas i aktivt läge. Strategy väljer en algoritm eller regel. Observer avgör vilka som får veta att något ändrats. Att alla kan använda interface gör dem inte till samma mönster. F8, sida 15.

## Ändringsnotifiering och preview-målning har olika orsaker

CircleTool ändrar inte dokumentet under dragning, men preview behöver fortfarande uppdateras. Därför kan panelen begära repaint efter en musgest utan en modellnotifiering. Det betyder ”visa det tillfälliga läget”, inte ”dokumentet har ändrats”. F8, sida 14.

Vem ska då anropa `changed` efter en beständig ändring? Det beror på kontraktet. I F8 ändrar `MoveTool` en figur direkt och meddelar sedan dokumentet. Dokumentets `add` går däremot via `replace`, som redan notifierar. Ett extra `changed` i CircleTool skulle då meddela samma dokumentändring ytterligare en gång. I F15 är det MoveCommand och RotateCommand som notifierar efter sina lyckade geometriska ändringar; AddCommand och GroupCommand använder `replace`, som redan gör det. F8, sidor 12–14; F15, sida 8.

## Ett fel ska inte lämna halva ändringen kvar

Anta att en linjes startpunkt kan översättas men att slutpunktens nya koordinat blir oändlig och avvisas. Följande ordning är riskabel:

```java
start = start.translated(dx, dy);
end = end.translated(dx, dy);
```

Om den andra raden kastar har den första redan ändrat linjen. Beräkna i stället allt före första tilldelningen:

```java
Point2 nextStart = start.translated(dx, dy);
Point2 nextEnd = end.translated(dx, dy);
start = nextStart;
end = nextEnd;
```

Nu lämnar en avvisad beräkning båda fälten oförändrade. Det skyddar en enskild Line-operation, inte automatiskt en hel grupp av flera figurer. Gruppens förkontroll behandlas i F10 och F12. `document.changed()` ska ske först efter en lyckad modelländring. Ett undantag före den raden gör att ingen framgångsnotifiering skickas. F8, sidor 18–23.

## Övningar

**HO1 – Följ cirkelgesten.** Anropa `pressed((2,3), false)`, `dragged((5,7))`, `cancel()` och sedan `released((5,7))`. Hur många cirklar skapas? Vad blir skillnaden om cancel tas bort? Beskriv också dokumentet före släpp.

**HO2 – Beräkna flyttningen.** Anta att musned utan Shift träffar en giltig målfigur och att alla flyttningar lyckas. Start är `(10,10)`, två dragpunkter är `(13,12)` och `(15,16)`, släpp sker på `(18,17)`. Beräkna varje delta i F6-versionen. Jämför med F15-versionens enda kommando. Vad händer om Escape körs i stället för släpp, direkt efter den andra dragpunkten?

**HO3 – Pröva notifieringen utan fönster.** Skriv en Runnable-lyssnare som räknar anrop. Registrera den och kontrollera att registreringen inte ökar räknaren. Anropa `changed()` två gånger och kontrollera resultatet. Beskriv hur testet kompletteras vid en avvisad flyttning.

**HO4 – Hitta två ansvarsfel.** Ett CircleTool kör `document.add(circle); document.changed();`. Ett Line.move tilldelar `start` innan det beräknar nästa `end`. Förklara problemen och skriv principen för korrigeringen. Varför räcker det inte att lägga en catch i History?

# Kapitel 6. Composite och rotation

**Underlag:** F9, PDF-sidor 2–18; F10, PDF-sidor 3–13; F12, PDF-sidor 5 och 19–22. Exemplen nedan är förklaringar och studieövningar, inte ytterligare labbkrav. Kodutdragen använder föreläsningarnas typer och är inte ett komplett startprojekt.

**Förkunskaper:** Du behöver förstå interface, polymorfism, referenser, listor och varför en operation ska lämna objekt giltiga.

**Mål:** Du ska kunna följa ett anrop genom ett figurträd, beräkna ett gruppcentrum och förklara varför grupprotation behöver en gemensam pivot och förkontroll.

## En figur kan vara en helhet

Anta att du ritar ett trafikljus av en rektangel och tre cirklar. Du vill kunna flytta trafikljuset som en enda symbol. Senare ska flera trafikljus och vägmarkeringar kunna ingå i en större korsning.

Ett första försök är att göra en särskild metod för varje nivå: `drawCircle`, `drawTrafficLight` och `drawIntersection`. Då måste klienten känna till vilka delar varje helhet består av. När en ny slags helhet tillkommer måste klienten lära sig mer.

Composite löser det genom att låta både grundfiguren och gruppen uppfylla `Figure`. Ett löv är en figur utan exponerade delar, exempelvis `Circle`. En sammansättning är en figur som innehåller andra figurer, exempelvis `CompositeFigure`. Klienten kan anropa `draw`, `move` och `rotate` genom samma kontrakt.

Gruppen blir alltså själv ett objekt med figurförmågor. Den är inte bara en lista som klienten måste hantera på egen hand. Det är detta som gör att en grupp kan innehålla en annan grupp. Se F9, sidor 3–5.

```java
Figure a = new Line(new Point2(10, 0), new Point2(20, 0));
Figure b = new Circle(new Point2(20, 0), 2);
Figure c = new Rectangle(new Point2(20, -2), 10, 4);

Figure g2 = new CompositeFigure(List.of(b, c));
Figure g1 = new CompositeFigure(List.of(a, g2));
```

Objektstrukturen är:

```text
g1
├── a: Line
└── g2: CompositeFigure
    ├── b: Circle
    └── c: Rectangle
```

`g1` har två direkta barn: `a` och `g2`. Den har inte tre direkta barn. `b` och `c` ligger en nivå längre ned. Skilj därför mellan *direkta delar* och *alla underliggande grundfigurer*.

## Var uppstår rekursionen?

En grupp kan rita sina barn så här:

```java
public void draw(DrawingContext context) {
    for (Figure child : parts) {
        child.draw(context);
    }
}
```

Vid `g1.draw(context)` händer följande:

1. `g1` anropar `a.draw(context)`. Linjen skickar sina ritoperationer till ritkontexten.
2. `g1` anropar `g2.draw(context)`.
3. `g2` kör samma gruppimplementation och anropar i sin tur `b.draw(context)` och `c.draw(context)`.
4. När `g2` är klar återgår kontrollen till `g1`.

Metoden innehåller inget uttryck som bokstavligen heter `this.draw(...)`. Rekursionen uppstår ändå, eftersom ett barns dynamiska typ kan vara `CompositeFigure`. En grupp anropar samma sorts beteende på en mindre del av trädet. Grundfigurerna avslutar kedjan genom att faktiskt rita. Se F9, sidor 9–10.

Barnens ordning spelar roll. Om figurer målas ovanpå varandra blir den senare ritade figuren normalt den översta. En grupp får därför inte sortera sina barn godtyckligt.

## Vad ska parts() betyda?

Föreläsningarnas kontrakt ger alla figurer en fråga om direkta delar:

```java
public interface Figure extends Drawable, Movable, Rotatable {
    Point2 getCenter();

    default List<Figure> parts() {
        return List.of();
    }
}
```

En `Circle` kan ärva standardresultatet: en tom lista. En `CompositeFigure` returnerar sina direkta barn. Då kan en klient vandra genom ett träd utan att först fråga om objektets konkreta klass. En tom lista avslutar vandringen naturligt. Se F9, sidor 7–8.

`parts()` är en fråga. Den delar inte upp en grupp i dokumentet, tömmer inte gruppen och skapar inte kopior av barnen.

Fältet kan skyddas så här:

```java
private final List<Figure> parts;

public CompositeFigure(List<Figure> parts) {
    this.parts = List.copyOf(parts);
}

@Override
public List<Figure> parts() {
    return parts;
}
```

`List.copyOf` gör liststrukturen omodifierbar. Om klienten ändrar sin ursprungliga lista efter konstruktionen ska gruppens medlemslista inte påverkas. Däremot är barnen fortfarande samma figurobjekt. Att skydda liststrukturen och att förbjuda mutation av figurerna är två olika löften.

## Gruppens centrum är ett kontrakt

I dessa föreläsningar definieras gruppens centrum som medelvärdet av de **direkta barnens centrum**. En tom grupp har centrum `(0, 0)`. En nästlad grupp räknas som ett barn. Se F9, sida 14.

Anta att gruppen `g` har två barn vid `(0, 0)` och `(10, 0)`. Då blir dess centrum `(5, 0)`. Anta sedan att `root` har barnen `g` och en cirkel vid `(100, 0)`. Då blir:

```text
g.center    = ((0 + 10) / 2, 0) = (5, 0)
root.center = ((5 + 100) / 2, 0) = (52.5, 0)
```

Det är inte medelvärdet av alla tre lövcentrum, vilket skulle vara ungefär `(36.67, 0)`. Det är inte heller centrum av en omslutande rektangel eller ett masscentrum. Sådana regler kan vara rimliga i andra program, men du måste följa det kontrakt som gäller här.

Konsekvensen är att trädets struktur kan påverka beräknat centrum även om grundfigurerna ligger kvar på samma koordinater. Därför är gruppcentrum mer än en fråga om vilken bild som syns på skärmen.

## Rotation behöver en tydlig pivot

En pivot är punkten som en rotation sker kring. Föreläsningen använder två operationer:

```java
void rotate(double radians);
void rotate(double radians, Point2 pivot);
```

Varianten med ett argument väljer figurens eget centrum som pivot. Varianten med två argument får pivoten uttryckligen. Vinkeln anges i radianer: ett helt varv är `2 * Math.PI` och ett kvarts varv är `Math.PI / 2`. Se F9, sidor 11–13.

För ett punktvärde `(x, y)` och pivot `(px, py)` kan beräkningen förstås i tre steg:

1. Flytta till ett koordinatsystem där pivoten är origo: `dx = x - px`, `dy = y - py`.
2. Rotera förskjutningen med vinkeln `v`:
   `rx = dx*cos(v) - dy*sin(v)` och `ry = dx*sin(v) + dy*cos(v)`.
3. Flytta tillbaka: `(px + rx, py + ry)`.

I det matematiska koordinatsystemet motsvarar en positiv vinkel moturs. På en bildskärm där y ökar nedåt kan samma koordinatberäkning uppfattas som medurs. Följ därför koordinaterna och projektets kontrakt när du kontrollerar resultatet.

För cirklar med centrum `(10, 0)` och `(20, 0)` ger ett kvarts varv kring `(0, 0)` centrum `(0, 10)` respektive `(0, 20)`. Radierna ändras inte. Om varje cirkel i stället roteras kring sitt eget centrum står båda centrumen kvar. En cirkels kontur ser då likadan ut, trots att operationen kan vara korrekt.

En grupp måste skicka vidare **samma pivot** till alla barn:

```java
TransformChecks.rotation(this, radians, pivot);
for (Figure child : parts) {
    child.rotate(radians, pivot);
}
```

Om du skriver `child.rotate(radians)` väljer varje barn sitt eget centrum. Det håller inte ihop gruppen som en gemensamt roterad helhet. Samma externa pivot måste även nå barnen i en nästlad grupp.

## En roterad rektangel behöver bevara formen

För en rektangel som alltid ligger längs koordinataxlarna kan ett hörn, bredd och höjd räcka. Efter en godtycklig rotation räcker de värdena inte ensamma för att beskriva orienteringen. Du behöver exempelvis bevara fyra hörn i kantordning eller bevara ett centrum, mått och en vinkel.

Föreläsningens ritport har `polygon(List<Point2> vertices)`. Den ritar en sluten kontur genom hörnen och tillbaka till första hörnet. Hörnen måste ligga i kantordning; en godtycklig ordning kan ge korsande linjer. Figuren ansvarar fortfarande för att rektangelns räta vinklar och motstående sidors längder bevaras. Ritporten ritar det den får. Se F9, sidor 15–16 och F12, sida 21.

## Varför ska hela gruppen förkontrolleras?

En flyttning kan ge ogiltiga koordinater, exempelvis genom att en ändlig beräkning övergår till oändlighet. Om du först flyttar barn 1 och sedan upptäcker felet i barn 2, har gruppen blivit delvis ändrad. Det bryter löftet att en avvisad operation ska lämna geometrin oförändrad.

Lösningen i föreläsningarna är att kontrollera hela operationen före första mutationen. `TransformChecks.translation` respektive `TransformChecks.rotation` kontrollerar geometrin innan barnen ändras. F12, sida 22 beskriver att kontrollen använder ritoperationerna för att läsa geometrin utan mutation. Efter lyckad kontroll kan ändringsfasen börja.

Det är inte samma sak som att bara fånga ett undantag efteråt. `catch` återställer inga redan ändrade koordinater automatiskt. Förkontrollen och figurernas egna kontrakt måste tillsammans ge den önskade säkerheten. Se F10, sida 12.

## Vanliga misstag

- Att beräkna centrum från samtliga löv när kontraktet säger direkta barn.
- Att tro att `parts()` gör en uppdelning i dokumentet.
- Att skicka `rotate(v)` till varje barn när hela gruppen ska använda en gemensam pivot.
- Att behandla `List.copyOf` som en djup kopia av muterbara figurer.
- Att ändra barnen medan resten av gruppens indata fortfarande kontrolleras.

## Övningar

**Övning C1 – Följ anropen.** En grupp `root` har barnen `a` och `g`. Gruppen `g` har barnen `b` och `c`. Alla tre grundfigurer överlappar. Skriv anropsordningen för `root.draw(ctx)`. Vilken grundfigur ritas sist?

**Övning C2 – Räkna centrum.** `g` har barncentrum `(2, 4)` och `(6, 8)`. `root` har barnen `g` och en cirkel med centrum `(20, 10)`. Beräkna båda gruppcentrum. Beräkna även vad du skulle få om du felaktigt tog medelvärdet av alla löv.

**Övning C3 – Hitta rotationsfelet.** En grupps implementation gör `for (Figure child : parts) child.rotate(radians);`. Gruppen ska roteras ett kvarts varv kring `(0, 0)`. Barnen är cirklar vid `(10, 0)` och `(20, 0)`. Vad blir fel? Visa den avgörande kodraden som måste ändras.

**Övning C4 – Skriv ett kontraktstest.** Skriv ett JUnit-test som använder de två cirklarna från C3 och kontrollerar resultatet av en gemensam rotation. Använd tolerans för beräknade `double`-värden.

**Övning C5 – Resonera om felvägen.** En grupp har tre barn. En föreslagen flyttning är giltig för de första två men ogiltig för det tredje. Beskriv vad som kan gå fel i en enkel flyttloop och hur ett test ska skilja en säker implementation från en halv mutation.

# Kapitel 7. Ägarskap, likhet och generiska samlingar

**Underlag:** F10, PDF-sidor 2–14; F11, PDF-sidor 2–17. Läs gärna kapitlet efter Composite, eftersom skillnaden mellan lista, objekt och trädstruktur är central här.

**Mål:** Du ska kunna gruppera och dela upp utan att klona figurer, välja rätt sorts likhet och förstå vad `List<? extends Figure>` respektive `List<? super Circle>` tillåter.

## En plats i dokumentet är inte samma sak som en referens

Anta att dokumentets toppnivå är `[a, b]` och att du skapar en grupp `g` med barnet `a`. Om du sedan låter toppnivån vara `[a, b, g]` förekommer samma figurobjekt på två platser i dokumentträdet. Vid en vandring över hela dokumentet kan `a` bli ritad eller flyttad två gånger.

Föreläsningarnas dokumentregel är därför att samma figurobjekt får finnas på endast en plats i dokumentträdet. Om `a` blir barn till `g` ska `a` tas bort från toppnivån. Det hindrar inte att en lokal variabel, ett verktyg eller ett test fortfarande har en referens till `a`. Regeln gäller placeringar i dokumentets struktur, inte att det bara får finnas en referens i hela programmet. Se F10, sidor 3 och 10.

Detta är en viktig skillnad vid UML-modellering. Composite beskriver hur del och helhet erbjuder gemensamt beteende. En diamant i UML uttrycker ytterligare relationer om del/helhet och eventuellt ägarskap. Motivera relationens betydelse utifrån modellens regler; välj inte en symbol enbart för att klassnamnet innehåller ordet Composite.

## Gruppering ändrar strukturen

Utgångsläget är `[a, b, c]`, där `c` ritas sist och överst. Användaren markerar `a` och `c`. Gruppen `g` skapas med delarna `[a, c]`, och dokumentets nya toppnivå blir `[b, g]`. Se F10, sida 5.

Tre saker behöver bevaras:

1. **Identitet:** Gruppens delar är samma `a` och `c` som tidigare. De klonas inte.
2. **Geometri:** Koordinater och mått ändras inte av själva grupperingen.
3. **Inbördes ordning:** `a` ligger före `c` även inne i gruppen.

Gruppen ersätter den översta valda figurens plats enligt föreläsningens regel. Det kan ändra överlappningen mot en ovald figur: tidigare ritades `a` före `b`, men efter grupperingen ritas hela `g` efter `b`. Därmed ritas även `a` efter `b`. Gruppering kan alltså påverka ritordningen mellan utvalda och ovalda figurer, trots bevarad geometri.

Ett säkert arbetssätt är att bygga nästa toppnivålista först och därefter göra ett dokumentbyte med `replace(next)`. Då behöver dokumentet inte passera flera synliga mellanlägen medan figurer plockas bort och läggs tillbaka. Se F10, sida 6.

## Uppdelning går en nivå ned

Anta att dokumentet är `[x, g, y]`, där `g.parts()` är `[a, h]` och `h.parts()` är `[b, c]`. Uppdelning av `g` ger `[x, a, h, y]`. Den nästlade gruppen `h` är fortfarande en grupp. Uppdelningen är alltså inte en rekursiv plattning. Se F10, sida 7.

En gemensam regel kan uttryckas så här för en markerad figur:

```java
List<Figure> children = figure.parts();
List<Figure> replacements = children.isEmpty()
        ? List.of(figure)
        : children;
next.addAll(replacements);
```

En grundfigur ger en tom lista och blir kvar. Även en tom grupp ger en tom lista och blir kvar enligt kursens regel. Det är ett medvetet kontrakt; du ska inte ta bort en tom grupp som en oavsiktlig följd av att lägga till noll barn.

Den gamla gruppen `g` tömmer inte sin barnlista. Om en annan referens fortfarande når `g` kan den fortfarande fråga efter dess delar. Det är dokumentets toppnivå som ersätts. Se F10, sidor 8–9.

## Tre frågor som liknar varandra men har olika svar

| Fråga | Vad jämförs? | Exempel |
|---|---|---|
| Är det samma objekt? | Referensidentitet med `==` | Är just denna figur markerad? |
| Har objekten samma värde? | `equals` enligt typens kontrakt | Har två Point2 samma koordinater? |
| Ska objekten ligga på samma sorteringsposition? | En `Comparator` | Har två figurer samma centrum-x? |

Två `Point2`-objekt kan vara olika objekt men samma punktvärde. Eftersom `Point2` är ett record är dess `equals` värdebaserad. `p == q` kan därför vara `false` samtidigt som `p.equals(q)` är `true`. Se F11, sidor 2–3.

Två cirklar kan däremot vara två separata redigerbara figurer med samma geometri. När användaren markerar den ena ska den andra inte automatiskt vara markerad. `Selection.contains` i föreläsningarna använder därför `==`. En vanlig `List.contains` använder `equals`. Metoder med samma namn kan alltså ha olika likhetskontrakt. Se F11, sidor 4–5.

En sortering efter x-koordinat säger ännu mindre om identitet. Om två figurer båda har centrum-x `10` kan jämförelsen ge noll, men de behöver varken vara samma figur eller ha samma geometri.

## Hashning bygger på ett avtal

En `HashSet` eller `HashMap` använder en hashkod för att hitta ett område där den kan söka efter objekt. Därför måste objekt som är lika enligt `equals` också ha samma `hashCode`. Det omvända behöver inte gälla: olika objekt får ha samma hashkod, och då får `equals` skilja dem åt. Se F11, sida 6.

Om du lägger in två Point2 med samma koordinater i en `HashSet<Point2>` blir mängdens storlek ett. Du har lagt in två referenser, men bara ett distinkt punktvärde enligt mängdens likhetsregel.

En risk uppstår om de fält som styr `equals` och `hashCode` ändras medan objektet används som nyckel i en hashbaserad samling. Objektet kan då ligga kvar på en plats som valdes med den gamla hashkoden, samtidigt som en sökning använder den nya. Undvik därför nycklar vars relevanta värde kan ändras under lagringen. Föreläsningens oföränderliga Point2 passar bättre som värdenyckel än ett muterbart namnobjekt. Se F11, sida 7.

## Hur upptäcker dokumentet upprepad identitet?

F11, sida 8 använder en `IdentityHashMap` under en trädvandring. Den jämför nycklar med objektidentitet och påverkas därför inte av att två olika figurer råkar ha lika geometri.

```java
private static void visit(
        Figure figure,
        IdentityHashMap<Figure, Boolean> seen) {
    if (seen.put(figure, Boolean.TRUE) != null) {
        throw new IllegalArgumentException("Upprepad figur");
    }
    for (Figure child : figure.parts()) {
        visit(child, seen);
    }
}
```

Skapa `seen` en gång före hela dokumentvandringen. Om du skapar en ny karta för varje gren upptäcker du inte att två olika grenar når samma objekt. Koden markerar ett objekt innan den går vidare till dess barn. En återkomst till samma identitet avvisas i stället för att vandringen fortsätter obegränsat.

## Varför är List<Circle> inte List<Figure>?

En Circle kan användas som Figure, men en `List<Circle>` får ändå inte tilldelas till `List<Figure>`:

```java
List<Circle> circles = new ArrayList<>();
// List<Figure> figures = circles; // kompileringsfel
```

Anta att tilldelningen tilläts. Då skulle klienten kunna skriva `figures.add(new Line(...))`. Samma lista skulle nu innehålla en Line, trots att `circles` lovar att dess element är Circle. Förbudet skyddar det löftet. Generiska listtyper är invarianta på detta sätt. Se F11, sida 9.

## extends: läs genom en gemensam övre gräns

Om metoden bara behöver hämta element som Figure kan den ta emot en lista av någon okänd undertyp till Figure:

```java
static void moveAll(List<? extends Figure> items) {
    for (Figure figure : items) {
        figure.move(1, 0);
    }
}
```

Här kan både `List<Circle>` och `List<Figure>` användas. Metoden vet att varje hämtat element åtminstone kan behandlas som Figure. Den vet däremot inte exakt vilken elementtyp listan lovar. Därför får metoden inte lägga till en godtycklig Circle eller Line.

`? extends` betyder inte att listan eller figurerna är oföränderliga. Metoden ovan flyttar figurerna. Wildcard-typen förhindrar heller inte i sig alla strukturella operationer, som borttagning; om en sådan operation fungerar beror även på listans implementation. Typgränsen reglerar vilka elementtyper som är säkra att använda. Se F11, sidor 10–11.

## super: en mottagare av Circle

Om metoden ska lägga in Circle-objekt används i stället en nedre gräns:

```java
static void addCircle(List<? super Circle> target, Circle circle) {
    target.add(circle);
}
```

`List<Circle>`, `List<Figure>` och `List<Object>` kan alla vara mottagare. De kan alla lagra en Circle utan att bryta sitt elementkontrakt. Men vid läsning vet metoden bara att ett godtyckligt element är Object. En `List<Object>` kan ju redan innehålla en sträng. Se F11, sida 12.

Minnesregel: en källa som producerar värden åt din metod passar ofta med `extends`; en mottagare som tar emot värden passar ofta med `super`. Börja ändå med att fråga vad metoden faktiskt ska läsa och skriva, inte med att gissa en wildcard.

## Generiska metoder och stackar

En egen typparameter kan bevara samband mellan in- och utdata:

```java
static <T> T identity(T value) {
    return value;
}
```

Vid `Point2 p = identity(new Point2(1, 2));` kan kompilatorn härleda Point2 som relevant resultattyp. Metoden behöver inte returnera Object och tvinga klienten att casta.

På samma sätt kan en `Stack<T>` lagra och lämna tillbaka element av samma typ. `Stack<Command>` passar senare för historik. Java behöver inte skapa en separat källkodsimplementation av stacken för varje typ. Använd typargument och undvik råa typer som `List` utan `<...>`, eftersom de försvagar kompilatorns kontroll. Se F11, sidor 13–16.

## Övningar

**Övning A1 – Gruppera och dela upp.** Dokumentet är `[a,b,c]`. Gruppera `a` och `c` till `g` enligt föreläsningens regel. Skriv toppnivå och barnlista. Dela därefter upp `g`. Förklara varför du inte nödvändigtvis får exakt den ursprungliga toppnivåordningen tillbaka.

**Övning A2 – Nästlad och tom grupp.** Dokumentet är `[x,g,e,y]`. `g.parts()` är `[a,h]`, `h.parts()` är `[b,c]` och `e` är en tom grupp. Både `g` och `e` är markerade och ska delas upp. Skriv resultatet. Ska `g.parts()` ändras?

**Övning A3 – Välj rätt kontroll.** Välj `assertSame`, `assertEquals` eller en kontroll av sorteringsresultat: (a) gruppering ska bevara samma Circle-objekt, (b) en beräknad mittpunkt ska ha värdet `(5,7)`, (c) figurer ska ordnas från lägre till högre centrum-x. Förklara också varför två lika x-koordinater inte bevisar identitet.

**Övning A4 – Tillåtna anrop.** Vilka av följande anrop fungerar typmässigt? Anta modifierbara listor. Förklara varje svar.

```java
List<Circle> circles = new ArrayList<>();
List<Figure> figures = new ArrayList<>();
List<Object> objects = new ArrayList<>();

moveAll(circles);
moveAll(figures);
moveAll(objects);
addCircle(circles, circle);
addCircle(figures, circle);
addCircle(objects, circle);
```

**Övning A5 – Hitta två designfel.** En dokumentkontroll skapar en ny vanlig `HashSet<Figure>` i varje rekursivt anrop och använder den för att hitta upprepade figurer. Förklara varför detta inte uppfyller ett uttryckligt identitetskontrakt och varför upprepade objekt i olika grenar kan missas.

# Kapitel 8. SOLID och design

**Underlag:** F12, PDF-sidor 2–23; kopplingar till F9–F11. Det här kapitlet hjälper dig att motivera designbeslut utifrån ett konkret förändringsbehov.

**Mål:** Du ska kunna förklara varje SOLID-princip med ett CAD-exempel och samtidigt säga vilken förändring, vilket kontrakt och vilken klient ditt resonemang gäller.

## Börja med ett problem som går att se

Designprinciper blir användbara när de förklarar en konkret kostnad. Anta att en ny figurtyp kräver ändringar i ritpanelen, i gruppens ritloop, i exporten och i varje testhjälp som räknar upp klasser. Då sprids kunskap om figurtyper till många ställen. Risken är att ett ställe glöms bort.

Fråga därför: Vilket beteende vill klienten använda? Vilken kunskap måste klienten ha? Vilken sorts förändring vill vi kunna göra med få följdändringar? SOLID ger fem sätt att undersöka sådana frågor. Det är inte ett krav att skapa fler interface eller klasser i varje situation.

## SRP: samla ansvar som har samma skäl att ändras

Single Responsibility Principle handlar om sammanhängande ansvar och skäl till förändring. En Circle känner sin geometri och ser till att radien är giltig. Ett användargränssnitt bestämmer hur ett fel presenteras. En lagringsdel bestämmer hur figurens värden skrivs till ett format. Dessa regler kan förändras av olika skäl. Se F12, sidor 3–5.

Anta att `Circle.setRadius` både kontrollerar radien, visar en Swing-dialog och skriver en loggrad i ett visst filformat. Då påverkas Circle om du byter från dialogrutor till text i formuläret, även om geometrireglerna är identiska. Klassen får tekniska beroenden som inte behövs för själva cirkelbegreppet.

Ett tydligare samarbete är att figuren avvisar ogiltig geometri genom sitt felkontrakt. Anroparen som kan hjälpa användaren fångar felet och väljer presentation. Samma figur kan då användas i GUI, test eller ett annat gränssnitt.

SRP betyder inte att en klass bara får ha en metod. `getCenter`, `move`, `rotate` och `draw` kan höra ihop med figurens ansvar. Bedöm om klassens uppgifter utgör en begriplig helhet, inte bara antalet metoder.

F12 visar också ansvar på olika nivåer: Point2 kan beräkna ett nytt roterat punktvärde, grundfiguren uppdaterar sin representation och gruppen delegerar till barnen med gemensam pivot. Det minskar onödig duplicering utan att lägga all geometri i en enda stor klass.

## OCP: skydda en klient mot en bestämd sorts utökning

Open/Closed Principle betyder att en vald klient kan vara öppen för nya varianter av ett beteende utan att själva klientkoden behöver ändras. Föreläsningens ritloop är:

```java
for (Figure figure : document.figures()) {
    figure.draw(context);
}
```

En ny Triangle som uppfyller Figure och själv beskriver sin ritning kan fungera i samma loop. En `CompositeFigure` som delegerar `child.draw(context)` behöver inte heller känna till trianglar. Se F12, sidor 6–9.

Jämför med en klient som skriver `if (figure instanceof Circle) ... else if (figure instanceof Rectangle) ...`. Där måste en ny gren införas för Triangle. Om den uppräkningen finns på flera ställen sprids ändringen.

Men OCP betyder inte att en ny figurtyp aldrig kräver ändringar någonstans. Användaren behöver kunna välja eller skapa triangeln. Lagringsmappningen behöver kunna spara och läsa den. Den polymorfa ritloopen är skyddad mot denna utökning; hela programmet är inte automatiskt färdigt. F12, sida 11 påpekar dessutom att en central typväxling i lagringsformatet kan vara en rimlig projektstart.

Ställ en exakt fråga: "Kan ritklienten hantera en ny Figure-implementation utan ändring?" Det är mer användbart än att hävda att hela systemet alltid måste vara stängt för ändring.

## LSP: en undertyp måste hålla klientens löften

Liskov Substitution Principle handlar om utbytbarhet i beteendet. Om en klient förväntar sig ett visst kontrakt från Rectangle måste en undertyp som används där uppfylla det kontraktet.

Tänk dig att Rectangle lovar att bredd och höjd kan ändras oberoende. En klient sätter först bredden till 8 och sedan höjden till 3 och förväntar sig area 24. En Square som ändrar båda måtten varje gång för att bevara lika sidor kan bryta klientens förväntan. Problemet är inte ordet kvadrat i matematiken; problemet är det programmerade kontraktet för mutation. Se F12, sidor 12 och 18.

En lösning kan vara att Square använder en intern Rectangle men bara exponerar operationer som bevarar kvadratregeln. Det är komposition och delegering: "har en" i stället för att lova att vara utbytbar mot hela Rectangle-gränssnittet.

LSP hjälper också att tolka till synes tomma operationer. En cirkel som roteras kring sitt eget centrum kan se likadan ut och ändå uppfylla kontraktet. Samma cirkel ska kunna byta centrum vid rotation kring en extern pivot. En Line som ignorerar alla rotationer bryter däremot rotationslöftet. Bedöm beteendet mot det specifika kontraktet, inte bara om bilden råkade se förändrad ut. Se F12, sida 19.

Även en nollvinkel behöver följa argumentvalideringen i kontraktet. Att "inget ska flyttas" är inte ett generellt skäl att hoppa över kontroll av exempelvis en ogiltig pivot.

## ISP: ge klienten det kontrakt den behöver

Interface Segregation Principle riktar uppmärksamheten mot klientens behov. En metod som bara ska rita behöver vanligtvis Drawable. En metod som bara ska flytta kan använda Movable. Den behöver inte automatiskt känna till Figurens centrum, rotation eller delar. Se F12, sida 13.

```java
static void render(Drawable item, DrawingContext context) {
    item.draw(context);
}
```

En liten parameterstyrd förmåga gör metoden användbar även för objekt som kan ritas men inte är fullständiga redigerbara figurer. Samtidigt kan Figure vara ett rimligt sammansatt kontrakt där klienten verkligen behöver flera figurförmågor.

`Figure.parts()` visar att ISP kan innebära en avvägning. Fördelen med en gemensam fråga är att uppdelning och trädvandring kan fungera utan typkontroller. Kostnaden är att även ett löv erbjuder en strukturell fråga. I föreläsningen är tom lista ett meningsfullt svar: figuren har inga exponerade delar. Därför behöver metoden inte vara ett onaturligt tvång. Se F12, sida 14.

Skapa inte ett interface per metod av vana. Undersök om det finns en klient som får ett tydligare och mer stabilt kontrakt av uppdelningen.

## DIP och DI: skilj beroendets riktning från hur det lämnas in

Dependency Inversion Principle innebär i föreläsningens exempel att en redigeringstjänst använder ett lagringskontrakt, exempelvis `DrawingStore`, i stället för att själv behöva känna till filimplementationens detaljer. En filimplementation och en minnesimplementation uppfyller samma kontrakt. Se F12, sidor 15–16.

```java
final class EditorService {
    private final DrawingStore store;

    EditorService(DrawingStore store) {
        this.store = Objects.requireNonNull(store);
    }
}
```

Dependency Injection, DI, är här mekanismen som lämnar in det valda beroendet genom konstruktorn. Kod vid uppstarten kan skapa en filimplementation; ett test kan skapa en minnesimplementation. Ingen särskild DI-ram behövs för denna form av konstruktorinjektion.

Skillnaden är värd att kunna formulera: DIP handlar om att den viktiga klienten bygger på ett lämpligt kontrakt. DI handlar om hur klienten får den implementation som används. Ett injicerat beroende till en mycket konkret klass blir inte automatiskt en väl vald abstraktion bara för att det kom via en konstruktor.

## Kohesion och koppling hjälper dig bedöma resultatet

Kohesion är hur väl en klass uppgifter hör ihop. Koppling är vad den behöver känna till om andra delar. Du vill kunna förklara varför en förändring hamnar i en viss klass och vilka andra delar som behöver påverkas.

Om en geometriregel ändras ska det vara begripligt vilka figurer eller geometrihjälpare som berörs. Om ett felmeddelandes utseende ändras ska du kunna hitta GUI-ansvaret. Om filformatet ändras ska du kunna hitta mappning och lagring. Målet är begriplig fördelning av kunskap, inte att varje del saknar beroenden. Ett samarbete utan några beroenden alls skulle inte kunna göra något tillsammans. Se F12, sida 17.

## En liten metod för att granska en design

Ta en konkret förändring, till exempel "lägg till en triangel". Gör sedan följande:

1. Beskriv det beteende användaren ska få: skapa, rita, flytta, rotera och lagra triangeln.
2. Följ vilka kontrakt som redan stödjer detta: Figure, ritportens polygonoperation och lagringens dataflöde.
3. Markera vilka klienter som bör kunna lämnas oförändrade, exempelvis den allmänna ritloopen.
4. Markera de kopplingar som faktiskt behövs, exempelvis nytt skapandeverktyg och lagringsmappning.
5. Välj tester för nya figurens löften och för att den fungerar som barn i en grupp.

Detta ger ett granskningsbart resonemang. Det räcker inte att skriva "vi använder SOLID" utan att förklara vad designen gör lättare eller säkrare.

## Övningar

**Övning D1 – Hitta ansvarsblandningen.** `Circle.move` räknar nya koordinater och visar dessutom en Swing-dialog vid ogiltig flytt. Vilka två skäl till förändring blandas? Beskriv ett bättre samarbete utan att skriva ett helt program.

**Övning D2 – Utöka med Triangle.** En klient har den polymorfa ritloopen ovan och en grupp delegerar draw till sina barn. Vilka av dessa behöver ändras för att rita Triangle? Vilka andra delar kan ändå behöva ny kod för att användaren ska få komplett triangelstöd?

**Övning D3 – Pröva utbytbarhet.** Rectangle-kontraktet lovar att `setWidth(8)` bevarar höjden och att `setHeight(3)` bevarar bredden. En Square-implementation sätter båda måtten lika i varje setter. Visa en konkret klientsekvens som avslöjar kontraktsbrottet och föreslå ett alternativ.

**Övning D4 – Välj parametertyp.** Vilket av Drawable, Movable och Figure är ett rimligt kontrakt för (a) en metod som bara anropar draw, (b) en metod som bara anropar move, (c) en metod som både frågar efter centrum och vandrar direkta delar? Motivera utifrån behov, inte namnets storlek.

**Övning D5 – Skilj DIP från DI.** En `EditorService` skapar själv `new FileDrawingStore(...)`. Du vill kunna testa tjänsten utan en verklig fil. Beskriv vilket kontrakt tjänsten ska använda, hur implementationen lämnas in och vilken kod som väljer mellan fil och minne. Förklara vad som är DIP respektive DI.

# Kapitel 9. Command och historik: gör handlingar möjliga att ångra

**Underlag:** F13 s. 2–21 och F15 s. 2–12. Läs efter kapitlen om interface, verktyg och grupper.

**Efter kapitlet ska du kunna:** beskriva vem som skapar, utför och minns ett kommando; rita undo- och redo-stackarna; göra en hel musgest till ett ångersteg; förklara vad som måste ske vid fel.

## Varför ett vanligt metodanrop inte räcker

Om du anropar `circle.move(10, 0)` har cirkeln flyttats. Men anropet skapar ingen varaktig beskrivning av vad du gjorde. En Ångra-knapp behöver åtminstone veta vilken figur som flyttades, med vilken förskjutning och när det skedde i förhållande till andra ändringar.

Command-mönstret gör handlingen till ett objekt. Ett `MoveCommand` sparar målfiguren, dokumentet och förskjutningen. Objektet kan utföra handlingen och ångra den. Konstruktionen av kommandot ska i detta upplägg bara spara uppgifterna; den ska inte flytta figuren.

```java
interface Command {
    void execute();
    void undo();
}
```

History kan nu använda samma interface för flyttning, rotation och gruppering. Det är polymorfism igen: History anropar `command.undo()` och objektets klass bestämmer hur ångrandet går till. History behöver ingen lista med `if (command instanceof MoveCommand)`.

## Ansvarsfördelningen steg för steg

| Del | Ansvar |
|---|---|
| MoveTool | Tolkar gesten och skapar ett kommando vid släpp |
| MoveCommand | Vet mål, förskjutning och hur handlingen utförs/ångras |
| Figure | Genomför flyttningen och skyddar geometrins regler |
| DrawingDocument | Publicerar ändringen till lyssnare |
| History | Anropar kommandon och håller ordning på historiken |

I föreläsningens `MoveCommand` är utförandet en flytt med `(dx,dy)` och ångrandet en flytt med `(-dx,-dy)`. Detta är en invers operation. Det är ett annat designval än att lagra två positioner och sätta tillbaka dem. Flyttalsberäkningar kan innebära små avrundningsskillnader, så numeriska tester behöver lämplig tolerans.

Kodutdraget förutsätter kursens Figure- och DrawingDocument-typer:

```java
public void execute() {
    figure.move(dx, dy);
    document.changed();
}

public void undo() {
    figure.move(-dx, -dy);
    document.changed();
}
```

Om `move` kastar ett undantag kommer programflödet inte fram till `changed`. Men detta skyddar bara notifieringen. Figuren måste också se till att den inte har hunnit ändra halva sin geometri innan felet uppstår.

## Två stackar och varför ordningen spelar roll

En stack följer LIFO: det som lades dit sist tas ut först. Ångra ska börja med den senaste lyckade handlingen. Redo ska börja med den senast ångrade handlingen.

Vi skriver stackar som `[topp, ... , botten]`. Startläget är två tomma stackar.

| Händelse | Undo | Redo |
|---|---|---|
| execute(A) | [A] | [] |
| execute(B) | [B,A] | [] |
| undo() | [A] | [B] |
| redo() | [B,A] | [] |
| undo() | [A] | [B] |
| execute(C), lyckas | [C,A] | [] |

Efter ett nytt lyckat C har du valt en ny väg efter A. Den tidigare B-vägen finns därför inte kvar som redo. Om C avvisas ska däremot läget fortfarande vara `[A]` och `[B]`.

Här är ett komplett, förenklat History som använder Java-standardbiblioteket. Lägg till `Command` ovan i samma fil utan `public`, eller i en separat fil.

```java
import java.util.ArrayDeque;
import java.util.Deque;
import java.util.Objects;

final class History {
    private final Deque<Command> undo = new ArrayDeque<>();
    private final Deque<Command> redo = new ArrayDeque<>();

    public void execute(Command command) {
        Objects.requireNonNull(command).execute();
        undo.push(command);
        redo.clear();
    }

    public void undo() {
        if (undo.isEmpty()) return;
        Command command = undo.peek();
        command.undo();
        undo.pop();
        redo.push(command);
    }

    public void redo() {
        if (redo.isEmpty()) return;
        Command command = redo.peek();
        command.execute();
        redo.pop();
        undo.push(command);
    }

    public int undoCount() { return undo.size(); }
    public int redoCount() { return redo.size(); }
}
```

`peek()` hämtar posten utan att ta bort den. Först efter lyckat `undo()` eller `execute()` flyttas posten. Om anropet kastar finns posten kvar på sin ursprungliga stack. Lägg också märke till att `redo()` anropar kommandots execute direkt. Om den i stället anropade `History.execute(command)` skulle alla övriga redo-poster raderas.

## En hel dragning ska bli en handling

I F6, labb 2, flyttas figuren direkt i flera små steg under dragningen. Då räknar verktyget skillnaden mellan föregående muspunkt och nästa. Den varianten är bra för att förstå händelser, men den ger inte automatiskt ett enda ångersteg.

I F15, labb 4, är upplägget ändrat:

1. Vid `pressed` sparas startpunkten och målfiguren.
2. Vid `dragged` sparas aktuell muspunkt. Preview visar ett förslag. Figuren har ännu inte flyttats.
3. Vid `released` beräknas hela förskjutningen från start till släpp.
4. Ett kommando skapas och körs genom History.
5. Gesttillståndet rensas, även om utförandet misslyckas.

Start `(10,20)`, mellansteg `(13,22)`, släpp `(18,25)` ger alltså kommandot `(8,5)`. Du ska inte både ha flyttat modellen under dragningen och sedan lägga på hela `(8,5)` vid släpp; då räknas rörelsen två gånger.

Om användaren avbryter före släpp lämnar preview-varianten modellen oförändrad och skapar inget kommando. Om förskjutningen är noll skapas inget flyttkommando enligt F15 s. 5. Samma grundidé används för rotation, men då måste den valda pivoten också sparas.

## Rotation och gruppering behöver olika minne

En rotation sparar vinkel och pivot. Undo använder negativ vinkel och samma pivot. Att räkna fram en ny pivot från den ändrade geometrin kan ge ett annat rotationscentrum och därmed fel resultat.

Gruppering ändrar objektstruktur, ordning och markering. I F13 sparar GroupCommand före- och efterlistor. Redo ska använda samma gruppobjekt som det första lyckade utförandet. En ny grupp varje gång skulle ändra identiteten. Dessa listor är strukturellt minne; de är inte en djup kopia av varje figurs geometri.

Notifiering måste också ha en bestämd ägare. F15 skiljer på kommandon som ändrar en figur direkt och därför anropar `changed`, och kommandon som använder `document.replace`, vilket redan notifierar. Ett extra `changed` i det senare fallet ger dubbel notifiering.

## Vanliga misstag

- Att tömma redo innan kommandot har lyckats. Då försvinner en giltig redo-väg vid ett avvisat försök.
- Att ta bort en stackpost innan operationen har lyckats. Då tappas posten vid fel.
- Att tro att History reparerar felaktig geometri. Den hanterar ordningen, inte generell återställning.
- Att skapa en historikpost för varje musrörelse i preview-varianten.
- Att låta en gammal gest leva kvar efter verktygsbyte.

## Övningar

### H1 · Spåra stackarna · grund

Utför `execute(A), execute(B), execute(C), undo(), undo(), redo(), execute(D)`. Skriv båda stackarna efter varje steg, med toppen först. Vilka kommandon kan till sist ångras respektive göras om?

### H2 · Hitta historikfelet · mellan

En metod gör `redo.clear(); command.execute(); undo.push(command);`. Förklara felet med ett konkret läge där redo innehåller B. Skriv den korrigerade ordningen och förklara vad som händer om execute kastar.

### H3 · Räkna en gest · mellan

Start är `(2,3)`. Tre dragged-händelser har punkterna `(5,4)`, `(8,7)` och `(9,9)`. Släpp sker vid `(12,13)`. Vilket flyttkommando skapas i F15-varianten? Hur många beständiga flyttningar sker under dragged? Vad ska Escape före släpp göra?

### H4 · Utforma ett historiktest · fördjupning

Du har kört ett giltigt A och sedan undo. Beskriv ett test som försöker köra ett ogiltigt B. Testet ska kontrollera mer än att ett undantag kastas. Avsluta med redo av A. Vilket fel i History och vilket fel i Figure kan testet skilja på?

# Kapitel 10. Undantag, kontraktstester och säker refaktorering

**Underlag:** F3 s. 4–8, F4 s. 14–23, F8 s. 18–23 och F16 s. 2–22.

**Efter kapitlet ska du kunna:** följa programflödet när ett undantag kastas; skilja `throw`, `throws`, `catch` och `finally`; testa även tillståndet efter ett fel; ändra struktur utan att ändra observerbart beteende.

## Ett undantag är ett avbrutet normalt förlopp

Ett felaktigt argument kan göra att en operation inte kan uppfylla sitt löfte. En cirkel med negativ radie ska därför inte skapas som om allt vore bra. Konstruktionen avvisas genom ett undantag. Ett lyckat anrop följer normalvägen; ett kastat undantag följer en annan väg genom anropskedjan.

```java
try {
    System.out.println("före");
    throw new IllegalArgumentException("fel");
} catch (IllegalArgumentException error) {
    System.out.println("fångat");
} finally {
    System.out.println("städning");
}
System.out.println("efter");
```

Utskriften är före, fångat, städning, efter. Om ingen matchande catch finns här går undantaget vidare till anroparen efter finally. Det avbrutna try-blocket återupptas inte där felet inträffade.

## Checked och unchecked

`Throwable` är övertypen. Under den finns bland annat `Exception` och `Error`. `RuntimeException` är en underklass till Exception. RuntimeException och dess undertyper är unchecked; Error och dess undertyper är också unchecked. Övriga Throwable-typer är checked i föreläsningens indelning.

Unchecked betyder att kompilatorn inte kräver att du fångar eller deklarerar dem. Det betyder inte att de aldrig inträffar eller att de är oviktiga. Checked betyder att ett möjligt undantag som kan lämna metoden måste fångas eller deklareras. Båda sorterna kastas när programmet körs.

| Ord | Vad det gör | Vad det inte gör |
|---|---|---|
| throw | Kastar ett undantagsobjekt här | Deklarerar inte hela metodens felkontrakt |
| throws | Anger undantag som får lämna metoden | Fångar eller reparerar inte felet |
| catch | Tar emot ett matchande undantag | Återupptar inte den avbrutna raden |
| finally | Kör städningen när detta kontrollflöde lämnar try/catch | Återställer inte automatiskt modellen |
| try-with-resources | Stänger deklarerade resurser | Ersätter inte catch/throws för checked fel |

Exempel med standardbiblioteket, som behöver importerna `java.io.IOException`, `java.nio.file.Files` och `java.nio.file.Path`:

```java
static String readText(Path path) throws IOException {
    return Files.readString(path);
}
```

Metoden lämnar beslutet till sin anropare. GUI-lagret kan fånga IOException och erbjuda ett nytt filval. Geometriobjektet ska däremot inte behöva känna till dialogrutor för att avvisa en felaktig radie. Detta förenar felkontrakt och ansvarsfördelning.

Catch-block prövas uppifrån. En specifik `InvalidRadiusException` ska fångas före dess övertyp `IllegalArgumentException`. Om det breda blocket kommer först kan det specifika aldrig nås; den ordningen kompilerar inte.

## Oförändrat tillstånd efter avvisad operation

En linje har två ändpunkter. Följande tanke är farlig: beräkna och tilldela den första, beräkna och tilldela sedan den andra. Om andra beräkningen misslyckas är linjen halvflyttad.

Gör i stället alla beräkningar och valideringar i lokala variabler innan du publicerar det nya tillståndet:

```java
Point2 nextStart = start.translated(dx, dy);
Point2 nextEnd = end.translated(dx, dy);
start = nextStart;
end = nextEnd;
```

Kodutdraget förutsätter att translated returnerar en giltig ny punkt eller kastar utan att ändra den gamla. Det förutsätter också att övriga kontraktskontroller sker före tilldelningarna. För grupper måste motsvarande förkontroll omfatta hela trädet innan någon del ändras. History kan inte gissa vilka delar som redan flyttats.

## Hur ett bra kontraktstest byggs

Ett test behöver ett utgångsläge, en handling och observationer. Fråga vilket löfte du prövar. Ett test av mittpunkt ska inte bara konstatera att metoden kör utan fel, utan kontrollera resultatet och att ursprungspunkterna är oförändrade.

```java
Point2 p = new Point2(2, 4);
Point2 q = new Point2(8, 10);
Point2 m = p.midpoint(q);
assertEquals(new Point2(5, 7), m);
assertEquals(new Point2(2, 4), p);
assertEquals(new Point2(8, 10), q);
```

Detta är ett JUnit-utdrag, med kursens Point2 och statiska imports från `org.junit.jupiter.api.Assertions`. Den första kontrollen prövar returvärdet. De sista två prövar frånvaron av sidoeffekter.

För beräknade double-värden kan exakt likhet bli fel test eftersom decimaltal inte alltid kan representeras exakt i binärt flyttal. `assertEquals(0.15, actual, 1e-12)` anger en tolerans. Välj toleransen efter problemet; den ska inte vara så stor att ett riktigt geometrifel döljs. `assertSame` testar samma objekt, medan `assertEquals` använder likhetskontraktet.

Testa både normala fall, gränser och fel. Om kontraktet kräver positiv ändlig radie är noll, negativt värde, NaN och oändlighet olika viktiga felindata. Skapa ett eget utgångsläge i varje test så att testordningen inte påverkar resultatet.

Ett grönt beteendetest visar inte automatiskt att ett fält är private. Representationens synlighet granskas i koden. Tester prövar valda beteenden; de är inte bevis för alla möjliga körningar.

## Refaktorering som ett kontrollerat steg

Refaktorering ändrar strukturen medan det observerbara beteendet bevaras. Skriv eller välj först ett test som uttrycker kontraktet. Gör en liten strukturändring, kör testet och jämför. Om du samtidigt ändrar geometri, notifieringar och filformat blir det svårt att veta vad som orsakade en skillnad.

Exempel: att flytta duplicerad punktrotation till `Point2.rotated` kan vara refaktorering om resultat, felbeteende och sidoeffekter bevaras. Att dessutom acceptera NaN där det tidigare avvisades är en beteendeförändring och måste behandlas separat.

## Övningar

### E1 · Följ kontrollflödet · grund

En metod skriver A i try, kastar IOException, skriver B efter kastet, skriver C i catch för IOException, D i finally och E efter hela konstruktionen. Vilka bokstäver skrivs? Vad ändras om catch tas bort och metoden deklarerar throws IOException? Behandla detta som en flödesskiss, inte som kod med en direkt onåbar sats efter throw.

### E2 · Korrigera en checked felväg · mellan

`static String read(Path p) { return Files.readString(p); }` kompilerar inte. Visa två principiellt olika lösningar. Förklara vem som väljer åtgärd i respektive lösning och varför `throws` inte betyder att felet är löst.

### E3 · Testa mer än undantaget · mellan

En grupp med tre delar ska flyttas. Den sista delens föreslagna koordinat blir oändlig. Skriv en testplan som upptäcker om de första två delarna har flyttats innan felet uppstår. Ta även med notifiering och historik.

### E4 · Välj rätt assertion · fördjupning

Välj assertion och motivera: (a) en grupp innehåller samma Circle-instans som före gruppering; (b) en mittpunkt är `(5,7)`; (c) en beräknad x-koordinat är ungefär 0.15; (d) en konstruktor avvisar NaN. Räcker sista kontrollen för att visa att en redan befintlig modell är oförändrad?

# Kapitel 11. Memento: ett stabilt tillstånd att återvända till

**Underlag:** F14 s. 2–22. Detta är ett frivilligt designalternativ i materialet, inte ett uttalat krav i labb 4 eller projektet.

**Efter kapitlet ska du kunna:** skilja en invers handling från sparat tillstånd; förklara Mementos roller; se varför en ytlig kopia inte automatiskt fryser muterbara figurer.

## Problemet med flera delsteg

Tänk en makrohandling som först flyttar en cirkel och sedan försöker sätta en negativ radie. Det första steget kan ha lyckats när det andra avvisas. History lägger visserligen inte till en lyckad historikpost, men det återställer inte den genomförda flytten. Vi behöver antingen säker förkontroll eller en genomtänkt återställningsmekanism för hela handlingen.

F14 visar ett separat exempeldokument, MementoDrawing. Det är inte labbens DrawingDocument. Exemplet använder oföränderliga CircleValue-värden med id, koordinater och radie. Varje ändring ersätter värden/lista i stället för att mutera gamla värdeobjekt. Därför kan en tidigare lista vara ett stabilt sparat tillstånd.

## De tre rollerna

| Roll | I exemplet | Ansvar |
|---|---|---|
| Originator | MementoDrawing | Känner representationen, skapar och återläser memento |
| Memento | Nästlat tillståndsobjekt | Kapslar in ett stabilt sparat tillstånd |
| Caretaker | AtomicMacro | Förvarar before/after utan att läsa innehållet |

En snapshot är en ögonblicksbild. Memento är ett sätt att kapsla in en sådan bild så att den som förvarar den inte behöver få tillgång till dokumentets interna representation. En godtycklig offentlig lista med alla fält är därför inte automatiskt Memento-mönstret.

Ägaren kan också kontrollera att ett memento hör till just detta dokument. Ett memento från en annan ritning ska inte råka återställas in i den aktuella.

## Före, försök, efter eller återställning

Första execute följer i teorins exempel denna ordning:

1. Spara startläget lokalt.
2. Kör samtliga delsteg.
3. Om allt lyckas, spara efterläget och behåll before/after.
4. Om ett delsteg kastar RuntimeException, återställ startläget och kasta samma fel vidare.
5. Publicera bara ett lyckat slutläge, inte mellanlägena.

Undantaget måste fortsätta till History. Om makrot fångar felet, återställer och sedan returnerar normalt tror History att handlingen lyckades och bokför den.

Undo återställer före-läget. Redo återställer efter-läget; delstegen körs inte igen i F14:s variant. Det skiljer sig från labbens inversbaserade MoveCommand, där redo använder execute för att flytta framåt igen.

## Varför List.copyOf inte räcker i alla modeller

`List.copyOf(figures)` ger en skyddad liststruktur, men elementreferenserna pekar fortfarande på samma figurer. Om en figur är muterbar kommer både originallistan och kopian att se dess nya koordinater. Kopian har inte bevarat de gamla koordinaterna.

F14:s delning är säker därför att både listan och dess CircleValue-element är oföränderliga. Om du vill överföra idén till muterbara CAD-figurer måste du bestämma hur geometri fångas och hur identitet bevaras. Att ersätta allt med kloner kan bryta andra referenser till gamla figurer. Stabilt id eller återställning av befintliga objekt är möjliga designval, men inget av dem sker automatiskt genom att kopiera listan.

## Övningar

### M1 · Välj strategi · grund

Förklara varför en ensam flyttning kan ångras med en invers, medan ett makro med flera olika delsteg kan behöva ett sparat före-läge. Är Memento enda möjliga lösningen?

### M2 · Kopian som ändrades · mellan

`before = List.copyOf(figures)` följs av `figures.get(0).move(10,0)`. Figuren är muterbar. Varför visar även `before.get(0)` den nya geometrin? Rita referenserna och föreslå vad som måste sparas för ett stabilt tillstånd.

### M3 · Makro med fel · fördjupning

Start: centrum `(0,0)`, radie 5, undo tom, redo innehåller A. Makrot flyttar till `(20,0)` och försöker sedan sätta radien till -1. Beskriv slutläget med F14:s återställning. Vad blir fel om catch återställer men inte kastar vidare?

# Kapitel 12. Swing: bygg gränssnittet och måla från modellen

**Bygger på:** F17, sidor 4–27; EDT-grunden även i F6, sida 25. **Mål:** kunna skapa ett litet gränssnitt där händelser ändrar modellen och målningen läser den. F17 nämner F19 om längre arbete, men F19 ingår inte i ditt material och behandlas inte här.

## Fönster, innehåll och komponenter har olika roller

`JFrame` är programmets fönster. En `JPanel` är en yta eller behållare inuti det. `JLabel` visar text. `JButton` är en knapp. Att skapa knappen gör inte att den vet vad ett klick ska innebära; beteendet kopplas in med en lyssnare.

Ett typiskt startflöde är att skapa ramen, välja innehåll, anropa `pack` och sedan visa den. `pack` beräknar storleken från innehållets önskade storlekar. I ett fristående övningsprogram gör `EXIT_ON_CLOSE` att programmet avslutas när ramen stängs. F17, sidor 4–7.

Swing-arbetet samordnas på **EDT**, Event Dispatch Thread. Föreläsningens `SwingUtilities.invokeLater(Example::show)` registrerar `show` för senare körning på EDT. Metodreferensen kör inte show på den plats där den skrivs. Mus- och knapphanteringen ska göra kort arbete, så tråden hinner ta emot nästa händelse och uppdatera skärmen. F17, sida 5; F6, sida 25.

## Layout anger hur utrymmet fördelas

Du behöver inte beräkna varje knapps pixelposition själv. Varje panel kan ha en egen layout, och paneler kan nästlas:

| Layout eller behållare | Roll | Användning i övningen |
|---|---|---|
| `BorderLayout` | Fördelar ytan i fem regioner | Knapprad NORTH, rityta CENTER, status SOUTH |
| `FlowLayout` | Lägger komponenter i ett flöde | Flera knappar bredvid varandra |
| `GridLayout` | Ger lika stora celler, fylls radvis | Etikett och val på varje formulärrad |
| `JTabbedPane` | Visar en av flera flikar | Rita och Hjälp |
| `JScrollPane` | Omsluter innehåll med rullning | En lista eller större textyta |

De två sista är behållare, inte layout managers. I BorderLayout ska flera knappar inte läggas som separata konkurrerande komponenter i samma NORTH-region. Samla dem i en underpanel med FlowLayout och placera den panelen i NORTH. En WEST-panel kan i sin tur ha en egen NORTH-region för formuläret. Regionnamnet är alltid relativt den panel som anropet gäller. F17, sidor 8–16.

`JComboBox<String>` visar val ur en samling strängar. `JList<String>` visar en lista; `SINGLE_SELECTION` begränsar valet till ett alternativ. Listans markering och data har egna Swing-modeller. Dessa ska inte förväxlas med ritprogrammets domänmodell. En vald rad kan ge ett förslag till formuläret utan att den beständiga ritningen ändras. F17, sidor 12 och 15.

## Behåll tillståndet i modellen, inte i den tillfälliga målningen

Föreställ dig att du klickar ”Visa cirkel”. Det är frestande att se klicket som ett direkt ritkommando till skärmen. Men fönstret kan täckas över, byta storlek eller behöva målas igen. Därför behöver programmet lagra vad som ska visas.

Modellen kan exempelvis hålla figurval och färg. Knapplyssnaren läser kontrollerna och uppdaterar modellen. Därefter anropas `repaint`. När Swing senare anropar `paintComponent` läser panelen modellens aktuella värden och återskapar bilden. F17, sidor 20–24.

```text
Klick → lyssnare → ändra modell → begär repaint
                                         ↓ senare
                              paintComponent läser modell
```

`paintComponent` ska inte flytta figuren, lägga till en ny figur eller ändra modellen. Antalet målningar är inget användaren har beställt som redigeringssteg. Om varje målning flyttade cirkeln skulle exempelvis en storleksändring kunna ändra dokumentet. Börja med `super.paintComponent(g)` så panelens vanliga bakgrund målas före din grafik. F17, sidor 19 och 23.

## Genomgånget körbart exempel

Följande är ett fristående undervisningsexempel för Java 17, inspirerat av F17. Det använder en liten visningsmodell, inte hela labbens Figure-modell. Spara som `SwingStudy.java` och kör som en Java-applikation. Alla imports som exemplet behöver finns med.

```java
import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Dimension;
import java.awt.FlowLayout;
import java.awt.Graphics;
import javax.swing.JButton;
import javax.swing.JComboBox;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.SwingUtilities;

public final class SwingStudy {
    private static final class PreviewModel {
        private String shape = "Cirkel";
        private Color colour = Color.BLUE;

        void select(String shape, Color colour) {
            this.shape = shape;
            this.colour = colour;
        }
    }

    private static final class DrawingPanel extends JPanel {
        private final PreviewModel model;

        DrawingPanel(PreviewModel model) {
            this.model = model;
            setBackground(Color.WHITE);
            setPreferredSize(new Dimension(600, 350));
        }

        @Override
        protected void paintComponent(Graphics g) {
            super.paintComponent(g);
            g.setColor(model.colour);
            int x = getWidth() / 2 - 50;
            int y = getHeight() / 2 - 50;
            switch (model.shape) {
                case "Cirkel" -> g.fillOval(x, y, 100, 100);
                case "Rektangel" -> g.fillRect(x, y, 100, 100);
                case "Linje" -> g.drawLine(x, y, x + 100, y + 100);
            }
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(SwingStudy::show);
    }

    private static void show() {
        PreviewModel model = new PreviewModel();
        DrawingPanel canvas = new DrawingPanel(model);
        JComboBox<String> shapes = new JComboBox<>(
            new String[] {"Cirkel", "Rektangel", "Linje"});
        JComboBox<String> colours = new JComboBox<>(
            new String[] {"Blå", "Röd"});
        JButton apply = new JButton("Visa");
        JLabel status = new JLabel("Cirkel visas i Blå");

        apply.addActionListener(event -> {
            String shape = (String) shapes.getSelectedItem();
            Color colour = colours.getSelectedIndex() == 0
                ? Color.BLUE : Color.RED;
            model.select(shape, colour);
            status.setText(shape + " visas i " + colours.getSelectedItem());
            canvas.repaint();
        });

        JPanel buttons = new JPanel(new FlowLayout(FlowLayout.LEFT));
        buttons.add(shapes);
        buttons.add(colours);
        buttons.add(apply);

        JPanel root = new JPanel(new BorderLayout(8, 8));
        root.add(buttons, BorderLayout.NORTH);
        root.add(canvas, BorderLayout.CENTER);
        root.add(status, BorderLayout.SOUTH);

        JFrame frame = new JFrame("DVG345 - Swingövning");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setContentPane(root);
        frame.pack();
        frame.setLocationByPlatform(true);
        frame.setVisible(true);
    }
}
```

Följ klicket ”Rektangel, Röd” i detalj. Först ändrar användaren de två kontrollernas val. Visningsmodellen är fortfarande blå cirkel. Vid klicket anropar Swing den registrerade ActionListener-handlingen. Den läser valen, skriver rektangel och rött i modellen, uppdaterar status och begär repaint. Vid nästa målning läses de nya modellvärdena. Om du därefter ändrar fönstrets storlek kommer rektangeln att målas igen med samma val men en ny centrering utifrån panelens bredd och höjd.

Metodens argument till `fillOval` är x, y, bredd och höjd. De är inte centrum och radie. Därför tar vi bort 50 från mittpunkten för en figur med diameter 100. Skillnaden mellan ritportens cirkelbegrepp och Swings rektangulära begränsningsruta är ett exempel på varför tekniköversättning hör hemma vid gränsen till GUI:t.

Exemplets switch är avgränsad till tre alternativ i en visningsövning, precis som F17. I det utbyggda CAD-programmet kan ritytan i stället gå igenom Figure-objekt och låta deras polymorfa `draw` beskriva geometrin. Förväxla inte en enkel demonstration av komponenter med hela projektets slutliga arkitektur.

## Vanliga misstag

- Att bara ändra en variabel i formuläret och förvänta sig att en separat modell automatiskt får samma värde.
- Att anropa `paintComponent` själv i stället för att begära repaint.
- Att lägga modelländringar i målningen, vilket gör tillståndet beroende av Swings omritningar.
- Att lägga flera knappar direkt i samma BorderLayout-region utan underpanel.
- Att behandla `invokeLater` som om den körde metoden omedelbart på anropsplatsen.

## Övningar

**SW1 – Förklara komponentträdet.** Rita en JFrame med en root-panel. Root har knapprad NORTH, rityta CENTER och status SOUTH. Ange layout för root och knapprad. Var ska en JScrollPane placeras om en hjälpyta ska ligga i en separat flik?

**SW2 – Följ värdena.** Starta exemplet på nytt. Du väljer Linje och Röd men har ännu inte klickat Visa. Vilken figur visar exemplet? Vad ändras vid klicket? Vad händer efter att fönstret täcks över och sedan blir synligt igen?

**SW3 – Lägg till Återställ.** Utöka exemplet med en knapp som återställer både kontroller, modell och status till blå cirkel och begär ommålning. Skriv lyssnaren. Förklara varför det inte räcker att endast återställa komboboxarna.

**SW4 – Felsök en målning.** En student lägger `model.x += 10;` i paintComponent och säger att repaint betyder ”flytta figuren”. Förklara två problem. Beskriv rätt ansvarsfördelning och varför muslyssnaren bör göra kort arbete på EDT.

# Kapitel 13. JSON och lagring: spara betydelsen av ritningen

**Underlag:** F18 s. 2–26. Läs efter objektstruktur, felhantering och DIP.

**Efter kapitlet ska du kunna:** läsa och skriva ritningsdata som JSON; skilja syntax, format och domänregler; beskriva ett säkert öppna-/spara-flöde; testa lagring genom ett interface.

## Vad ska överleva när programmet stängs?

En sparad ritning behöver information för att återskapa dokumentets innehåll: figurtyper, geometri, stil, ritordning och gruppernas nästling. Ett fönster, en knapp, Graphics-objektet, en lyssnare och en halvfärdig musgest hör däremot till en viss körning. De byggs upp igen när programmet startar.

I projektets öppna-flöde enligt F18 blir markering och historik tomma. Nya figurinstanser skapas från sparade värden. Därför betyder en lyckad round-trip inte att `oldCircle == loadedCircle` måste vara sant. Den betyder att relevanta värden, ordning och struktur motsvarar det sparade dokumentet.

## JSON:s byggstenar

Ett JSON-objekt skrivs inom `{ }` och har namngivna medlemmar. Namnet är en sträng, kolon skiljer namn från värde och komma skiljer medlemmar. En array skrivs inom `[ ]` och bevarar elementordningen. JSON:s värdetyper är sträng, tal, boolean, null, objekt och array.

```json
{
  "formatVersion": 1,
  "figures": [
    {
      "type": "circle",
      "center": { "x": 100, "y": 80 },
      "radius": 25
    }
  ]
}
```

`25` är ett tal; `"25"` är en sträng. `figures: []` betyder här en tom ritning. Medlemmen måste skrivas med citattecken i riktig JSON, som i kodexemplet ovan. Typnyckeln `circle` är en del av vårt format. Den behöver inte bytas bara för att en Java-klass döps om.

Strängar kan innehålla åäö i UTF-8. Citattecken och radbrytningar måste representeras korrekt. Låt ett JSON-bibliotek hantera syntax och escapning i stället för att sätta ihop text med strängkonkatenering.

## En grupp blir en nästlad datastruktur

```json
{
  "formatVersion": 1,
  "figures": [
    {
      "type": "group",
      "parts": [
        {
          "type": "circle",
          "center": { "x": 10, "y": 20 },
          "radius": 5
        },
        {
          "type": "line",
          "start": { "x": 0, "y": 0 },
          "end": { "x": 30, "y": 40 }
        }
      ]
    }
  ]
}
```

Dokumentets figures är en ordnad lista av figurposter. En grupp har i sin tur en ordnad lista av delar. Samma läsregel används rekursivt för varje del. För ett ägt träd, där samma figur inte förekommer på flera ställen, behövs vanligen inte objekt-id i filen. Om modellen i stället tillåter delade korsreferenser måste formatet ta hänsyn till dem; det är en annan struktur än föreläsningens ägda träd.

## Tre kontroller som svarar på olika frågor

| Kontroll | Fråga | Exempel på fel |
|---|---|---|
| JSON-syntax | Kan texten tolkas som JSON? | Saknad avslutande klammer |
| Dokumentformat | Stämmer version, fält och datatyper? | radius är en sträng eller versionen är okänd |
| Domän | Kan data bilda giltiga figurer? | Radien är -25 |

Ett JSON-bibliotek kan läsa ett negativt tal utan problem. Det betyder inte att en Circle får ha negativ radie. Efter syntax och formatkontroller ska data fortfarande gå igenom objektens validering. Konstruktorns skydd ska inte kopplas bort bara för att informationen kommer från en fil.

En formatversion anger vilken tolkning som gäller. Ett nytt valfritt fält kan ha ett dokumenterat standardvärde. Byter du enheter eller betydelsen av ett fält kan migrering behövas. En okänd version ska avvisas tydligt enligt föreläsningen; att gissa kan ge en ritning som ser rimlig ut men har fel värden.

## Öppna utan att förstöra det gamla

Tänk att filens första nio figurer är giltiga och den tionde är felaktig. Om du först tömmer det aktuella dokumentet och därefter lägger till en figur i taget har användaren förlorat sitt gamla arbete och fått en halv ny ritning.

Bygg därför ett förslag vid sidan av:

1. Läs filinnehållet.
2. Tolka JSON och kontrollera dokumentformatet.
3. Skapa och validera hela det nya figurträdet.
4. Först efter framgång: ersätt dokumentet och tillämpa öppna-flödets regler för markering och historik.
5. Vid fel: behåll det gamla dokumentet, markeringen och historiken och ge användaren återkoppling.

Detta följer samma idé som för en säker flyttning: räkna och kontrollera först, publicera ändringen efteråt. Tyst felhantering som visar en tom ritning som om öppningen lyckats bryter den avsikten.

Vid sparning ska du på motsvarande sätt undvika att först radera den fungerande filen. Skriv ett nytt förslag innan den gamla filen ersätts. Exakt ersättningsstrategi och felgarantier beror på filsystem och valt API; anta inte att varje flytt alltid är atomär. Handboken föreskriver ingen otestad universallösning.

## Ett lagringsinterface gör tekniken utbytbar

```java
interface DrawingStore {
    void save(DrawingSnapshot data) throws IOException;
    DrawingSnapshot load() throws IOException;
}
```

Detta är en kontraktsskiss; DrawingSnapshot är projektets dokumentdata, inte en här levererad färdig klass. En FileDrawingStore känner filen och formatet. En EditorService känner i stället bara interfacet och kan få implementationen genom konstruktorn. Det är dependency injection. Att tjänsten beror på ett abstrakt lagringskontrakt uttrycker DIP.

Testet kan lämna in en minnesimplementation eller en implementation som avsiktligt kastar IOException. Då går det att testa beslutet vid läsfel utan att vara beroende av en viss trasig fil på datorn. Notera skillnaden mot Memento: här betyder snapshot data för lagring; ordet gör inte automatiskt lagringsflödet till Memento-mönstret.

Try-with-resources stänger resurser även när blocket lämnas på grund av fel. Det löser resursens livslängd. Det avgör inte om användaren ska välja en annan fil och det ersätter inte catch eller throws.

## Övningar

### J1 · Läs data · grund

Utgå från den första cirkelfilen i kapitlet. Ange formatversion, antal toppnivåfigurer, centrum och radie. Vad ändras i betydelse om radius blir `"25"`? Vad betyder en tom figures-array?

### J2 · Skriv en nästlad ritning · mellan

Skriv ett JSON-dokument med version 1. Första toppnivåfiguren ska vara en grupp med cirklarna `(0,0), radie 2` och `(10,0), radie 3`. Andra toppnivåfiguren ska vara en linje från `(1,1)` till `(4,5)`. Bevara den angivna ordningen.

### J3 · Placera felet i rätt lager · mellan

Klassificera fyra fel: sista klammern saknas; radius är `"25"`; radius är -25; formatVersion är 99. Vad ska hända med det gamla dokumentet vid vart och ett av felen?

### J4 · Utforma round-trip och feltest · fördjupning

Du sparar en nästlad grupp med överlappande figurer och läser den igen. Lista vilka observationer som ska jämföras och vilka identitetskrav som inte följer av filformatet. Beskriv sedan ett separat test där load kastar IOException och det gamla dokumentet ska överleva.

# Kapitel 14. UML och helheten: se hur delarna samarbetar

**Underlag:** F1 s. 19–23; F2 s. 12–19 och 21; F5 s. 17; F6 s. 16 och 23; F10 s. 10; F13 s. 5 och 11–18; F15 s. 3 och 7. Förklaringarna och slutövningarna är studieförslag.

**Efter kapitlet ska du kunna:** välja rätt diagram för rätt fråga och beskriva ett helt redigeringsflöde utan att blanda ihop typ, objekt och metodanrop.

## Klassdiagram: vilka typer och löften finns?

Ett klassdiagram beskriver struktur på typnivå. En ruta kan innehålla klassens namn, attribut och operationer. Det visar inte att en viss cirkel skapades klockan tio eller att en knapp just klickades.

Notation som används i föreläsningarna:

| Notation | Betydelse | Exempel |
|---|---|---|
| `«interface»` | Rutan beskriver ett interface | Drawable |
| `-` före medlem | private | `- radius: double` |
| `+` före medlem | public | `+ getCenter(): Point2` |
| `namn: Typ` | Attributets/parameterns typ | `center: Point2` |
| `metod(p: Typ): Resultat` | Operationens signatur | `draw(context: DrawingContext): void` |
| Heldragen linje med tom triangel | Generalisering/extends | Figure-interface utökar Drawable |
| Streckad linje med tom triangel | Realisering/implements | Circle implementerar Figure |

Triangeln pekar mot den mer allmänna typen. Circle blir inte en separat cirkel plus ett separat Figure-objekt bara för att två rutor finns i diagrammet. En enda Circle-instans uppfyller kontraktet.

En vanlig referensrelation visar att en typ känner till en annan. Rollen `center: Point2` uttrycker att en cirkel har en punkt som centrum. Gör inte om den till ett arv bara för att båda hör till geometri. En relation kan också ha multiplicitet, exempelvis många figurreferenser i ett dokument.

F10 påminner om att en tom eller fylld diamant behöver motiveras av regler för del/helhet och ägarskap. Att programmet använder Composite-mönstret väljer inte automatiskt en viss UML-diamant åt dig. Skriv först vilka objekt som får delas och vem som ansvarar för strukturen.

## Objektdiagram: vilka konkreta objekt delas just nu?

När frågan gäller alias är ett objektdiagram bättre. Rita exempelvis ett objekt `c1: Circle` och ett punktobjekt med värden `(0,0)`. Rita variablerna `a` och `b` som två referenser till samma `c1`, samt cirkelns centrumreferens till punktobjektet. Diagrammet visar ett ögonblick under körning.

Men låt inte en variabelruta och ett objekt få samma betydelse. Om både a och b pekar på samma cirkel ska du rita ett cirkelobjekt och två referenser, inte två cirklar som råkar ha samma radie. Om c är skapad med ett nytt new ska den ha ett eget objekt även om värdena är identiska.

Objektdiagram är särskilt bra för att pröva gruppering: samma a och c flyttas från toppnivålistan in i gruppens barnlista. Geometrin kan vara oförändrad samtidigt som referensstrukturen byts.

## Sekvensdiagram: vem anropar vem, i vilken ordning?

Ett sekvensdiagram beskriver ett förlopp. Deltagare skrivs `namn: Typ`. Tiden går nedåt längs livslinjerna. En heldragen pil visar ett anrop; en streckad pil visar återgång. Det är ett anropsflöde, inte i sig en beskrivning av flera parallella trådar.

För en flyttning genom historiken kan du läsa samarbetet så här:

```text
MoveTool -> History: execute(command)
History -> MoveCommand: execute()
MoveCommand -> Figure: move(dx, dy)
Figure --> MoveCommand: normal retur
MoveCommand -> DrawingDocument: changed()
DrawingDocument -> lyssnare: run()
lyssnare -> panel: repaint()
MoveCommand --> History: normal retur
History: push undo, clear redo
```

Här ligger ändringen i Figure, notifieringen i dokumentflödet och bokföringen i History. Om Figure kastar avbryts normalvägen. Då ska varken framgångsnotifiering eller lyckad historikbokföring ske. Kom ihåg att en eventuell framtida `paintComponent` sker senare efter repaint-begäran; den är inte samma sak som repaint-anropet.

## CRC: kontrollera ansvar innan du kodar

CRC står för Class, Responsibilities, Collaborators: klass, ansvar och samarbeten. Ett kort för MoveCommand kan säga: "Vet mål och förskjutning. Kan utföra och ångra flytten. Samarbetar med Figure och DrawingDocument." Ett kort för History kan säga: "Minns ordningen och anropar Command. Behöver inte känna cirkelgeometri."

Om du inte kan skriva klassens ansvar tydligt finns ofta en oklarhet att reda ut före implementationen. En lista över alla fält är inte samma sak som en förklaring av ansvaret.

## Övningar

Den här uppgiften för samman kursens delar. Gör den först på papper, sedan i kod om du har labbprojektet. Alla steg är studieövningar, inte nya inlämningskrav.

### U1 · Välj diagram · grund

Välj klassdiagram, objektdiagram eller sekvensdiagram för följande frågor: (a) Figure utökar Drawable; (b) två variabler når samma Circle; (c) en knapp orsakar undo och notifiering; (d) en grupp innehåller två befintliga figurinstanser. Motivera varje val.

### U2 · Fördela ansvar · mellan

Skriv CRC-kort för Circle, DrawingDocument, MoveTool, MoveCommand, History och FileDrawingStore. Varje kort ska ha en mening om ansvaret och högst tre viktiga samarbeten. Markera vilken del som validerar radien, vilken som minns undo-ordningen och vilken som kan filformatet.

### U3 · Följ en hel användning · fördjupning

Utgå från två cirklar a och b med centrum `(0,0)` och `(10,0)`, båda med radie 2. Topplistan är `[a,b]` och båda är markerade.

1. Gruppera dem till g och spara ett historiksteg.
2. Flytta g med `(4,6)` genom ett kommando.
3. Rotera g med `Math.PI/2` runt den externa pivoten `(0,0)` genom ett kommando.
4. Ångra rotationen, ångra flytten och gör sedan om flytten.
5. Spara och öppna ritningen igen.

Beskriv efter varje steg centrum, trädstruktur och stackar. Kräv samma figuridentiteter under redigeringen men skilj på dem och de nya instanserna vid filöppning. Ange också när notifieringar hör hemma.

### U4 · Bryt flödet med ett fel · fördjupning

I läget direkt efter att rotationen ångrats försöker du utföra en ogiltig flyttning som skulle ge en icke-ändlig koordinat. Beskriv vad som måste vara kvar och varför den tidigare rotationen fortfarande ska kunna göras om. Föreslå vilka observationer som gör testet starkare än en ensam assertThrows.

# Begrepp att kunna förklara

| Begrepp | Förklaring i kursens sammanhang |
|---|---|
| Abstraktion | Det klienten behöver kunna göra utan att känna representationen |
| Inkapsling | Klassen kontrollerar vägar till representation och ändringar |
| Identitet | Vilket enskilt objekt en referens når |
| Värdelikhet | Objekt jämförs efter värden enligt equals-kontraktet |
| Alias | Flera referenser når samma objekt |
| Invariant | Regel som giltiga objekt och lyckade operationer ska bevara |
| Förvillkor | Det anropet kräver av klienten |
| Eftervillkor | Det som lovas efter ett lyckat anrop |
| Felkontrakt | Vad som avvisas och vilket tillstånd som ska vara kvar |
| Mutable/immutable | Tillstånd kan ändras / förblir oföränderligt efter konstruktion |
| Härlett attribut | En egenskap som beräknas ur annan information |
| Interface | Kontrakt för vilka operationer en typ erbjuder |
| Statisk typ | Typen kompilatorn använder för att kontrollera uttrycket |
| Dynamisk typ | Det faktiska objektets klass vid körning |
| Polymorfism | Ett gemensamt kontrakt används för flera implementationer |
| Överlagring | Samma namn, olika parameterlistor; signaturen väljs vid kompilering |
| Override | En implementation av en ärvd metod med motsvarande signatur |
| Komposition | Ett objekt använder eller innehåller andra objekt för sitt beteende |
| Lambda | En kort implementation av ett funktionellt interfaces handling |
| Metodreferens | Ett befintligt metodanrop används som beteende med passande signatur |
| Observer | Registrerade mottagare får veta när något ändrats |
| State | Aktivt tillståndsobjekt påverkar hur nästa händelse hanteras |
| Strategy | En utbytbar algoritm eller regel |
| Composite | Del och helhet används genom samma kontrakt i ett träd |
| Pivot | Den punkt en rotation sker kring |
| Generics | Typparametrar som uttrycker och kontrollerar samband mellan typer |
| Hashkod | Underlag för hashbaserad placering; lika objekt måste ha lika hashkod |
| Kohesion | Hur väl klassens uppgifter hör ihop |
| Koppling | Vilka andra delar och detaljer en del behöver känna till |
| DI | Beroendet lämnas in utifrån, exempelvis i konstruktorn |
| DIP | Klienten använder ett lämpligt abstrakt kontrakt för beroendet |
| Command | Ett objekt som beskriver en handling som kan utföras och ångras |
| Memento | Inkapslat sparat tillstånd som dess ägare kan återställa |
| LIFO | Sist in, först ut; ordningen som passar undo-stackar |
| Atomär ändring | Hela den avsedda ändringen lyckas eller lämnar det relevanta tillståndet oförändrat |
| EDT | Swing-tråden som samordnar GUI-händelser och målning |
| Round-trip | Spara, läs igen och jämför relevanta observerbara värden |
| Refaktorering | Strukturändring med bevarat observerbart beteende |


# Facit med förklaringar

## Kapitel 1. Objekt, referenser och ansvar

**1A.** Två cirkelobjekt skapas. `a` och `b` når samma cirkel, vars centrum efter flytten är `(5,1)`. `c` når den andra cirkeln, vars centrum är kvar på `(1,2)`. Den ursprungliga punkten är oföränderlig; den flyttade cirkeln behöver ett nytt punktvärde. `b = null` ändrar en referensvariabel och flyttar, raderar eller avmarkerar inte i sig något i dokumentet.

**1B.** Utskrifterna är `false`, `true`, `true`, `true`. De första punkterna är olika objekt med lika komponenter. `r = q` delar referens. När `q` tilldelas en ny punkt finns den gamla punkten fortfarande kvar genom `r`.

**1C.** Parametern har fått en kopia av referensvärdet. Omtilldelning av parametern ändrar inte `p`. Låt metoden returnera referensen och tilldela resultatet i anroparen:

```java
static Point2 replacement() {
    return new Point2(9, 9);
}
Point2 p = new Point2(2, 3);
p = replacement();
```

**1D.** Dialogpresentationen hör till GUI:t. Circle behåller ansvaret för giltig geometri och kan kasta ett dokumenterat undantag. Då kan geometrin användas i tester utan fönster. Markeringen är en samling referenser till valda figurer; att tömma den ska lämna dokumentets figurer kvar. Radering ändrar dokumentets innehåll.

**Kontroll före nästa del:** Rita ett referensdiagram utan att skriva kod. Förklara sedan med egna ord skillnaden mellan ett objekt, en variabel och en referens.

## Kapitel 2. Giltiga objekt, Point2 och kontraktstester

**2A.** Endast a godtas. b–e avvisas med `IllegalArgumentException`. f avvisas med `NullPointerException`. Noll är inte en positiv radie i detta kontrakt; det är kontraktet som bestämmer, inte en generell Java-regel.

**2B.** Recordens final-referens når samma muterbara lista som anroparen. Ta en kopia vid konstruktion:

```java
import java.util.List;

record Tags(List<String> values) {
    Tags {
        values = List.copyOf(values);
    }
}
```

`List.copyOf` ger en lista vars struktur inte kan ändras genom dess API, och en senare `add` på ursprungslistan ändrar inte den sparade liststrukturen. Här är elementen String-värden, så de är också oföränderliga. Byter man elementtyp till något muterbart är en skyddad liststruktur fortfarande inte en djup kopia av elementen.

**2C.** Testerna kan skrivas så här inne i en JUnit-testklass:

```java
@Test
void midpointHasRightValueAndNoInputMutation() {
    Point2 p = new Point2(2, 4);
    Point2 q = new Point2(8, 10);
    Point2 m = p.midpoint(q);
    assertEquals(new Point2(5, 7), m);
    assertEquals(new Point2(2, 4), p);
    assertEquals(new Point2(8, 10), q);
}

@Test
void rejectsNaNCoordinate() {
    assertThrows(IllegalArgumentException.class,
        () -> new Point2(Double.NaN, 0));
}
```

Det första upptäcker fel mittpunktsformel och otillåten förändring av argumentvärden. Det andra upptäcker en saknad NaN-kontroll. Gröna beteendetester bevisar inte att fälten har deklarerats privata; strukturkrav behöver också granskas i koden.

**2D.** Kontrollera nästa värde innan fältet tilldelas:

```java
public void setRadius(double radius) {
    if (!Double.isFinite(radius) || radius <= 0) {
        throw new IllegalArgumentException("Radie");
    }
    this.radius = radius;
}
```

Skapa cirkeln med radie 5, spara det observerade värdet, använd `assertThrows` för `setRadius(-1)` och kontrollera därefter att radien fortfarande är 5. Ett test som bara ser undantaget missar en implementation som först lagrar -1 och sedan kastar.

**Kontroll före nästa del:** Förklara hur Point2 kan vara oföränderlig samtidigt som en figur flyttas, och varför en record med en lista kräver mer eftertanke.

## Kapitel 3. Interface, arv och dynamisk bindning

**3A.** a, c och e kompilerar. b gör det inte: Movable erbjuder inte `getCenter`. d gör det inte: Drawable erbjuder inte `move`. Alla referenser kan fortfarande nå samma Circle-objekt. Det är referenstypens tillgängliga kontrakt som begränsar vilka anrop som kan skrivas.

**3B.** `objekt` följt av `flyttbar figur`. Överlagringens signatur bestäms vid kompilering utifrån argumentuttryckets typ. Variabeln `object` har typen Object även om objektet som nås är en Line. Dynamisk bindning väljer sedan implementation av den signatur som redan valts; den gör inte om överlagringsvalet efter argumentobjektets klass.

**3C.**

```java
static void moveRight(Movable target) {
    target.move(10, 0);
}
```

Varje konkret klass uppfyller Movable på sitt sätt. Circle flyttar centrum och Line flyttar ändpunkter, men klienten använder samma kontrakt. En metod som enbart ritar behöver Drawable, eftersom flyttförmåga inte behövs för dess uppgift.

**3D.** Utskriften är `0`. Child-objektets `int`-fält får först standardvärdet noll. När Base-konstruktorn anropar `value()` väljs Childs override dynamiskt. Childs fältinitialiserare `n = 7` har ännu inte körts. Först efter basklasskonstruktionen körs den. Undvik därför överstyrbara anrop i konstruktorer när de kan bero på ännu inte etablerat subklasstillstånd.

**Kontroll före nästa del:** Du ska kunna avgöra både om ett anrop är tillåtet och vilken implementation som körs. Ge sedan ett exempel där en basklass skulle dela fel antaganden om geometrin.

## Kapitel 4. Samlingar och lambda: lagra objekt och skicka beteenden

**SL1.** `a` har två element och `b` har ett. `a[0]`, `b[0]` och `c` når samma cirkel. Den har flyttats `(5, 0)` oavsett vilken av dessa referenser du använder för att läsa den. En riktig bild har två separata listobjekt men bara ett cirkelobjekt. Linjen nås enbart från `a`.

**SL2.**

```java
Runnable saved = () -> System.out.println("Sparat");
saved.run();

Predicate<Integer> positive = n -> n > 0;
boolean result = positive.test(3); // true

Consumer<Figure> moveDown = f -> f.move(0, 7);
moveDown.accept(figure);
```

Den första tar inga argument. Den andra måste returnera ett boolean. Den tredje tar en figur men returnerar inget. Namnet ”nedåt” för positivt y beror på koordinatsystemet; koden säger säkert att y ökar med 7.

**SL3.** Utskriften blir `A`, `C`, `B`, `B`, på var sin rad. Tilldelningen till `r` kör inte kroppen. Det finns två uttryckliga `run()`-anrop.

**SL4.**

```java
@Test
void zeroRadiusIsRejected() {
    assertThrows(IllegalArgumentException.class,
        () -> new Circle(new Point2(0, 0), 0));
}

// I en metod där figures finns:
figures.forEach(f -> f.move(4, 0));
```

`assertThrows` behöver kunna starta den prövade handlingen själv. Då kan det kontrollera undantaget. `forEach` lämnar i sin tur varje figur som lambdans `f`.

**Kontroll före nästa kapitel:** Du kan förklara skillnaden mellan att spara en referens, att spara en handling och att utföra handlingen.

## Kapitel 5. Händelser, verktyg och Observer: från musgest till modell

**HO1.** Ingen cirkel skapas. Cancel har rensat centrum, så released får inte avsluta den gamla gesten. Utan cancel skapas en cirkel med centrum `(2,3)` och radie 5. Före släpp finns den endast som preview; dokumentets lista är oförändrad. Testet hittar en cancel som glömmer att rensa den aktiva gesten.

**HO2.** F6 ger `(3,2)`, `(2,4)` och slutligen `(3,1)`, totalt `(8,7)`. F15 lämnar modellen orörd under båda draghändelserna och skapar vid släpp ett MoveCommand med `(8,7)`. Escape efter andra dragningen lämnar i F6 redan genomförda `(5,6)` kvar, medan F15 inte har utfört någon beständig flyttning. Detta är varför man måste veta vilket labbsteg man läser.

**HO3.**

```java
int[] calls = {0};
document.addListener(() -> calls[0]++);
assertEquals(0, calls[0]);
document.changed();
document.changed();
assertEquals(2, calls[0]);
```

Den fångade lokala referensen `calls` tilldelas inte på nytt; arrayelementet kan ändå ändras. För feltestet sparar du geometrin och räknarens värde, kör `assertThrows` runt flytten följd av `changed`, och kontrollerar båda efteråt. Kontrollera hela relevanta geometrin, inte enbart att ett undantag inträffade. Ingen extra notifiering ska ha skickats.

**HO4.** `add` notifierar redan via `replace` i föreläsningens dokument. Ta bort verktygets extra `changed`. I linjen ska båda nya punkterna först beräknas i lokala variabler och därefter tilldelas. History kan avstå från att uppdatera sina stackar vid fel, men det rullar inte automatiskt tillbaka fält som en figur redan har ändrat. Den operation som muterar måste skydda sitt kontrakt.

**Kontroll före nästa kapitel:** Du kan på ett papper skilja händelse, gesttillstånd, beständig modelländring, notifiering och framtida målning.

## Kapitel 6. Composite och rotation

**C1.** Anropen går `root.draw → a.draw → g.draw → b.draw → c.draw`. `g.draw` återgår först när båda dess barn är klara. `c` är sista grundfiguren och ritas därför sist i denna ordning. Gruppens egen metod behöver inte rita någon extra kontur.

**C2.** `g` får centrum `((2+6)/2, (4+8)/2) = (4, 6)`. `root` får `((4+20)/2, (6+10)/2) = (12, 8)`. Ett felaktigt lövmedelvärde blir `((2+6+20)/3, (4+8+10)/3) = (28/3, 22/3)`, ungefär `(9.33, 7.33)`. Skillnaden visar varför den hierarkiska regeln måste vara uttrycklig.

**C3.** Varje barn väljer eget centrum och cirklarnas centrum blir kvar på `(10, 0)` respektive `(20, 0)`. Gruppmetoden ska, efter lyckad kontroll, anropa `child.rotate(radians, pivot);`. Samma argument `pivot` ska användas genom hela trädet.

**C4.** Ett möjligt testutdrag, med föreläsningarnas typer:

```java
@Test
void rotatesBothChildrenAroundSameExternalPivot() {
    Circle a = new Circle(new Point2(10, 0), 2);
    Circle b = new Circle(new Point2(20, 0), 3);
    Figure group = new CompositeFigure(List.of(a, b));

    group.rotate(Math.PI / 2, new Point2(0, 0));

    assertEquals(0, a.getCenter().x(), 1e-9);
    assertEquals(10, a.getCenter().y(), 1e-9);
    assertEquals(0, b.getCenter().x(), 1e-9);
    assertEquals(20, b.getCenter().y(), 1e-9);
    assertSame(a, group.parts().get(0));
    assertSame(b, group.parts().get(1));
}
```

Toleransen hanterar avrundningsfel från trigonometrin. Identitetskontrollerna prövar dessutom att rotationen inte ersatte figurerna med nya objekt.

**C5.** En enkel loop kan hinna mutera barn 1 och 2 innan barn 3 kastar. Ett test sparar observerbar geometri för samtliga barn, försöker flytta, kräver förväntat undantag och jämför sedan all geometri med före-läget. Enbart `assertThrows` räcker inte, eftersom även en halv mutation kan kasta rätt undantag. Lösningen enligt föreläsningarna är förkontroll av hela trädet före första ändringen.

## Kapitel 7. Ägarskap, likhet och generiska samlingar

**A1.** Efter gruppering är toppnivån `[b,g]` och `g.parts()` är `[a,c]`. Efter uppdelning är toppnivån `[b,a,c]`. Uppdelningen ersätter gruppen på dess nuvarande plats med dess direkta barn. Den minns inte automatiskt vilka positioner barnen hade före grupperingen. Att ångra en gruppering med exakt återställd ursprungsordning kräver historikinformation, vilket behandlas i Command-delen av kursen.

**A2.** Resultatet blir `[x,a,h,e,y]`. `h` är fortfarande en grupp med `[b,c]`. Den tomma gruppen `e` blir kvar. `g.parts()` ändras inte: även om `g` inte längre ligger på toppnivån innehåller objektet fortfarande samma delar. Det är dokumentets struktur som byts ut.

**A3.** (a) `assertSame(original, group.parts().get(0))`, eftersom identitet ska bevaras. (b) `assertEquals(new Point2(5,7), actual)`, eftersom Point2 har värdelikhet; för beräkningar med känslig flyttalsavrundning kan koordinater kontrolleras med tolerans. (c) kontrollera ordningen enligt Comparatorn eller jämför den förväntade ordnade listans referenser. Samma x säger bara att sorteringsnycklarna är lika. Två olika figurer kan dela den nyckeln.

**A4.** `moveAll(circles)` och `moveAll(figures)` fungerar. `moveAll(objects)` fungerar inte, eftersom Object inte garanterar Figure-beteende. Samtliga tre `addCircle`-anrop fungerar: Circle kan lagras i en lista av Circle, Figure eller Object. `circle` antas vara ett giltigt Circle-objekt. Typmässig tillåtelse säger inte att varje konkret listimplementation tillåter tillägg, men uppgiften antar modifierbara listor.

**A5.** En vanlig HashSet använder `equals` och `hashCode`. Det gör kontrollen beroende av figurernas värdelikhet, trots att dokumentregeln gäller just samma objekt. Använd en identitetsbaserad struktur. Dessutom måste samma `seen` delas genom hela vandringen; annars ser varje gren bara sin egen nya, tomma mängd. Föreläsningens `IdentityHashMap` med en gemensam karta löser båda problemen.

## Kapitel 8. SOLID och design

**D1.** Geometrins giltighetsregler och GUI:ts felpresentation blandas. Circle bör följa sitt kontrakt och avvisa den ogiltiga flytten utan halv mutation. En lämplig anropare i GUI-flödet kan fånga felet och välja dialogruta eller annan återkoppling. Geometriklassen behöver då inte ändras enbart för att presentationssättet byts.

**D2.** Varken den allmänna ritloopen eller gruppens `child.draw(context)` behöver en ny typgren om Triangle uppfyller kontraktet. Triangle behöver egen giltig representation och implementation av figurförmågorna. Skapandeverktyg, GUI-val och lagringsmappning behöver stödja den nya typen. OCP-bedömningen gäller här ritklienten och den aktuella sortens utökning.

**D3.** En klient kan köra `r.setWidth(8); r.setHeight(3);` och därefter kräva bredd 8, höjd 3 och area 24. Om `r` är den beskrivna Square blir den till sist 3×3 och area 9. Ett alternativ är separata kontrakt där Square bara erbjuder operationer som bevarar lika sidor, eventuellt genom att delegera till en intern Rectangle. Du kan också utforma ett annat gemensamt kontrakt som båda faktiskt kan hålla; poängen är att inte lova oberoende måttändringar från Square.

**D4.** (a) Drawable räcker eftersom metoden bara behöver ritförmågan. (b) Movable räcker för flyttförmågan. (c) Figure är rimligt eftersom klienten behöver getCenter och parts enligt föreläsningarnas sammansatta figurkontrakt. Ett mindre interface är bra när det motsvarar det verkliga behovet, men det ska fortfarande erbjuda alla operationer klienten behöver.

**D5.** Låt tjänsten använda `DrawingStore` och få det i konstruktorn. Uppstartskoden väljer en `FileDrawingStore`; testet lämnar in en implementation i minnet. Att tjänstens beroende uttrycks som lagringskontraktet är DIP i detta exempel. Att implementationen levereras utifrån genom konstruktorn är DI. Testet kan då pröva tjänstens beteende utan att dess resultat beror på åtkomst till en verklig fil.

## Kapitel 9. Command och historik: gör handlingar möjliga att ångra

### H1 · Facit

| Steg | Undo | Redo |
|---|---|---|
| A | [A] | [] |
| B | [B,A] | [] |
| C | [C,B,A] | [] |
| undo | [B,A] | [C] |
| undo | [A] | [B,C] |
| redo | [B,A] | [C] |
| D | [D,B,A] | [] |

D, B och A kan ångras, i den ordningen. C är inte längre redo eftersom en ny handling skapade en annan fortsättning.

### H2 · Facit

Om undo är `[A]` och redo `[B]`, och det nya C avvisas, ska båda stackarna vara kvar som de var. Den felaktiga metoden hinner radera B innan den vet om C går att utföra. Rätt ordning är `command.execute(); undo.push(command); redo.clear();`. När execute kastar hoppas de två senare raderna över. Modellen måste separat garantera att den avvisade operationen inte har lämnat ändringar.

### H3 · Facit

`dx = 12 - 2 = 10`, `dy = 13 - 3 = 10`. Ett kommando med `(10,10)` körs vid släpp. Under dragged sker noll beständiga flyttningar i denna variant. Escape rensar start, mål och preview och lämnar modellen samt historiken oförändrade. De tre mellanstegen ska inte summeras som om de vore separata förskjutningar från start.

### H4 · Facit

Spara figurens geometri, markering om relevant, undo-/redo-antal och notifieringsantal efter att A ångrats. Kör B med `assertThrows`. Kontrollera att alla sparade observationer är oförändrade. Kör sedan redo och kontrollera A:s avsedda resultat. Om redo är borta trots oförändrad geometri finns sannolikt ett bokföringsfel. Om stackarna är rätt men geometrin har ändrats finns ett fel i figurens eller kommandots felhantering. Kontrollera både antal och redo-beteende: rätt antal bevisar inte ensamt att rätt kommando finns kvar.

## Kapitel 10. Undantag, kontraktstester och säker refaktorering

### E1 · Facit

A, C, D, E. B nås inte. Utan catch skrivs A och D, sedan lämnar undantaget metoden och E nås inte på denna felväg. `throws IOException` gör att anroparen behöver fånga eller själv deklarera IOException; det startar inte om try-blocket.

### E2 · Facit

Den första lösningen lämnar beslutet till anroparen:

```java
static String read(Path path) throws IOException {
    return Files.readString(path);
}
```

Den andra lösningen fångar felet där klienten kan agera. Här byter vi till en visningsoperation som inte lovar ett returvärde vid misslyckad läsning:

```java
static void showText(Path path) {
    try {
        String text = Files.readString(path);
        System.out.println(text);
    } catch (IOException error) {
        System.out.println("Kunde inte läsa filen.");
    }
}
```

Importera `java.io.IOException`, `java.nio.file.Files` och `java.nio.file.Path`. I det första fallet väljer anroparen åtgärden; i det andra väljer `showText` att rapportera felet och avsluta visningsförsöket. Att tyst returnera en tom sträng från den ursprungliga String-metoden skulle blanda ihop läsfel med en verkligt tom fil om kontraktet inte uttryckligen gör den skillnaden. `throws` är information om felvägen, inte återställning.

### E3 · Facit

Spara geometrin för samtliga delar och relevanta strukturreferenser. Förbered en redo-post genom en lyckad handling följd av undo. Spara stackarnas läge och notifieringsräknaren. Kör den ogiltiga gruppflytten med assertThrows. Kontrollera varje dels geometri, inte bara gruppens centrum: ett oförändrat medelvärde kan dölja enskilda fel. Kontrollera också oförändrad ordning/identitet, oförändrade stackar, ingen notifiering och fungerande redo av den gamla posten. Lösningen i implementationen är förkontroll av hela operationen före första mutation.

### E4 · Facit

(a) `assertSame(expectedCircle, actualPart)`. (b) `assertEquals(new Point2(5,7), actual)` när Point2 har värdelikhet. (c) `assertEquals(0.15, actualX, 1e-12)` med en motiverad tolerans. (d) `assertThrows(IllegalArgumentException.class, () -> new Point2(Double.NaN, 0))`. Sista kontrollen säger att rätt sorts fel kastas, men inte hur andra objekt påverkas. En redan befintlig modell kräver separata observationer före och efter.

## Kapitel 11. Memento: ett stabilt tillstånd att återvända till

### M1 · Facit

Flyttningen har en enkel motsatt operation: `(dx,dy)` följs av `(-dx,-dy)`. Ett makro kan innehålla operationer där tidigare värden krävs eller där ett sent fel kommer efter tidigare mutationer. Ett före-läge kan då ge en samlad återställning. Memento är inte enda lösningen; exempelvis fullständig förkontroll eller korrekt kompensering kan passa andra kontrakt. Valet beror på vad som måste kunna återställas.

### M2 · Facit

Två listor pekar på samma figur. En skyddad lista hindrar add/remove, men hindrar inte move på elementet. Spara stabila geometrivärden/strukturer eller använd en representation med oföränderliga värden. Bestäm samtidigt hur andra referenser till figuren ska fortsätta avse rätt identitet vid återställning.

### M3 · Facit

Centrum är åter `(0,0)` och radien 5. Undo är fortfarande tom och redo innehåller A. Inget lyckat mellanläge notifieras. Om catch återställer men returnerar normalt fortsätter History efter execute och lägger till makrot samt tömmer redo. Modellen kan se återställd ut, men historiken har fått en misslyckad handling och tappat A.

## Kapitel 12. Swing: bygg gränssnittet och måla från modellen

**SW1.** Root har BorderLayout. Dess NORTH-barn är en separat JPanel med FlowLayout som innehåller flera knappar och val. CENTER kan innehålla JTabbedPane med ritytan i fliken Rita och `new JScrollPane(help)` i fliken Hjälp. Om flikarna används är det flikbehållaren, inte en andra konkurrerande rityta, som läggs i root.CENTER. Statusetiketten ligger i SOUTH.

**SW2.** Före klicket visar modellen fortfarande sin tidigare blå cirkel; formulärvalen och visningsmodellen är separata tillstånd. Klicket kopierar valen till modellen, ändrar status och begär repaint. Den senare målningen läser röd linje. När fönstret blir synligt igen läses samma modellvärden, så linjen finns kvar. Programmet behöver inte minnas en gammal Graphics-instans.

**SW3.** Skapa knappen, lägg den i `buttons`, och registrera:

```java
JButton reset = new JButton("Återställ");
buttons.add(reset);
reset.addActionListener(event -> {
    shapes.setSelectedIndex(0);
    colours.setSelectedIndex(0);
    model.select("Cirkel", Color.BLUE);
    status.setText("Cirkel visas i Blå");
    canvas.repaint();
});
```

I detta exempel har komboboxarna ingen egen lyssnare som uppdaterar modellen. Enbart `setSelectedIndex` ändrar därför det som formuläret visar, men lämnar modellens tidigare figur och färg kvar. Lyssnaren återställer alla berörda delar till ett överensstämmande läge.

**SW4.** Swing kan måla av många skäl, så `model.x += 10` kan flytta figuren utan en avsiktlig redigeringshandling. En repaint-begäran ger dessutom inget löfte om en separat målning för varje anrop; den är en begäran om att uppdatera bilden. Låt en avsiktlig händelse ändra modellen, och låt paintComponent enbart läsa och rita dess aktuella tillstånd. EDT samordnar GUI-händelser och målning; långt arbete i en lyssnare hindrar tråden från att svara på nya händelser. F19:s mer utvecklade behandling av långsamt arbete ingår inte i underlaget.

**Kontroll före nästa del:** Du kan starta exemplet, förklara ett klick från registrerad lyssnare till senare målning och ändra fönsterstorlek utan att modellens val förändras.

## Kapitel 13. JSON och lagring: spara betydelsen av ritningen

### J1 · Facit

Version 1, en toppnivåfigur, centrum `(100,80)`, radie 25. `"25"` är en sträng och bryter det visade formatets krav på ett tal. Det är fortfarande giltig JSON-syntax. En tom figures-array anger en tom ritning, inte i sig ett läsfel.

### J2 · Facit

```json
{
  "formatVersion": 1,
  "figures": [
    {
      "type": "group",
      "parts": [
        { "type": "circle", "center": {"x": 0, "y": 0}, "radius": 2 },
        { "type": "circle", "center": {"x": 10, "y": 0}, "radius": 3 }
      ]
    },
    {
      "type": "line",
      "start": {"x": 1, "y": 1},
      "end": {"x": 4, "y": 5}
    }
  ]
}
```

Gruppen kommer före linjen i figures och cirkeln vid origo före den andra cirkeln i parts. Dessa arrayordningar ska bevaras vid läsning och skrivning.

### J3 · Facit

Saknad klammer är syntaxfel. Strängradie är format-/typfel. Negativ numerisk radie är domänfel. Version 99 är ett formatversionsfel om programmet inte stöder den. Vid samtliga misslyckade öppningar behålls det gamla dokumentet, markeringen och historiken. Användaren får återkoppling; programmet ska inte signalera lyckad öppning.

### J4 · Facit

Jämför geometri, figurtyper, stil som formatet stöder, toppnivåordning, varje grupps delordning och nästling. Överlappningen gör ritordningen observerbar. Kräv inte samma Java-instans för gamla och inlästa figurer: de konstrueras på nytt. Kontrollera också projektflödets tomma markering och historik efter lyckad öppning. I feltestet förbereder du ett befintligt dokument med markering och en relevant historik, använder en DrawingStore som kastar IOException och kontrollerar att alla dessa observationer är bevarade, att ingen framgång signaleras och att användaren får ett felmeddelande enligt klientens kontrakt.

## Kapitel 14. UML och helheten: se hur delarna samarbetar

### U1 · Facit

(a) Klassdiagram: relation mellan typer. (b) Objektdiagram: konkreta referenser och alias. (c) Sekvensdiagram: ordnade anrop mellan deltagare. (d) Objektdiagram när du vill visa just de två instanserna; ett klassdiagram kan beskriva att grupper i allmänhet kan ha Figure-barn, men visar inte deras konkreta identitet.

### U2 · Facit

| Klass | Sammanhängande ansvar | Viktiga samarbeten |
|---|---|---|
| Circle | Bevara och använda giltig cirkelgeometri | Point2, DrawingContext |
| DrawingDocument | Hålla ordnad dokumentstruktur och publicera ändringar | Figure, Runnable-lyssnare |
| MoveTool | Tolka gesten till ett mål och en förskjutning | panel/händelsegräns, dokument, History |
| MoveCommand | Utföra och ångra en bestämd flyttning | Figure, DrawingDocument |
| History | Bevara ordningen för undo/redo och anropa handlingar | Command |
| FileDrawingStore | Läsa och skriva dokumentdata med valt format | DrawingSnapshot, JSON-bibliotek, fil-API |

Exakta klasskopplingar beror på labbens API. Viktigast är att Circle skyddar radien, History minns ordningen och lagringsimplementationen känner filformatet. GUI:t väljer hur fel visas för användaren.

### U3 · Facit

Låt G vara grupperingskommandot, M flyttkommandot och R rotationskommandot. Stacktoppen skrivs först.

| Steg | a:s centrum | b:s centrum | Undo | Redo |
|---|---|---|---|---|
| Start | (0,0) | (10,0) | [] | [] |
| G | (0,0) | (10,0) | [G] | [] |
| M | (4,6) | (14,6) | [M,G] | [] |
| R | (-6,4) | (-6,14) | [R,M,G] | [] |
| undo R | (4,6) | (14,6) | [M,G] | [R] |
| undo M | (0,0) | (10,0) | [G] | [M,R] |
| redo M | (4,6) | (14,6) | [M,G] | [R] |

Koordinaterna anges som idealvärden; numerisk rotation testas med tolerans. Efter G och under de återstående redigeringsstegen är toppnivån `[g]` och g:s delar `[a,b]`. Ingen av dessa redigeringar ska klona a eller b. Gruppcentrum efter M är `(9,6)` och efter R `(-6,9)`.

Spara ska fånga det aktuella innehållet: g med a vid `(4,6)` och b vid `(14,6)`, ordning och radier. Själva sparningen ger inte automatiskt ett geometriskt redigeringssteg. Vid lyckad öppning skapas enligt F18 nya instanser med motsvarande sparade värden och struktur, och markering samt historik är tomma. Gamla Java-identiteter behöver därför inte återkomma.

Beständiga lyckade ändringar ska notifieras av den del som äger notifieringen i respektive kontrakt. Används replace, lägg inte till en extra changed. En preview kan däremot kräva ommålning utan beständig dokumentändring. Det är inte korrekt att räkna varje paintComponent som en notifiering eller en ny handling.

### U4 · Facit

Direkt efter undo R är a `(4,6)`, b `(14,6)`, undo `[M,G]` och redo `[R]`. Den ogiltiga flytten ska lämna båda figurerna, gruppstrukturen, ordningen, relevanta markeringar och historiken kvar. Ingen framgångsnotifiering ska ske. R ska fortfarande finnas och kunna göras om, eftersom den nya handlingen aldrig lyckades.

Spara och jämför varje barns geometri, figurobjektens identitet, toppnivå-/barnordning, undo-/redo-läge och notifieringsantal. Kör till sist redo och kontrollera rotationsresultatet. Detta avslöjar både halv mutation, kloning, fel ordning, dubbel notifiering och en redo-väg som raderats för tidigt.