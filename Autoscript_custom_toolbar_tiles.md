## Custom toolbar tiles

In het autoscript zijn twee plaatsen waar eigen tiles kunnen worden ingevoegd. Via de variabeln `SCRIPT.TOOLBAR.CUSTOMTILE1` en `SCRIPT.TOOLBAR.CUSTOMTILE2` kan markup worden geplaatst binnen de toolbar van het autoscript. Door hier de juiste opmaak te gebruiken voor een tile kan informatie als tile worden afgebeeld.

Onderstaande voorbeeld voegt een tile toe met de agent die het laatst een afsrpaak heeft gemaakt op de campagne.

```
<div style="display:inline">
<!--
 Adding services and controllers
-->
<script>
// add an API service
app.service("AutoscriptApiService", function ($http, $log) {

	this.Get = function (campaignid) {
		var r = $http.get("/api/Services/GetLastAppointmentMadeby/?CampaignID=" + campaignid);
		return r;
	}
})

// Add a new controller to our app
app.controller('LastAppointmentMadeByController', function($scope, $log, AutoscriptApiService) {
    Get();

    campaignid = -1;
	data = null;
	
	function Get() {
   	    $scope.campaignid = %CAMPAIGN.RESID%;
   		var serviceCall = AutoscriptApiService.Get($scope.campaignid);
   		serviceCall.then(function (data) {
   			$scope.data = data.data;
   		}, function(error)
   		{
   			$log.error('LastAppointmentMadeByController.Get: Something went wrong while fetching the data!')
   		})
	} 

});
</script>
<!--
 This is the Tile markup
 -->
<div ng-controller="LastAppointmentMadeByController" class="tile fg-white" data-role="tile">
	<div class="tile-content slide-up-2">
		<div class="slide">
			<div class="image-container image-format-fill" style="width: 100%; height: 100%;">
				<div class="frame">
					<div style="width: 100%; height: 100%; background-image: url(&quot;/controls/GetResourcePicture?agentid={{data.AgentID}}&quot;); background-size: cover; background-repeat: no-repeat; border-radius: 0px;"></div>
				</div>
			</div>
		</div>
		<div class="slide-over bg-orange text-small padding10">
			datum {{data.CreatedDtm}}<br/>
			<br/>
			agenda: {{data.CalendarName}} <br/>
		</div>
		<div class="tile-label">{{data.AgentName}}</div>
	</div>
</div>

</div>

```

Met de volgende code worden drie tiles toegevoegd met wachtrij informatie van de gekoppelde campagne. Meer informatie over welke data het API end-point teruggeeft kan worden gevonden door de url vanuit de browser of postman op te roepen en de json data te bekijken.

```
<!--
 Adding services and controllers
-->
<script>
    // http://stackoverflow.com/questions/6312993/javascript-seconds-to-time-string-with-format-hhmmss
    String.prototype.toHHMMSS = function () {
        var sec_num = parseInt(this, 10); // don't forget the second param
        var hours = Math.floor(sec_num / 3600);
        var minutes = Math.floor((sec_num - (hours * 3600)) / 60);
        var seconds = Math.floor(sec_num - (hours * 3600) - (minutes * 60));

        if (hours < 10) { hours = "0" + hours; }
        if (minutes < 10) { minutes = "0" + minutes; }
        if (seconds < 10) { seconds = "0" + seconds; }
        return hours + ':' + minutes + ':' + seconds;
    }
    Number.prototype.toHHMMSS = function () {
        var sec_num = this;
        var hours = Math.floor(sec_num / 3600);
        var minutes = Math.floor((sec_num - (hours * 3600)) / 60);
        var seconds = Math.floor(sec_num - (hours * 3600) - (minutes * 60));

        if (hours < 10) { hours = "0" + hours; }
        if (minutes < 10) { minutes = "0" + minutes; }
        if (seconds < 10) { seconds = "0" + seconds; }
        return hours + ':' + minutes + ':' + seconds;
    }
    Number.prototype.toMMSS = function () {
        var sec_num = this;
        var minutes = Math.floor(sec_num / 60);
        var seconds = Math.floor(sec_num - (minutes * 60));

        if (minutes < 10) { minutes = "0" + minutes; }
        if (seconds < 10) { seconds = "0" + seconds; }
        return minutes + ':' + seconds;
    }
    Number.prototype.toHH = function () {
        var sec_num = this;
        var hours = sec_num / 3600;
        return hours;
    }
</script>
<script>
// add an API service
app.service("QueueInfoForCampaignApiService", function ($http, $log) {

	this.Get = function (campaignid) {
		var r = $http.get("/api/Services/GetQueueInfoForCampaign?CampaignID=" + campaignid);
		return r;
	}
})

// Add a new controller to our app
app.controller('QueueInfoForCampaignController', function($scope, $log, QueueInfoForCampaignApiService) {
    Get();

    campaignid = -1;
	data = null;
	kpiAvgCallWaitTime = "";

	function Get() {
   	    $scope.campaignid = %CAMPAIGN.RESID%;
   		var serviceCall = QueueInfoForCampaignApiService.Get($scope.campaignid);
   		serviceCall.then(function (data) {
   			$scope.data = data.data;
            $scope.kpiAvgCallWaitTime = Number(data.data.AvgCallWaitTime).toMMSS();
   		}, function(error)
   		{
   			$log.error('QueueInfoForCampaignController.Get: Something went wrong while fetching the data!')
   		})
	} 

});
</script>
<!--
 This is the Tile markup
 -->
<div ng-controller="QueueInfoForCampaignController">
<div class="tile bg-green fg-white" data-role="tile">
  <div class="tile-content">
    <br>
    <h1 class="align-center">{{data.Calls}}</h1>
    <span class="tile-label">Inbound Calls</span>
  </div>
</div>

<div class="tile bg-orange fg-white" data-role="tile">
  <div class="tile-content">
    <br>
    <h1 class="align-center">{{kpiAvgCallWaitTime}}</h1>
    <span class="tile-label">Inbound Wachttijd</span>
  </div>
</div>

<div class="tile fg-white" data-role="tile">
	<div class="tile-content slide-up-2">
		<div class="slide">
			<div class="image-container image-format-fill" style="width: 100%; height: 100%;">
				<div class="frame bg-lightBlue">
					<span>Derde tile in CustomTile2 locatie</span>
				</div>
			</div>
		</div>
		<div class="slide-over bg-orange text-small padding10">
			
		</div>
		<div class="tile-label">CustomTile2.3</div>
	</div>
</div>

</div>
```