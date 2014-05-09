---
layout: default
title: PacemakerSimple
---

~~~
This model is a very simple version of the pacemaker as it has
#******************************************************
~~~
###Heart.vdmpp

{% raw %}
~~~
class Heart
types
public Trace = seq of [Event];
public Event = <A> | <V>;
instance variables
aperiod : nat := 15;
operations
public Heart: nat * nat ==> Heart
public IdealHeart: () ==> Trace
public FaultyHeart() tr : Trace
functions
public Periodic: Trace * Event * nat1 -> bool
end Heart
~~~{% endraw %}

###Pacemaker.vdmpp

{% raw %}
~~~
class Pacemaker
values
public wrongTR: Heart`Trace = 
operations
public Pace: Heart`Trace * nat1 * nat1 ==> Heart`Trace
end Pacemaker
~~~{% endraw %}
