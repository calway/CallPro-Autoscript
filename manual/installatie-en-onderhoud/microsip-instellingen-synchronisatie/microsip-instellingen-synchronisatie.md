# Microsip instellingen synchronisatie
**Scriptmodule vanaf v4.3.65**

Met de nieuwe script.net module (vanaf v4.3.2.65) wordt het beheer van
de microsip instellingen synchronisatie vereenvoudigd. De instellingen
van microsip worden overgenomen uit de Seat instellingen, en de Asterisk
instelling wordt van het dial apparaat gehaald. Hiervoor dient de
best-practise installatie richtlijn te worden gevolgd.

1.  Zorg ervoor dat de scriptmodule in een sub-folder staat waar ook de
    portable versie van microsip is geïnstalleerd.  
    ![](./media/image1.png)

2.  In de Config database van CallPro moet een key aanwezig zijn.
    
    1.  De settings override template (mergetool syntax) “Dialing,
        “MicrosipSettingsOverride” die de .ini onderdelen bevat die
        door CallPro moeten worden overschreven.

> ![](./media/image2.png)

3.  Dubbelcheck dat in de script.exe.config in de Scriptmodule folder
    geen uitzondering staat in de instelling “AsteriskHostname”.
    Verwijder deze instelling en controleer de ingestelde waarde met de
    instelling van punt 4.

4.  Controleer dat in de Dialing configuratie bij Dialer eigenschappen
    voor de “ASTERISK2” dialer de hostname of IP van de asterisk correct
    staat ingesteld.  
    ![](./media/image3.png)

5.  Zet de instelling “MicrosipDetection” op “1” of beter, verwijder de
    setting uit de script.exe.config. Als deze key niet aanwezig is, of
    op “1” staat zal de scriptmodule de auto-configuratie proberen.

> Met deze instellingen zal de scriptmodule Microsip instellingen
> gelijkhouden met de actuele Seat instellingen. Als Microsip al gestart
> is voordat de scriptmodule wordt gestart wordt deze automatisch
> afgesloten , aangepast en weer opnieuw gestart.
> 
> Dit betekent ook dat voor remote/thuis werkers de volgorde van
> opstarten kan worden aangepast naar:

1.  Start de VPN verbinding

2.  Start de Scriptmodule (dit start nu ook automatisch Microsip)