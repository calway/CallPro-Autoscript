## Gebruik Campaignmonitor om emails te sturen

Voor het versturen van emails adviseren wij het gebruik van de CallPro fulfilment module. Het is echter geen
probleem om in het script externe mailsystemen te gebruiken zoals Campaignmonitor of MailChimp.

In dit voorbeeld late nwe zien hoe je Campaignmonitor kunt gebruiken om emails te sturen. Als voorbereiding moet een
betaalde Campaignmonitor account aanwezig zijn die de DKIM setup is doorlopen. Daarna maak je een smartemail template en noteer je de ID. Noteer ook de API key die nodig is om een Bais Auth token te genereren.

Plaats onderstaande code in de SCRIPT.HEADER of eventueel in een speciaal HTML scriptveld. Omdat elk script afwijkend is moeten enkele variabelen op de juiste waarde voor het specifieke belscript worden gezet.  

In dit voorbeeld gebruiken we de scriptvelden `email`, `name_gender`, `name_first` en `name_last` om de smart template te vullen. Je kunt in de body json elk CallPro veld naar elk template veld mappen.


```
<script>
//
// Deze functie kan gebruikt worden vanaf autoscript v4.3.1.20659
//
function SendCampaignmonitorTransactionalEmail()
{
	// Vul hier het adres van de IIS server in waar het audoscript draait
    var AutoscriptBaseUrl = "http://localhost:63673"; 
	// Vul hier het API adres van Campaignmonitor in.
    var CampaignmonitorApiUrl = "https://api.createsend.com/api/v3.2"; 
	// Vul hier een geldige Basic auth token in. Deze kan gemaakt worden door de apikey
	// van Campaignmonitor als gebruikernaam en een leeg achtwoord te gebruiken in bijvoorbeeld Postman.
    var CampaignmonitorBasicAuth = "Basic xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"; 
	// Vul hier een geldige Campaignmonitor smartemail template id (guid) in
    var CampaignmonitorSmartemailTemplateId = "xxxxxxxxxx"; 

	// Hier halen we CallPro velden op die in de body variabel verder naar beneden worden gebruikt
    var email = getFieldValue("script_email");
    var name_gender = getFieldValue("script_name_gender");
    var name_first = getFieldValue("script_name_first");
    var name_last = getFieldValue("script_name_last");
   if(email!="" && name_gender!="" && name_first !="" && name_last !="")
    {
		// Kijk in de Cmapaignmonitor API handleiding welke velden nog meer gezet kunen nworden
		var body = {
			to: email,
			Data: {
				gender: name_gender,
				firstname: name_first,
				lastname: name_last
			},
			AddRecipientsToList: false,
			ConsentToTrack: "Yes"
		};
		jQuery.ajax({
			url: AutoscriptBaseUrl + "/api/services/RestApiPost?url=" + CampaignmonitorApiUrl + "/transactional/smartEmail/" + CampaignmonitorSmartemailTemplateId + "/send", 
			type: "POST",
			headers: { 
				"Authorization": CampaignmonitorBasicAuth 
			},
			contentType: "text/plain",
			dataType: "json",
			data: JSON.stringify(body),
		}).fail(function(data, status, err) {
			var campaignmonitorResult = data.responseText;
			alert("FOUT: Er ging iets fout bij het versturen van een email naar " + email + " vanwege: ["+ campaignmonitorResult + "]");
		}).done(function(data, status, err) {
			alert("SUCCES: Er is een email gestuurd naar "+ email + ".");
		});
    }
}
</script>
```
##### Plaats een knop in het belscript
Om de functie uit te voeren hoeft alleen ergens in het belscript, bij voorkeur in de buurt van het betreffende email adres veld ene knop te worden geplaats die deze functie oproept.
```
<input type="button" onclick="SendCampaignmonitorTransactionalEmail()" value="Stuur email" />
```
