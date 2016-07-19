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
        echo ' selected="selected"';
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
<img src="<?php echo rex_url::media("REX_MEDIA[id=1]");?>"
     width="REX_MEDIA[id=1 field=width]"
     height="REX_MEDIA[id=1 field=height]"
     alt="REX_MEDIA[id=1 field=title]" />
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
foreach($images as $image) { 
  $medium = rex_media::get($image);
?>
<img src="<?php echo $medium->getUrl();?>" alt="<?php
  echo $medium->getTitle();?>" height="<?php
  echo $medium->getHeight();?>" width="<?php
  echo $medium->getWidth();?>" />
<?php } ?> 
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
?>
<a href="<?php echo rex_url::frontendController(array(
'article_id' => "REX_LINK[id=1]",
));?>" title="<?php
echo $art->getValue('name');?>" /><?php echo $art->getValue("name");?></a>
<?php } ?>
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
        $href = rex_url::frontendController(array(
               'article_id' => $article_id,
              ));
        $title = $art->getValue('title');
        $name  = $art->getValue('name');

        echo '<a href="'.$href.'" title="'.$title.'" >'.$name.'</a>';
    }
}
?>
```
