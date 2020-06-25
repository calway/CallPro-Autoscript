## Herstel de terugbeltijd en agent van een eerdere belpoging
Dit is een toepassing die zeer specifiek is gemaakt voor een periodiek belproces. Het proces is als volgt bedacht: 
Klanten worden periodiek gebeld met een vraag of ze nog producten nodig hebben. Als er producten worden verkocht, of de 
klant geeft aan nog voldoende voorraad te hebben dan wordt met de klant afgeboekt en wordt de terugbeltijd ingesteld om 
na een bepaalde vertraging (bijvoorbeeld 6 maanden) weer te bellen. Klanten waren vast gekoppeld aan een verkoper.

Als nu in de tussentijd een erg leuke aanbieding beschikbaar komt die ook voor deze klant interessant kan zijn, dan wordt 
via een update import (of wijzigen belopdrahcten actie) de belopdracht aangepast en wordt een ACTIE code ingesteld zodat
 het belscript weet dat het gaat om een speciale actie.
De agent belt, en de klant gaat wel of niet in op de actie, maar hoe dan ook, als deze actie cyclus is beeindigt moet de belopdracht 
 worden hersteld naar de eerder ingestelde 6 maand cyclus. Het is niet voldoende om de belopdracht af te boeken om over 6 maanden weer te bellen, dan kan het zijn dat deze klant veel later dan bedoeld weer wordt teruggebeld. Het zou bijvoorbeeld kunnenzijn dat een klant al over 1 maand weer gebeld zou moeten worden binnen de 6 maand cyclus.

Door bij het afboeken in de actie cyclus gebruik te maken van de onderstaande functie wordt het status object aangepast op basis van de voorgaande belhistorie.
Er wordt gezocht naar eeen "terugbellen" status die een terugbeltijd instelling heeft die meer dan 14 dagen in de toekomst ligt
en als deze wordt gevonden wordt deze terugbeltijd gebruikt, en de terugbelagent, en de notitie van die belpoging. Als vervolgens met het 
status object wordt afgecodeert herstelt Callpro de instellingen.

``` javascript
// Support functie
Date.prototype.addDays = function(days) {
    this.setDate(this.getDate() + parseInt(days));
    return this;
};


//
// Zoek een vorige terugbeller op en zet de terugbeltijd en agent bij deze afcodering
// zodat het terugbel cyclus is hersteld nav deze extra ACTIE bel actie.
// Doe dit alleen als de terugbeltijd minstens 14 dagen in de toekomst ligt, anders
// kunnen we beter de nieuwe cyclus aanhouden die via de status al wordt ingesteld.
//
function ResetToPreviousCallback(status)
{
   var loScript = goCallPro.GetScript();
   var loEntry=loScript.GetEntry();
   var found=false;
   var now = new Date();
   var checkDate = now.addDays(14);
   var callbackExpr = "";
   var callbackAgent = "";
   var entrynote = "";
   var i=1;
   if (status.prioritycategory== 2 || status.prioritycategory== 6) {
      while(i<=loEntry.CLAttemptsCount && !found)
      {
         var loAttempt = loEntry.CLAttempts(i);
         if(loAttempt.StatPriority<=75 && loAttempt.StatDateTime>checkDate) {
            found=true;
            // We have a matching previous "contact" 
            // Now recreate the callbackexpr
            var statDateTime = new Date(loAttempt.StatDateTime);
            callbackExpr = "Readonly=1,Date=";
            callbackExpr += statDateTime.getFullYear();
            callbackExpr += "-";
            callbackExpr += statDateTime.getMonth()+1;
            callbackExpr += "-";
            callbackExpr += statDateTime.getDate();
            callbackExpr += ", Time=";
            callbackExpr += statDateTime.getHours();
            callbackExpr += ":";
            callbackExpr += statDateTime.getMinutes();
            // and get the callbackagent
            if(loAttempt.Callbackagent!=null) {
               callbackAgent = loAttempt.CallbackAgent.Fullpath + loAttempt.CallbackAgent.ResName;

            }
            // and the entrynote
            if(loAttempt.Notes!=null)
               entrynote = loAttempt.Notes;
            }
            i++;
        }

        if(found) {
           // Update the status object using the information gathered
           status.callbackexpr = callbackExpr;
           status.callbackagent = callbackAgent;
           // FIX AUTOSCRIPT DEFAULT BEHAVIOUR BY PREPENDING THE NEW NOTE TO
           // THE SPECIAL ENTRYNOTE FIELD THAT THE AGENT ENTERED. THIS IS NEEDED
           // BECAUSE THE AUTOSCRIPT OVERWRITES THE ENTRYNOTE WITH THIS VALUE.
           if (getFieldValue("script_sys_entrynote") != undefined) {
              var _entrynote = getFieldValue("entrynote");
              if (_entrynote != undefined) {
                 if (_entrynote > "") {
                    setFieldValue("entrynote", entrynote + "\nACTIE:\n"+ _entrynote);
                 }
                 else
                 {
                    setFieldValue("entrynote", entrynote);
                 }
             }
        }          
     }
   } 
}
```