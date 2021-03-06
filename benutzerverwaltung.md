# Benutzerverwaltung
- [Einleitung](#einleitung)
- [Benutzerliste](#liste)
- [Rollen](#rollen)
- [Anlegen einer Rolle](#rollenerstellen)
  - [Beschreibung](#beschreibung)
  - [Allgemein](#rolleallgemein)
  - [Optionen](#rolleoptionen)
  - [Extras](#rolleextras)
  - [Sprachen](#rollesprachen)
  - [Kategorien](#rollekategorien)
  - [Medienordner](#rollemedienordner)
  - [Module](#rollemodule)
- [Anlegen eines Benutzers](#benutzer)

<a name="einleitung"></a>
## Einleitung
In der Benutzerverwaltung (Benutzer) werden die Redakteure und Administratoren des CMS gepflegt. Du kannst die einzelnen Nutzer mit verschiednenen Rollen versehen und diesen Rollen unterschiedliche Rechte zuweisen. 
Sofern du Administrator bist oder dir das entsprechende Recht erteilt wurde, findest du die Benutzervaltung im Menüpunkt  **Benutzer**. 

<a name="liste"></a>
## Benutzerliste
![Systemcheck](/assets/v5.2.0-Benutzerverwaltung--liste.png)
Benutzerliste

Bei Aufruf der Benutzerverwaltung wird dir die Liste aller System-Benutzer aufgelistet.
Folgende Informationen zu den einzelen Benutzern werden aufgelistet: 
- ID
- Name
- Login
- Rolle
- letzter Login

Zum Bearbeiten eines Benutzers klickst du auf  **editieren**.
Zum Löschen eines Benutzers klickst du auf  **löschen**.
> Das Löschen des aktuell angemeldeten Nutzers (also deines eigenen Accounts) ist nicht möglich. 

[Einen neuen Benutzer](#benutzer) erstellt du durch Klick auf das Plus-Symbol. 

<a name="rollen"></a>
## Rollen 
Redaxo verwendet Rollen-Definitionen um die Rechte einzelner Nutzer oder Gruppen festzulegen. Bevor du neue Benutzer anlegst, macht es Sinn, sich über deren Rechte Gedanken zu machen und entsprechende Rollen zu definieren. 
Redakteure benötigen meist einen Bruchteil der verfügbaren Berechtigungen.  Die Rollen können unter ***Benutzer -> Rollen*** erstellt und editiert werden. 

<a name="rollenerstellen"></a>
## Anlegen einer Rolle
Klicke im Reiter Rollen auf das Plus-Symbol um eine neue Rolle zu erstellen. 
Eine Rollendefinition ist in mehrere Abschnitte unterteilt. Je nach installiertem Addon können weitere Abschnitte hinzukommen. Nachfolgend erklären wir dir die Standardabschnitte, die nach der Installation zur Verfügung stehen. 

<a name="beschreibung"></a>
### Beschreibung
Hinterlege eine Beschreibung für die Rolle um ggf. anderen Administratoren zu erklären für wen diese Rolle gedacht ist oder welche Einschränkungen du durchgeführt hast. 

<a name="rolleallgemein"></a>
### Allgemein

Im Abschnitt Allgemein wird der generelle Zugriff auf ein Addon oder Plugin definiert. Es kann sein, dass der Benutzer auf das Addon zugreifen können muss um die für die Redaktion erforderlichen Funktionen zu nutzen. Er benötigt aber nicht die Funktionen zur Konfiguration des Addons. 

> Ein Beispiel: Ein Benutzer soll eine Tabelle eines Glossars pflegen, aber nicht die Struktur der Tabelle verändern dürfen. 

<a name="rolleoptionen"></a>

### Optionen
Einige Addons oder Plugins haben mehrere zusätzliche Optionen. Diese können in dem Feld Optionen aktiviert werden.

<a name="rolleextras"></a>
### Extras
Hier werden weitere Berechtigungen angezeigt, die eventuell vorher definierte Rechte verfeinern. 

<a name="rollesprachen"></a>
### Sprachen
Manche Benutzer dürfen nur auf bestimmte Sprachen zugreifen. Der Vorteil ist, dass sich diese Benutzer nicht über andere Sprachen Gedanken machen müssen, sondern lediglich über die zugeordnete. Das macht das Backend übersichtlicher und reduziert die Gefahr dass falsche Inhalte manipuliert werden.

<a name="rollekategorien"></a>
### Kategorien
Hier legst Du fest auf welche Kategorien (Rubriken) der Redakteur Zugriff erhält. 

<a name="rollemedienordner"></a>
### Medienordner
Lege hier fest auf welche Medienpool-Kategorien der Benutzer eine Schreibberechtigung erhält. 
> Er besitzt weiterhin das Lese- und Nutzrecht für alle Medien, kann diese aber darüber hinaus nicht bearbeiten. 

<a name="rollemodule"></a>
### Module
Um das Backend für einige Benutzer noch einfacher bzw. übersichtlicher zu gestalten, kann der Zugriff auf wenige Module - oder auch Slices - beschränkt werden. Damit könnte man Benutzer, die lediglich News Pflegen können sollen, nur die News-Module freigeben.

<a name="benutzer"></a>
## Anlegen eines Benutzers
Um einen neuen Benutzer anzulegen klickst Du auf das Plus-Symbol in der Benutzerliste. 

![Systemcheck](/assets/v5.2.0-Benutzerverwaltung--benutzer.png)
Benutzer anlegen

Du kannst folgende Einstellungen durchfühen: 

- Benutzername 
- Passwort (*Kann durch den Nutzer im Profil geändert werden*) 
- Name
- Beschreibung (z.B. Chefredakteur)
- E-Mail-Adresse (wird evtl. von einigen Addons benötigt)
- [Admin](#qdmin)
- User ist aktiv
- [Rolle](#rollen)
- [Startseite](#startseite)
- Backendsprache

<a name="admin"></a>
## Admin
Der Admin erhält alle Rechte und Zugriff auf alle System- und Addonfunktionen. 
> Um Fehlbedienungen zu vermeiden sollten Redakteure geeignete Rechte über eine Rolle erhalten. 

<a name="startseite"></a>
## Startseite
Hier kannst du festlegen welche Seite direkt nach dem Login im Backend aufgerufen werden soll, Standard: Struktur. 
