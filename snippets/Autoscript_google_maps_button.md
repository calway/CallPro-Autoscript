## Google maps button

Ter ondersteuning van de agents tijdens het bellen kan het handig zijn om de locatie van het adres visueel weer te geven. 
In het belscript is het erg eenvoudig om een button te maken die Google maps opent in een nieuw venster en de adres gegevens doorgeeft.

In dit voorbeeld gaan we ervan uit dat de scriptdefinitie 3 adresvelden: addr_street, addr_number en addr_zip bevat. 
We plaatsen de button bij het straatnaam veld door in de scriptdefinitie bij het addr_street veld de volgende instellingen te kiezen:

Naam: `addr_street`
Datatype: Karakter(40) 
Weergave label: (Nee)
Weergave control: (Ja)
Control type: HTML
Custom HTML:

```
<script>
function ShowGoogleMap_addr_street() {
	var URL = "https://www.google.nl/maps/place/";
	var paramlist = getFieldValue("script_addr_street") + '+' + getFieldValue("script_addr_zip") + '+' + getFieldValue("script_addr_city");
	window.open(URL + paramlist,"googlemaps");
} 
</script>

<div class="input-control text full-size">
<input name="script_addr_street" title="Straat" id="script_addr_street" type="text" ng-model="script_addr_street">
<a href="#" onclick="ShowGoogleMap_addr_street()" class="button">
<span class="mif-earth"/>
</a>
</div>
```

Hier is het onderste deel HTML opmaak overgenomen uit een gegenereerde pagina en vervolgens de button toegevoegd.
Door op de knop te drukken met de globe opent Google maps in een apart browser venster voor het adres.

Het is ook mogelijk om een losse button te gebruiken zoals:
```
<button class="button mini-button primary" onclick="ShowGoogleMap_addr_street()">Kaart</button>
```
Deze button kan overal worden geplaatst, zolang het `<script></script>` blok ook in de pagina staat.