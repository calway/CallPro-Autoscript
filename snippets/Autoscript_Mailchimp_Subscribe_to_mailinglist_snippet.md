## Mailchimp aanmelden op mailinglijst

Om vanuit CallPro een lead aan te melden voor een Mailchimp mailinglijst is het volgende javascript blok nodig. 
Plaats deze code in de SCRIPT.HEADER of eventueel in een speciaal HTML scriptveld. Omdat elk script afwijkend is moeten
enkele variabelen op de juiste waarde voor het specifieke belscript worden gezet.  

In dit voorbeeld gebruiken we alleen een scriptveld `email`, `name_first` en `name_last` maar Mailchimp biedt de mogelijkheid om meer
velden aan de mailinglijst door te geven, kijk hiervoor in de [API documentatie van Mailchimp](https://us1.api.mailchimp.com/schema/3.0/Lists/Members/MergeField.json).


``` html
<script>
//
// Deze functie kan gebruikt worden vanaf autoscript v4.3.1.20659
//
function MailchimpSubsribeToList()
{
	// Vul hier het adres van de IIS server in waar het audoscript draait
    var AutoscriptBaseUrl = "http://autoscript"; 
	// Vul hier het API adres van Mailchimp is. de us3 dient gelijk te zijn aan de laatste tekens uit de ApiKey hieronder
    var MailchimpApiUrl = "https://us3.api.mailchimp.com/3.0"; 
	// Vul hier een geldige Mailchimp Apikey in
    var MailchimpApikey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx-us3"; 
	// Vul hier eengeldige Mailchimp list-id in
    var MailchimpListId = "xxxxxxxxxx"; 

	// Hier halen we CallPro velden op die in de body variabel verder naar beneden worden gebruikt
    var email = getFieldValue("script_email");
    var firstname = getFieldValue("script_name_first");
    var lastname = getFieldValue("script_name_last");
    if(email!="" && firstname!="" && lastname!="")
    {
		// Kijk in de Milchimp documentatie welke velden nog meer gezet kunenn worden
		//
		// https://api.mailchimp.com/schema/3.0/Lists/Members/Instance.json
		// https://us1.api.mailchimp.com/schema/3.0/Lists/Members/MergeField.json
		//
		var body = {
			email_address: email,
			status: "pending",
			merge_fields: {
				FNAME: firstname,
				LNAME: lastname
			}
		};
		jQuery.ajax({
			url: AutoscriptBaseUrl + "/api/services/RestApiPost?url="+MailchimpApiUrl+"/lists/"+ MailchimpListId + "/members", 
			type: "POST",
			headers: { 
				"Authorization": "apikey " + MailchimpApikey 
			},
			contentType: "text/plain",
			dataType: "json",
			data: JSON.stringify(body),
		}).fail(function(data, status, err) {
			var mailchimpResult = data.responseText;
			alert("FOUT: " + email + " kon niet worden aangemeld op de mailinglijst vanwege: ["+ mailchimpResult + "]");
		}).done(function(data, status, err) {
			alert("SUCCES: "+ email + " is aangemeld op de mailinglijst.");
		});
    }
}
</script>
```
##### Plaats een knop in het belscript
Om de functie uit te voeren hoeft alleen ergens in het belscript, bij voorkeur in de buurt van het betreffende email adres veld ene knop te worden geplaats die deze functie oproept.
``` html
<input type="button" onclick="MailchimpSubsribeToList()" value="Aanmelden" />
```
