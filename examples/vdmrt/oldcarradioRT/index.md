---
layout: default
title: oldcarradio
---

~~~
This was the first model Marcel Verhoef tried to make of the car
#******************************************************
~~~
###AbstractTask.vdmrt

{% raw %}
~~~

class AbstractTask
instance variables
  -- the queue for normal events to be handled by this task
  -- a link to the dispatcher for out-going messages (events)
operations
  public getName: () ==> seq of char
  -- setEvent is a call-back used by the EventDispatcher to insert
  -- getEvent is called by the event loop of this AbstractTask instance to 
  -- handleEvent shall be overloaded by the derived classes to implement
  -- sendMessage is used to send a message to another task
  -- raiseInterrupt is used to send a high-priority message
sync
end AbstractTask

~~~{% endraw %}

###AbstractTaskEvent.vdmrt

{% raw %}
~~~

class AbstractTaskEvent
instance variables
operations
  public getFields: () ==> AbstractTask * Event
end AbstractTaskEvent

~~~{% endraw %}

###BasicTask.vdmrt

{% raw %}
~~~

class BasicTask is subclass of AbstractTask
operations
protected handleEvent: Event ==> ()
-- BasicTask just implements the standard event handling loop
end BasicTask

~~~{% endraw %}

###EnvironmentTask.vdmrt

{% raw %}
~~~

class EnvironmentTask is subclass of AbstractTask
instance variables
  -- we limit the number of inserted stimuli
  -- administration for the event traces
functions
operations
  protected handleEvent: Event ==> ()
  public getNum: () ==> nat
  -- setEvent is overloaded. Incoming messages are immediately handled
  -- Run shall be overloaded to implement the event generation loop
  -- logEnvToSys is used to register when an event was inserted into
  -- logSysToEnv is used to register when an event was received from
  -- getMinMaxAverage calculates the minimum, maximum and average
sync
end EnvironmentTask

~~~{% endraw %}

###Event.vdmrt

{% raw %}
~~~

class Event
instance variables
operations
  public getEvent: () ==> nat
end Event

~~~{% endraw %}

###EventDispatcher.vdmrt

{% raw %}
~~~

class EventDispatcher is subclass of Logger
instance variables
operations
  -- setEvent is used to maintain temporary queues for the event loop of
  -- getEvent is used to retrieve events from the temporary event 
  -- SendNetwork is typically called by a system or an environment
  -- SendInterrupt is typically called by a system or an environment
-- the event handler of EventDispatcher. note that we can simulate the overhead
sync
end EventDispatcher

~~~{% endraw %}

###InsertAddress.vdmrt

{% raw %}
~~~

class InsertAddress is subclass of EnvironmentTask
operations
  -- handleEvent receives the responses from the system
  -- createSignal generates the stimuli for the system
  -- start the thread of the task
thread
end InsertAddress

~~~{% endraw %}

###InterruptEvent.vdmrt

{% raw %}
~~~

class InterruptEvent is subclass of Event
operations
end InterruptEvent

~~~{% endraw %}

###Logger.vdmrt

{% raw %}
~~~

class Logger
instance variables
operations
  -- printInterruptEvent writes a time trace to the file mytrace.txt
end Logger

~~~{% endraw %}

###MMIHandleKeyPressOne.vdmrt

{% raw %}
~~~

class MMIHandleKeyPressOne is subclass of BasicTask
operations
  -- we do not specify *what* the operation does
  protected handleEvent: Event ==> ()
end MMIHandleKeyPressOne

~~~{% endraw %}

###MMIHandleKeyPressTwo.vdmrt

{% raw %}
~~~

class MMIHandleKeyPressTwo is subclass of BasicTask
operations
  -- we do not specify *what* the operation does
  protected handleEvent: Event ==> ()
end MMIHandleKeyPressTwo

~~~{% endraw %}

###MMIUpdateScreenAddress.vdmrt

{% raw %}
~~~

class MMIUpdateScreenAddress is subclass of BasicTask
operations
  public UpdateScreen: () ==> ()
  -- we do not specify *what* the operation does
end MMIUpdateScreenAddress

~~~{% endraw %}

###MMIUpdateScreenTMC.vdmrt

{% raw %}
~~~

class MMIUpdateScreenTMC is subclass of BasicTask
operations
  -- we do not specify *what* the operation does
  protected handleEvent: Event ==> ()
end MMIUpdateScreenTMC

~~~{% endraw %}

###MMIUpdateScreenVolume.vdmrt

{% raw %}
~~~

class MMIUpdateScreenVolume is subclass of BasicTask
operations
  -- we do not specify *what* the operation does
  protected handleEvent: Event ==> ()
end MMIUpdateScreenVolume

~~~{% endraw %}

###NavigationDatabaseLookup.vdmrt

{% raw %}
~~~

class NavigationDatabaseLookup is subclass of BasicTask
operations
  -- we do not specify *what* the operation does
  protected handleEvent: Event ==> ()
end NavigationDatabaseLookup

~~~{% endraw %}

###NavigationDecodeTMC.vdmrt

{% raw %}
~~~

class NavigationDecodeTMC is subclass of BasicTask
operations
  -- we do not specify *what* the operation does
  protected handleEvent: Event ==> ()
end NavigationDecodeTMC

~~~{% endraw %}

###NetworkEvent.vdmrt

{% raw %}
~~~

class NetworkEvent is subclass of Event
operations
end NetworkEvent

~~~{% endraw %}

###RadioAdjustVolume.vdmrt

{% raw %}
~~~

class RadioAdjustVolume is subclass of BasicTask
operations
  -- we do not specify *what* the operation does
  protected handleEvent: Event ==> ()
end RadioAdjustVolume

~~~{% endraw %}

###RadioHandleTMC.vdmrt

{% raw %}
~~~

class RadioHandleTMC is subclass of BasicTask
operations
  -- we do not specify *what* the operation does
  protected handleEvent: Event ==> ()
end RadioHandleTMC

~~~{% endraw %}

###RadNavSys.vdmrt

{% raw %}
~~~

class RadNavSys
types
instance variables
operations
  public RadNavSys: () ==> RadNavSys
  -- the constructor initialises the system and starts the application
  -- the addApplicationTask helper operation instantiates the callback
  -- the addEnvironmentTask helper operation instantiates the callback
  -- the Run operation creates and starts the appropriate environment tasks
end RadNavSys

~~~{% endraw %}

###TransmitTMC.vdmrt

{% raw %}
~~~

class TransmitTMC is subclass of EnvironmentTask
operations
  -- handleEvent receives the responses from the system
  -- createSignal generates the stimuli for the system
  -- start the thread of the task
thread
end TransmitTMC

~~~{% endraw %}

###VolumeKnob.vdmrt

{% raw %}
~~~

class VolumeKnob is subclass of EnvironmentTask
operations
  -- handleEvent receives the responses from the system
  -- createSignal generates the stimuli for the system
  -- start the thread of the task
thread
end VolumeKnob

~~~{% endraw %}
