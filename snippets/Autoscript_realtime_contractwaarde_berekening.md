## Realtime contractwaarde berekening ##

Voor de realtime berekening van contractwaarde, of elke ander soort berekening kan het onderstaande 
voorbeeld met vaste en mobiele telefonie als basis worden gebruikt. We gaan ervanuit dat er 3 velden aanwezig zijn

veldnaam | type | inhoud
---------|------|-------
vast | karakter(10) | bevat de maandelijkse contractwaarde voor vaste telefonie
mobiel | karakter(10) | bevat de maandelijkse contractwaarde voor mobiele telefonie
orderwaarde_totaal | karakter(1) | een dummy veld dat alleen wordt gebruikt om de totaal regel af te beelden.

De diverse teksten die worden weergegeven kunnen vrijlijk worden aangepast. Ook andere onderdelen kunnen 
worden aangepast zoals kleur, breedte of het weglaten van opties maar dat vergt meer wijzigingen om het 
voorbeeld werkend/functioneel te houden.


Het scriptveld `vast` met de contractwaarde per maand van de vaste telefonie  
Naam: `vast`  
Datatype: Karakter(10)  
Algemeen/Vraag: (leeg)  
CustomHTML:
```
<script type="text/javascript">
function BerekenOrderwaarde()
{
var vast = getFieldValue("script_vast");
var mobiel = getFieldValue("script_mobiel");
var factor_vast = getFieldValue("factor_vast");
var factor_mobiel = getFieldValue("factor_mobiel");
var orderwaarde_vast = vast * factor_vast;
var orderwaarde_mobiel = mobiel * factor_mobiel;

setFieldValue("orderwaarde_vast", orderwaarde_vast);
setFieldValue("orderwaarde_mobiel", orderwaarde_mobiel);
setFieldValue("orderwaarde_totaal", orderwaarde_vast+orderwaarde_mobiel);

}
</script>
<div class="row cells12">

<div class="cell colspan4">
<label>Contractwaarde vaste telefonie/maand</label>
</div>

<div class="cell colspan7">
<div class="input-control text full-size">
  <input name="script_vast"  id="script_vast" type="text" style="width:200px" onblur="BerekenOrderwaarde();">
  <select name="factor_vast" style="width:200px" tabindex="-1" onblur="BerekenOrderwaarde();" title="Looptijd contract">
    <option value="12">1</option>
    <option value="24">2</option>
    <option value="36" selected="selected">3</option>
    <option value="48">4</option>
    <option value="60">5</option>
  </select>
  <div class="button bg-green" id="orderwaarde_vast"></div>
</div>
</div>

</div>
```

Het veld `mobiel` met de contractwaarde per maand voor mobiele telefonie.  
Naam: `mobiel`  
Datatype: Karakter(10)  
Algemeen/Vraag: (leeg)  
Custom HTML:
```
<div class="row cells12">

<div class="cell colspan4">
<label>Contractwaarde mobiele telefonie/maand</label>
</div>

<div class="cell colspan7">
<div class="input-control text full-size">
  <input name="script_mobiel"  id="script_mobiel" type="text" style="width:200px" onblur="BerekenOrderwaarde();">
  <select name="factor_mobiel" style="width:200px" tabindex="-1" onblur="BerekenOrderwaarde();" title="Looptijd contract">
    <option value="12">1</option>
    <option value="24" selected="selected">2</option>
    <option value="36">3</option>
    <option value="48">4</option>
    <option value="60">5</option>
  </select>
  <div class="button bg-green" id="orderwaarde_mobiel"></div>
</div>
</div>

</div>
```

Regel voor de totalecontractwaarde van beide groepen. Deze wordt alleen op het scherm afgebeeld het veld mag dus een karakter(1) zijn.  
Naam: `orderwaarde_totaal` 
Datatype: Karakter(1)  
Algemeen/Vraag: (leeg)  
Custom HTML:
```
<div class="row cells12">

<div class="cell colspan4">
<label>Totale contractwaarde</label>
</div>

<div class="cell colspan7">
<div class="input-control text full-size">
<div class="button bg-green place-right" id="orderwaarde_totaal" ></div>
</div>
</div>

</div>
```
