# GDI+
Eine Sammlung von Tipps und Tricks zum Thema Grafikprogrammierung mit GDI+.

## Table of Contents

- [Klassen / Ereignisse](#klassen--ereignisse)
  - [Timer](#timer)
    - [Beispiel Timer](#beispiel-timer)
  - [Graphics](#graphics)
    - [Beispiel Graphics](#beispiel-graphics)
- [Tipps und Tricks](#tipps-und-tricks)
  - [Benötigte Variablen](#benötigte-variablen)
  - [Bewegung animieren](#bewegung-animieren)
  - [Objekte mit Tasten steuern](#objekte-mit-tasten-steuern)
  - [Verhindern, dass ein Spieler aus dem Bild läuft](#verhindern-dass-ein-spieler-aus-dem-bild-läuft)
  - [Spiel pausieren](#spiel-pausieren)
   


## Klassen / Ereignisse
### Timer
Ein Timer führt in regelmäßigen Abständen ein `Tick`-Event aus. Der [`System.Windows.Forms`.`Timer`](https://learn.microsoft.com/de-de/dotnet/api/system.windows.forms.timer?view=windowsdesktop-8.0&viewFallbackFrom=net-6.0) kann über die Toolbox auf die GUI gezogen werden. 

Anschließend können wir 
- die Zeit zwischen jedem `Tick`-Event einstellen (`+ Interval { get; set; }: int`) und
- den Timer starten (`+ Start():void`) oder
- stoppen (`+ Stop():void`) oder
- den Zustand abfragen. (`+ Enabled{ get; set; }: bool`) (Standard ist ausgeschaltet: `enabled = false`)

#### Beispiel Timer

Hier ist ein einfaches Beispiel, das zeigt, wie man den Timer in C# verwendet:

```csharp
using System;
using System.Windows.Forms;

namespace TimerExample
{
    public partial class Form1 : Form
    {
        Timer timer;

        public Form1()
        {
            InitializeComponent();

            // Timer erstellen
            timer = new Timer();
            timer.Interval = 1000; // 1000 Millisekunden = 1 Sekunde
            timer.Tick += Timer_Tick;
        }

        private void Timer_Tick(object sender, EventArgs e)
        {
            // Diese Methode wird bei jedem Tick-Ereignis des Timers aufgerufen
            label1.Text = DateTime.Now.ToString(); // Aktuelle Zeit aktualisieren
        }

        private void btnStart_Click(object sender, EventArgs e)
        {
            // Timer starten
            timer.Start();
        }

        private void btnStop_Click(object sender, EventArgs e)
        {
            // Timer stoppen
            timer.Stop();
        }
    }
}
```

### Graphics

Die [`System.Drawing`.`Graphics`](https://learn.microsoft.com/de-de/dotnet/api/system.drawing.graphics?view=net-6.0) Klasse ist eine zentrale Komponente in der GDI+ (Graphics Device Interface) Grafikbibliothek von .NET. Sie bietet eine umfangreiche Palette von Methoden und Eigenschaften zum Zeichnen von Grafiken auf verschiedenen Oberflächen, wie z. B. auf einem Formular, einem Bitmap-Bild, einem Drucker und mehr.



#### Beispiel Graphics

Hier ist ein einfaches Beispiel, das zeigt, wie man mit der `Graphics`-Klasse ein Rechteck und einen Kreis auf einem Formular zeichnet:

```csharp
private void FrmFrogger_Paint(object sender, PaintEventArgs e)
{
    // Erstelle ein Graphics-Objekt aus dem PaintEventArgs
    Graphics g = e.Graphics;

    // Zeichne ein Rechteck mit blauem Rand und grüner Füllung
    Rectangle rect = new Rectangle(50, 50, 100, 100);
    Pen pen = new Pen(Color.Blue, 2);
    Brush brush = new SolidBrush(Color.Green);
    g.DrawRectangle(pen, rect);
    g.FillRectangle(brush, rect);

    // Zeichne einen roten Kreis mit Mittelpunkt (200, 200) und Radius 50
    Pen redPen = new Pen(Color.Red, 2);
    g.DrawEllipse(redPen, 150, 150, 100, 100);

    //Befüllen aktuelles Hindernis Beispiel
     foreach (Hindernis aktuellesHindernis in alleHindernisse){
        e.Graphics.FillRectangle(
         aktuellesHindernis.Brush,
         aktuellesHindernis.X,
         aktuellesHindernis.Y,
         aktuellesHindernis.Width,
         aktuellesHindernis.Height);
 }
}
```


## Tipps und Tricks
Ergänzen Sie hier die notwendigen Code-Ausschnitte, um zu zeigen, wie man es macht. 
- Sie können [CodeBlöcke mit Syntax-Highlighting](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-and-highlighting-code-blocks#syntax-highlighting) einsetzen
- Wird es zu unübersichtlich? Sie können auch Unterordner mit Beispiel-Code anlegen und auf die entsprechenden Dateien verlinken. [Inspiration](https://github.com/gsoTH/flaskShowcase/tree/master/datenbanken).
- Die folgende Liste kann gerne ergänzt werden :)

### Benötigte Variablen

```csharp
breite = this.ClientSize.Width;
hoehe = this.ClientSize.Height;
```


### Bewegung animieren
Um Bewegungen zu animieren, können Sie die `tmrGameTick_Tick` Methode verwenden, die durch den Timer-Event ausgelöst wird. Innerhalb dieser Methode können Sie die Positionen der Objekte aktualisieren und die `Refresh` Methode aufrufen, um das Fenster neu zu zeichnen und die Animation darzustellen. .Die x- und y-Werte müssen entsprechend angepasst werden, um das Objekt zu bewegen. Dann kann mit den neuen x und y Werten ein neues Objekt erstellt werden und dieses kann nur Liste alleObjekte hinzugefügt werden.


```csharp
private void tmrGameTick_Tick(object sender, EventArgs e)
{
    alleHindernisse.Add(new Hindernis(breite, yWertDerBahn, 60, hoeheJeBereich, hindernisGeschwindigkeit, Color.Red));
    

    foreach (Hindernis aktuellesHindernis in alleHindernisse)
    {
        aktuellesHindernis.Move();
        // .Move = X und Y Werte ändern
    }

    this.Refresh();

}
```

### Objekte mit Tasten steuern
Um Objekte mithilfe von Tasten zu steuern, können Sie den `FrmFrogger_KeyDown` Event verwenden. In diesem Event können Sie die gedrückte Taste überprüfen und entsprechend die Position des Spielers anpassen.

```csharp
private void FrmFrogger_KeyDown(object sender, KeyEventArgs e)
{
    if (e.KeyCode == Keys.Up)
    {
        // Code für Bewegung nach oben
    }
    else if (e.KeyCode == Keys.Down)
    {
        // Code für Bewegung nach unten
    }
    else if (e.KeyCode == Keys.Left)
    {
        // Code für Bewegung nach links
    }
    else if (e.KeyCode == Keys.Right)
    {
        // Code für Bewegung nach rechts
    }
}
```
### Verhindern, dass ein Spieler aus dem Bild läuft

Um sicherzustellen, dass der Spieler nicht aus dem Bild läuft, müssen Sie die Grenzen des Spielfelds berücksichtigen und sicherstellen, dass die Bewegung des Spielers innerhalb dieser Grenzen bleibt. Dies kann durch einfache Überprüfung der Position des Spielers und der Spielfeldgröße in der `FrmFrogger_KeyDown`-Methode erreicht werden. Hier ist eine Möglichkeit, wie Sie dies implementieren können:

```csharp
private void FrmFrogger_KeyDown(object sender, KeyEventArgs e)
{
    if (e.KeyCode == Keys.Up)
    {
        // Spieler darf nicht nach oben aus dem Spielfeld laufen
        if (spieler.Y - hoeheJeBereich >= 0) // Überprüfen, ob die Bewegung nach oben innerhalb des Spielfelds bleibt
        {
            spieler.Y -= hoeheJeBereich;
        }
    }
    else if (e.KeyCode == Keys.Down)
    {
        // Spieler darf nicht nach unten aus dem Spielfeld laufen
        if (spieler.Y + spieler.Height + hoeheJeBereich <= hoehe) // Überprüfen, ob die Bewegung nach unten innerhalb des Spielfelds bleibt
        {
            spieler.Y += hoeheJeBereich;
        }
    }
    else if (e.KeyCode == Keys.Left)
    {
        // Spieler darf nicht nach links aus dem Spielfeld laufen
        if (spieler.X - breite / anzahlBereiche >= 0) // Überprüfen, ob die Bewegung nach links innerhalb des Spielfelds bleibt
        {
            spieler.X -= breite / anzahlBereiche;
        }
    }
    else if (e.KeyCode == Keys.Right)
    {
        // Spieler darf nicht nach rechts aus dem Spielfeld laufen
        if (spieler.X + spieler.Width + breite / anzahlBereiche <= breite) // Überprüfen, ob die Bewegung nach rechts innerhalb des Spielfelds bleibt
        {
            spieler.X += breite / anzahlBereiche;
        }
    }
}
```
### Spiel pausieren

Um das Spiel zu pausieren, können Sie den Timer stoppen und damit die Bewegung der Objekte einfrieren. Dies kann durch das Deaktivieren des Timers erreicht werden. Zusätzlich können Sie eine Pause-Taste implementieren, die den Timer stoppt und das Spiel pausiert.

Hier ist eine Möglichkeit, wie Sie die Spielunterbrechung implementieren können:

```csharp
private void FrmFrogger_KeyDown(object sender, KeyEventArgs e)
{
    if (e.KeyCode == Keys.P) // Überprüfen, ob die Taste (P) gedrückt wurde
    {
        if (tmrGameTick.Enabled) // Überprüfen, ob das Spiel läuft
        {
            // Spiel pausieren
            tmrGameTick.Stop(); // Timer stoppen, um die Bewegung der Objekte anzuhalten
        }
        else
        {
            // Spiel fortsetzen
            tmrGameTick.Start(); // Timer starten, um die Bewegung der Objekte fortzusetzen
        }
    }
}
```
