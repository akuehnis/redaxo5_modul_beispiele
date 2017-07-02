# redaxo5_modul_beispiele

## Modulbeispiele

### Textzeile
Eingabe:
```php
<fieldset class="form-horizontal">
  <div class="form-group">
    <label class="col-sm-2 control-label" for="text_1">Text 1</label>
    <div class="col-sm-10">
      <input type="text" class="form-control" id="text_1" name="REX_INPUT_VALUE[1]" value="REX_VALUE[1]" />
    </div>
  </div>
</fieldset>
```
Ausgabe:
```php
REX_VALUE[1]
```

### Mehrzeiliger Text
Eingabe:
```php
<fieldset class="form-horizontal">
  <div class="form-group">
    <label class="col-sm-2 control-label" for="text_2">Text 2</label>
    <div class="col-sm-10">
        <textarea rows="5" class="form-control" id="text_2" name="REX_INPUT_VALUE[2]" >REX_VALUE[2]</textarea>
    </div>
  </div>
</fieldset>
```
Ausgabe:
```php
<!-- Ohne weitere Angaben werden Umbrüche in <br/> umgewandelt (nl2br) -->
REX_VALUE[2]

<!-- Ausgabe als HTML -->
REX_VALUE[id="2" output="html"]
```

### Auswahlfeld (Dropdown)
Eingabe:
```php
<?php
$options = array(
  ''                    => 'Default',
  'jumbotron'           => 'Jumbotron',
  'alert alert-info'    => 'Info',
  'alert alert-success' => 'Success',
  'alert alert-warning' => 'Warning',
  'alert alert-danger'  => 'Danger',
);
?>
<fieldset class="form-horizontal">
  <div class="form-group">
    <label class="col-sm-2 control-label" for="select_example">Style</label>
    <div class="col-sm-10">
      <select id="select_example" name="REX_INPUT_VALUE[20]" class="form-control">
<?php
foreach($options as $key=>$option){
    echo '<option value="'.$key.'"';
    if( "REX_VALUE[20]" == $key){
        echo ' selected="selected"';
    }
    echo '>'.$option.'</option>';
}
?></select>
    </div>
  </div>
</fieldset>
```
Ausgabe:
```
Gewählt: REX_VALUE[20]
```

### Radio-Auswahl
Eingabe:
```php
<?php
$options = array(
  'wahl1'           => 'Wahl 1',
  'wahl2'           => 'Wahl 2',
  'wahl2'           => 'Wahl 2',
);
?>
<fieldset class="form-horizontal">
  
<?php
foreach($options as $key=>$option){
?>
<div class="checkbox">
<label><input type="radio" name="REX_INPUT_VALUE[19]" value="<?php echo $key;?>"
    <?php                          
    if( "REX_VALUE[19]" == $key){
        echo ' checked="checked"';
    }
    ?> /> <?php echo $option;?></label>
</div>
<?php 
}
?>
</fieldset>
```
Ausgabe:
```php
Gewählt: REX_VALUE[19]
```

### Ein Bild
Eingabe
```php
REX_MEDIA[id=1 widget=1 preview=1]
```
Ausgabe
```php
<?php if(rex::isBackend()):?>
    <img src="index.php?rex_media_type=rex_mediapool_preview&rex_media_file=REX_MEDIA[id=1]" />
<?php elseif ('' != 'REX_MEDIA[id=1]'): ?>
  <img src="<?php echo rex_url::media("REX_MEDIA[id=1]");?>"
       width="REX_MEDIA[id=1 field=width]"
       height="REX_MEDIA[id=1 field=height]"
       alt="REX_MEDIA[id=1 field=title]" />
<?php endif; ?>
```

### Mehrere Bilder
Eingabe
```php
REX_MEDIALIST[id=1 widget=1]
```
Ausgabe
```php
<?php 
if('' == "REX_MEDIALIST[id=1]"):
  $images = array();
else:
  $images = explode(',', "REX_MEDIALIST[id=1]");
endif;
if (rex::isBackend()):?>

<?php foreach($images as $image) :?> 
    <img src="index.php?rex_media_type=rex_mediapool_preview&rex_media_file=<?php echo $image;?>" />
<?php endforeach;?>

<?php else: ?>

<?php foreach($images as $image) :
  $medium = rex_media::get($image);
?>
<img src="<?php echo $medium->getUrl();?>" alt="<?php
  echo $medium->getTitle();?>" height="<?php
  echo $medium->getHeight();?>" width="<?php
  echo $medium->getWidth();?>" />
<?php endforeach; ?>

<?php endif;?> 
```

### Link zu Seite
Eingabe
```php
REX_LINK[id=1 widget=1]
```
Ausgabe
```php
<?php
$art = rex_article::get("REX_LINK[id=1]");
if ($art) {
  $href = rex_getUrl("REX_LINK[id=1]");
  $title = $art->getValue('title');
  $name = $art->getValue('name');
  echo '<a href="'.$href.'" title="'.$title.'" >'.$name.'</a>';
}
?>
```

### Linkliste
Eingabe
```php
REX_LINKLIST[id=1 widget=1]
```

Ausgabe
```php
<?php
if ('' == "REX_LINKLIST[id=1]"){
    $articles = array();
}else{
    $articles = explode(',', "REX_LINKLIST[id=1]");
}
foreach($articles as $article_id){
    $art = rex_article::get($article_id);
    if (null !== $art){
        $href = rex_getUrl($article_id);
        $title = $art->getValue('title');
        $name  = $art->getValue('name');

        echo '<a href="'.$href.'" title="'.$title.'" >'.$name.'</a>';
    }
}
?>
```
### Weiterleitung intern
Eingabe
```php
<fieldset class="form-horizontal">
  <div class="form-group">
    <label class="col-sm-2 control-label" for="text_2">Artikel wird weitergeleitet nach *</label>
    <div class="col-sm-10">
      REX_LINK[id=1 widget=1]
    </div>
  </div>
</fieldset>
```

Ausgabe
```php
<?php
  if (!rex::isBackend() && rex_article::getCurrentId() != 'REX_LINK[id=1]' && 'REX_LINK[id=1]' != '') {
    if (REX_LINK[id=1] != 0) {
      rex_redirect(REX_LINK[id=1], rex_clang::getCurrentId());
    }
  }
  else {
    if (REX_LINK[id=1] != '') {
      echo "Weiterleitung nach <a href='index.php?page=content&article_id=REX_LINK[id=1]&mode=edit'>Artikel REX_LINK[id=1]</a>";
    }
  }
?>
```

### Weiterleitung extern
Eingabe
```php
<fieldset class="form-horizontal">
  <div class="form-group">
    <label class="col-sm-2 control-label" for="text_1">URL</label>
    <div class="col-sm-10">
      <input type="text" class="form-control" id="text_1" name="REX_INPUT_VALUE[1]" value="REX_VALUE[1]" />
    </div>
  </div>
</fieldset>
```

Ausgabe
```php
<?php 
if (rex::isBackend()):
    echo 'Weiterleitung nach: REX_VALUE[1]';
elseif (!empty('REX_VALUE[1]')):
    header("Location: REX_VALUE[1]");
    die();
endif;
?>
```

### Mblock Personenliste

Das Addon mblock muss installiert sein.

Eingabe
```php
<?php
$id = 1; // Mblock wird das Feld value1 in der DB nutzen 
$mform = new MForm(); 
$mform->addFieldset('Person'); 
$mform->addTextField("$id.0.title", array('label'=>'Bezeichnung')); 
$mform->addTextareaField("$id.0.description", array('label'=>'Beschreibung')); 
$mform->addMediaField(1, array('label'=>'Bild')); 
$args = array();
echo MBlock::show($id, $mform->show(), $args);
?>
```

Ausgabe
```php
<?php $list = rex_var::toArray("REX_VALUE[1]");?>
<?php foreach ($list as $row):?>
<div class="row person" >
    <div class="col-xs-3">
    <img class="img-circle img-responsive" src="index.php?rex_media_type=person&rex_media_file=<?php echo 
    $row['REX_MEDIA_1'];?>" />
    </div>
    <div class="col-xs-9 person_text">
    <h6><?php echo $row['title'];?></h6>
    <p><?php echo nl2br($row['description']);?></p>
    </div>
</div>
<?php endforeach;?>
```


### Profil für Redactor2 Editor

```
fullscreen,alignment,bold,italic,underline,deleted,sub,sup,unorderedlist,orderedlist,blockquote,table,cleaner,grouplink[email|external|internal|media],heading1,heading2,heading3,heading4,horizontalrule,media,paragraph,properties,source
```


