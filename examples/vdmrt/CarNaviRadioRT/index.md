---
layout: default
title: CarNaviRadio
---

~~~

The origin of the car radio navigation example comes from Marcel
#******************************************************
~~~
###EnvTask.vdmrt

{% raw %}
~~~
class EnvironmentTask 
instance variables
  -- we limit the number of inserted stimuli
  -- administration for the event traces
functions
operations
  public getNum: () ==> nat
  -- Run shall be overloaded to implement the event generation loop
  public HandleEvent: nat ==> ()
  -- logEnvToSys is used to register when an event was inserted into
  -- logSysToEnv is used to register when an event was received from}
  -- getMinMaxAverage calculates the minimum, maximum and average
public static IsFinished: () ==> ()
sync
  per IsFinished => #fin(logSysToEnv) > 0;
end EnvironmentTask
~~~{% endraw %}

###InsertAdress.vdmrt

{% raw %}
~~~
class InsertAddress
operations
  public HandleEvent: nat ==> ()
  public Run: () ==> ()
  createSignal: () ==> ()
thread
end InsertAddress
~~~{% endraw %}

###mmi.vdmrt

{% raw %}
~~~
class MMI
operations
  async 
end MMI
~~~{% endraw %}

###navigation.vdmrt

{% raw %}
~~~
class Navigation
operations
  async 
end Navigation
~~~{% endraw %}

###radhavsys.vdmrt

{% raw %}
~~~
system RadNavSys
instance variables
  -- create an Radio class instance
  -- create an Navigation class instance
  -- create a communication bus that links the three CPU's together
operations
end RadNavSys
~~~{% endraw %}

###radio.vdmrt

{% raw %}
~~~
class Radio
operations
  async 
end Radio
~~~{% endraw %}

###Test.vdmrt

{% raw %}
~~~
class Test
instance variables
mmi   : MMI        := new MMI();
traces
TT: let x in set {1,2,3}
end Test
~~~{% endraw %}

###TransmitTMC.vdmrt

{% raw %}
~~~
class TransmitTMC
operations
  public HandleEvent: nat ==> ()
  public Run: () ==> ()
  createSignal: () ==> ()
thread
end TransmitTMC
~~~{% endraw %}

###VolumeKnob.vdmrt

{% raw %}
~~~
class VolumeKnob
operations
  public HandleEvent: nat ==> ()
  public Run: () ==> ()
  createSignal: () ==> ()
thread
end VolumeKnob
~~~{% endraw %}

###World.vdmrt

{% raw %}
~~~
class World
types
instance variables
operations
  public RunScenario1 : () ==> map seq of char to perfdata
  public RunScenario2 : () ==> map seq of char to perfdata 
end World
~~~{% endraw %}
