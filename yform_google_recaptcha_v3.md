# Verwendung von Google Recpatcha V3 in Yform

Implementierung:

1. Erstelle eine Klasse mit der Validierungsfunktion
2. Yform-Formbuilder implementierung

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
            // is invalid
            return true;
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
            // is valid
            return false;
        }
        // is invalid
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

Informationen zu den Zeilen:

* html|gcv3|...: Sendebutton, RECAPTCHA-PUBLIC-KEY ist zu ersetzen.
* html|gccallback|...: Callback-Funktion für Google Recaptcha. Mehr Infos:https://developers.google.com/recaptcha/docs/v3. 
* html|gcscript|...: Google-Recaptcha API-Script. Falls dieses schon im Theme geladen wird sollte diese Zeile weg gelassen werden.
* submit|name|... : wird hinzugefügt, damit Yform nicht automatisch einen Senden-Button anhängt. Die Klasse 'd-none' blendet den Button aus, gilt aber nur für Bootstrap 4 CSS.
* validate|customfunction|...: Die Validierungs-Funktion. RECAPTCHA-PRIVATE-KEY ist zu ersetzen.

## Yform-Formbuilder als Modal-Formular

Wenn das Formular in einem Modal-Window geladen wird, dann muss die Captcha-Callback-Funktion nicht submit() aufrufen sondern die entsprechende Javascript Funktion welche den Ajax-Call macht.

```
html|gcv3|gcv3|<button class="btn btn-primary g-recaptcha" data-sitekey="RECAPTCHA-PUBLIC-KEY" data-callback='sendeKontaktFormular' data-action='submit'>Senden</button>
html|gccallback|gccallback|<script>function sendeKontaktFormular(token) { onCtaModalSubmit(); }</script>
html|gcscript|gcscript|<script src="https://www.google.com/recaptcha/api.js"></script>
submit|name|Senden (versteckt)||||d-none
validate|customfunction|gcv3|GoogleCaptchaV3::isInvalidValidCaptcha|RECAPTCHA-PRIVATE-KEY|Captcha-Prüfung fehl geschlagen|normal
```

Dazu zum Beispiel Javascript wie folgt: 
```
// Modal-Ajax-Kontaktformular

const onCtaModalLoad = async function(evt){
	evt.preventDefault();
	let modal = document.getElementById("ctaModal")
	let url = modal.getAttribute("data-ajax-action")
	let response = await fetch(url)
	let content = await response.text()
	$("#ctaModalBody").html(content)
	$("#ctaModal").modal("show")
	$("#ctaModalThema").val(thema)
	$("#ctaModal form").on("submit", onCtaModalSubmit)
}

const onCtaModalSubmit = async function(evt){
	if (evt && evt.hasOwnProperty('preventDefault')){
		evt.preventDefault();
	}
    let modal = document.getElementById("ctaModal")
	let url = modal.getAttribute("data-ajax-action")
	let form = document.querySelector("#ctaModal form")
	let formData = new FormData(form)
	let response = await fetch(url, {
		method: "POST",
		body: formData
	})
	let content = await response.text()
	$("#ctaModalBody").html(content)
	$("#ctaModal form").on("submit", onCtaModalSubmit)
}
document.addEventListener("DOMContentLoaded", function(){
	$("button[target=ctaModal]").on("click", onCtaModalLoad)
})

// Modal-Ajax-Kontaktformular
```
