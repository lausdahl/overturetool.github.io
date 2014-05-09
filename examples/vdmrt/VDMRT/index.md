---
layout: default
title: VDM
---

~~~

#******************************************************
~~~
###Config.vdmrt

{% raw %}
~~~

-----------------------------------------------
--
--
--
--
--



~~~{% endraw %}

###Controller.vdmrt

{% raw %}
~~~

-----------------------------------------------
--
--
--keep track of whom we have communicated with.
private traffic : Traffic;
--
public Controller : Vehicle ==> Controller

async public AddOncomingVehicle: VehicleData ==> ()

async public AddTrafficData:  nat * seq of TrafficData ==> ()
 --did we already exchange information?
 -- add traffic 
 for d in data do 

private AddInternalTrafficData: TrafficData ==> ()
);

public GetTrafficData: () ==> seq of TrafficData 

public GetVehicleID : () ==> nat

public GetPosition : () ==> Position

public GetDirection: () ==> Types`Direction 

public getVehicle : () ==> Vehicle

public getVehicleDTO : () ==> VehicleData

public Step: () ==> ()
 --check expired internal data
 --check for lowgrip, and check if already set at position.  
 --check for turnindicator, and check if already set at position.  
  --check for congestion, and check if already set at position.  
);

async public run : () ==> ()
--
--

--
--
end Controller



~~~{% endraw %}

###Environment.vdmrt

{% raw %}
~~~

-----------------------------------------------
--
--
private vdmCtrl : VDMController;
--
public Environment: seq of char ==> Environment
	def mk_(-,input) = io.freadval[InputTP](filename) in

public Events: () ==> ()
    while not done do
			      	let light = vdmCtrl.getTrafficLight(event.ID) in 
			     	eventOccurred := true;
					 let c = vdmCtrl.getController(event.ID) in 
					 eventOccurred := true;
			     	eventOccurred := true;
			    eventOccurred := true;
			     eventOccurred := true;
			     eventOccurred := true;
		if eventOccurred 
		eventOccurred := false;
   public handleEvent : seq of char ==> ()

   public report : () ==> ()
   public isFinished : () ==> () 
   public setVDMCtrl : VDMController ==> ()
   public run : () ==> ()
--

--
 Printer`OutAlways("No more events;");
end Environment


~~~{% endraw %}

###Position.vdmrt

{% raw %}
~~~

-----------------------------------------------
--
--
private x: int;
--
--
public Position: int * int ==> Position
public X: () ==> int
public Y: () ==> int
public setX : int ==> ()
);
public setY: int ==> ()
);
public toString : () ==> seq of char
public inRange : Position * int ==> bool
public deepCopy : () ==> Position
--

--
end Position


~~~{% endraw %}

###Printer.vdmrt

{% raw %}
~~~

-----------------------------------------------
--
instance variables

--
  public static Echo : bool ==> ()
  public static OutAlways: seq of char ==> ()

  public static OutWithTS: seq of char ==> ()
  public static natToString : nat ==> seq of char 
  public static intToString : int ==> seq of char 
end Printer


~~~{% endraw %}

###Test.vdmrt

{% raw %}
~~~
class Test
operations
end Test

~~~{% endraw %}

###TestCase.vdmrt

{% raw %}
~~~


class TestCase
instance variables
operations
  public GetName: () ==> seq of char
  protected AssertTrue: bool ==> ()
  protected AssertFalse: bool ==> ()
  public Run: TestResult ==> ()
  protected SetUp: () ==> ()
  protected RunTest: () ==> ()
  protected TearDown: () ==> ()
end TestCase


~~~{% endraw %}

###TestController.vdmrt

{% raw %}
~~~

------------------------------------------------
--
--
protected SetUp: () ==> ()
protected RunTest: () ==> ()
  AssertTrue(ctrl.getVehicle() = vec);
  --test get traffic data
   )
  vec.SetSpeed(0);
  ctrl.AddOncomingVehicle(ctrl2.getVehicleDTO());
  --  --test add of traffic data. Test that adding loops when more than five
  --test that the same vehicle can't communicate until pass threshold. 
  -- actually this can't be automatically tested. 
);
protected TearDown: () ==> ()
end TestController


~~~{% endraw %}

###TestPosition.vdmrt

{% raw %}
~~~

-----------------------------------------------
--
--
--
protected SetUp: () ==> ()
protected RunTest: () ==> ()
 AssertTrue(pos.X() = 2);
 pos.setX(10);
 pos.setY(4);
 AssertTrue(pos.toString() = "position X: 10 Y: 4");
 testInRange();
protected TearDown: () ==> ()

public testInRange : () ==> ()
  AssertTrue(p.inRange(p2 , 1));

public testDeepCopy : () ==> ()
 AssertFalse(p = p2);

public testCompare : () ==> ()
 AssertTrue(Position`Compare(p,p2));


end TestPosition


~~~{% endraw %}

###TestResult.vdmrt

{% raw %}
~~~

class TestResult
instance variables
operations
  public Print: seq of char ==> ()
  public Show: () ==> ()
end TestResult


~~~{% endraw %}

###TestSuite.vdmrt

{% raw %}
~~~

class TestSuite
instance variables
types
public
operations
  public Run: TestResult ==> ()
  public AddTest: TestKinds ==> ()
end TestSuite


~~~{% endraw %}

###TestTraffic.vdmrt

{% raw %}
~~~

------------------------------------------------
--
--
protected SetUp: () ==> ()
protected RunTest: () ==> ()
  AssertFalse(traf.ExistVehicle(vec));
  let vs = traf.GetVehicles() in
   traf.AddVehicle(vec3);
   let vs = traf.GetVehicles() in
   traf.AddVehicle(vec6);
   testCongestion();
protected TearDown: () ==> ()
--public Step: () ==> ()
public testCongestion : () ==> ()
 dcl traf : Traffic := new Traffic(),
 dcl traf : Traffic := new Traffic();
 let vs = [vec,vec2,vec3,vec4] in
--start vehicle
--sequential model only
  AssertTrue(traf.Congestion());
)


~~~{% endraw %}

###TestTrafficData.vdmrt

{% raw %}
~~~

-----------------------------------------------
--
--
protected SetUp: () ==> ()
protected RunTest: () ==> ()
 AssertTrue(td.GetPosition().X() = 5);
 AssertTrue(td2.GetPosition().X() = 5);
 AssertTrue(td3.GetPosition().X() = 5);
 testExpired();
protected TearDown: () ==> ()
public testExpired : () ==> ()
end TestTrafficData


~~~{% endraw %}

###TestTrafficLight.vdmrt

{% raw %}
~~~

-----------------------------------------------
--
--
protected SetUp: () ==> ()
protected RunTest: () ==> ()
 testGreenLightPath();
protected TearDown: () ==> ()

--sequential model only
public testGreenLightPath : () ==> ()

public testCrossDirection : () ==> ()
);

end TestTrafficLight


~~~{% endraw %}

###TestVDMComplete.vdmrt

{% raw %}
~~~

-----------------------------------------------
--
class TestVDMComplete

instance variables

--
public Execute: () ==> ()
end TestVDMComplete


~~~{% endraw %}

###TestVDMController.vdmrt

{% raw %}
~~~

------------------------------------------------
--
--
protected SetUp: () ==> ()
protected RunTest: () ==> ()

private runner : () ==> ()
 --test call of inrange and data exchange
 start(vdmCtrl);
 vdmCtrl.Step();
  --test opposite direction
  -- test trafficlight

private IsFinished : () ==> ()

protected TearDown: () ==> ()

thread
--
end TestVDMController


~~~{% endraw %}

###TestVehicle.vdmrt

{% raw %}
~~~

-----------------------------------------------
--
--
private dir: Types`Direction;

--
public TestVehicle: seq of char ==> TestVehicle

protected SetUp: () ==> ()
protected RunTest: () ==> ()
 AssertTrue(vec <> vec2);
protected TearDown: () ==> ()

protected initData : () ==> Vehicle

protected testGetDirection: () ==> ()
protected testSetDirection: ()  ==> ()
protected testGetSpeed: () ==> () 
protected testSetSpeed: () ==> () 
protected testgetLowGrip: () ==> () 
protected testsetLowGrip: () ==> () 
protected testTurnIndicator: () ==> () 
protected testsetTurnIndicator: () ==> () 
protected testGetPosition: () ==> () 
protected testSetPosition: () ==> () 

protected testStep: () ==> ()
 v.Move();
 v.Move();
 v.SetDirection(<NORTH>);
 v.SetDirection(<WEST>);

 v.SetDirection(<SOUTH>);

);
-- sequential model only
end TestVehicle


~~~{% endraw %}

###Traffic.vdmrt

{% raw %}
~~~

-----------------------------------------------
--
--
private vehicles: seq of Vehicle := [];
--
public AddVehicle: Vehicle ==> () 

public ExistVehicle : Vehicle ==> bool

public ExistVehicleData : VehicleData ==> bool

public GetVehicles: () ==> seq of Vehicle 

public Congestion: () ==> bool
 for v in vehicles do
 if card inrange = 0
 let avgspeed = AverageSpeed(inrange) 

private AverageSpeed: set of Vehicle ==> nat
 -- compare the range of two vehicles

-- compare the range of a single vehicle to a set of vehicles moving in 

--
--
end Traffic



~~~{% endraw %}

###TrafficData.vdmrt

{% raw %}
~~~

-----------------------------------------------
--
--
--
--
public GetPosition: () ==> Position 
public GetMessage: () ==> MessageType
public GetDirection: () ==> Types`Direction 
public Expired : () ==> bool
public ToString : () ==> seq of char 
--
public static MessageTypeToString : MessageType -> seq of char 
--
end TrafficData



~~~{% endraw %}

###TrafficLight.vdmrt

{% raw %}
~~~

-----------------------------------------------
--
--
private pos: Position;
--
public TrafficLight: nat * Position * nat1 ==> TrafficLight
	greenDir := <NORTH>
public AddTrafficData: TrafficData ==> ()
public GetTrafficData: () ==> set of TrafficData 
public GetPosition: () ==> Position 
public GreenLightPath: () ==> Types`Direction 
public GetID: () ==> nat
private Step: () ==> ()
--
public static CrossDirection : Types`Direction -> Types`Direction
--
--

--
end TrafficLight



~~~{% endraw %}

###Types.vdmrt

{% raw %}
~~~

-----------------------------------------------
--
types   
public Event = VechicleRun | TrafficLightRun | VehicleUpdateSpeed 
public VechicleRun ::
public TrafficLightRun ::
public VehicleUpdateSpeed ::
public VehicleUpdatePosition ::
public VehicleUpdateDirection ::
public VehicleLowGrip ::
public VehicleTurnIndication ::
functions

end Types



~~~{% endraw %}

###VDM.vdmrt

{% raw %}
~~~


-----------------------------------------------
--
--
cpu0 : CPU := new CPU (<FP>,1E6);		-- changed for setPriority to work
bus1 : BUS := new BUS (<FCFS>,1E6,{cpu0,cpu1, cpu2, cpu3, cpu4, cpu5, cpu6,
-- Vehicles
public static ctrl2 : Controller := new Controller(
public static ctrl3 : Controller := new Controller(
public static ctrl4 : Controller := new Controller(
public static ctrl5 : Controller := new Controller(
public static ctrl6 : Controller := new Controller(
public static ctrl7 : Controller := new Controller(
public static ctrl8 : Controller := new Controller(
public static ctrl9 : Controller := new Controller(
public static ctrl10 : Controller := new Controller(
public static ctrl11 : Controller := new Controller(
public static ctrl12 : Controller := new Controller(
public static ctrl13 : Controller := new Controller(
public static ctrl14 : Controller := new Controller(



--traffic lights
-- environment 
--
public VDM: () ==> VDM

 cpu0.deploy(vdmCtrl);
 );
end VDM



~~~{% endraw %}

###VDMController.vdmrt

{% raw %}
~~~

-----------------------------------------------
--
--
--
--
public addController: Controller ==> ()
public addTrafficLight: TrafficLight ==> ()
public getController : nat ==> Controller
public getTrafficLight : nat ==> TrafficLight
public Step: () ==> ()
public CalculateInRange: () ==> ()
         let vehicleDTO = unit.getVehicleDTO() in 
--

-- compare the range of a single vehicle/controller to a 

  -- compare the range of two vehicles/controllers

-- compare the range of a single vehicle/controller to a set of 

--

--
--



~~~{% endraw %}

###VDMUtil.vdmrt

{% raw %}
~~~
class VDMUtil
-- 	VDMTools STANDARD LIBRARY: MiscUtils
functions
-- Returns a context information tuple which represents
-- Converts a VDM value into a seq of char.
-- converts VDM value in ASCII format into a VDM value
end VDMUtil
~~~{% endraw %}

###Vehicle.vdmrt

{% raw %}
~~~

-----------------------------------------------
--
--
private dir: Types`Direction;
public Vehicle:  nat * Position * nat * Types`Direction ==> Vehicle

public Vehicle:  VehicleData ==> Vehicle

public GetDirection: () ==> Types`Direction 
async public SetDirection: Types`Direction  ==> ()
public GetSpeed: () ==> nat 
async public SetSpeed: nat ==> () 
public getLowGrip: () ==> bool 
async public setLowGrip: bool ==> () 
public TurnIndicator: () ==> Indicator 
async public setTurnIndicator: Indicator ==> () 
public GetPosition: () ==> Position 
async public SetPosition: Position ==> () 
public GetID: () ==> nat
public Move : () ==> ()
);
public getDTO : () ==> VehicleData
--
public static IndicatorToString : Indicator -> seq of char 


--
--
end Vehicle



~~~{% endraw %}

###VehicleData.vdmrt

{% raw %}
~~~

-----------------------------------------------
--
--
private dir: Types`Direction;
public VehicleData : nat * Position * nat * Types`Direction * bool 
public GetDirection: () ==> Types`Direction 
public GetSpeed: () ==> nat 
public getLowGrip: () ==> bool 
public TurnIndicator: () ==> Indicator 
public GetPosition: () ==> Position 
public GetID: () ==> nat

--
--
end VehicleData



~~~{% endraw %}

###World.vdmrt

{% raw %}
~~~

-----------------------------------------------
--
--
public static env : [Environment] := nil;
--
--
public World: () ==> World
 --vehicle

 VDM`vdmCtrl.addTrafficLight(VDM`tl1);
 Printer`OutAlways("World created: "  
public Run: () ==> ()
public static Verbose : bool ==> ()
--
--
end World



~~~{% endraw %}
