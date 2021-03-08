# Verwendung von Google Recpatcha V3 in Yform

Implementierung:

1. Erstelle eine Klasse mit der Validierungsfunktion
2. Hinzufügen des Senden-Buttons und ausblenden des Default-Buttons

## PHP-Klasse mit Validierungsfunktion

```
<?php
// redaxo/src/addons/project/lib/GoogleCaptchaV3.php

class GoogleCaptchaV3
{


    /**
     * Aufruf siehe lib/yform/validate/customfunction.php
     */
    public static function isInvalidValidCaptcha($names, $ObjectValues, $private_key) 
    {

        $url = "https://www.google.com/recaptcha/api/siteverify";

        if (!isset($_POST['g-recaptcha-response'])) {
            return false;
        }

        $data = [
            'secret' => $private_key,
            'response' => $_POST['g-recaptcha-response'],
        ];
        
        $ch = curl_init();
        curl_setopt($ch, CURLOPT_URL, $url);
        curl_setopt($ch, CURLOPT_POST, 1);
        curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
        $server_output = curl_exec($ch);
        curl_close ($ch);
        
        $ret = json_decode($server_output, true);

        if (is_array($ret) && isset($ret['success']) && true == $ret['success']) {
            return false;
        }
        return true;

    }
}
```


## Yform-Formbuilder

```
html|gcv3|gcv3|<button class="btn btn-primary g-recaptcha" data-sitekey="RECAPTCHA-PUBLIC-KEY" data-callback='sendeKontaktFormular' data-action='submit'>Senden</button>
html|gccallback|gccallback|<script>function sendeKontaktFormular(token) { document.getElementById("formular").submit(); }</script>
html|gcscript|gcscript|<script src="https://www.google.com/recaptcha/api.js"></script>
submit|name|Senden (versteckt)||||d-none
validate|customfunction|gcv3|GoogleCaptchaV3::isInvalidValidCaptcha|RECAPTCHA-PRIVATE-KEY|Captcha-Prüfung fehl geschlagen|normal
```

Die Zeile submit|... wird hinzugefügt, damit Yform nicht automatisch einen Senden-Button anhängt. Die Klasse 'd-none' blendet den Button aus, gilt aber nur für Bootstrap 4 CSS. 


