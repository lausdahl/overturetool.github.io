---
layout: default
title: CarNaviRadioValConj
---

~~~

This example is a modified version of the car radio navigation
The origin of the car radio navigation example comes from Marcel
#******************************************************
~~~
###mmi.vdmrt

{% raw %}
~~~
class MMI
operations
  async 
end MMI
~~~{% endraw %}

###Navigation.vdmrt

{% raw %}
~~~
class Navigation
operations
  async 
end Navigation
~~~{% endraw %}

###Radio.vdmrt

{% raw %}
~~~
class Radio
values 
instance variables
operations
  async public AdjustVolumeUp : () ==> ()
  async public AdjustVolumeDown : () ==> ()
  async public HandleTMC: () ==> ()
end Radio
~~~{% endraw %}

###RadNavSys.vdmrt

{% raw %}
~~~
system RadNavSys
  -- create CPUs (policy, capacity)
  -- create a bus (policy, capacity, topology)
operations
/* timing invariants
end RadNavSys
~~~{% endraw %}

###Test.vdmrt

{% raw %}
~~~
class Testing
operations

  private block : () ==> ()
  private op : () ==> ()
sync
per block => time > 5000000

thread
  periodic(1000E6,0,0,0)(op)
end Testing

~~~{% endraw %}

###World.vdmrt

{% raw %}
~~~
class World
types
instance variables

operations
  public RunScenario1 : () ==> ()


end World
~~~{% endraw %}
