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
