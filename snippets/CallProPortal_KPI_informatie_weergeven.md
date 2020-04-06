##  Weergaven van KPI informatie uit CallProPortal

Vanaf v4.3.2.31 heeft CallProPortal enkele KPI API's die mbv SignalR gegevens kunnen aanleveren. Met dit snippet kunnen enkele van deze gegevens worden opgehaald en afgebeeld in een autoscript pagina.

Variabele: `BREAK.INFO`
data-type: memo
```
<!--
 Reference CallProPortal signalR
-->
<script src="http://callproportal/assets/signalr.js/jquery.signalR.min.js"></script>
<script src="http://callproportal/signalr/hubs"></script>
<!-- SignalR script to update the agentstate data.-->
<script>
var SignalR_BaseUrl = "http://callproportal";

function StartSignalRConnection() {
  // Reference the auto-generated proxy for the hub.
  var agentstate = $.connection.AgentDialHub;
  if(agentstate) {
    // Start the AgentDialHub connection.
    $.connection.hub.start().done(function () {
      // Success
      console.log("AgentDialHub connection started succesfully");
      // Now configure client settings
      agentstate.server.setAgentInfo(%AGENT.RESID \SW"","null"%);
    });
  }
  else
    console.log("AgentDialHub not connected");
}

$(function () {
	//Set the hubs URL for the connection
    var connection = $.connection;
    if(connection)
    {
		$.connection.hub.url = SignalR_BaseUrl + "/signalr";

		// Reference the auto-generated proxy for the hub.
		var agentstate = $.connection.AgentDialHub;
		if(agentstate) {
			// Create a function that the hub can call back to update the agentstate.
			agentstate.client.updateAgentState = function (data) {
				console.log("updateAgentState: receive data");
				if(data != null) {
				  data.sort(function (a, b) {
					return (a.Agent < b.Agent) ? -1 : (a.Agent > b.Agent) ? 1 : 0;
				  });

				  agentList = "";
				  for(i = 0; i < data.length; i++) {
					var AgentName = data[i].Agent;
					var AgentState = data[i].AgentState;
					var tagColor = "";
					var tagInfo = "";
					switch(AgentState) {
					case 1: 
					  tagColor = "info";
					  tagInfo = "Break";
					  break;
					case 3: 
					  tagColor = "success";
					  tagInfo = "Free";
					  break;
					default: 
					  tagColor = "alert";
					  tagInfo = "Busy";
					  break;
					}
					agentList += "<span class='tag " + tagColor + "' title='" + tagInfo + "'>" + AgentName + "</span><br/>";
				  }
				  if(agentList=="")
					agentList="Geen agenten";
				  $("#AgentList").html(agentList);
				}            
			};
			// Handle reconnecting events
			$.connection.hub.reconnecting(function() {
				console.log("Reconnecting fired!");
			});
			$.connection.hub.reconnected(function() {
				console.log("Reconnected fired!");
			});

			// Configure a reconnect task
			$.connection.hub.disconnected(function() {
				if ($.connection.hub.lastError) 
					console.log("Disconnected. Reason: " +  $.connection.hub.lastError.message); 

				setTimeout(function() {
					StartSignalRConnection();
				}, 5000); // Restart connection after 5 seconds.
			});
		}
		else
			console.log("AgentDialHub not connected");
	}
	else
		console.log("SignalR connection not established!");
	
	StartSignalRConnection();
});
</script>

<!-- Custom tile -->
<div class="row">
	<div class="col-12">
		<div class="tile tile-wide bg-taupe fg-white" data-role="tile">
		  <div class="tile-content padding10">
			<span id="AgentList"></span>
			<span class="tile-label">Agent status</span>
		  </div>
		</div>
	</div>
</div>
```

Met de bovenstaande code wordt een tile afgebeeld die de status van de agetnen in hetzelfde team als de ingelogde agent weergeeft. Aan de kleur is te zien of een agent in pauze staat, beschikbaar is of in een gesprek zit.
