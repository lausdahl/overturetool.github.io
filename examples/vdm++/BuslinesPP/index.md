---
layout: default
title: Buslines
---

~~~
This example models bus lines in a city, in which passengers are to be 
 Remote Debugger must be set to remote class:
#******************************************************
~~~
###Bus.vdmpp

{% raw %}
~~~
class Bus
	instance variables
		--bus line number
		-- the route of road the bus is moving along
	operations
			wps := waypoints;
		public GetOn : set of Passenger ==> ()
		public GotOff : set of Passenger ==> ()
		public GetWaypoints : () ==> seq of Waypoint
		public GetStops : () ==> seq of Busstop

		private NextWaypoint : () ==> Waypoint
			--next road
		--functionality to ensure thread start
		private ThreadStarted : () ==> ()
	sync
	thread
		--loop which moves bus along route, and lets passengers on and off at stops
			World`graphics.busInRouteTo(line,
			--add to penalty for moving along road
			--next on bus route
				let nextId = next.GetId() in
						let central : Busstop = next in
									-- leave busstop and enter bus seats
									Printer`OutWithTS("%Bus " ^ Printer`natToString(line) ^ ": " ^ 
									--add to time penalty for stopping
					-- let passengers off at their destination
								World`env.TransportedPassengers(card gettingOff);
								--add to time penalty for stopping
		);
	operations
			--smaller than limit, return
			--recusive
end Bus
~~~{% endraw %}

###Busstop.vdmpp

{% raw %}
~~~
class Busstop is subclass of Waypoint
	instance variables
	operations
		--number of passenger waiting
		--get passengers waiting
		-- get passengers waiting on a bus which passes specific stops 
		--passenger arrived at the busstop
		--passenger got on a bus
sync
end Busstop
~~~{% endraw %}

###City.vdmpp

{% raw %}
~~~
class City
	instance variables
		stops : inmap Waypoint`BusStops to Busstop := {|->};	
	operations

		--add a new busStop to the city

		--add waypoint to the city, pasengers can not get off at a waypoint

		--add road to the city, a road are end to end and must have to waypoints and a roadnumber 


		--overloaded addRoad, which allows for the speedlimit of the road to be change from
		--add a new bus with a particular road 
					--find busstops on the route, starting from central

				if (hd busstops <> busstops(len(busstops))) then
				--creat bus	

		private findRoadsFromRoadNumber : seq of Road`RoadNumber ==> seq of Road

		public getCentralStation : () ==> Busstop
		--passenger inflow
		public getInflow : () ==> nat
		public getBuses : () ==> set of Bus
		--sync functionality for ensuring thread is started, 
		private ThreadStarted : () ==> ()
	sync
	thread
			World`env.handleEvent(Printer`natToString(central.GetWaitingCount()) ^ " passengers waiting a central station");
			--check annoyance of waiting passengers
			World`timerRef.WaitRelative(5);
end City
~~~{% endraw %}

###ClockTick.vdmpp

{% raw %}
~~~
class ClockTick
thread 
 end ClockTick
~~~{% endraw %}

###Config.vdmpp

{% raw %}
~~~
-----------------------------------------------
--
--
--max passengers on bus
end Config
~~~{% endraw %}

###Environment.vdmpp

{% raw %}
~~~
-----------------------------------------------
--
instance variables
	public city : City;
	private passengersTransported : nat := 0;
types   
operations
	public Environment: seq of char ==> Environment
		def mk_(-,input) = io.freadval[InputTP](filename) in
		BuildCityMap();
	private BuildCityMap : () ==> ()
		a := city.addBusstop(<A>);
		wp1 := city.addWaypoint(<WP1>);
		city.addRoad(a, b, <R1>, 40);

	public Events: () ==> ()
		while not done do
						let b = city.addBus(event.ID, event.route) in  
							Printer`Out("\nStops:");
							let wps = b.GetStops() in 
				    	 eventOccurred := true;
				     	eventOccurred := true;
						); 
			if eventOccurred then
			eventOccurred := false;
	public handleEvent : seq of char ==> ()
	private SetInflow : nat  ==> ()
		city.setInflow(flow);
	public IncreaseInflow : () ==> ()
	public DecreaseInflow : () ==> ()
	public TransportedPassengers : nat ==> ()
	--public AnnoyedPassenger : nat * Waypoint`WaypointsEnum ==> ()
		if(goal not in set dom passengersAnnoyedStops) then 
	public PassengerCount : () ==> ()
	public report : () ==> ()

		for all waypoint in set dom passengersAnnoyedStops do
		);
	public isFinished : () ==> () 
  	public goEnvironment : () ==> () 
	public run : () ==> ()
thread
 	Printer`Out("No more events;");
sync
end Environment
~~~{% endraw %}

###gui_Graphics.vdmpp

{% raw %}
~~~
class gui_Graphics
	    public init : () ==> ()
		public busInRouteTo : nat * seq of char * seq of char * nat ==> ()
		public move : () ==> ()
		public sleep : () ==> ()
		public passengerAtCentral : nat * seq of char  ==> ()
		public passengerAnnoyed : nat==> ()
		public passengerGotOnBus : nat ==> ()
		public inflowChanged : nat ==> ()
		public busAdded : nat ==> ()
		public busStopping : nat ==> ()
		public busPassengerCountChanged : nat * nat ==> ()
end gui_Graphics

~~~{% endraw %}

###Passenger.vdmpp

{% raw %}
~~~
class Passenger
	instance variables
		passengerId : nat;
		annoyanceLimit : nat;
	operations
			World`env.PassengerCount();
		public GetDestination : () ==>  Waypoint
		public GotOnBus : () ==> () 
		public IsAnnoyedOfWaiting : () ==> bool
		-- report to environment when  passenger becomes annoyed.
		public Id : () ==> nat
		private GetNextId : () ==> nat
		);
	sync
end Passenger
~~~{% endraw %}

###Printer.vdmpp

{% raw %}
~~~
class Printer
	operations		

		public static natToString : nat ==> seq of char 

		public static OutWithTS: seq of char ==> ()

		public static intToString : int ==> seq of char 
end Printer
~~~{% endraw %}

###Road.vdmpp

{% raw %}
~~~
class Road
	instance variables
	operations
		public Road : RoadNumber *  set of Waypoint * nat * nat ==> Road
		public Covers : set of Waypoint ==> bool
		public GetWaypoints : () ==> set of Waypoint
		public OppositeEnd : Waypoint ==> Waypoint

		public GetSpeedLimit : () ==> nat
		public GetLength : () ==> nat
		public GetRoadNumber : () ==> RoadNumber
		public GetTimePenalty : () ==> nat
end Road
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


public WaitRelative : nat ==> ()


public WaitAbsolute : nat ==> ()


AddToWakeUpMap : nat * nat ==> ()


public NotifyThread : nat ==> ()


public NotifyAll : () ==> ()


public NotifyAndIncTime : () ==> ()


public GetTime : () ==> nat


public Awake: () ==> ()


--public SyncWithTimeIncrement : () ==> ()
--public YieldTimeIncrement: () ==> ()

sync
  per NotifyAndIncTime => (card {th | th in set dom wakeUpMap & wakeUpMap(th) = currentTime +1} > 0) ;  --The magic one,  only allow run
  mutex(NotifyAll);
end TimeStamp

~~~{% endraw %}

###Types.vdmpp

{% raw %}
~~~
class Types
types   
public Event = BusRoute | Inflow | Simulate | WasteTime;
public BusRoute ::
public Inflow ::
public Simulate ::
public WasteTime ::
functions 
end Types
~~~{% endraw %}

###WaitNotify.vdmpp

{% raw %}
~~~

class WaitNotify
instance variables
waitset : set of nat := {}
operations
public Wait : () ==> ()
public Notify : () ==> ()
public NotifyThread: nat ==> ()
public NotifyAll: () ==> ()
private AddToWaitSet : nat ==> ()
private Awake : () ==> ()
sync
end WaitNotify

~~~{% endraw %}

###Waypoint.vdmpp

{% raw %}
~~~
class Waypoint
	types
	instance variables
	operations
		public GetId : () ==> WaypointsEnum
		public IsStop: () ==> bool

end Waypoint
~~~{% endraw %}

###World.vdmpp

{% raw %}
~~~
-----------------------------------------------
-- Rules of this world
class World
instance variables
public static env : [Environment] := new Environment("inputvalues.txt");
operations
	public World: () ==> World
	public Run: () ==> ()
		env.report();
	  	Printer`Out("End of this world");



end World
~~~{% endraw %}
