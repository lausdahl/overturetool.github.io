---
layout: default
title: HomeAutomation
---

~~~
This is a distributed real-time version of a home automation example constructed
More information can be found in:
#******************************************************
~~~
###Actuator.vdmrt

{% raw %}
~~~
-----------------------------------------------
--
--
  protected ID   : nat;
--
public GetID: () ==> nat
public GetType: () ==> NetworkTypes`nodeType
public Step: () ==> ()
end Actuator
~~~{% endraw %}

###Environment.vdmrt

{% raw %}
~~~
-----------------------------------------------
--
--
  private io       : IO := new IO();
--
-- Input file: TempIn, HumidIn, TimeIn
--
public Environment: seq of char ==> Environment
private CreateSignal: () ==> ()
private ShowResults: () ==> ()
  for all i in set inds outlines
public HandleEvent: nat * nat * nat ==> ()
public SetTemp: nat ==> ()
public SetHumid: nat ==> ()
public ReadTemp: () ==> int
public IncTemp: () ==> ()
public DecTemp: () ==> ()
public ReadHumid: () ==> nat
public IncHumid: () ==> ()
public DecHumid: () ==> ()
public IsFinished: () ==> ()
sync
  mutex(IncTemp);
--
-- period of thread (period, jitter, delay, offset)
end Environment
~~~{% endraw %}

###HomeAutomation.vdmrt

{% raw %}
~~~
-----------------------------------------------
--
-- 
  -- cpu for host controller
  -- bus connecting host controller and sensors
  public static Host      : HostController := new HostController(20, 75);
--
public HA: () ==> HA
end HA
~~~{% endraw %}

###HostController.vdmrt

{% raw %}
~~~
-----------------------------------------------
--
values
  private OVERSHOOT_CNT : nat = 2;
--
  private finished    : bool := false;
  private TargetTemp  : nat;
  private incTempCnt  : nat := 0;
--
  public algType = <THTW> | <TTW> | <TT> | <TW> | <HW> | <NONE>;
--
public HostController: nat * nat ==> HostController
public UpdateValues: () ==> ()
public GetAlgo: () ==> algType
public GetTemp: () ==> nat
public GetHumid: () ==> nat
public Algorithm: () ==> ()
  cases Algo:
-- Avoid overshooting target values
private TTWAlgo: () ==> ()
private TTAlgo: () ==> ()
private TWAlgo: () ==> ()
private HWAlgo: () ==> ()
private UpdateAlgorithm: () ==> ()
public AddNode: nat * NetworkTypes`nodeType ==> ()
public RemoveNode: nat * NetworkTypes`nodeType ==> ()
private PeriodicOp: () ==> ()
public IsFinished: () ==> ()
public Finish: () ==> ()
sync
--
-- period of thread (period, jitter, delay, offset)
end HostController
~~~{% endraw %}

###HumidSensor.vdmrt

{% raw %}
~~~
-----------------------------------------------
--
--
  finished : bool := false;
--
public HumidSensor: nat * NetworkTypes`nodeType * nat ==> HumidSensor
public PeriodicOp: () ==> ()
public IsFinished: () ==> ()
sync
--
-- period of thread (period, jitter, delay, offset)
end HumidSensor
~~~{% endraw %}

###NetworkTypes.vdmrt

{% raw %}
~~~
-----------------------------------------------
--
--
public nodeType   = <TEMPSENSOR> | <HUMIDSENSOR> | <WINDOW> | <THERMOSTAT> | <HOSTCONTROL> | <NONE>;
end NetworkTypes
~~~{% endraw %}

###Sensor.vdmrt

{% raw %}
~~~
-----------------------------------------------
--
--
  protected ID    : nat;
--
public GetID: () ==> nat
public GetType: () ==> NetworkTypes`nodeType
public ReadValue: () ==> int
public Step: () ==> ()
end Sensor
~~~{% endraw %}

###TemperatureSensor.vdmrt

{% raw %}
~~~
-----------------------------------------------
--
--
  finished : bool := false;
--
public TemperatureSensor: nat * NetworkTypes`nodeType * int ==> TemperatureSensor
public PeriodicOp: () ==> ()
public IsFinished: () ==> ()
sync
--
-- period of thread (period, jitter, delay, offset)
end TemperatureSensor
~~~{% endraw %}

###Thermostat.vdmrt

{% raw %}
~~~
-----------------------------------------------
--
--
  finished : bool := false;
--
public Thermostat: nat * NetworkTypes`nodeType 
private PeriodicOp: () ==> ()
async public SetCorrection: NetworkTypes`correction ==> ()
public GetCorrection: () ==> NetworkTypes`correction
public IsFinished: () ==> ()
sync
--
-- period of thread (period, jitter, delay, offset)
end Thermostat
~~~{% endraw %}

###Window.vdmrt

{% raw %}
~~~
-----------------------------------------------
--
--
  finished : bool := false;
--
public Window: nat * NetworkTypes`nodeType ==> Window
private PeriodicOp: () ==> ()
async public SetCorrection: NetworkTypes`correction ==> ()
public GetCorrection: () ==> NetworkTypes`correction
public IsFinished: () ==> ()
sync
--
-- period of thread (period, jitter, delay, offset)
end Window
~~~{% endraw %}

###World.vdmrt

{% raw %}
~~~
-----------------------------------------------
--
--
  public static env : [Environment] := nil;
--
public World: () ==> World
  start(HA`TempNode);
public Run: () ==> ()
end World
~~~{% endraw %}
