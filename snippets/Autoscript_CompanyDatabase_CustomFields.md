## Dropdown CompanyDatabase kenmerken

Met de CompanyDatabase plugin worden per bedrijf specifieke velden bijgehouden. Deze waarden kunnen als volgt worden gebruikt in keuzelijsten in het script.

In het onderstaande voorbeeld worden gegevens uit de CompanyDatabase plugin gehaald van kenmerken die voor bedrijven zijn bewaard. Alleen de 4e en 6e regel bevatten  specifieke waarden die per veld/klant verschillen. Alle overige code is algemeen en kan direct voor andere velden gebruikt worden. De naam van het veld is ook de key voor het kenmerk in de CompanyDatabase, hier de `mob_provider` (of Mobiele provider). De url naar de API is ook klant/instalatie specifiek. Deze url is niet via internet bereikbaar, maar alleen vanuit het call center (klant lokatie). 

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
