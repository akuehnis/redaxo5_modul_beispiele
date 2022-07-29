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
