# GDI+
Eine Sammlung von Tipps und Tricks zum Thema Grafikprogrammierung mit GDI+.

## Table of Contents

- [Timer](#timer)
- [Tipps und Tricks](#tipps-und-tricks)
  - [Bewegung animieren](#bewegung-animieren)
  - [Objekte mit Tasten steuern](#objekte-mit-tasten-steuern)
  - [Verhindern, dass ein Spieler aus dem Bild läuft](#verhindern-dass-ein-spieler-aus-dem-bild-läuft)
  - [Spiel pausieren](#spiel-pausieren)
- [Klassen / Ereignisse](#klassen--ereignisse)
  - [Timer](#timer)

## Klassen / Ereignisse
### Timer
Ein Timer führt in regelmäßigen Abständen ein `Tick`-Event aus. Der [`System.Windows.Forms`.`Timer`](https://learn.microsoft.com/de-de/dotnet/api/system.windows.forms.timer?view=windowsdesktop-8.0&viewFallbackFrom=net-6.0) kann über die Toolbox auf die GUI gezogen werden. 

Anschließend können wir 
- die Zeit zwischen jedem `Tick`-Event einstellen (`+ Interval { get; set; }: int`) und
- den Timer starten (`+ Start():void`) oder
- stoppen (`+ Stop():void`) oder
- den Zustand abfragen. (`+ Enabled{ get; set; }: bool`) (Standard ist ausgeschaltet: `enabled = false`)



## Tipps und Tricks
Ergänzen Sie hier die notwendigen Code-Ausschnitte, um zu zeigen, wie man es macht. 
- Sie können [CodeBlöcke mit Syntax-Highlighting](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-and-highlighting-code-blocks#syntax-highlighting) einsetzen
- Wird es zu unübersichtlich? Sie können auch Unterordner mit Beispiel-Code anlegen und auf die entsprechenden Dateien verlinken. [Inspiration](https://github.com/gsoTH/flaskShowcase/tree/master/datenbanken).
- Die folgende Liste kann gerne ergänzt werden :)

### Bewegung animieren
Um Bewegungen zu animieren, können Sie die `tmrGameTick_Tick` Methode verwenden, die durch den Timer-Event ausgelöst wird. Innerhalb dieser Methode können Sie die Positionen der Objekte aktualisieren und die `Refresh` Methode aufrufen, um das Fenster neu zu zeichnen und die Animation darzustellen.

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
