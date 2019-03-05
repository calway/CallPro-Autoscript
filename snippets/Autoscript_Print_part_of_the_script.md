## Print een deel van het script ##

Meestal wil je voorkomen dat gegevens geprint worden, maar in specifieke situaties kan het zijn dat je toch een printje wilt hebben van de gegevens in het belscript.

Met deze simpele methode kan ene deel van het belscript snel worden geprint. Voor afwijkende opmaak moet het deel dat wordt geprint zelf opgemaakt worden. In dit voorbeeld gebruiken we direct de belscript opmaak van het autoscript om enkele velden te printen.

Veld: PRINT_HTML_BEGIN  
Type: karakter(1)  
Opties:  
[x] Laatste veld op regel  
[x] Genereer geen regel opmaak (html) om de velden  
control-type: HTML  
HTML:
```
<!-- Deze CSS en de open div section-to-print  zijn bedoeld voor het printen van het belscript -->
<style>
@media print {
  body * {
    visibility: hidden;
  }
  #section-to-print, #section-to-print * {
    visibility: visible;
  }
  #section-to-print {
    position: absolute;
    left: 0;
    top: 0;
  }
}
</style>
<input type="button" class="btn btn-info" onclick="window.print()" value="Print NAWT" />

<div id="section-to-print">
<!-- LET OP DEZE DIV MOET NA HET LAATSTE VELD DAT GEPRINT MOET WORDEN WEER WORDEN GESLOTEN -->
```

Na het laatste veld in het belscript dat moet meekomen in de print wordt het veolgende veld geplaatst. Dit zorgt ervoor dat de eerder geopende `<div>` weer is gesloten.

Veld: PRINT_HTML_END  
Type: karakter(1)  
Opties:  
[x] Laatste veld op regel  
[x] Genereer geen regel opmaak (html) om de velden  
control-type: HTML  
HTML:
```
<!-- DIT IS DE CLOSE DIV van de section-to-print -->
</div>
```

We plaatsen hier een print knop helemaal bovenaan, maar deze kan in principe overal staan waar het gewenst is. We gebruiken de standaard print mogelijkheid van de browser en met de twee bovenstaande velden wordt bepaald welke delen van het belscript in de afdruk terecht komen.

CSS voorbeeld code van: https://stackoverflow.com/questions/21221724/how-to-show-div-when-media-is-print-hide-at-all-other-times
