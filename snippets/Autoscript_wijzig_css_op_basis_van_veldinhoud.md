## Wijzig CSS opmaak op basis van veldinhoud

Om in een belscript een veld, of een stuk tekst te markeren of van opmaak te wijzigen op basis van een bepaalde conditie dient gebruik te wordne gemaakt van javascript. We geven hier een eenvoudig voorbeeld dat kan worde naangepast op de specifieke toepassing.

In dit voorbeeld controleren we of het bedrijfsnaamveld `COMP_NAME` gevuld is met de tekst "Calway" en maken de achtergrondkleur in dat geval rood.  
Hiervoor moet in de `SCRIPT.HEADER` variabele op de scriptdefinitie een javascript functie worden geplaatst die dit regelt. Als er al gebruik wordt gemaakt van een `ready` functie dient de code te worden toegevoegd aan deze bestaande functie.

``` javascript
$(document).ready(function() {
window.setTimeout(function()
{ 
var content = getfieldValue("script_comp_name");
if(content=="Calway") {
 $("#script_comp_name").css("background-color","red");
}
}, 3000);
});
```

Omdat eerst het belscript wordt getoond, en CallPro daarna pas de velden in het belscript vult moeten we gebruik maken van de `setTimeout` functie om het uitvoeren van onze controle iets te vertragen. Zo heeft CallPro tijd om de velden te vullen.

#### Update: server-side methode
Een alternatieve methode die vanaf 4.3.1 beschikbaar is werkt ook zonder het gebruik van de `setTimeout` functie. Deze methode kan niet in de `SCRIPT.HEADER` worden geplaatst maar moet in een scriptveld Control HTML blok worden gezet.

``` javascript
$(document).ready(function() {
var content = "%script.comp_name%";
if(content=="Calway") {
 $("#script_comp_name").css("background-color","red");
}
});
```
Hier gebruiken we de veldinhoud uit de database direct door op de server de waarde al in de pagina te plaatsen met behulp van de fulfilment syntax `%script.comp_name%`. 