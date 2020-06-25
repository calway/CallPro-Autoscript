## Email opt-in registratie

Voor het vastleggen van een email opt-in kan een J/N vraag gesteld worden in het belscript. 
Als ook moet worden vastgelegd wanneeer de opt-in is afgegeven kan een datum/tijd veld worden gebruikt.
Om alles netjes in het script te plaatsen geven wij dit voorbeeld.

De opt-in gebruikt 3 opties, blanco als zijnde "nog niet gevraagd", "J" voor opt-in afgegeven en "N" voor opt-out:

Naam: `optin`
Datatype: Karakter(1) 
Weergave label: (Ja)
Label: Email opt-in
Weergave control: (Ja)
Control type: HTML
Custom HTML:

``` html
<label class="input-control radio small-check">
    <input type="radio" name="script_optin" value="" onclick="onSelectionChanged_optin(this)">
    <span class="check"></span>
    <span class="caption">Niet gevraagd</span>
</label>
<label class="input-control radio small-check">
    <input type="radio" name="script_optin" value="J" onclick="onSelectionChanged_optin(this)">
    <span class="check"></span>
    <span class="caption">Ja, opt-in</span>
</label>
<label class="input-control radio small-check">
    <input type="radio" name="script_optin" value="N" onclick="onSelectionChanged_optin(this)">
    <span class="check"></span>
    <span class="caption">Nee, opt-out</span>
</label>
<script>
// Get current value during script render from server
var current_optin='%SCRIPT.OPTIN%';
var current_optin_date='%SCRIPT.OPTIN_DATE%';

function onSelectionChanged_optin(control)
{
  if(control.checked)
  {
    if(control.value!=current_optin)
    {
      // We selected a new value set/update optin_date
      setFieldValue("script_optin_date", new Date().toISOString());
    }
    else
    {
      // We returned to the original value, restore optin_date
      setFieldValue("script_optin_date", current_optin_date);
    }
  }
}
</script>
```

Naam: `optin_date`
Datatype: Datumtijd 
Weergave label: (Nee)
Weergave control: (Ja)
Control type: Hidden


Voeg deze scriptvelden in op de plaats waar de vraag moet worden gesteld.  Als een keuze wordt gemaakt dan wordt in 
het scriptveld optin_date de huidige datum tijd vastgelegd. Als een andere keuze wordt gemaaktals de huidige keuze die 
in de database is vastgelegd dan wordt opnieuw de optin_date gezet.

Op deze manier kan het optin_date veld bij exports worden gebruikt om te laten zien dat de optin keuze in de belperiode, of 
export periode valt. 
