---
layout: listing
title: metro
---

~~~
This model presents three different abstract specifications of a metro
door management system in VDM-SL. The purpose of the presentation is
to describe alternatives to the Metro specification developed in the
European research project called SPECTRUM. 

#******************************************************
#  AUTOMATED TEST SETTINGS
#------------------------------------------------------
#AUTHOR= Sten Agerholm
#LANGUAGE_VERSION=classic
#INV_CHECKS=true
#POST_CHECKS=true
#PRE_CHECKS=true
#DYNAMIC_TYPE_CHECKS=true
#SUPPRESS_WARNINGS=false
#DOCUMENT=metro.tex
#ENTRY_POINT=
#EXPECTED_RESULT=NO_ERROR_TYPE_CHECK
#******************************************************
~~~
###metro.tex

~~~
\documentclass{article}
\usepackage{overture}%vdmsl-2e}

\title{Abstract Metro Specifications in VDM-SL}
\author{Sten Agerholm\\{\small IFAD, Forskerparken 10, DK-5230 Odense M,
  Denmark}\\{\small Email: sten@ifad.dk}}
\begin{document}
\maketitle

\section{Introduction}
Below I'll provide three different abstract specifications of a metro
door management system in VDM-SL. The purpose of the presentation is
to describe alternatives to the Metro specification developed in the
SPECTRUM project (Esprit project no.\ 23173) and presented in the
Proceedings of FMSP'98, published by ACM SIGSOFT, March
1998\footnote{Also available at {\small
    ftp://ftp.ifad.dk/pub/papers/final.ps.gz}.}. The present document
is a draft and the specifications have not tested, so there may be
bugs!

Basically, the metro door management system can be described as
follows. There are three actors: the train driver, the train itself,
and the doors. The train driver has four button that he can use to
control the train and the doors: accelerate, break, open and
close. Our main concern is the safety requirement that the train
should not be able to move while the doors are open.

\section{A First Straightforward Specification}
The specification below models actions (or buttons) as VDM-SL
operations. The state of the doors and the train is held in a VDM-SL state.

\include{generated/latex/specification/metro.vdmsl}

\end{document}



~~~
###metro.vdmsl

~~~
\begin{vdm_al}
module Metro1

exports all

definitions 

state Metro of
  doors: <Open> | <Closed>
  train: <Moving> | <Stopped>
inv mk_Metro(doors,train) == not (doors = <Open> and train = <Moving>)
init metro == metro = mk_Metro(<Closed>,<Stopped>)
end

operations
  Accelerate: () ==> ()
  Accelerate() == 
    (train:= <Moving>)
  pre doors = <Closed>;

  Break: () ==> ()
  Break() == 
    (train:= <Stopped>);
  
  Open: () ==> ()
  Open() == 
    (doors:= <Open>)
  pre train = <Stopped>;
  
  Close: () ==> ()
  Close() == 
    (doors:= <Closed>)

end Metro1
\end{vdm_al}

The following variant of Metro1 has booleans instead of enumeration types and 
implicit instead of explicit functions (note that these are not executable).

\begin{vdm_al}
module Metro1a

exports all

definitions 

state Metro of
  doorsopen: bool
  trainmoving: bool
inv mk_Metro(doorsopen,trainmoving) == not (doorsopen and trainmoving)
init metro == metro = mk_Metro(false,false)
end

operations
  Accelerate() 
  ext wr trainmoving
      rd doorsopen
  pre not doorsopen
  post trainmoving;

  Break()
  ext wr trainmoving
  post not trainmoving;

  Open()
  ext wr doorsopen
      rd trainmoving
  pre not trainmoving
  post doorsopen;

  Close()
  ext wr doorsopen
  post not doorsopen

end Metro1a
\end{vdm_al}

\section{Adding the Bell Warning and Time} 

There is a requirement that a bell must ring three seconds before
doors closes. The close button must remain depressed for this period.
This kind of requirement is a bit problematic to handle, but I believe
that the suggestion below works fairly well. Time is handled almost
entirely by the environment, so it is not part of the state. However,
the time when the bell goes on, i.e.\ starts ringing, is recorded in
the state.  The operation Close is split into two operations, one for
depressing the close button and one for releasing it. The operations
CloseDepress and CloseRelease have current time as a parameter.

\begin{vdm_al} 
module Metro2

exports all

definitions 

state Metro of
  doors: <Open> | <Closed>
  train: <Moving> | <Stopped>
  bellon: [Time]  -- The bell is not ringing if bellon is nil
inv mk_Metro(doors,train,bellon) == 
  not (doors = <Open> and train = <Moving>) 
init metro == metro = mk_Metro(<Closed>,<Stopped>,nil)
end

types

  Time = nat;

operations
  Accelerate: () ==> ()
  Accelerate() == 
    (train:= <Moving>)
  pre doors = <Open>;

  Break: () ==> ()
  Break() == 
    (train:= <Stopped>);
  
  Open: () ==> ()
  Open() == 
    (doors:= <Open>)
  pre train = <Stopped>;
  
  CloseDepressed: Time ==> ()
  CloseDepressed(t) == 
    (bellon:= t)
  pre bellon = nil;

  CloseReleased: Time ==> ()
  CloseReleased(t) == 
    (if t+3 >= bellon 
     then doors:= <Closed> 
     else skip;
     bellon:= nil)
  pre bellon <> nil

end Metro2
\end{vdm_al}
Note that my interpretation here is that the bell rings as long as the
close button is depressed and that the doors close when it is
released. Hence, I have abstracted away from the closing assistance,
but this is included in the next version.

\section{Formalizing the Requirements} 

Finally I have taken a radically different approach. The idea is to
model a system scenario, or a system ``life-time''. Here
I view the system as five sequences of time intervals, denoting when
the close button is depressed, the bell is ringing, the doors are
open, the train is moving, and the closing assistance is
activated. The correct behaviour and safety requirements of the system
are defined as functions on these sequences included in the invariant
of the System datatype.  Note that I have abstracted away from the
accelerate and break buttons.  Perhaps more surprisingly we don't need
the open button.

However the model includes the doors closing assistance, which must be
activated when the bell has been ringing for 3 seconds.

\begin{vdm_al}
module Metro3

exports all

definitions

types
  Time = real
  inv t == t>0;

  Interval:: start: Time
             stop: Time
  inv mk_Interval(s,e) == s < e;

  LifeTime = seq of Interval
  inv s == 
    forall i in set {1,...,len s - 1} & s(i).stop < s(i+1).start;

  System::
    train   : LifeTime  -- intervals for moving
    doors   : LifeTime  -- intervals for open
    bell    : LifeTime  -- intervals for ringing
    closebut: LifeTime  -- intervals for depressed
    closeassist: LifeTime -- intervals for activated
  inv mk_System(train,doors,bell,closebut,closeassist) ==
    NotMovingAndOpen(train,doors) and
    BellOnWhenCloseBut(bell,closebut) and
    CloseAssistAfter3Secs(closeassist,bell);
    
functions
  NotMovingAndOpen: LifeTime * LifeTime -> bool
  NotMovingAndOpen(train,doors) == 
    forall i in set inds train, j in set inds doors &
      not OverlappingIntervals(train(i),doors(j));

  CloseAssistAfter3Secs: LifeTime * LifeTime -> bool
  CloseAssistAfter3Secs(closeassist,bell) == 
    forall i in set inds closeassist &
      exists j in set inds bell &
        bell(j).stop >= bell(j).start+3 and
        closeassist(i).start = bell(j).start+3;

  BellOnWhenCloseBut: LifeTime * LifeTime -> bool
  BellOnWhenCloseBut(bell,closebut) == 
    forall i in set inds bell &
      exists j in set inds closebut &
        SubInterval(bell(i),closebut(j)) 
  -- Too loose, correct?

functions -- Auxiliary functions

  OverlappingIntervals: Interval * Interval -> bool
  OverlappingIntervals(int1,int2) == 
    undefined; -- not specified yet.

  SubInterval: Interval * Interval -> bool
  SubInterval(int1,int2) == 
    undefined; -- not specified yet.

end Metro3
\end{vdm_al}
I have left the last two functions as an exercise! You could probably
find more requirements to model here.

A lot of questions arised during the development of this last
specification, for example, when does the bell go on and off in
relation to the close assistance and the close button, does the bell
sound for longer than 3 secs while doors are closing?




~~~