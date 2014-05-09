---
layout: default
title: concfactorial
---

~~~

This example is made by Nick Battle and it illustrates how one can 
#******************************************************
~~~
###concfactorial.vdmpp

{% raw %}
~~~
class Factorial
operations
end Factorial
class Multiplier
operations
	doit : () ==> ()
	public giveResult : () ==> nat1
sync
	per doit => #fin (calculate) > #act(doit);
thread
end Multiplier
~~~{% endraw %}
