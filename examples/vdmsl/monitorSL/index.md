---
layout: listing
title: monitor
---

~~~
This example comes from the VDM-SL book by John Fitzgerald and Peter Gorm
Larsen. It is the running example through the chapter about logic. 
Suppose we are asked to develop the software for a temperature
monitor for a reactor vessel in the plant. The monitor is connected to
a temperature sensor inside the vessel from which it receives a
reading(in degrees Celsius) every minute.

The monitor records the five most recent temperature readings in the
order in which they were received from the sensor.

#******************************************************
#  AUTOMATED TEST SETTINGS
#------------------------------------------------------
#AUTHOR= John Fitzgerald and Peter Gorm Larsen
#LANGUAGE_VERSION=classic
#INV_CHECKS=true
#POST_CHECKS=true
#PRE_CHECKS=true
#DYNAMIC_TYPE_CHECKS=true
#SUPPRESS_WARNINGS=false
#ENTRY_POINT=DEFAULT`OverLimit([4,2,8,555,123])
#EXPECTED_RESULT=NO_ERROR_TYPE_CHECK
#******************************************************
~~~
###monitor.vdmsl

~~~
-- A temperature monitor
-- For Chapter 4 (Logic)

types

  TempRead = seq of int
  inv temp == len temp = 5

functions

-- the last reading in a sample is greater than the first

  Rising: TempRead -> bool
  Rising(temp) ==
   temp(1) < temp(5);

-- there is a reading in excess of 400 degrees

  OverLimit: TempRead -> bool
  OverLimit(temp) ==
    temp(1) > 400 or temp(2) > 400 or 
    temp(3) > 400 or temp(4) > 400 or 
    temp(5) > 400;

-- alternative formulation using a quantified expression

  OverLimit2: TempRead -> bool
  OverLimit2(temp) ==
    exists i in set inds temp & temp(i) > 400;

-- all readings in a sample exceed 400 degrees

  ContOverLimit: TempRead -> bool
  ContOverLimit(temp) ==
    temp(1) > 400 and temp(2) > 400 and 
    temp(3) > 400 and temp(4) > 400 and 
    temp(5) > 400;

-- alternative formulation using a quantified expression

  ContOverLimit2: TempRead -> bool
  ContOverLimit2(temp) ==
    forall i in set inds temp & temp(i) > 400;

-- detecting whether a reactor can be considered safe

  Safe: TempRead -> bool
  Safe(temp) ==
    temp(3) > 400 => temp(5) < 400;

-- detecting whether an alarm should be raised

  RaiseAlarm(temp: TempRead) alarm : bool
  post not Safe(temp) <=> alarm;

  MixQuant: TempRead -> bool
  MixQuant(temp) ==
    exists min in set {1,...,5} &
       forall i in set {1,...,5} &
          i <> min =>
          temp(i) > temp(min)

~~~