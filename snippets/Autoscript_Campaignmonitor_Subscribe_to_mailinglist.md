## Campaignmonitor aanmelden op mailinglijst ##

Om vanuit CallPro een lead aan te melden voor een Campaignmonitor mailinglijst is het volgende javascript blok nodig. 
Plaats deze code in de SCRIPT.HEADER of eventueel in een speciaal HTML scriptveld. Omdat elk script afwijkend is moeten
enkele variabelen op de juiste waarde voor het specifieke belscript worden gezet.  

In dit voorbeeld gebruiken we de scriptvelden `email`, `name_gender`, `name_first` en `name_last` en `bron`.


```
<script>
//
// Deze functie kan gebruikt worden vanaf autoscript v4.3.1.20659
//
function CampaignmonitorSubsribeToList()
{
	// Vul hier het adres van de IIS server in waar het audoscript draait
    var AutoscriptBaseUrl = "http://autoscript"; 
	// Vul hier het API adres van Campaignmonitor in.
    var CampaignmonitorApiUrl = "https://api.createsend.com/api/v3.2"; 
	// Vul hier een geldige Basic auth token in. Deze kan gemaakt worden door de apikey
	// van Campaignmonitor als gebruikernaam en een leeg achtwoord te gebruiken in bijvoorbeeld Postman.
    var CampaignmonitorBasicAuth = "Basic xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"; 
	// Vul hier een geldige Campaignmonitor API Subecriber List ID  in
    var CampaignmonitorListId = "xxxxxxxxxx"; 

	// Hier halen we CallPro velden op die in de body variabel verder naar beneden worden gebruikt
    var email = getFieldValue("script_email");
    var name_gender = getFieldValue("script_name_gender");
    var name_first = getFieldValue("script_name_first");
    var name_last = getFieldValue("script_name_last");
	var bron = getFieldValue("script_bron");
	var name = name_gender + " " + name_first + " " + name_last;
   if(email!="" && name_last !="")
    {
		// Kijk in de Campaignmonitor API handleiding welke velden nog meer gezet kunnen worden
		var body = {
			EmailAddress: email,
			Name: name,
			CustomFields: {
				Key: "bron",
				Value: bron
			},
			ConsentToTrack: "Yes"
		};
		jQuery.ajax({
			url: AutoscriptBaseUrl + "/api/services/RestApiPost?url=" + CampaignmonitorApiUrl + "/subscribers/" + CampaignmonitorListId + ".json", 
			type: "POST",
			headers: { 
				"Authorization": CampaignmonitorBasicAuth 
			},
			contentType: "text/plain",
			dataType: "json",
			data: JSON.stringify(body),
		}).fail(function(data, status, err) {
			var campaignmonitorResult = data.responseText;
			alert("FOUT: " + email + " kon niet worden aangemeld op de mailinglijst vanwege: ["+ campaignmonitorResult + "]");
		}).done(function(data, status, err) {
			alert("SUCCES: "+ email + " is aangemeld op de mailinglijst.");
		});
    }
}
</script>
```
##### Plaats een knop in het belscript #####
Om de functie uit te voeren hoeft alleen ergens in het belscript, bij voorkeur in de buurt van het betreffende email adres veld een knop te worden geplaatst die deze functie oproept.
```
<input type="button" onclick="CampaignmonitorSubsribeToList()" value="Aanmelden" />
```
