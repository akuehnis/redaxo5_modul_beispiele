# YCom Reset Password form

Mailprogramme rufen Links in Emails unter Umständen auf, der direkt-Login-Link Funktioniert dann nicht mehr.

Alternative
* Klasse ProjectYcomPasswordReset anlegen
* Formular anlegen mit Formbuilder
* Im Passwort-Reset-Email mit der URL verbinden auf dem sich das Passwort-Formular befindet


```
<?php
// redaxo/src/addons/project/lib/ProjectYcomPasswordReset.php

class ProjectYcomPasswordReset
{


    /**
     * Prüft ob rex_ycom_activation_key und rex_ycom_id (email) stimmen
     * 
     * Einbau in Formular:
     * validate|customfunction|rex_ycom_activation_key,rex_ycom_id|ProjectYcomPasswordReset::isInvalidPasswordReset||Token not valid|normal
     * 
     * Aufruf siehe lib/yform/validate/customfunction.php
     */
    public static function isInvalidPasswordReset($names, $ObjectValues) 
    
    {
        $db = rex_sql::factory();
        $users = $db->getArray("
            SELECT * FROM rex_ycom_user 
            WHERE email=?
            AND activation_key=?
        ", [
            $ObjectValues['rex_ycom_id'],
            $ObjectValues['rex_ycom_activation_key'],
        ]);
        $user = array_shift($users);
        if (!$user){
            return true;
        }
        return false;
        
    }


    /**
     * Setzt das neue Passwort
     * 
     * Einbau in Formular:
     * action|callback|ProjectYcomPasswordReset::updatePassword
     */
    public static function updatePassword($callback_obj){
        $values = $callback_obj->getParam('value_pool')['email'];
        $db = rex_sql::factory();
        $users = $db->getArray("
            SELECT * FROM rex_ycom_user 
            WHERE email=?
            AND activation_key IS NOT NULL 
            AND activation_key != '' 
            AND activation_key = ?
        ", [
            $values['rex_ycom_id'],
            $values['rex_ycom_activation_key'],
        ]);
        $user = array_shift($users);
        if ($user){
            $doublehashed = password_hash(sha1($values['password_2']), \PASSWORD_DEFAULT);
            $db->setTable('rex_ycom_user');
            $db->setWhere(['id' => $user['id']]);
            $db->setValue('password', $doublehashed);
            $db->setValue('last_action_time', date('Y-m-d H:i:s'));
            $db->setValue('activation_key', '');
            $db->update();
        }
    }
}
```

Formbuilder-Formular

```
html|before|before|<div class="row"><div class="col-12 col-md-6">
ycom_auth_password|password|New password*|{"length":{"min":10},"letter":{"min":1},"lowercase":{"min":1},"uppercase":{"min":1},"digit":{"min":1},"symbol":{"min":1}}|Password must be at least 10 chars long, contain numbers, special characters, upper and lower characters.
hidden|rex_ycom_activation_key|rex_ycom_activation_key|GET
hidden|rex_ycom_id|rex_ycom_id|GET
password|password_2|Repeat password||no_db
submit|submit_form|Submit||1|
html|after|after|</div></div>
validate|empty|password|Please enter a password.
validate|compare|password|password_2|!=|Please enter the same password twice.
validate|customfunction|rex_ycom_activation_key,rex_ycom_id|ProjectYcomPasswordReset::isInvalidPasswordReset||Token not valid|normal
action|callback|ProjectYcomPasswordReset::updatePassword
action|showtext|Your new password is now active. <a href="/login/">Login</a>|<div class="alert alert-success">|</div>|1
```
