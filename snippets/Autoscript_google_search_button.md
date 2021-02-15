## Google Search button

Ter ondersteuning van de agents tijdens het bellen kan het handig zijn om informatie over het bedrijf weer te geven. 
In het belscript is het erg eenvoudig om een button te maken die Google search opent in een nieuw venster en de bedrijfsnaam gegevens, of andere velden doorgeeft.

In dit voorbeeld gaan we ervan uit dat de scriptdefinitie een  scritpveld `comp_name` bevat. 
We plaatsen de button bij het bedrijfsnaam veld door in de scriptdefinitie bij het `comp_name` veld de volgende instellingen te kiezen:

Naam: `comp_name`
Datatype: Karakter(50) 
Weergave label: (Ja)
Label: Bedrijfsnaam
Weergave control: (Ja)
Control type: HTML
Custom HTML:

``` html
<script>
function ShowGoogleSearch_comp_name() {
	var URL = "https://www.google.com/search?query=";
	URL += getFieldValue("script_comp_name");
	window.open(URL,"googlesearch");
} 
</script>

<div class="input-control text full-size">

<input type="text" id="script_comp_name" name="script_comp_name" ng-model="script_comp_name" 
title="Bedrijfsnaam" size="50" maxlength="50">

<a href="#" onclick="ShowGoogleSearch_comp_name()" class="button">
<span class="mif-search"/>
</a>

</div>
```

Hier is het onderste deel HTML opmaak overgenomen uit een gegenereerde pagina en vervolgens de button toegevoegd.
Door op de knop te drukken met de loep opent Google search in een apart browser venster voor de bedrijfsnaam.

Het is ook mogelijk om een losse button te gebruiken zoals:
```
<button class="button mini-button primary" onclick="ShowGoogleSearch_comp_name()">Zoek bedrijf</button>
```
Deze button kan overal worden geplaatst, zolang het `<script></script>` blok ook in de pagina staat.
