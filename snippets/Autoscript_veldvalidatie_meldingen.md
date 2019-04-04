## Veldvalidatie meldingen

In de `Custom_ValidationCheck` functie van het autoscript kunnen controles worden gemaakt die voorkomen dat een Agent een belopdrachtstatus gebruikt 
die niet mag bij bepaalde scriptveld combinaties.
Het is dan handig om een uniforme melding naar de agent te geven. 
Voor dat doel is vanaf v4.30 van het autoscript een extra collectie `errors` toegevoegd waar deze meldingen in kunnen worden opgenomen. 
CallPro raadpleegd deze collectie en gebruikt de items om een samenvatting van de invoer problemen te geven.
De `Custom_ValditionCheck` functie krijgt als paramater een status object van de status die de agent heeft gekozen om de belopdracht mee af te sluiten. 
Als deze functie uiteindelijk `false` teruggeeft wordt hiermee aangegeven aan CallPro dat het toekennen van de belopdrachtstatus niet verder mag gaan. 
Door items in de status.errors collectie te plaatsen worden deze meldingen gebruikt om aan de agent duidelijk te maken wat er mis is en waarom niet kan worden afgecodeerd.
```
function Custom_ValidationCheck(status) {
var validatieResultaat = true;
switch(status.code)
{
case "780":
  // Bij een afspraak is het email adres verplicht vanwege de afspraakbevestiging
  if(getFieldValue("script_name_email")=="") {
status.errors.add("Een email adres is verplicht voor de afspraakbevestiging email");
validatieResultaat = false;
  }
  break;
case "INFO":
  // Bij INFO is het email adres verplicht vanwege de informatie email
  if(getFieldValue("script_name_email")=="") {
status.errors.add("Een email adres is verplicht voor de informatie toezending");
validatieResultaat = false;
  }
  // Bij INFO moet een informatiepakket keuze zijn gemaakt
  if(getFieldValue("script_info")=="") {
status.errors.add("Er is geen keuze gemaakt voor het informatiepakket dat moet worden toegezonden");
validatieResultaat = false;
  }
  break;
}
return validatieResultaat;
}  
```
Hier is voor 2 belopdrachstatussen een validatie controle opgenomen. 
Bij code `780` wordt het `name_email` scriptveld gecontroleerd en bij code `INFO` worden zowel `name_email` als het info veld gecontroleerd. 
Als controle kan elke conditie worden gebruikt en zelfs combinaties van velden, wat belangrijk is is dat de `status.errors` collectie wordt gevuld en 
dat de hele functie `false` teruggeeft als 1 conditie niet voldoet.
