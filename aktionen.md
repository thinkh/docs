# Aktionen

Es gibt in der Standardkonfiguration von REDAXO drei Events bzw. Aktionen für Module die automatisch ausgeführt werden.

Aktion | Beschreibung
------------- | -------------
Preview	| Wird vor dem Anzeigen des Moduls ausgeführt.
Presave	| Wird vor dem Speichern des Moduls ausgeführt.
Postsave | Wird nach dem Speichern des Moduls ausgeführt.


## Aktion erstellen

Es ist wichtig die korrekte Aktion zu verwenden, Presave eignet sich sehr gut um Daten vor dem Speichervorgang zu Manipulieren. Postsave eignet sich um Daten nach dem Speichervorgang weiter zu verarbeiten.

### Auf Werte zugreifen

Normalerweise werden beispielsweise in Presave-Aktionen Werte überprüft bzw. verändert, die zuvor in einem Modulblock eingegeben wurden. Auf die entsprechenden Werte kann mit

    $text = $this->getValue(1);  // Beispiel für REX_VALUE[1]
    
oder

    $medialist1 = $this->sql->getValue('medialist1'); // Beispiel für das Feld medialist1

zugegriffen werden.

Wenn die Werte in einer Presave-Aktion geändert werden, so können diese mit

    $this->setValue(1,$text); // Beispiel für REX_VALUE[1]
    
oder 

    $this->sql->setValue('medialist2','ichbinaucheinbild.jpg'); // Beispiel für das Feld medialist2

gespeichert werden.

Es kann auf alle Werte im Slice zugegriffen werden. Auch eigene Datenfelder, die zuvor in der Tabelle rex_article_slice angelegt wurden, sind möglich.


## Beispiele

### Preview Aktion

In einem Modul soll es ein Feld für die Datumseingabe geben, das Feld soll aber änderbar sein. Beim Anlegen soll das Feld mit dem aktuellen Wert belegt werden. Das Beispiel geht davon aus, dass es sich um das Feld REX_VALUE[2] handelt.

    <?php
    if ($this->getValue(2) == '') {
       $this->setValue(2,date('d.m.Y'));
    }
    ?>


### Presave Aktionen

#### Beispiel 1: HTML Text aus Wysiwyg Editoren modifizieren

In diesem Beispiel wird eine Presave Aktion gezeigt. In dieser Aktion geht es darum, HTML Text aus Wysiwyg Editoren (z.B. Redactor, TinyMCE) zu prüfen und gegebenenfalls zu ändern.

Diesen Code ins Feld "Presave-Aktion" eintragen und den Events Add und Edit zuweisen

    <?php
    // REX_VALUE[1] auslesen
    $text = $this->getValue(1);
    // alle style="..." Stellen löschen
    $text = preg_replace('/ style=".*?"/', '', $text);
    // alle leeren Absätze löschen
    $text = preg_replace('/<p>\s*<\/p>/', '', $text);
    // alle Urls aus Ankerlinks löschen
    $text = preg_replace('/ href="(.*?)#(.{1,30})"/', ' href="#$2"', $text);
    // Alle Zeilenumbrüche raus
    $text = preg_replace("/\r|\n|\r\n/", ' ', $text);
    // ersetzten Text wieder in REX_VALUE[1] schreiben
    $this->setValue(1,$text);                            
    ?>

Die fertige Aktion dann dem Modul mit dem Wysywig Text zuweisen. Das Modul muss den Text für dieses Beispiel in die REX_VALUE[1] speichern.
    
#### Beispiel 2: Multiselect aus Datenbank speichern

Um ein Multiselectfeld in einem Modul für das Backend zu erstellen, muss man einen kleinen Umweg gehen, da die REX_VALUE-Felder Text zurückgeben, Werte aus Multiselectfeldern aber sinnvollerweise als Array verarbeitet werden.
Die im Multiselect ausgewählten Werte (in diesem Falle Ids) sollen kommasepariert im REX_VALUE[1] abgelegt werden.

Der komplette Modulinput kann dann beispielsweise so aussehen:

    <?php
      // Datenbankelemente für Multiselecfeld auslesen
      $sql = rex_sql::factory();
      $qry = "SELECT name, id FROM rex_meinetabelle ORDER BY name";
      $sql->setQuery($qry);
      $res = $sql->getArray();

      // Multiselectwerte aus REX_VALUE[1] auslesen 
      $multiselect = explode(',',"REX_VALUE[1]");
    ?>

    // Selectfeld kann einen beliebigen Namen haben, muss aber als Array definiert werden
    <select name="my_multiselect[]" multiple="multiple" size="5">
      <?php foreach ($res as $o) : ?>
       <option value="<?= $o['id'] ?>" <?= in_array($o['id'],$multiselect) ? 'selected="selected" ' : '' ?>><?= $o['name'] ?> ?></option>
       <?php endforeach ?>
    </select>
    
Die hierzu passende Presave Aktion nimmt die Werte von my_multiselect entgegen, macht einen kommaseparierten String daraus und gibt den String an value1 weiter.

    <?php
    $this->sql->setValue('value1',implode(',',rex_post('my_multiselect','array')));
    ?>
    
#### Beispiel 3: Validierung

Eingaben können in REDAXO Modulen per Javascript validiert werden, doch manchmal ist eine Aktion die bessere Wahl. Auch hierzu wird wieder die Presave Aktion verwendet. Im Beispiel wird geprüft, ob REX_VALUE[1] leer ist. Ist die Bedingung erfüllt, wird der Block nicht gespeichert und eine Meldung ausgegeben.

    <?php
    if ($this->getValue(1) == '') {
       // Der Block wird nicht gespeichert
       $this->save = false;
       // Meldung ausgeben
       $this->messages[] = 'Bitte Wert in Feld 1 eintragen.';   
    }
    ?>



### Postsave Aktion

Postsave Aktionen werden hauptsächlich verwendet, um nach dem Speichern des Slices weitere Aktionen durchzuführen, z.B. eine Benachrichtung absetzen oder eine Meldung auf Twitter zu hinterlegen. Hierfür wird manchmal auch die Id des Slices benötigt. Die Slice Id wird jedoch erst beim Speichervorgang vergeben und ist daher bei einem neu angelegten Slice noch nicht direkt verfügbar. Im Beispiel seht ihr, wie man dann trotzdem an die Slice Id kommt. Das Beispiel zeigt ebenfalls, wie man an die Werte von REX_ARTICLE_ID, REX_CLANG_ID, REX_CTYPE_ID und REX_MODULE_ID kommt.

    <?php
    $slice_id = "REX_SLICE_ID";
    if ($slice_id == 0 && $this->event == 'add') {
       $qry = 'SELECT id id FROM '.rex::getTablePrefix().'article_slice WHERE '
             . 'clang_id = REX_CLANG_ID AND '
             . 'ctype_id = REX_CTYPE_ID AND '
             . 'article_id = REX_ARTICLE_ID AND '
             . 'module_id = REX_MODULE_ID AND '
             . 'createuser = "'.rex::getUser()->getValue('login').'"'
             . ' ORDER BY id DESC';

       $sql = rex_sql::factory();
       $sql->setQuery($qry);
       if ($res = $sql->getArray()) {
          $slice_id = $res[0]['id'];
       }   
    }
    ?>

  > **Hinweis:** 
Eine Aktion kann einem Modul erst zugewiesen werden, wenn das Modul gespeichert wurde. Es ist also nicht möglich, die Aktion direkt bei der Erstellung eines Moduls zuzuweisen. Die Auswahlmöglichkeit für die Zuweisung einer Aktion zu einem Modul erscheint erst, wenn das Modul bereits existiert.

