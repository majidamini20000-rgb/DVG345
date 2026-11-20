# DVG345 – facit med förklaringar

# Kapitel 1. Objekt, referenser och ansvar

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

# Kapitel 2. Giltiga objekt, Point2 och kontraktstester

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

# Kapitel 3. Interface, arv och dynamisk bindning

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

# Kapitel 4. Samlingar och lambda: lagra objekt och skicka beteenden

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

# Kapitel 5. Händelser, verktyg och Observer: från musgest till modell

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

# Kapitel 6. Composite och rotation

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

# Kapitel 7. Ägarskap, likhet och generiska samlingar

**A1.** Efter gruppering är toppnivån `[b,g]` och `g.parts()` är `[a,c]`. Efter uppdelning är toppnivån `[b,a,c]`. Uppdelningen ersätter gruppen på dess nuvarande plats med dess direkta barn. Den minns inte automatiskt vilka positioner barnen hade före grupperingen. Att ångra en gruppering med exakt återställd ursprungsordning kräver historikinformation, vilket behandlas i Command-delen av kursen.

**A2.** Resultatet blir `[x,a,h,e,y]`. `h` är fortfarande en grupp med `[b,c]`. Den tomma gruppen `e` blir kvar. `g.parts()` ändras inte: även om `g` inte längre ligger på toppnivån innehåller objektet fortfarande samma delar. Det är dokumentets struktur som byts ut.

**A3.** (a) `assertSame(original, group.parts().get(0))`, eftersom identitet ska bevaras. (b) `assertEquals(new Point2(5,7), actual)`, eftersom Point2 har värdelikhet; för beräkningar med känslig flyttalsavrundning kan koordinater kontrolleras med tolerans. (c) kontrollera ordningen enligt Comparatorn eller jämför den förväntade ordnade listans referenser. Samma x säger bara att sorteringsnycklarna är lika. Två olika figurer kan dela den nyckeln.

**A4.** `moveAll(circles)` och `moveAll(figures)` fungerar. `moveAll(objects)` fungerar inte, eftersom Object inte garanterar Figure-beteende. Samtliga tre `addCircle`-anrop fungerar: Circle kan lagras i en lista av Circle, Figure eller Object. `circle` antas vara ett giltigt Circle-objekt. Typmässig tillåtelse säger inte att varje konkret listimplementation tillåter tillägg, men uppgiften antar modifierbara listor.

**A5.** En vanlig HashSet använder `equals` och `hashCode`. Det gör kontrollen beroende av figurernas värdelikhet, trots att dokumentregeln gäller just samma objekt. Använd en identitetsbaserad struktur. Dessutom måste samma `seen` delas genom hela vandringen; annars ser varje gren bara sin egen nya, tomma mängd. Föreläsningens `IdentityHashMap` med en gemensam karta löser båda problemen.

# Kapitel 8. SOLID och design

**D1.** Geometrins giltighetsregler och GUI:ts felpresentation blandas. Circle bör följa sitt kontrakt och avvisa den ogiltiga flytten utan halv mutation. En lämplig anropare i GUI-flödet kan fånga felet och välja dialogruta eller annan återkoppling. Geometriklassen behöver då inte ändras enbart för att presentationssättet byts.

**D2.** Varken den allmänna ritloopen eller gruppens `child.draw(context)` behöver en ny typgren om Triangle uppfyller kontraktet. Triangle behöver egen giltig representation och implementation av figurförmågorna. Skapandeverktyg, GUI-val och lagringsmappning behöver stödja den nya typen. OCP-bedömningen gäller här ritklienten och den aktuella sortens utökning.

**D3.** En klient kan köra `r.setWidth(8); r.setHeight(3);` och därefter kräva bredd 8, höjd 3 och area 24. Om `r` är den beskrivna Square blir den till sist 3×3 och area 9. Ett alternativ är separata kontrakt där Square bara erbjuder operationer som bevarar lika sidor, eventuellt genom att delegera till en intern Rectangle. Du kan också utforma ett annat gemensamt kontrakt som båda faktiskt kan hålla; poängen är att inte lova oberoende måttändringar från Square.

**D4.** (a) Drawable räcker eftersom metoden bara behöver ritförmågan. (b) Movable räcker för flyttförmågan. (c) Figure är rimligt eftersom klienten behöver getCenter och parts enligt föreläsningarnas sammansatta figurkontrakt. Ett mindre interface är bra när det motsvarar det verkliga behovet, men det ska fortfarande erbjuda alla operationer klienten behöver.

**D5.** Låt tjänsten använda `DrawingStore` och få det i konstruktorn. Uppstartskoden väljer en `FileDrawingStore`; testet lämnar in en implementation i minnet. Att tjänstens beroende uttrycks som lagringskontraktet är DIP i detta exempel. Att implementationen levereras utifrån genom konstruktorn är DI. Testet kan då pröva tjänstens beteende utan att dess resultat beror på åtkomst till en verklig fil.

# Kapitel 9. Command och historik: gör handlingar möjliga att ångra

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

# Kapitel 10. Undantag, kontraktstester och säker refaktorering

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

# Kapitel 11. Memento: ett stabilt tillstånd att återvända till

### M1 · Facit

Flyttningen har en enkel motsatt operation: `(dx,dy)` följs av `(-dx,-dy)`. Ett makro kan innehålla operationer där tidigare värden krävs eller där ett sent fel kommer efter tidigare mutationer. Ett före-läge kan då ge en samlad återställning. Memento är inte enda lösningen; exempelvis fullständig förkontroll eller korrekt kompensering kan passa andra kontrakt. Valet beror på vad som måste kunna återställas.

### M2 · Facit

Två listor pekar på samma figur. En skyddad lista hindrar add/remove, men hindrar inte move på elementet. Spara stabila geometrivärden/strukturer eller använd en representation med oföränderliga värden. Bestäm samtidigt hur andra referenser till figuren ska fortsätta avse rätt identitet vid återställning.

### M3 · Facit

Centrum är åter `(0,0)` och radien 5. Undo är fortfarande tom och redo innehåller A. Inget lyckat mellanläge notifieras. Om catch återställer men returnerar normalt fortsätter History efter execute och lägger till makrot samt tömmer redo. Modellen kan se återställd ut, men historiken har fått en misslyckad handling och tappat A.

# Kapitel 12. Swing: bygg gränssnittet och måla från modellen

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

# Kapitel 13. JSON och lagring: spara betydelsen av ritningen

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

# Kapitel 14. UML och helheten: se hur delarna samarbetar

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