---
layout: default
title: SAFER
---

~~~
This specification is a VDM++ model of the SAFER 
Here Appendix C contains a complete listing of the 
More explanation about this work can be found in the papers: 
#******************************************************
~~~
###AAH.vdmpp

{% raw %}
~~~

class AAH
types
values
instance variables
operations
  public
  public
  public
  public
  public
  public
  public
  public
  ButtonTransition : HandControlUnit`Button * nat ==> EngageState
end AAH

~~~{% endraw %}

###Clock.vdmpp

{% raw %}
~~~

class Clock
instance variables
operations
  public
  public
end Clock

~~~{% endraw %}

###Command.vdmpp

{% raw %}
~~~

class Command
  public
  public
values
instance variables
operations
  public
  public
  public
  public

end Command

~~~{% endraw %}

###HandControlUnit.vdmpp

{% raw %}
~~~

class HandControlUnit
types
instance variables

operations
  public
  public
  public
  public

end HandControlUnit

~~~{% endraw %}

###IntegratedCommand.vdmpp

{% raw %}
~~~

class IntegratedCommand is subclass of SixDOfCommand
instance variables
operations
  public
  CombineRotCmds : () ==> ()

end IntegratedCommand

~~~{% endraw %}

###Interface.vdmpp

{% raw %}
~~~

class Interface
instance variables
types 
public
public
operations
public
public
functions
TransformInput: Input -> Command`Direction * Command`Direction *
ConvertAxisCmd: nat -> Command`Direction
GenerateThrusterMatrix: set of ThrusterControl`ThrusterPosition +> 
GenerateThrusterLabel: ThrusterControl`ThrusterPosition +> nat * nat
values
thrusters = mk_(<Pos>,<Zero>,<Zero>,<Zero>,<Tran>,<Down>,
end Interface

~~~{% endraw %}

###RotationCommand.vdmpp

{% raw %}
~~~

class RotationCommand is subclass of Command
end RotationCommand

~~~{% endraw %}

###SixDOfCommand.vdmpp

{% raw %}
~~~

class SixDOfCommand
operations
  public
  public
-- Alternative formulation of ConvertGrip

end SixDOfCommand

~~~{% endraw %}

###Test.vdmpp

{% raw %}
~~~

class Test is subclass of WorkSpace
values
instance variables
  w : WorkSpace := new WorkSpace();
operations
  public HugeTest: () ==> nat
traces
BT : w.SetupTopology(); let x, pitch, yaw_y, roll_z in set DirectionSet in 
HT: w.SetupTopology();
end Test

~~~{% endraw %}

###Thruster.vdmpp

{% raw %}
~~~

class Thruster
types
instance variables

operations
  public
  public
end Thruster

~~~{% endraw %}

###ThrusterControl.vdmpp

{% raw %}
~~~

class ThrusterControl
values  -- two thruster selection tables...
  bf_thrusters1 : ThrusterSelectionTable`ThrSelMap = 
  lrud_thrusters : ThrusterSelectionTable`ThrSelMap = {|->};
  lrud_thrusters1 : ThrusterSelectionTable`ThrSelMap =
  public ThrusterSet : set of ThrusterPosition = 
types
  public
instance variables
operations
  public
  PrintSelected : () ==> set of ThrusterPosition
  public
  public
  public
  public
  InitializeTables : () ==> ()
end ThrusterControl

~~~{% endraw %}

###ThrusterSelectionTable.vdmpp

{% raw %}
~~~

class ThrusterSelectionTable 
types
instance variables

operations
  public
end ThrusterSelectionTable

~~~{% endraw %}

###TranslationCommand.vdmpp

{% raw %}
~~~

class TranslationCommand is subclass of Command
end TranslationCommand

~~~{% endraw %}

###ValveDriveAssembly.vdmpp

{% raw %}
~~~

class ValveDriveAssembly
instance variables
operations
  public
end ValveDriveAssembly

~~~{% endraw %}

###WorkSpace.vdmpp

{% raw %}
~~~

class WorkSpace 
values
instance variables
operations
  public
  ThrusterConsistency : set of ThrusterControl`ThrusterPosition ==> bool 
end WorkSpace

~~~{% endraw %}
