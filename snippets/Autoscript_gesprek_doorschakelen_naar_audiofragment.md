## Gesprek doorschakelen naar audiofragment ##

Hoewel wij er geen voorstander van zijn om voor gebakken audio fragmenten in te spreken op iemands voicemail 
zijn er toepasssingen waar dit toch handig/nodig kan zijn.

Om dit mogelijk te maken dient van te voren met een aantal nummer te owrden gereserveerd waar een audiofragment op 
kan worden ingesproken. Als deze nummers intern worden gebeld wordt het fragment afgespeeld en wordt daarna de verbinding 
verbroken.

Voor deze voorbeeld code gaan we ervanuit dat een nummerreeks 8000..8099 is gereserveerd voor dit doel. 
We gebruiken hier de nummer 8010, 8011, 8012 als voorbeeld waar van te voren een tekst is ingesproken.


Naam: `voicemail`
Datatype: Karakter(1) 
Weergave label: (Nee)
Weergave control: (Ja)
Control type: HTML
Custom HTML:

```
<div data-role="group" data-group-type="one-state">
<button class="button primary mini-button" onclick="CallTransferToServiceNumber('8010')">Introductie project volledig</button>
<button class="button primary mini-button" onclick="CallTransferToServiceNumber('8011')">Introductie project verkort</button>
<button class="button danger mini-button" onclick="CallTransferToServiceNumber('8012')">Alleen terugbelbericht</button>
</div>

<script>
function CallTransferToServiceNumber(toTelNr) {
	if (toTelNr != "") {
		if (goCallPro != null) {
			var loScriptAction = goCallPro.GetScriptAction();
			var loScriptParams = loScriptAction.CreateParams();
			var lcCommand = "SCRIPT_TRANSFER";
			loScriptParams.Add("TYPE", "BLIND");
			loScriptParams.Add("CONTEXT", "internal");
			loScriptParams.Add("TARGET", toTelNr);
			var llResult = loScriptAction.ExecCommand(lcCommand, loScriptParams);
		}
	}
}
</script>
```

Voeg dit scriptveld in op de plaats waar deze buttons moetne worden afgebeeld. 
Uiteraard zijn er alternatieve mogelijkheden. Zo kan het `<script></script>` ook in de SCRIPT.HEADER 
variabele worden ondergebracht, en kunnen enkele buttons dan op verschillende plaatsen wara dit voor het 
script nodig is worden ingevoegd. Ze hoeven niet perse allemaal bij elkaar te staan.
