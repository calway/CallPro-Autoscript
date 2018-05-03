## tabcontrol ##

Om een tabcontrol te maken met verschillende tabbladen waar de scriptvelden in worden afgebeeld moet het HTML_BEGIN voor het 
eerste veld en HTML_END na het laatste veld worden geplaatst. Het resultaat is een tabcontrol met 1 tab. 
Er kunnen ook meerdere blokken worden samengevoegd. Om met meerdere blokken te werken moet elk blok een unieke prefix krijgen, als voorbeeld NAW.

Veld: NAW_HTML_BEGIN  
Type: karakter(1)  
control-type: HTML  
HTML:
```
    <div class="tabcontrol" data-role="tabcontrol">
    <ul class="tabs">
        <li><a href="#frame_1">Contactpersoon</a></li>
        <li><a href="#frame_2">Adres</a></li>
        <li><a href="#frame_3">Bedrijfsgegevens</a></li>
    </ul>
    <div class="frames">
        <div class="frame" id="frame_1">
```
Veld: NAW_HTML_END  
Type: karakter(1)  
control-type: HTML  
HTML:
```
        </div>
    </div>
</div>
```
Om tussen de BEGIN en END een extra tabcontrol te maken (dit hoort te corresponderen met de tabs die bij HTML_BEGIN zijn gedefinieerd moet hetvolgende worden ingevoegd voor de respectievelijke tabbladen


Veld: NAW_HTML_ADRES  
Type: karakter(1)  
control-type: HTML  
HTML:
```
        </div>
        <div class="frame" id="frame_2">
```

Hier moet de id="frame_2" overeenkomen met een corresponderende regel in het HTML_BEGIN blok (hier regel `<li><a href="#frame_2">Adres</a></li>` ) die verwijst naar dezelfde id waarde.
