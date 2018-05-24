## accordion control ##

Om een accordion te maken met velden moet het HTML_BEGIN voor het eerste veld en HTML_END na het laatste veld worden geplaatst. 
Het resultaat is een uitklap blok met alle velden. Om met meerdere blokken te werken moet elk blok een unieke prefix krijgen, als voorbeeld NAW.

Veld: NAME_HTML_BEGIN  
Type: karakter(1)  
control-type: HTML  
HTML:
```
<div class="accordion" data-role="accordion">
<!-- Begin accordion -->

<div class="frame active">
<div class="heading">Contactpersoon gegevens</div>
<div class="content">
```

Veld: NAME_HTML_END  
Type: karakter(1)  
control-type: HTML  
HTML:
```
</div>
</div>

<!-- Einde accordion -->
</div>
```

Om tussen de BEGIN en END een extra accordion te maken kan dit worden ingevoegd


Veld: ADDRESS_HTML_MIDDLE  
Type: karakter(1)  
control-type: HTML  
HTML:
```
</div>
</div>
<div class="frame active">
<div class="heading">Adresgegevens</div>
<div class="content">
```
