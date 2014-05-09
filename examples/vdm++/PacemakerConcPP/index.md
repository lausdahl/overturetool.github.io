---
layout: default
title: PacemakerConc
---

~~~
This model is made by Hugo Macedo as a part of his MSc thesis of a
Hugo Macedo, Validating and Understanding Boston Scientific Pacemaker
Hugo Daniel Macedo, Peter Gorm Larsen and John Fitzgerald, Incremental 
#******************************************************
~~~
###Accelerometer.vdmpp

{% raw %}
~~~

class Accelerometer is subclass of GLOBAL
operations
 public 
end Accelerometer

~~~{% endraw %}

###BaseThread.vdmpp

{% raw %}
~~~
class BaseThread
instance variables
protected period : nat1 := 1;
operations
protected BaseThread : () ==> BaseThread
public SetPeriod : nat1 ==> ()
Step : () ==> ()
thread
end BaseThread
~~~{% endraw %}

###Environment.vdmpp

{% raw %}
~~~

class Environment is subclass of GLOBAL, BaseThread
 types 
public Inpline = (Sense * Chamber * ActivityData * Time);
public Outline = (Pulse * Chamber * Time);  
 instance variables
-- Input/Output 
inplines : seq of Inpline := [];
-- Environment  
busy : bool := true;
-- Amount of time we want to simulate
 instance variables
-- Leads
leads : map Chamber to Lead := {|->};
-- Accelerometer

 operations
-- Constructor

public 
public 


private 


public 

public

public 
public Step: () ==> ()
  if World`timerRef.GetTime() >= simtime
--thread
--  );
sync 
mutex (handleEvent,showResult);
per isFinished => not busy;
end Environment


~~~{% endraw %}

###GLOBAL.vdmpp

{% raw %}
~~~

class GLOBAL
types 

-- Sensed activity
-- Heart chamber identifier

-- Accelerometer output

-- Paced actvity
-- Operation mode

-- PPM

-- Time
end GLOBAL

~~~{% endraw %}

###HeartController.vdmpp

{% raw %}
~~~

class HeartController is subclass of GLOBAL, BaseThread
instance variables 
 leads     : map Chamber to Lead;

operations
 public 


 public 

 public 

 private
 private
 private
 public 
 public 
 public 
 public 
 public 
public Step: () ==> ()

--thread
-- (while true do
sync
per isFinished => sensed = {|->} and finished;
mutex(sensorNotify,pace);
end HeartController

~~~{% endraw %}

###Lead.vdmpp

{% raw %}
~~~

class Lead is subclass of GLOBAL, BaseThread
instance variables
 private chamber : Chamber;       
operations
 public 

 public 

 public 

 public 

 public 
 private 
   );


 private 
public Step: () ==> ()

--thread


sync
--per followPlan => scheduledPulse <> nil;
mutex(Step);
end Lead 

~~~{% endraw %}

###Pacemaker.vdmpp

{% raw %}
~~~

class Pacemaker 
 instance variables
 public static 
 public static 

 instance variables
 public static 
 public static 
 instance variables
 public static 
end Pacemaker

~~~{% endraw %}

###RateController.vdmpp

{% raw %}
~~~

class RateController is subclass of GLOBAL, BaseThread
instance variables


instance variables
inv threshold < 8
operations
 public 
public
 private

 public 
 private

 private
 public 
 public 
public Step: () ==> ()


--thread

sync
mutex(increaseRate,decreaseRate,getInterval);
per isFinished => finished;
mutex(Step);

values
--V-LOW 1
end RateController

~~~{% endraw %}

###TimeStamp.vdmpp

{% raw %}
~~~

class TimeStamp


values
public stepLength : nat = 1;
instance variables
currentTime  : nat   := 0;
operations
public TimeStamp : nat ==> TimeStamp
public RegisterThread : BaseThread ==> ()
public UnRegisterThread : () ==> ()
public IsInitialising: () ==> bool
public DoneInitialising: () ==> ()
public WaitRelative : nat ==> ()
public WaitAbsolute : nat ==> ()
BarrierReached : () ==> ()
AddToWakeUpMap : nat * [nat] ==> ()
public NotifyThread : nat ==> ()
public GetTime : () ==> nat
Awake: () ==> ()
public ThreadDone : () ==> ()
sync
mutex(IsInitialising);
  mutex(AddToWakeUpMap, NotifyThread);
  mutex(AddToWakeUpMap, NotifyThread, BarrierReached);
end TimeStamp
~~~{% endraw %}

###World.vdmpp

{% raw %}
~~~

class World is subclass of GLOBAL
types
instance variables
public static env      : [Environment] := nil;
operations
public World: seq of char * Mode ==> World
     -- bind leads to the environment
     -- bind accelerometer to the environment
     -- bind leads to the controler
     -- set up mode
     --start(Pacemaker`heartController);
     --start(Pacemaker`ventricleLead);

public Run: () ==> ()

end World

~~~{% endraw %}
