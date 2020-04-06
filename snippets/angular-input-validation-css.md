## Visuele weergave van input validatie met Angular

Het autoscript (Metro stijl) markeert alle input velden met Angular attributen. Hierdoor kan voor input validatie gebruik worden gemaakt van de css opmaak opties van Angular. Het autoscript gebruikt zelfdeze visuele markering niet zelf maar laat het aan de gebruiker om dit toe te passsen. Dit kan eenvoudig door  css opmaak te koppelen aan de ng-invalid en ng-valid.

Met onderstaande css in de FEATURE.CSS variable van de scriptdefinitie worden alle input velden rood die niet correct zijn ingevuld. Dit werkt out-of-the-box met verplichte velden, en url en email velden die een onjuiste inhoud hebben.

``` css
input.ng-invalid, select.ng-invalid, textarea.ng-invalid {
    background-color: red; 
}
```

Dit kan worden toegepast voor eigen controle functies die ng-invalid of ng-valid toevoegen aan de class attribuut. 


Er zijn nog meer classes en opties die Angular biedt. Kijk bijvoorbeeld eens op [W3 Schools AngularJS](https://www.w3schools.com/angular/angular_validation.asp) voro meer opties en mogelijkheden. 
