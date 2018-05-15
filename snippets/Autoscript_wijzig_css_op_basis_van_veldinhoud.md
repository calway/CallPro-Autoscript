## Wijzig CSS opmaak op basis van veldinhoud ##

Om na het laden van het script code uit te voren die reageert op de inhoud van scriptvelden kan in de `SCRIPT.HEADER` variabele op de scriptdefinitie een javascript functie worden geplaatst die heirvoor zorgt. Let wel op dat er niet meerdere `ready` functies worden gebruikt.
In dit voorbeeld controleren we of het bedrijfsnaamveld `COMP_NAME` gevuld is met de tekst "Calway" en maken de achtergrondkleur in dat geval rood.

```
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

#### Update: server-side methode ####
Een alternatieve methode die vanaf 4.3.1 beschikbaar is werkt niet in de `SCRIPT.HEADER` maar moet in een scriptveld Control HTML blok worden geplaatst.

```
$(document).ready(function() {
var content = "%script.comp_name%";
if(content=="Calway") {
 $("#script_comp_name").css("background-color","red");
}
});
```
Hier gebruiken we de database direct door op de server de waarde in de pagina te plaatsen met behulp van de fulfilment syntax `%script.comp_name%`. 