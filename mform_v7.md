# Modul-Vorlagen für MForm Version 7

Ab Version 7 von MForm

### Redactor-Editor

Input

```
<?php 
$mform = Mform::factory();
$mform->addTextareaField(1, [
    'label' => 'Text',
    'class' => 'form-control redactor-editor--full']);
$mform->addSelectField("2.bgColor", [
  'bg-transparent' => 'Transarent', 
  'bg-white' => 'White', 
  'bg-hellgrau' => 'Light grey'
  ], [
  'label' => 'Hintergrund-Farbe'
  ]);

$mform->addSelectField("2.bgLayout", [
    "boxed" => "Boxed",
    "boxed_bg_fluid" => "Boxed, Hintergrund bis Aussen",
    "fluid" => "Hintergrund und Inhalt bis Aussen"], 
    ['label' => 'Block Layout']);

echo $mform->show();
?>
```
Output
