# DVG345 – övningsbok

Lös först utan facit. Alla uppgifter och deras sammanhang förklaras i lärhandboken. Utdrag förutsätter angivna kurstyper; det här är inget fristående labbprojekt.

# Kapitel 1. Objekt, referenser och ansvar

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

**HO1 – Följ cirkelgesten.** Anropa `pressed((2,3), false)`, `dragged((5,7))`, `cancel()` och sedan `released((5,7))`. Hur många cirklar skapas? Vad blir skillnaden om cancel tas bort? Beskriv också dokumentet före släpp.

**HO2 – Beräkna flyttningen.** Anta att musned utan Shift träffar en giltig målfigur och att alla flyttningar lyckas. Start är `(10,10)`, två dragpunkter är `(13,12)` och `(15,16)`, släpp sker på `(18,17)`. Beräkna varje delta i F6-versionen. Jämför med F15-versionens enda kommando. Vad händer om Escape körs i stället för släpp, direkt efter den andra dragpunkten?

**HO3 – Pröva notifieringen utan fönster.** Skriv en Runnable-lyssnare som räknar anrop. Registrera den och kontrollera att registreringen inte ökar räknaren. Anropa `changed()` två gånger och kontrollera resultatet. Beskriv hur testet kompletteras vid en avvisad flyttning.

**HO4 – Hitta två ansvarsfel.** Ett CircleTool kör `document.add(circle); document.changed();`. Ett Line.move tilldelar `start` innan det beräknar nästa `end`. Förklara problemen och skriv principen för korrigeringen. Varför räcker det inte att lägga en catch i History?

# Kapitel 6. Composite och rotation

**Övning C1 – Följ anropen.** En grupp `root` har barnen `a` och `g`. Gruppen `g` har barnen `b` och `c`. Alla tre grundfigurer överlappar. Skriv anropsordningen för `root.draw(ctx)`. Vilken grundfigur ritas sist?

**Övning C2 – Räkna centrum.** `g` har barncentrum `(2, 4)` och `(6, 8)`. `root` har barnen `g` och en cirkel med centrum `(20, 10)`. Beräkna båda gruppcentrum. Beräkna även vad du skulle få om du felaktigt tog medelvärdet av alla löv.

**Övning C3 – Hitta rotationsfelet.** En grupps implementation gör `for (Figure child : parts) child.rotate(radians);`. Gruppen ska roteras ett kvarts varv kring `(0, 0)`. Barnen är cirklar vid `(10, 0)` och `(20, 0)`. Vad blir fel? Visa den avgörande kodraden som måste ändras.

**Övning C4 – Skriv ett kontraktstest.** Skriv ett JUnit-test som använder de två cirklarna från C3 och kontrollerar resultatet av en gemensam rotation. Använd tolerans för beräknade `double`-värden.

**Övning C5 – Resonera om felvägen.** En grupp har tre barn. En föreslagen flyttning är giltig för de första två men ogiltig för det tredje. Beskriv vad som kan gå fel i en enkel flyttloop och hur ett test ska skilja en säker implementation från en halv mutation.

# Kapitel 7. Ägarskap, likhet och generiska samlingar

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

**Övning D1 – Hitta ansvarsblandningen.** `Circle.move` räknar nya koordinater och visar dessutom en Swing-dialog vid ogiltig flytt. Vilka två skäl till förändring blandas? Beskriv ett bättre samarbete utan att skriva ett helt program.

**Övning D2 – Utöka med Triangle.** En klient har den polymorfa ritloopen ovan och en grupp delegerar draw till sina barn. Vilka av dessa behöver ändras för att rita Triangle? Vilka andra delar kan ändå behöva ny kod för att användaren ska få komplett triangelstöd?

**Övning D3 – Pröva utbytbarhet.** Rectangle-kontraktet lovar att `setWidth(8)` bevarar höjden och att `setHeight(3)` bevarar bredden. En Square-implementation sätter båda måtten lika i varje setter. Visa en konkret klientsekvens som avslöjar kontraktsbrottet och föreslå ett alternativ.

**Övning D4 – Välj parametertyp.** Vilket av Drawable, Movable och Figure är ett rimligt kontrakt för (a) en metod som bara anropar draw, (b) en metod som bara anropar move, (c) en metod som både frågar efter centrum och vandrar direkta delar? Motivera utifrån behov, inte namnets storlek.

**Övning D5 – Skilj DIP från DI.** En `EditorService` skapar själv `new FileDrawingStore(...)`. Du vill kunna testa tjänsten utan en verklig fil. Beskriv vilket kontrakt tjänsten ska använda, hur implementationen lämnas in och vilken kod som väljer mellan fil och minne. Förklara vad som är DIP respektive DI.

# Kapitel 9. Command och historik: gör handlingar möjliga att ångra

### H1 · Spåra stackarna · grund

Utför `execute(A), execute(B), execute(C), undo(), undo(), redo(), execute(D)`. Skriv båda stackarna efter varje steg, med toppen först. Vilka kommandon kan till sist ångras respektive göras om?

### H2 · Hitta historikfelet · mellan

En metod gör `redo.clear(); command.execute(); undo.push(command);`. Förklara felet med ett konkret läge där redo innehåller B. Skriv den korrigerade ordningen och förklara vad som händer om execute kastar.

### H3 · Räkna en gest · mellan

Start är `(2,3)`. Tre dragged-händelser har punkterna `(5,4)`, `(8,7)` och `(9,9)`. Släpp sker vid `(12,13)`. Vilket flyttkommando skapas i F15-varianten? Hur många beständiga flyttningar sker under dragged? Vad ska Escape före släpp göra?

### H4 · Utforma ett historiktest · fördjupning

Du har kört ett giltigt A och sedan undo. Beskriv ett test som försöker köra ett ogiltigt B. Testet ska kontrollera mer än att ett undantag kastas. Avsluta med redo av A. Vilket fel i History och vilket fel i Figure kan testet skilja på?

# Kapitel 10. Undantag, kontraktstester och säker refaktorering

### E1 · Följ kontrollflödet · grund

En metod skriver A i try, kastar IOException, skriver B efter kastet, skriver C i catch för IOException, D i finally och E efter hela konstruktionen. Vilka bokstäver skrivs? Vad ändras om catch tas bort och metoden deklarerar throws IOException? Behandla detta som en flödesskiss, inte som kod med en direkt onåbar sats efter throw.

### E2 · Korrigera en checked felväg · mellan

`static String read(Path p) { return Files.readString(p); }` kompilerar inte. Visa två principiellt olika lösningar. Förklara vem som väljer åtgärd i respektive lösning och varför `throws` inte betyder att felet är löst.

### E3 · Testa mer än undantaget · mellan

En grupp med tre delar ska flyttas. Den sista delens föreslagna koordinat blir oändlig. Skriv en testplan som upptäcker om de första två delarna har flyttats innan felet uppstår. Ta även med notifiering och historik.

### E4 · Välj rätt assertion · fördjupning

Välj assertion och motivera: (a) en grupp innehåller samma Circle-instans som före gruppering; (b) en mittpunkt är `(5,7)`; (c) en beräknad x-koordinat är ungefär 0.15; (d) en konstruktor avvisar NaN. Räcker sista kontrollen för att visa att en redan befintlig modell är oförändrad?

# Kapitel 11. Memento: ett stabilt tillstånd att återvända till

### M1 · Välj strategi · grund

Förklara varför en ensam flyttning kan ångras med en invers, medan ett makro med flera olika delsteg kan behöva ett sparat före-läge. Är Memento enda möjliga lösningen?

### M2 · Kopian som ändrades · mellan

`before = List.copyOf(figures)` följs av `figures.get(0).move(10,0)`. Figuren är muterbar. Varför visar även `before.get(0)` den nya geometrin? Rita referenserna och föreslå vad som måste sparas för ett stabilt tillstånd.

### M3 · Makro med fel · fördjupning

Start: centrum `(0,0)`, radie 5, undo tom, redo innehåller A. Makrot flyttar till `(20,0)` och försöker sedan sätta radien till -1. Beskriv slutläget med F14:s återställning. Vad blir fel om catch återställer men inte kastar vidare?

# Kapitel 12. Swing: bygg gränssnittet och måla från modellen

**SW1 – Förklara komponentträdet.** Rita en JFrame med en root-panel. Root har knapprad NORTH, rityta CENTER och status SOUTH. Ange layout för root och knapprad. Var ska en JScrollPane placeras om en hjälpyta ska ligga i en separat flik?

**SW2 – Följ värdena.** Starta exemplet på nytt. Du väljer Linje och Röd men har ännu inte klickat Visa. Vilken figur visar exemplet? Vad ändras vid klicket? Vad händer efter att fönstret täcks över och sedan blir synligt igen?

**SW3 – Lägg till Återställ.** Utöka exemplet med en knapp som återställer både kontroller, modell och status till blå cirkel och begär ommålning. Skriv lyssnaren. Förklara varför det inte räcker att endast återställa komboboxarna.

**SW4 – Felsök en målning.** En student lägger `model.x += 10;` i paintComponent och säger att repaint betyder ”flytta figuren”. Förklara två problem. Beskriv rätt ansvarsfördelning och varför muslyssnaren bör göra kort arbete på EDT.

# Kapitel 13. JSON och lagring: spara betydelsen av ritningen

### J1 · Läs data · grund

Utgå från den första cirkelfilen i kapitlet. Ange formatversion, antal toppnivåfigurer, centrum och radie. Vad ändras i betydelse om radius blir `"25"`? Vad betyder en tom figures-array?

### J2 · Skriv en nästlad ritning · mellan

Skriv ett JSON-dokument med version 1. Första toppnivåfiguren ska vara en grupp med cirklarna `(0,0), radie 2` och `(10,0), radie 3`. Andra toppnivåfiguren ska vara en linje från `(1,1)` till `(4,5)`. Bevara den angivna ordningen.

### J3 · Placera felet i rätt lager · mellan

Klassificera fyra fel: sista klammern saknas; radius är `"25"`; radius är -25; formatVersion är 99. Vad ska hända med det gamla dokumentet vid vart och ett av felen?

### J4 · Utforma round-trip och feltest · fördjupning

Du sparar en nästlad grupp med överlappande figurer och läser den igen. Lista vilka observationer som ska jämföras och vilka identitetskrav som inte följer av filformatet. Beskriv sedan ett separat test där load kastar IOException och det gamla dokumentet ska överleva.

# Kapitel 14. UML och helheten: se hur delarna samarbetar

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