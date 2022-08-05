# Modul-Vorlagen für MForm Version 7

Ab Version 7 von MForm

### Redactor-Editor

Input

```
<?php 
$mform = Mform::factory();
$fieldset = Mform::factory();
$fieldset->addTextareaField(1, [
    'label' => 'Text',
    'class' => 'form-control redactor-editor--full']);
$mform->addFieldsetArea('Inhalt', $fieldset);

echo $mform->show();
?>
```

Output
```

<div class="container">
    REX_VALUE[id="1" output="html"]
</div>

```

### Bild

Input 

```
<?php
$mform = MForm::factory();
$mform->addMediaField(1, array('label'=>'Bild'));
echo $mform->show();
``` 

Output

```
<?php if ('' != "REX_MEDIA[id=1]"):?>
<div class="container">
    <img src="/media/REX_MEDIA[id=1]" class="img-fluid" />
</div>
<?php endif; ?>
```

### Mehrere Bilder

Input 

```
<?php
$mform = MForm::factory();
$mform->addImagelistField(2, ['label' => 'Image List'])
echo $mform->show();
?>
``` 

Output

```
dump('REX_MEDIALIST[id=2]');
```

### Select

Input 

```
$group = MForm::factory();
// addSelectField($id, array $options, array $attributes, int $size, string $defaultValue)
$group->addSelectField(
    "2.type", 
    [
        'news' => 'News', 
        'customer' => 'Kunde', 
        'event' => 'Event'
        
    ],
    [
        'label' => 'Typ', 
        'default' => 'event'
    ], 
    1,
    'event');
$mform = MForm::factory();
$mform->addFieldsetArea('Typ-Auswahl', $group);
echo $mform->show();
?>
```
Output

```
<?php 
dump(rex_var::toArray('REX_VALUE[id=2]'));
?>
```

### Weiterleitung intern

Input

``` 
<?php 
$fieldset = Mform::factory();
$fieldset->addLinkField(1, ['label' => 'Zielseite']);
$mform = MForm::factory();
$mform->addFieldsetArea('Weiterleitung', $fieldset);
echo $mform->show();
?>
```
Output
```
<?php
  if (!rex::isBackend() && rex_article::getCurrentId() != 'REX_LINK[id=1]' && 'REX_LINK[id=1]' != '') {
    if (intval(REX_LINK[id=1]) != 0) {
      rex_redirect(intval(REX_LINK[id=1]), rex_clang::getCurrentId());
    }
  }
  else {
    if (REX_LINK[id=1] != '') {
      echo "Weiterleitung nach <a href='index.php?page=content&article_id=REX_LINK[id=1]&mode=edit'>Artikel REX_LINK[id=1]</a>";
    }
  }
?>
```

### Tabs im Eingabebereich

Input 
```
<?php 
$mform = Mform::factory();
$fieldset = Mform::factory();
$fieldset->addTextareaField(1, [
    'label' => 'Text',
    'class' => 'form-control redactor-editor--full']);
$mform->addTabElement('Inhalt', $fieldset, /*active*/ true, /*align right*/ false);

$fieldset = Mform::factory();
$fieldset->addSelectField("2.bgColor", [
  'bg-transparent' => 'Transarent', 
  'bg-white' => 'White', 
  'bg-hellgrau' => 'Light grey'
  ], [
  'label' => 'Hintergrund-Farbe'
  ]);
$fieldset->addSelectField("2.insetY", [
    'py-0' => 'Keiner', 
    'py-1' => 'Sehr klein',
    'py-2' => 'Klein',
    'py-3' => 'Mittel',
    'py-4' => 'Gross',
    'py-5' => 'Sehr Gross',
], [
'label' => 'Innenabstand oben und unten'
]);

$fieldset->addSelectField("2.boxLayout", [
    "boxed" => "Boxed",
    "boxed_bg_fluid" => "Boxed, Hintergrund bis Aussen",
    "fluid" => "Hintergrund und Inhalt bis Aussen"
    ], 
    ['label' => 'Block Layout']);
$mform->addTabElement('Layout', $fieldset, /*active*/ false, /*align right*/ false);

echo $mform->show();
?>
```
