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
<?php if ('' != REX_MEDIA[id=1]):?>
<div class="container">
    <img src="/media/REX_MEDIA[id=1]" class="img-fluid" />
</div>
<?php endif; ?>
```
