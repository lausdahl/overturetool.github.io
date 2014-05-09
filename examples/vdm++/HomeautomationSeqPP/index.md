---
layout: default
title: HomeautomationSeq
---

~~~
This is a sequential VDM++ version of a home automation example constructed
More information can be found in:
#******************************************************
~~~
###Actuator.vdmpp

{% raw %}
~~~
--The Actuator Class
-----------------------------------------------
--
--
protected ID	: nat;
--
public GetID: () ==> nat
public GetType: () ==> NetworkTypes`nodeType
public Step: () ==> ()
end Actuator
~~~{% endraw %}

###Environment.vdmpp

{% raw %}
~~~
--The Environment Class
-----------------------------------------------
--
--
private envTemp		: nat;
private ha       : HA;
private finished : bool := false;
--
-- Input file: Temp, Humid, Time
--
public Environment: seq of char ==> Environment
    ha := new HA();
public Run: () ==> ()
private CreateSignal: () ==> ()
public ReadTemp: () ==> nat
public IncTemp: () ==> ()
public DecTemp: () ==> ()
public SetTemp: nat ==> ()
public ReadHumid: () ==> nat
public IncHumid: () ==> ()
public DecHumid: () ==> ()
public SetHumid: nat ==> ()
public isFinished : () ==> bool
end Environment
~~~{% endraw %}

###HomeAutomation.vdmpp

{% raw %}
~~~
--The HA Class
-----------------------------------------------
--
--
public static Host	: HostController := new HostController(22, 75);
--
--
public HA: () ==> HA
--
--
end HA
~~~{% endraw %}

###HostController.vdmpp

{% raw %}
~~~
--The HostController Class
-----------------------------------------------
--
--
private finished	: bool := false;
private TargetTemp	: nat;
private NodeList	: map nat to NetworkTypes`nodeType := { |-> };
--
public algType	= <THTW> | <TTW> | <TT> | <TW> | <HW> | <NONE>;
--
public HostController: nat * nat ==> HostController
public UpdateValues: () ==> ()
public GetAlgo: () ==> algType
public GetTemp: () ==> nat
public GetHumid: () ==> nat
public Algorithm: () ==> ()
private THTWAlgo: () ==> ()
private TTWAlgo: () ==> ()
private TTAlgo: () ==> ()
private TWAlgo: () ==> ()
private HWAlgo: () ==> ()
private TargetReachedPrint: nat ==> ()
private UpdateAlgorithm: () ==> ()
public AddNode: nat * NetworkTypes`nodeType ==> ()
public RemoveNode: nat * NetworkTypes`nodeType ==> ()
public Step: () ==> ()
end HostController
~~~{% endraw %}

###HumidSensor.vdmpp

{% raw %}
~~~
--The HumidSensor Class
-----------------------------------------------
--
--
public HumidSensor: nat * NetworkTypes`nodeType * nat ==> HumidSensor
public Step: () ==> ()
end HumidSensor
~~~{% endraw %}

###NetworkTypes.vdmpp

{% raw %}
~~~
--The NetworkTypes Class
-----------------------------------------------
--
--
--
public nodeType 	= <TEMPSENSOR> | <HUMIDSENSOR> | <WINDOW> | <THERMOSTAT> | <HOSTCONTROL> | <NONE>;
--
--
--
end NetworkTypes
~~~{% endraw %}

###Sensor.vdmpp

{% raw %}
~~~
--The Sensor Class
-----------------------------------------------
--
--
protected ID	: nat;
--
public GetID: () ==> nat
public GetType: () ==> NetworkTypes`nodeType
public ReadValue: () ==> nat
public Step: () ==> ()
end Sensor
~~~{% endraw %}

###Surroundings.vdmpp

{% raw %}
~~~
--The Surroundings Class
-----------------------------------------------
--
--
private envTemp	: nat;
--
public Surroundings: () ==> Surroundings
public ReadTemp: () ==> nat
public IncTemp: () ==> ()
public DecTemp: () ==> ()
public SetTemp: nat ==> ()
public ReadHumid: () ==> nat
public IncHumid: () ==> ()
public DecHumid: () ==> ()
public SetHumid: nat ==> ()
end Surroundings
~~~{% endraw %}

###TemperatureSensor.vdmpp

{% raw %}
~~~
--The TemperatureSensor Class
-----------------------------------------------
--
--
public TemperatureSensor: nat * NetworkTypes`nodeType * nat ==> TemperatureSensor
public Step: () ==> ()
end TemperatureSensor
~~~{% endraw %}

###Test.vdmpp

{% raw %}
~~~
--The Test Class
-----------------------------------------------
--
--
public Run: TestResult ==> ()
end Test
~~~{% endraw %}

###TestActuator.vdmpp

{% raw %}
~~~
--The TestActuator Class
-----------------------------------------------
--
--
win 		: Window;
--
public TestActuator: seq of char ==> TestActuator
protected SetUp: () ==> ()
protected Test: () ==> ()
		AssertTrue(therm.GetID() = 4);
protected RunTest: () ==> ()
protected TearDown: () ==> ()
end TestActuator
~~~{% endraw %}

###TestCase.vdmpp

{% raw %}
~~~
--The TestCase Class
-----------------------------------------------
--
--
protected name : seq of char
--
public TestCase: seq of char ==> TestCase
public GetName: () ==> seq of char
protected AssertTrue: bool ==> ()
protected AssertFalse: bool ==> ()
public Run: TestResult ==> ()
protected SetUp: () ==> ()
protected RunTest: () ==> ()
protected TearDown: () ==> ()
end TestCase
~~~{% endraw %}

###TestComplete.vdmpp

{% raw %}
~~~
--The TestComplete Class
-----------------------------------------------
--
--
public Execute: () ==> ()
end TestComplete
~~~{% endraw %}

###TestHostController.vdmpp

{% raw %}
~~~
--The TestHostController Class
-----------------------------------------------
--
--
--env		: Environment;
--
public TestHostController: seq of char ==> TestHostController
protected SetUp: () ==> ()
protected Test: () ==> ()
--	HA`Host.AddNode(HA`TempNode.GetID(),HA`TempNode.GetType());
--	HA`Host.AddNode(HA`WinNode.GetID(),HA`WinNode.GetType());
--	HA`TempNode.Step();
--	HA`Host.UpdateValues();
--	HA`Host.Algorithm();
-- ****************************************************
--	HA`TempNode.Step();
--	HA`Host.UpdateValues();
--	HA`Host.Algorithm();
-- ****************************************************
	HA`TempNode.Step();
	HA`Host.UpdateValues();
	HA`Host.Algorithm();
-- ****************************************************
	HA`TempNode.Step();
	HA`Host.UpdateValues();
	HA`Host.Algorithm();
-- ****************************************************
	HA`TempNode.Step();
	HA`Host.UpdateValues();
	HA`Host.Algorithm();
-- ****************************************************
	HA`Host.RemoveNode(HA`ThermNode.GetID(),HA`ThermNode.GetType());
	HA`Host.AddNode(HA`WinNode.GetID(),HA`WinNode.GetType());
	HA`Host.AddNode(HA`HumidNode.GetID(),HA`HumidNode.GetType());
	HA`HumidNode.Step();
	HA`Host.UpdateValues();
	HA`Host.Algorithm();
-- ****************************************************
	HA`Host.AddNode(HA`TempNode.GetID(),HA`TempNode.GetType());
	HA`TempNode.Step();
	HA`Host.UpdateValues();
	HA`Host.Algorithm();
-- ****************************************************
	HA`Host.AddNode(HA`HumidNode.GetID(),HA`HumidNode.GetType());
protected RunTest: () ==> ()
protected TearDown: () ==> ()
end TestHostController
~~~{% endraw %}

###TestResult.vdmpp

{% raw %}
~~~
--The TestResult Class
-----------------------------------------------
--
--
failures : seq of TestCase := []
--
public AddFailure: TestCase ==> ()
public Print: seq of char ==> ()
public Show: () ==> ()
end TestResult
~~~{% endraw %}

###TestSensor.vdmpp

{% raw %}
~~~
--The TestSensor Class
-----------------------------------------------
--
--
tempSensor 	: TemperatureSensor;
--
public TestSensor: seq of char ==> TestSensor
protected SetUp: () ==> ()
protected Test: () ==> ()
		AssertTrue(humidSensor.GetID() = 2);
protected RunTest: () ==> ()
protected TearDown: () ==> ()

end TestSensor
~~~{% endraw %}

###TestSuite.vdmpp

{% raw %}
~~~
--The TestSuite Class
-----------------------------------------------
--
--
tests : seq of Test := [];
--
public Run: () ==> ()
public Run: TestResult ==> ()
public AddTest: Test ==> ()
end TestSuite
~~~{% endraw %}

###TestSurroundings.vdmpp

{% raw %}
~~~
--The TestSurroundings Class
-----------------------------------------------
--
--
env		: Environment;
--
public TestSurroundings: seq of char ==> TestSurroundings
protected SetUp: () ==> ()
protected Test: () ==> ()
		env.IncTemp();
		env.IncHumid();
protected RunTest: () ==> ()
protected TearDown: () ==> ()
end TestSurroundings
~~~{% endraw %}

###Thermostat.vdmpp

{% raw %}
~~~
--The Thermostat Class
-----------------------------------------------
--
--
public Thermostat: nat * NetworkTypes`nodeType ==> Thermostat
public Step: () ==> ()
public SetCorrection: NetworkTypes`correction ==> ()
public GetCorrection: () ==> NetworkTypes`correction
end Thermostat
~~~{% endraw %}

###timer.vdmpp

{% raw %}
~~~

class Timer
instance variables
currentTime : nat := 0;
values
stepLength : nat = 10;
operations
public StepTime : () ==> ()
public GetTime : () ==> nat
end Timer

~~~{% endraw %}

###Window.vdmpp

{% raw %}
~~~
--The Window Class
-----------------------------------------------
--
--
public Window: nat * NetworkTypes`nodeType ==> Window
public Step: () ==> ()
public SetCorrection: NetworkTypes`correction ==> ()
public GetCorrection: () ==> NetworkTypes`correction
end Window
~~~{% endraw %}

###World.vdmpp

{% raw %}
~~~
--The World Class
-----------------------------------------------
--
instance variables
static public env : Environment := new Environment("scenario.txt");
operations
public Run: () ==> ()
end World
~~~{% endraw %}
