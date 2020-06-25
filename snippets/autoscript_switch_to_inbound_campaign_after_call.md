## Schakel bij afcoderen van huidige call over naar een inbound campagne met een blanco call

Voor een project waarbij het call center ZenDesk chat voor een opddrachtgever ging opvangen was het nodig dat agents tijdens het outbound bellen 
als er een Chat verzoek kwam de mogelijkheid kregen om over te switchen naar een chat registratie. Tijdens het outbound bellen ziet een Agent dat er een chat verzoek binnenkomt die hij/zij kan oppakken. Door in de belscripts onderstaande een button te plaatsen geeft de Agent aan dat na de huidige call een
blanco inbound registratie moet owrnde gestart voor de Chat campagne.
Dit is nodig om in het call center een vastlegging te krijgen van de tijd die agents besteden aan de Chat campagne. Het is niet nodig om voor elke chat een nieuwe inbound call te registreren, het doel was hier vooral om de tijdregistratie te vereenvoudigen.

Agenten die zitten te wachten op een nieuwe call kunnen zelf makkelijk een Inbound registratie starten, maar als het erg druk is op inbound, en/of gewerkt wordt met agents die callblending werken kan het geburen dat he tdoor de drukte niet mogelijk is ** handmatig ** een nieuwe registratie voor de Chat cmpagne te beginnen. Met deze functie kan worde nafgeweken van he tstandaard gedrag van Callpro om zelf een wachtende inbound call op te pakkeen maar in plaats daarvan een specifieke registratie te beginnen.

``` html
<script>
//
// Snippet: Schakel over naar inbound campagne en maak een blanco entry
// Met behulp van deze code wordt na de huidige call automatisch overgeschakeld
// naar de opgegeven campagne en met de optionele NEWENTRY parameter wordt direct 
// een blanco entry gestart. 
// 
// Eenvoudige UI code bijgevoegd
//
var SwitchToChatCampaign = false;

function ToggleSwitchToChat() {
if(SwitchToChatCampaign) {
  $("#SwitchToChat").toggleClass("primary", true);
  $("#SwitchToChat").toggleClass("danger", false);
  $("#SwitchToChat").text("Naar ZenDesk Chat campagne overschakelen na afcoderen");
  window.location="#script_switchdialmode?clear&newentry";
}
else {
  $("#SwitchToChat").toggleClass("primary", false);
  $("#SwitchToChat").toggleClass("danger", true);
  $("#SwitchToChat").text("Annuleer het overschakelen na afcoderen");
  // campaignid = de unieke ID van de CallPro campagne waar naar toe moet worden overgeschakeld
  window.location="#script_switchdialmode?set&noconfirm&campaignid=12345&newentry";
}
SwitchToChatCampaign = !SwitchToChatCampaign;
}
</script>
<button class="button primary" id="SwitchToChat" onclick="ToggleSwitchToChat()">Naar ZenDesk Chat campagne overschakelen na afcoderen</button>

```

Deze code kan in de SCRIPT.HEADER of SCRIPT.INFO van de scriptdefinitie worden gezet.
