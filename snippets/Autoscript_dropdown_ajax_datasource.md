## Dropdown ajax datasource

Vaak hebben keuzelijsten vaste waarden die je makkelijk in een lijstje in CallPro kunt invoeren. Soms zijn deze lijsten zo lang en komen ze van een externe bron dat het eenvoudiger is om ze vanuit een externe bron te laden. Het autoscript gebruikt de select2 dropdown control die ook externe data kan gebruiken om de lijst te vullen.

In dit voorbeeld plaatsen we een bestand met functies in een blob container op azure. We gebruiken deze lijst om de functies weer te geven. Omdat we nu aan kale json datasource gebruiken werkt de zoekfunctie van de dropdown helaas niet. 

``` html
<select id="script_name_func" name="script_name_func" title="Functie" ng-model="script_name_func" style="width:100%;"></select>
<script>
$('#script_name_func').select2({
  minimumInputLength: 0,
  minimumResultsForSearch: -1,
  ajax: {
    url: 'https://callprostorage.blob.core.windows.net/public-json/func-list-0001.json',
    dataType: 'json'
  }
});
</script>
```

Om het zoekgedrag te herstellen moet een server-side api gebruikt worden die query string paramaters accepteert zoals beschreven op de [website van select2](https://select2.org/data-sources/ajax#request-parameters)

Voor toepassingen waarbij de zoekfunctie essentieel is, maar geen externe api kan worden gemaakt is het toch mogelijk om een eenvoudig databestand (json) te gebruiken maar dan met iets aangepastte javascript.

``` html
<select id="script_name_func" name="script_name_func" title="Functie" ng-model="script_name_func" style="width:100%;"></select>
<option value=""> -- Keuze -- </option>
<script>
$.ajax({
  url: 'https://callprostorage.blob.core.windows.net/public-json/func-list-0001.json',
  dataType: 'json'
}).done( function(data) {
  var control = $('#script_name_func').select2({
  minimumInputLength: 0,
  data: data.results
  });
  // Om problemen bij het filteren te voorkomen voegen we altijd de huidige veldinhoud als optie toe en maken deze de actieve keuze. 
  var value = '%SCRIPT.NAME_FUNC \TRA%';
  control.append(new Option(value, value, true, true));
});
</script>
```

Hier wordt de data los opgehaald en als array aan de select2 dropdown meegegeven. Een bijkomend voordeel, naast de zoekfunctie is dat de array (data.results) ook van te voren nog kan worden bewerkt zoals sorteren voordat deze aan de select2 control wordt gegeven.

In dit laatste voorbeeld worden gegevens uit de CompanyDatabase plugin gehaald van kenmerken die voro bedrijven zijn bewaard. Alleen de 4e en 6e regel bevatten  specifieke waarden die per veld/klant verschillen. Alle overige code is algemeen en kan direct voor andere velden gebruikt worden. De naam van het veld is ook de key voor het kenmerk in de CompanyDatabase, hier de mob_provider (of Mobiele provider). De url naar de API is ook klant/instalatie specifiek. Deze url is niet via internet bereikbaar, maar alleen vanuit het call center (klant lokatie). 

``` html
<script>
$(document).ready(function() {
  var transformedArrayForSelect2 = [];
  var ScriptFieldValue = '%SCRIPT.mob_provider%';
  $.ajax({
    url: 'https://companydatabase.calway.callpro.nl/api/CustomFields/GetDistinctValues/%SCRIPTDEFFIELD.FLDNAME \CC3%',
    dataType: 'json',	   
    success: function (data) {
      // Transforms the top-level key of the response object from 'items' to 'results'
      transformedArrayForSelect2 = _.transform(data, function(result, o) {
        result.push({ id: o.value.trim(), text: o.value.trim(), selected: o.value.trim()===ScriptFieldValue });
      }, []);
      // If none selected add the current value as selected value;
      transformedArrayForSelect2.push({ id: ScriptFieldValue, text: ScriptFieldValue, selected: true });
      // Now create the select2 control
      $('#script_%SCRIPTDEFFIELD.FLDNAME \CC3%').select2({
        tags: true,
        data: transformedArrayForSelect2
      }); 
    }
  });
});
</script>	
<select class="input-control text full-size" name="script_%SCRIPTDEFFIELD.FLDNAME \CC3%" id="script_%SCRIPTDEFFIELD.FLDNAME \CC3%" data-placeholder="%SCRIPTDEFFIELD.FRIENDLYNAME%">
</select>
```

