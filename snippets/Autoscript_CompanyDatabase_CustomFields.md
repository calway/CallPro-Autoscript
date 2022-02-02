## Dropdown CompanyDatabase kenmerken

Met de CompanyDatabase plugin worden per bedrijf specifieke velden bijgehouden. Deze waarden kunnen als volgt worden gebruikt in keuzelijsten in het script.

In het odnerstaande voorbeeld wordt een kenmerk `mob_provider` opgevraagd met ene lijst van mobiele providers die eerder in projecten is vastlegd.

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