## Terugbelplanning

Met dit overzicht kan een agent zien hoeveel gerichtte terugbelafspraken hij/zij al heeft gemaakt per uur van de dag voor de komende X dagen.
Als shortcut kan ook op een tijdvak worden geklikt om direct een terugbelafspraak in te plannen. De status die hiervoor gebruikt wordt staat standaard ingesteld op statcode 310 maar kan eenvoudig worden aangepast.


``` html
<div class="accordion" data-role="accordion">
<div class="frame">
  <a href="#" class="heading">Terugbelplanning</a>
  <div class="content">
	<script>
	// Adding services and controllers
	// https://tc39.github.io/ecma262/#sec-array.prototype.find
	if (!Array.prototype.find) {
	  Object.defineProperty(Array.prototype, 'find', {
		value: function(predicate) {
		// 1. Let O be ? ToObject(this value).
		if (this == null) {
			throw new TypeError('"this" is null or not defined');
		}
		var o = Object(this);
		// 2. Let len be ? ToLength(? Get(O, "length")).
		var len = o.length >>> 0;
		// 3. If IsCallable(predicate) is false, throw a TypeError exception.
		if (typeof predicate !== 'function') {
			throw new TypeError('predicate must be a function');
		}
		// 4. If thisArg was supplied, let T be thisArg; else let T be undefined.
		var thisArg = arguments[1];
		// 5. Let k be 0.
		var k = 0;
		// 6. Repeat, while k < len
		while (k < len) {
			// a. Let Pk be ! ToString(k).
			// b. Let kValue be ? Get(O, Pk).
			// c. Let testResult be ToBoolean(? Call(predicate, T, « kValue, k, O »)).
			// d. If testResult is true, return kValue.
			var kValue = o[k];
			if (predicate.call(thisArg, kValue, k, o)) {
			  return kValue;
			}
			// e. Increase k by 1.
			k++;
		}
		// 7. Return undefined.
		return undefined;
		}
	});
	}

	// add an API service
	app.service("AgentCallbackCalendarApiService", function ($http, $log) {
		//
		// MaxCallbackDay -> Number of days into the furute to show callbacks (default=10)
		// MinCallHH -> Earliest hour of the day to show (default=9)
		// MaxCallHH -> Latest hour of the day to show (default=22)
		//
		this.GetAgentCallbackCalendar = function (agentid) {
			var r = $http.get("/api/Services/GetAgentCallbackCalendar?AgentID=" + agentid + "&MaxCallbackDays=10&MinCallHH=9&MaxCallHH=22&nocache=true");
			return r;
		}
	})

	// Add a new controller to our app
	app.controller('AgentCallbackCalendarController', function($scope, $log, AgentCallbackCalendarApiService) {
		GetAgentCallbackCalendar();

		agentid = -1;
		data = null;
		hours = [];   
	function GenerateHours()
	{
	  var hours = [];      
	  for(var i=$scope.data.MinCallHH;i<=$scope.data.MaxCallHH;i++) {
		hours.push(i);
	  }
	  $scope.hours = hours;
	}

	dates = []
	function GetDistinctDates()
	{
	  var dates = [];      
	  for(var i=0; i< $scope.data.MaxCallbackDays;i++)
	  {
		var newDate = new Date();
		newDate.setDate(newDate.getDate() + i);
		dates.push(newDate.toISOString().split("T")[0]);
	  }
	  $scope.data.calendar.forEach(function (item,index) { 
		var checkDate = new Date(item.CallDate).toISOString().split("T")[0];
		var exists = dates.find(function (value) { return value==checkDate;});
		if(!exists)
		  dates.push(checkDate);
	  })
	  $scope.dates = dates;
	}

	$scope.GetCallbacksForDateAndTime = function (date, hour)
	{
	  var result = "";
	  $scope.data.calendar.forEach(function (item,index) { 
		itemDate = new Date(item.CallDate).toISOString().split("T")[0];
		if(itemDate == date && item.CallHH == hour) {
		  result = item.Calls;
		}
	  });
	  return result;
	}

	$scope.SelectDateAndTime = function (date,hour) {
      var callbacks = $scope.GetCallbacksForDateAndTime(date, hour);
      if(callbacks <= 5)
	    window.location='#SCRIPT_SELECTSTAT?STATCODE=310&CALLBACKEXPR=Date='+ date + ',Time='+ hour+ ':00';
	}

	$scope.GetCellStyles = function(date,hour) {
		var now = new Date();
        var currentDate = now.toISOString().split("T")[0];
        if(date == currentDate && hour <= now.getHours())
   			return { "background-color": "#999999" };
        var callbacks = $scope.GetCallbacksForDateAndTime(date, hour);
        if(callbacks > 5)
            return { "background-color": "red" };

    	return { "cursor": "pointer" };
	}
    $scope.GetDisabledState = function(date,hour) {
		var now = new Date();
        var currentDate = now.toISOString().split("T")[0];
        if(date == currentDate)
    		if(hour <= now.getHours())
	    		return true;
		    else 
			     return false;
    }
	function GetAgentCallbackCalendar () {
		$scope.agentid = %AGENT.RESID%;

		var serviceCall = AgentCallbackCalendarApiService.GetAgentCallbackCalendar($scope.agentid);
		serviceCall.then(function (data) {
			$scope.data = data.data;
			GenerateHours();
			GetDistinctDates();

		}, function(error)
		{
			$log.error('Oops! Something went wrong while fetching the data!')
		})
	} 



	});
	// generate the table
	</script>
	<table ng-controller="AgentCallbackCalendarController" class="table cell-hovered border bordered">
	<thead>
	<tr>
	<th>Datum</th>
	<th ng-repeat="hour in hours">{{hour}}</th>
	</tr>
	</thead>
	<tbody>
	<tr ng-repeat="date in dates">
	<td>{{date}}</td>
	<td ng-repeat="hour in hours" ng-click="SelectDateAndTime(date, hour)" ng-disabled="GetDisabledState(date,hour)" ng-style="GetCellStyles(date,hour)">
	  {{ GetCallbacksForDateAndTime(date,hour) }}
	</td>
	</tr>

	</tbody>
	</table>

  </div>
</div>

</div>
```
