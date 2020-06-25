## Weergave van de gespreksfase in het belscript met maximale tijd signalering

Op het belvloer overzicht wordt de gesprekstijd en belfase weergeven inclusief signalering met kleur. Voor het belscript is dit niet automatisch beschikbaar.
Wel kan met onderstaande scriptcode een signalering op maat worden gemaakt zowel qua uiterlijk, als qua timing. In dit voorbeeld is de maximale tijd per gespreksfase wacht, call en wrapup in te stellen.
De weergave gebeurd hier met een button die van kleur veranderd als de maximale tijd is verstreken. 

``` html
<script>
// Initialisatie: Stel een time in voor het controlerne van de CallState
var oCallStateTimer = window.setTimeout("CallStateTimer()", 1000);

function CallStateTimer()
{
var loCallPro = window.external;
var loScript = loCallPro.GetScript();
var loDialer = loScript.oDialer;

var monitor = loDialer.oAgentStateMonitor;
var notifier = loCallPro.oAgentStateNotifier;
var callState = "";
var maxTime = 0;
switch(monitor.DialState) {
  case 4:
    // wrapup
    callState="Wrapup";
    maxCallTime = 0.5;
    break;
  case 6:
    // call
    callState="Call";
    maxTime = 10;
    break;
  default:
    callState="Wait";
    maxTime = 1;
    break;
}
var startTime = new Date(notifier.StartTime);
var currentTime = new moment(new Date());
var duration = moment.duration(currentTime.diff(startTime)).asMinutes();
if(duration>maxTime)
{
  var button = $("#CallStateTimer_warning");
  if(!button.hasClass("danger"))
    button.addClass("danger");
}
callStateTime.innerHTML = callState + ": " + duration.toFixed(1);
// Nu gaan we in een 1s loop
oCallStateTimer = window.setTimeout("CallStateTimer()", 1000)
} 
</script>
<button id="CallStateTimer_warning" class="button loading-cube"><label id="callStateTime">...</label></button>

```
