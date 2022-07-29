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
<?php

<section>
    <div class="container">
        <div class="row">
            <div class="col">REX_VALUE[id="1" output="html"]</div>
        </div>
    </div>
</section>
?>
```
