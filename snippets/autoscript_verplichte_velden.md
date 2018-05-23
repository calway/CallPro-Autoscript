## Verplichte velden bij afcoderen ##

In de scriptdefinitie kunnen velden als verplicht worden gemarkeerd. Dit zorgt ervoor dat als een belresultaat (belopdrachtstatus) wordt gebruikt die als eindresultaat geldt dat CallPro controleert of de verplichte velden wel gevuld zijn.
Vaak zijn er situaties waarbij de controle op een verplicht veld afhankelijk is van een speciale afcodering. De standaard controle methode voldoet dan niet en er moet een ander emethode worden ingezet.

### Maak een veld verplicht op basis van de belopdrachtstatus ###

Als voorbeeld gebruiken we een belopdrachtstatus "Stuur informatie" waarbij een email gestuurd gaat worden en dan na enkele dagen wordt teruggebeld. In dat geval willen we controleren dat er een geldig email adres is ingevuld (scriptveld `info_email_to`) en ook dat is aangegeven welk informatiepakket gestuurd moet worden (scriptveld `info`).

Bij de keuze van een belopdrachtstatus in de linker statusbalk roept het autoscript altijd eerst een javascript functie `Custom_ValidationCheck` op. In deze functie kunnen invoercontroles worden geplaatst.

In de onderstaande functie is alleen de "Stuur informatie" controle opgenomen. Binnen het `switch` commando kunnen meerdere `case` condities voro verschillende statussen worden opgenomen.

```
function Custom_ValidationCheck(status) {
var validationResult = true;
switch(status.code)
{
case "INFOCALLBACK":
    if(getFieldValue("script_info")=="")
    {
      status.errors.add("Kies eerst een informatiepakket");
      validationResult = false;
    }
    if(getFieldValue("script_info_email_to")=="")
    {
      status.errors.add("Kies een email adres voor de verzending");
      validationResult = false;
    }
    if(!checkValidEmail("script_info_email_to"))
    {
      status.errors.add("Het opgegeven email adres is niet geldig");
      validationResult = false;
    }
    if(validationResult)
    {
        // Last check still valid make sure we send the email again
        setFieldValue("script_exp_info","");
    }
	break;
}
return validationResult;
}
```
Als is gekozen voor een status belopdrachtstatus met code `INFOCALLBACK` dan wordt gecontroleerd of het `info` veld een waarde heeft. Dit veld toont een dropdown keuzelijst met verschillende informatiepakketten die per email gestuurd kunnen worden. 
Daarna wordt gecontroleerd of het `info_email_to` veld een waarde heeft, en vervolgens of deze waard eeen geldig email adres is. Als laatste wordt als alles goed is een scriptveld `exp_info` leeggemaakt dat als markering dient op welke datum/tijd het informatiepakket door de CalLpro fulfilment module is verstuurd.

Alslaatste geeft deze javascript functie een boolean functie resultaat terug waarmee wordt aangegeven of alles goed is , of dat er iets mis is en het afcoderen moet worden afgebroken. De meldingen die zijn gezet worden gebruikt om een duidelijke melding naar de Agent te geven.

