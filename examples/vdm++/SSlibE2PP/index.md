---
layout: default
title: SSlibE2
---

~~~
This example contains a large collection of test classes that can be
#******************************************************
~~~
###AllT.vdmpp

{% raw %}
~~~
/*
	in
end AllT

~~~{% endraw %}

###Calendar.vdmpp

{% raw %}
~~~
class CalendarDefinition
end CalendarDefinition
values
	private namesOfDayOfTheWeek = [<Sun>,<Mon>,<Tue>,<Wed>,<Thu>,<Fri>,<Sat>];
	private daysInYear = 365.25;
	io = new IO();
instance variables
	protected differenceWithGMT : real := 0;
functions
----Comparing magnitude functions
public LT: Date * Date -> bool
public GT: Date * Date -> bool
public LE: Date * Date -> bool
public GE: Date * Date -> bool
-- Is date1 value equal date2 value?
public min : Date -> Date -> Date
public max : Date -> Date -> Date
----Query
public isDateString : 
-- is leap year?
public getNumberOfDayOfTheWeek: Date -> NumberOfDayOfTheWeek
public getYyyymmdd: Date -> int * int * int
public getNameOfDayOfTheWeek : Date -> NameOfDayOfTheWeek
public getNumberOfDayOfTheWeekFromName : NameOfDayOfTheWeek -> NumberOfDayOfTheWeek
public firstDayOfTheWeekInMonth : int * int * NameOfDayOfTheWeek -> Date
-- Get the last date which has the specified name of day of the week
-- Get the n-th day of the week of specified month
--new Calendar().getFirstDayOfMonth(2001,7).get_yyyy_mm_dd() = mk_( 2001,7,1 )
--new Calendar().getLastDayOfMonth(2001,7).get_yyyy_mm_dd() = mk_( 2001,7,31 )
public isSunday : Date -> bool
public isSaturday : Date -> bool
public isWeekday : Date -> bool
public isNotDayOff : Date -> bool
public isWeekday : NameOfDayOfTheWeek -> bool
-- Return how many days between date1 and date2 of nameOfDayOfTheWeek.  
pre
post
--æ´æ°ç³»ãç°ï¼ringï¼ã¨è¦ã¦ããã®åç°ï¼quotient ringï¼ã¸ã®æºååååãããããã®ä»£æ°ç³»ä¸ã§äºå¾æ¡ä»¶ãæºãããã­ã°ã©ã ãä½ã
ã¨ããã¨ã
q â¦ A â¦ q+1
ãæãç«ã¤ããªããªãã
ä»»æã®é£ç¶ããï¼æ¥éã«ã¯ãå¿ãwææ¥ãã¡ããã©ï¼æ¥å­å¨ããã
æ¬¡ã«ã
x ++ y = (x + y) \ 7
T = {h(f)..h(f) ++ (r â´ 1)}
A â¡ if wâT then q + 1 els q end
ããã§ã
x minus y = if x â§ y then x - y els x - y + 7 end
wâT â (w minus h(f)) + 1 â¦ r
wâT	â {0..(r â´ 1)}âwï¾ = w minus h(f)
å¾ã£ã¦ããã­ã°ã©ã ã¯ä»¥ä¸ã®ããã«ãªãã
A(f, t w)â¡
private subtractDayOfTheWeek: int * int -> int
--dateããããã®dateã®å±ããyearãæ±ããã
--dateããããã®dateã®å±ããmonthãæ±ããã
--dateãããdayãæ±ããã
--new Date().daysFromNewYear(getDateFrom_yyyy_mm_dd(2001,12,31)) = 365
daysFromTheBeginningOfTheMonth: Date -> int
daysFromTheBeginningOfTheMonthAsReal: Date -> real
monthAux: Date -> int
yyyymmddModifyAux: Date -> real
yearAux: Date -> int
public getVernalEquinoxOnGMT: int -> Date
public getSummerSolsticeOnGMT: int -> Date
public getAutumnalEquinoxOnGMT: int -> Date
public getWinterSolsticeOnGMT: int -> Date

  public getVernalEquinox : int -> Date
 public getSummerSolstice : int -> Date
 public getAutumnalEquinox : int -> Date
 -- Now, I can't get the right Winter Solstice in leap year

----calculation
public dateAdding: Date * int -> Date
public diffOfDates: Date * Date -> int
--dateããnumOfDaysãæ¸ç®ããdateãè¿ã
----Conversion
public mjd2Jd: real -> real
public julianDate2ModifiedJulianDate: real -> real
--yyyymmddãéå¸¸ã®å¤ã®ç¯å²åã«å¤æããã
--å¹´æãéå¸¸ã®å¤ã®ç¯å²åã«å¤æããã

--ï¼æ´æ°ä¸ã¤çµã®ï¼date2Year(2001,7,1) = 2001.5
public date2Str : Date +> seq of char
public convertDateFromString : seq of char +> [Date]
--ä»¥ä¸ã¯ãä¼æ¥ã®èæ®ãããæ©è½ã§ããµãã¯ã©ã¹ã§ä¼æ¥ã®éåãå®ç¾©ããå¿è¦ãããã
/* Query */
--ï¼ã¤ã®dateã®éã®ä¼æ¥ã®æ°ãè¿ããæ¥ææ¥ã§ããä¼æ¥ãå«ãããä¼æ¥ã§ãªãæ¥dayOfWeekã¯å«ã¾ãªãã
--ï¼ã¤ã®dateã®éã®ä¼æ¥ãããã¯æ¥ææ¥ã®æ°ãè¿ãï¼startDateãå«ãï¼
--ï¼ã¤ã®dateã®éã®ä¼æ¥ãããã¯æ¥ææ¥ã®æ°ãè¿ãï¼startDateãå«ã¾ãªãï¼
private getSetOfNotSundayDayOff : Date * Date -> set of Date
--æ¥ææ¥ã§ããä¼æ¥ã®éåãè¿ã
/* Conversion */
--ä¼æ¥ã§ãªãdateãè¿ãï¼æªæ¥ã¸åãã£ã¦æ¢ç´¢ããï¼
--ä¼æ¥ã§ãªãdateãè¿ãï¼éå»ã¸åãã£ã¦æ¢ç´¢ããï¼
getPastWeekdaymeasure : Date +> nat
--ä¸ããããå¹³æ¥ã«ãå¹³æ¥næ¥åãå ç®ãã
public addWeekdayAux : Date * int -> Date
--ä¸ããããå¹³æ¥ã«ãå¹³æ¥næ¥åãæ¸ç®ãã
public subtractWeekdayAux : Date * int -> Date
/* Query */
public isDayOff : Date -> bool 
public isSundayOrDayoff : Date -> bool
public isInDateSet :  Date * set of Date -> bool
operations
public modifiedJulianDate2Date: real ==> Date
public getDateFrom_yyyy_mm_dd: int * int * int  ==> Date
public getDateFromString :
public getDateInStandardTime : Date ==> Date	
public getDayOfTheWeekInYear : int * NameOfDayOfTheWeek ==> set of Date
public getDifferenceWithGMT : () ==> real
public setDifferenceWithGMT : (real) ==> ()
public setTheSetOfDayOffs: int ==> ()
public getSetOfDayOff: int ==> set of Date 
--read todayfrom a file
--stub functions for getting today
--todayã®dateãæå®ããreadFromFileã
public setToday : Date ==> ()
end Calendar
~~~{% endraw %}

###CalendarT.vdmpp

{% raw %}
~~~
class CalendarT is subclass of TestDriver 
class CalendarT01 is subclass of TestCase
class CalendarT02 is subclass of TestCase
class CalendarT03 is subclass of TestCase
class CalendarT04 is subclass of TestCase
class CalendarT05 is subclass of TestCase
class CalendarT06 is subclass of TestCase, CalendarDefinition
class CalendarT07 is subclass of TestCase
class CalendarT09 is subclass of TestCase, CalendarDefinition
end CalendarT09
class CalendarT10 is subclass of TestCase
class CalendarT11 is subclass of TestCase
class CalendarT12 is subclass of TestCase
~~~{% endraw %}

###Character.vdmpp

{% raw %}
~~~
class Character
values
vChapitalCharOrderMap =
vSmallCharOrderMap =
vCharOrderMap = vNumOrderMap munion vChapitalCharOrderMap munion vSmallCharOrderMap;
functions
static public asDigit: char -> int | bool
static public asDictOrder : char -> int
static public isDigit : char -> bool
static public isLetter : char -> bool
static public isLetterOrDigit : char -> bool
static public isCapitalLetter : char -> bool
static public isLowercaseLetter : char -> bool
static public LT: char * char -> bool
static public LT2: char -> char -> bool
static public LE : char * char -> bool
static public LE2 : char -> char -> bool
static public GT : char * char -> bool
static public GT2 : char -> char -> bool
static public GE : char * char -> bool
static public GE2 : char -> char -> bool
end Character
~~~{% endraw %}

###CommonDefinition.vdmpp

{% raw %}
~~~
class CommonDefinition is subclass of Object
/*
functions
public NumericOrder : NumericalValue * NumericalValue -> bool
public DateOrder : Date * Date -> bool
public AmountOfMoneyOrder : AmountOfMoney * AmountOfMoney -> bool
types
end CommonDefinition
~~~{% endraw %}

###Date.vdmpp

{% raw %}
~~~
class Date  is subclass of CalendarDefinition	-- date
Therefore, it is judged that the date is equal by using EQ operation. 
instance variables
private ModifiedJulianDate : real := 0;
/*
functions
----Query
public getNumberOfDayOfTheWeek: () -> Calendar`NumberOfDayOfTheWeek
public getNameOfDayOfTheWeek : () -> Calendar`NameOfDayOfTheWeek
--æå®ãããææ¥ããselfã¨dateã®éã«ä½æ¥ããããè¿ãã 
--selfã¨dateã®éã®ä¼æ¥ãããã¯æ¥ææ¥ã®æ°ãè¿ãï¼startDateãå«ãï¼
--selfã¨dateã®éã®ä¼æ¥ãããã¯æ¥ææ¥ã®æ°ãè¿ãï¼startDateãå«ã¾ãªãï¼
--dateããããã®dateã®å±ããå¹´ãæ±ããã
--dateããããã®dateã®å±ããæãæ±ããã
--dateãããæ¥ãæ±ããã
/* calculation  */
--ä¼æ¥ã§ãªãdateãè¿ãï¼æªæ¥ã¸åãã£ã¦æ¢ç´¢ããï¼
--ä¼æ¥ã§ãªãdateãè¿ãï¼éå»ã¸åãã£ã¦æ¢ç´¢ããï¼
--selfã«ãå¹³æ¥næ¥åãå ç®ãã
--selfã«ãå¹³æ¥næ¥åãæ¸ç®ãã
/* checking */
public isSunday : () -> bool
public isSaturday : () -> bool
public isWeekday : () -> bool
public isNotDayOff : () -> bool
public isDayOff : () -> bool 
public isSundayOrDayoff : () -> bool 
--new Date().getDateFrom_yyyy_mm_dd(2001,12,31).daysFromNewYear() = 365
/* conversion */
public get_yyyy_mm_dd: () -> int * int * int
private toStringAux: int -> seq of char
public date2Str: () -> seq of char
operations
----conversion
public asString: () ==> seq of char
public print: ()   ==> seq of char

----æ¯è¼
/*
public GT: Date ==> bool
public LE: Date ==> bool
public GE: Date ==> bool
--èªèº«ã¨ä¸ããããdateãEQãå¤å®ããã
--èªèº«ã¨ä¸ããããdateãç­ãããªããå¤å®ããã
----calculation
--èªèº«ã«numOfDaysãå ç®ããdateãè¿ã
--èªèº«ããnumOfDaysãæ¸ç®ããdateãè¿ã
--ã¤ã³ã¹ã¿ã³ã¹å¤æ°ã¸ã®ã¢ã¯ã»ã¹æä½
--ModifiedJulianDate
public getModifiedJulianDate: () ==> real
public calendar : () ==> Calendar
--Constructor
end Date
~~~{% endraw %}

###DateT.vdmpp

{% raw %}
~~~
class DateT is subclass of TestDriver 
class DateT01 is subclass of TestCase, CalendarDefinition
class DateT02 is subclass of TestCase
class DateT03 is subclass of TestCase
class DateT04 is subclass of TestCase
class DateT05 is subclass of TestCase
class DateT06 is subclass of TestCase, CalendarDefinition
class DateT07 is subclass of TestCase, CalendarDefinition
~~~{% endraw %}

###DoubleListQueue.vdmpp

{% raw %}
~~~
/*   
class DoubleListQueue
functions
static public isEmpty[@T] : (seq of @T * seq of @T) -> bool
static public enQueue[@T] : @T * (seq of @T * seq of @T) -> seq of @T * seq of @T
static public deQueue[@T] : (seq of @T * seq of @T) -> [seq of @T * seq of @T]
static public top[@T] : (seq of @T * seq of @T) -> [@T]
static public fromList[@T] : seq of @T * (seq of @T * seq of @T) -> seq of @T * seq of @T
static fromListMeasure[@T] : seq of @T *  (seq of @T * seq of @T) +> nat
static public toList[@T] : (seq of @T * seq of @T) -> seq of @T
static toListMeasure[@T] :  (seq of @T * seq of @T) +> nat
end DoubleListQueue
~~~{% endraw %}

###DoubleListQueueT.vdmpp

{% raw %}
~~~
class DoubleListQueueT is subclass of TestDriver
class DoubleListQueueT01 is subclass of TestCase
;
~~~{% endraw %}

###FHashtable.vdmpp

{% raw %}
~~~

--"$Id"
functions
static public Put[@T1, @T2] : 
static public PutAll[@T1, @T2] : 
static public PutAllAux[@T1, @T2] :
static public Get[@T1, @T2] : (map @T1 to (map @T1 to  @T2)) -> (@T1 -> @T1) -> @T1  -> [@T2]
static public Remove[@T1, @T2] : (map @T1 to (map @T1 to  @T2)) -> (@T1 -> @T1) -> @T1 -> (map @T1 to (map @T1 to  @T2))
static public Clear[@T1, @T2] : () -> (map @T1 to (map @T1 to  @T2))
static public KeySet[@T1, @T2] : (map @T1 to (map @T1 to  @T2)) -> set of  @T1
static public ValueSet[@T1, @T2] : (map @T1 to (map @T1 to  @T2)) -> set of  @T2
static public Size[@T1, @T2] : (map @T1 to (map @T1 to  @T2)) -> nat
static public IsEmpty[@T1, @T2] : (map @T1 to (map @T1 to  @T2)) -> bool
static public Contains[@T1, @T2] : (map @T1 to (map @T1 to  @T2)) -> @T2 -> bool
static public ContainsKey[@T1, @T2] : (map @T1 to (map @T1 to  @T2)) -> @T1 -> bool
end FHashtable

~~~{% endraw %}

###FHashtableT.vdmpp

{% raw %}
~~~

class FHashtableT
functions
static t1 : () -> FTestDriver`TestCase
	mk_FTestDriver`TestCase(
static t2 : () -> FTestDriver`TestCase
	mk_FTestDriver`TestCase(
static t3 : () -> FTestDriver`TestCase
	mk_FTestDriver`TestCase(
static t4 : () -> FTestDriver`TestCase
	mk_FTestDriver`TestCase(
static t5 : () -> FTestDriver`TestCase
	mk_FTestDriver`TestCase(
static t6 : () -> FTestDriver`TestCase
	mk_FTestDriver`TestCase(
end FHashtableT

~~~{% endraw %}

###FMap.vdmpp

{% raw %}
~~~

--"$Id"
functions
static public Get[@T1, @T2] : map @T1 to @T2 -> @T1 -> [@T2]
static public Contains[@T1, @T2] : map @T1 to @T2 -> @T2 -> bool
static public ContainsKey[@T1, @T2] : map @T1 to @T2 -> @T1 -> bool
end FMap

~~~{% endraw %}

###FSequence.vdmpp

{% raw %}
~~~

class FSequence
functions 

static public Sum[@T] : seq of @T ->  @T
static public Prod[@T] : seq of @T ->  @T
static public Plus[@T] : @T -> @T -> @T
static public Product[@T] : @T -> @T -> @T
static public Append[@T] : seq of @T -> @T -> seq of @T
static public Average[@T]: seq of @T ->  [real]
static AverageAux[@T] : @T -> @T -> seq of @T -> real
static public IsAscendingInTotalOrder[@T] : (@T * @T -> bool) -> seq of @T -> bool
static public IsDescendingInTotalOrder[@T] : (@T * @T -> bool) -> seq of @T -> bool
static public IsAscending [@T]: seq of @T -> bool
static public IsDescending[@T]: seq of @T -> bool
static public Sort[@T] : (@T * @T -> bool) -> seq of @T -> seq of @T
static public AscendingSort[@T] : seq of @T -> seq of @T
static public DescendingSort[@T] : seq of @T -> seq of @T
static public IsOrdered[@T] : seq of (@T * @T -> bool) -> seq of @T -> seq of @T -> bool
static public Merge[@T] : (@T * @T -> bool) -> seq of @T -> seq of @T -> seq of @T
static public InsertAt[@T]: nat1 -> @T -> seq of @T -> seq of @T
static public RemoveAt[@T]: nat1 -> seq of @T -> seq of @T
static public RemoveDup[@T] :  seq of @T ->  seq of @T
static public RemoveMember[@T] :  @T -> seq of @T -> seq of @T
static public RemoveMembers[@T] :  seq of @T -> seq of @T -> seq of @T
static public UpdateAt[@T]: nat1 -> @T -> seq of @T -> seq of @T
static public Take[@T] : int -> seq of @T -> seq of @T
static public TakeWhile[@T] : (@T -> bool) -> seq of @T ->seq of @T
static public Drop[@T]: int -> seq of @T -> seq of @T
static public DropWhile[@T] : (@T -> bool) -> seq of @T ->seq of @T
static public Span[@T] : (@T -> bool) -> seq of @T -> seq of @T * seq of @T
static public SubSeq[@T]: nat -> nat -> seq1 of @T -> seq of @T
static public Last[@T]: seq of @T -> @T
static public Fmap[@T1,@T2]: (@T1 -> @T2) -> seq of @T1 -> seq of @T2
static public Filter[@T]: (@T -> bool) -> seq of @T -> seq of @T
static public Foldl[@T1, @T2] : (@T1 -> @T2 -> @T1) -> @T1 -> seq of @T2 -> @T1
static public Foldr[@T1, @T2] : 
static public IsMember[@T] : @T -> seq of @T -> bool
static public IsAnyMember[@T]:  seq of @T -> seq of @T -> bool
static public IsDup[@T] : seq of @T -> bool
static public Index[@T]: @T -> seq of @T -> int
static public IndexAux[@T] : @T -> seq of @T -> int -> int
static public IndexAll[@T] : @T -> seq of @T -> set of nat1
static public Flatten[@T] : seq of seq of @T -> seq of @T
static public Compact[@T] : seq of [@T] -> seq of @T
static public Freverse[@T] : seq of @T -> seq of @T
static public Permutations[@T]: seq of @T -> set of seq of @T
static public IsPermutations[@T]: seq of @T -> seq of @T -> bool
static public Unzip[@T1, @T2] : seq of (@T1 * @T2) -> seq of @T1 * seq of @T2
static public Zip[@T1, @T2] : seq of @T1 * seq of @T2 -> seq of (@T1 * @T2)
static public Zip2[@T1, @T2] : seq of @T1 -> seq of @T2 -> seq of (@T1 * @T2)
end FSequence

~~~{% endraw %}

###FTestDriver.vdmpp

{% raw %}
~~~

--$Id: TestDriver.vpp,v 1.1 2005/10/31 02:09:59 vdmtools Exp $
types
functions
static public run: seq of FTestDriver`TestCase +> bool
static public isOK: FTestDriver`TestCase +> bool
static public GetTestResult : FTestDriver`TestCase +> bool
static public GetTestName: FTestDriver`TestCase +> seq of char
end FTestDriver


~~~{% endraw %}

###FTestLogger.vdmpp

{% raw %}
~~~

--$Id: TestLogger.vpp,v 1.1 2005/10/31 02:09:59 vdmtools Exp $
functions
static public Success: FTestDriver`TestCase +> bool
static public Failure: FTestDriver`TestCase +> bool
static public SuccessAll : seq of char +> bool
static public FailureAll :  seq of char +> bool
static public Print : seq of char -> bool
static public Fprint : seq of char -> bool
operations
static public Pr : seq of char ==> ()
static public Fpr : seq of char ==> ()
end FTestLogger

~~~{% endraw %}

###Function.vdmpp

{% raw %}
~~~

class Function
functions 
static public Funtil[@T] : (@T -> bool) -> (@T -> @T) -> @T -> @T
static public Fwhile[@T] : (@T -> bool) -> (@T -> @T) -> @T -> @T
static public Seq[@T] : seq of (@T -> @T) -> @T -> @T
--static length[@T] : seq of (@T -> @T) -> @T -> nat
static public readFn[@T] : seq of char -> [@T]
end Function

~~~{% endraw %}

###FunctionT.vdmpp

{% raw %}
~~~
class FunctionT is subclass of TestDriver 
class FunctionT01 is subclass of TestCase
class FunctionT02 is subclass of TestCase
class FunctionT03 is subclass of TestCase
functions
operations 
~~~{% endraw %}

###Hashtable.vdmpp

{% raw %}
~~~
/* Responsibilityï¼
 * Usageï¼
 * From historical reason, there are functional programming style and object-oriented style functions.
types
instance variables
operations
public Hashtable : () ==> Hashtable
public Hashtable : Contents ==> Hashtable
--Hashtableã®ã¯ãªã¢ã¼
public getBuckets : () ==> Bucket 
public setBuckets : Bucket ==> ()
public keySet : () ==> set of Object
public put : Object * Object ==> ()
public putAll : Contents ==> ()
public get : Object  ==> [Object]
public remove : Object ==> [Object]
public valueSet : () ==> set of Object
functions
public size : () -> nat
public isEmpty : () -> bool
public contains : Object -> bool
public containsKey : Object -> bool

-----------Functional Programming part
functions
static public Put[@T1, @T2] : 
static public PutAll[@T1, @T2] : 
static public PutAllAux[@T1, @T2] :
static public Get[@T1, @T2] : (map @T1 to (map @T1 to  @T2)) -> (@T1 -> @T1) -> @T1  -> [@T2]
static public Remove[@T1, @T2] : (map @T1 to (map @T1 to  @T2)) -> (@T1 -> @T1) -> @T1 -> (map @T1 to (map @T1 to  @T2))
--Hashtableã®ã¯ãªã¢ã¼
static public KeySet[@T1, @T2] : (map @T1 to (map @T1 to  @T2)) -> set of  @T1
static public ValueSet[@T1, @T2] : (map @T1 to (map @T1 to  @T2)) -> set of  @T2
static public Size[@T1, @T2] : (map @T1 to (map @T1 to  @T2)) -> nat
static public IsEmpty[@T1, @T2] : (map @T1 to (map @T1 to  @T2)) -> bool
static public Contains[@T1, @T2] : (map @T1 to (map @T1 to  @T2)) -> @T2 -> bool
static public ContainsKey[@T1, @T2] : (map @T1 to (map @T1 to  @T2)) -> @T1 -> bool
end Hashtable
~~~{% endraw %}

###HashtableT.vdmpp

{% raw %}
~~~
/*
instance variables
functions
public equals : Object -> bool
operations
public getContent : () ==> [seq of char]
end StringObj
instance variables
functions
public equals : Object -> bool
operations
public getContent : () ==> [int]
end IntObj
class HashtableT01 is subclass of TestCase, CommonDefinition
protected test: () ==> bool
class HashtableT02 is subclass of TestCase, CommonDefinition
class HashtableT03 is subclass of TestCase, CommonDefinition
class HashtableT04 is subclass of TestCase, CommonDefinition
class HashtableT05 is subclass of TestCase, CommonDefinition
	in
class HashtableT06 is subclass of TestCase, CommonDefinition
class HashtableT07 is subclass of TestCase, CommonDefinition
	)
class HashtableT52 is subclass of TestCase, CommonDefinition
class HashtableT53 is subclass of TestCase, CommonDefinition
class HashtableT54 is subclass of TestCase, CommonDefinition
class HashtableT55 is subclass of TestCase, CommonDefinition
class HashtableT56 is subclass of TestCase, CommonDefinition
class HashtableT57 is subclass of TestCase, CommonDefinition
~~~{% endraw %}

###Integer.vdmpp

{% raw %}
~~~
class Integer
functions 
static public asString: int -> seq1 of char 
static public asStringAux: nat -> seq1 of char 
static ndiv10 : nat +> nat
-- Convert integer to COBOL type number string (like ZZZ9.ZZ). 
 static public asStringZAux: seq of char -> nat * bool -> seq1 of char 
static length : seq of char -> nat * bool -> nat
static public asCharZ : char -> nat * bool ->  seq1 of char | bool
static public asChar : int -> seq1 of char | bool
static public GCD : nat -> nat -> nat
static GCDMeasure : nat -> nat -> nat
static public LCM : nat -> nat -> nat
end Integer
~~~{% endraw %}

###IntegerT.vdmpp

{% raw %}
~~~
class IntegerT is subclass of TestDriver
class IntegerT02 is subclass of TestCase
~~~{% endraw %}

###JapaneseCalendar.vdmpp

{% raw %}
~~~
class JapaneseCalendar is subclass of Calendar 
values
functions
static private toStringAux: int -> seq of char
static public getJapaneseDateStr : Date -> seq of char
operations
public setTheSetOfDayOffs: int ==> ()
public JapaneseCalendar : () ==> JapaneseCalendar
public getWeekdayBetweenDayOff : set of Date ==> set of Date
functions
end JapaneseCalendar
~~~{% endraw %}

###Map.vdmpp

{% raw %}
~~~
class Map 
functions
static public Contains[@T1, @T2] : map @T1 to @T2 -> @T2 -> bool
static public ContainsKey[@T1, @T2] : map @T1 to @T2 -> @T1 -> bool
end Map
~~~{% endraw %}

###MapT.vdmpp

{% raw %}
~~~
class MapT is subclass of TestDriver
~~~{% endraw %}

###Number.vdmpp

{% raw %}
~~~
class Number
functions 
static public isComputable[@e]: @e -> bool
static public min[@e] :( @e * @e -> bool) -> @e -> @e -> @e
static public max[@e] : ( @e * @e -> bool) -> @e -> @e -> @e
end Number
~~~{% endraw %}

###NumberT.vdmpp

{% raw %}
~~~
class NumberT is subclass of TestDriver 
functions
class NumberT01 is subclass of TestCase
class NumberT02 is subclass of TestCase
class NumberT03 is subclass of TestCase
~~~{% endraw %}

###Object.vdmpp

{% raw %}
~~~
class Object
Abstract
functions 
public hashCode : () -> int
public equals : Object -> bool
operations
end Object
~~~{% endraw %}

###Product.vdmpp

{% raw %}
~~~
class Product 
functions
static public Curry[@T1, @T2, @T3] : (@T1 * @T2 -> @T3) -> @T1 -> @T2 -> @T3
static public Uncurry[@T1, @T2, @T3] : (@T1 -> @T2 -> @T3) -> @T1 * @T2 -> @T3
end Product
~~~{% endraw %}

###ProductT.vdmpp

{% raw %}
~~~
class ProductT is subclass of TestDriver 
class ProductT01 is subclass of TestCase
~~~{% endraw %}

###Queue.vdmpp

{% raw %}
~~~
class Queue
functions
static public isEmpty[@T] : seq of @T -> bool
static public enQueue[@T] : @T * seq of @T -> seq of @T
static public deQueue[@T] : seq of @T -> seq of @T
static public top[@T] : seq of @T -> [@T]
end Queue
~~~{% endraw %}

###QueueT.vdmpp

{% raw %}
~~~
class QueueT is subclass of TestDriver
class QueueT01 is subclass of TestCase
;
~~~{% endraw %}

###Real.vdmpp

{% raw %}
~~~
class Real
values
functions
static public EQ : real -> real -> bool
static public EQE : real -> real -> real -> bool
static public numberOfDigit : real -> nat
static public aNumberOfIntegerPartDigit : int -> nat
static public aNumberOfIntegerPartDigitAux : int * nat -> nat
static idiv10 : int * nat +> nat
static public isNDigitsAfterTheDecimalPoint : real * nat -> bool
static public getNumberOfDigitsAfterTheDecimalPoint : real -> nat
static getNumberOfDigitsAfterTheDecimalPointAux : real * nat -> nat
static public roundAterDecimalPointByNdigit : real * nat -> real
static public Differentiate : (real -> real) ->real -> real
--Newton's method to solve the equation
-- Integration by Trapezoidal rule algorithm. This is too bad :-)
operations
functions
static public getTotalPrincipal : real * int -> real
static getInterestImplicitSpec_Math_version : real * int -> real
static getInterestImplicitSpec_Computer_version : real * int -> real
--getInterest explicit specification
end Real
~~~{% endraw %}

###RealT.vdmpp

{% raw %}
~~~
class RealT is subclass of TestDriver 
class RealT01 is subclass of TestCase
class RealT02 is subclass of TestCase
class RealT03 is subclass of TestCase
class RealT04 is subclass of TestCase
class RealT05 is subclass of TestCase
class RealT06 is subclass of TestCase
class RealT07 is subclass of TestCase
class RealT08 is subclass of TestCase
operations 




~~~{% endraw %}

###SBCalendar.vdmpp

{% raw %}
~~~

class SBCalendar is subclass of JapaneseCalendar -- date
values
instance variables
public iTodayOnBusiness : [Date] := nil;	-- This value express today for test.
functions
static public isCorrectContractMonth: seq of char -> bool
static public getExerciseDate :  seq of char -> Date
static public getContractDate : Date -> Date 
static public getMonthOf6monthsLater :  int * int ->  int * int
static public getCandidateDate : int * int * int -> Date
static public isDayoffFromTheBeginingOfMonthToCandidateDate : Date -> bool
static public getPreviousMonth : int * int -> int
static public isDateNil: [Date] -> bool
static public systemDate : () -> Date
operations
--todayOnBusinessãreadFromFile
--get today for test
public readFromFiletodayOnBusiness: seq of char ==> Date
public setTodayOnBusiness : Date ==> ()
-- get today for system. For example, many business systems thinks just after midnight is as "today"
public setTodayOnCompany : seq of char * Date ==> ()
public readSystemTime : () ==> [Time]
public systemTime : () ==> Time
public setSystemTime : Time ==> ()
public SBCalendar : () ==> SBCalendar
end SBCalendar

~~~{% endraw %}

###SBCalendarT.vdmpp

{% raw %}
~~~
class SBCalendarT is subclass of TestDriver
class SBCalendarT01 is subclass of TestCase
class SBCalendarT02 is subclass of TestCase
class SBCalendarT03 is subclass of TestCase
class SBCalendarT04 is subclass of TestCase
class SBCalendarT05 is subclass of TestCase, CalendarDefinition
class SBCalendarT06 is subclass of TestCase
~~~{% endraw %}

###Sequence.vdmpp

{% raw %}
~~~

class Sequence
functions 
static public Sum[@T]: seq of @T ->  @T
static SumAux[@T] : seq of @T -> @T -> @T
static length_measure[@T] : seq of @T +> nat
static public Product[@T]: seq of @T ->  @T
static ProductAux[@T] : seq of @T -> @T -> @T
static public GetAverage[@T]: seq of @T ->  [real]
static GetAverageAux[@T] : seq of @T -> @T -> @T -> real
static public isAscendingTotalOrder [@T]:
static public isDescendingTotalOrder [@T]:
static public isAscendingOrder [@T]: seq of @T -> bool
static public isDescendingOrder[@T]: seq of @T -> bool
static public sort[@T] : (@T * @T -> bool) -> seq of @T -> seq of @T
static public ascendingOrderSort[@T] : seq of @T -> seq of @T
static public descendingOrderSort[@T] : seq of @T -> seq of @T
static public isOrdered[@T] : seq of (@T * @T -> bool) -> seq of @T -> seq of @T -> bool
static public Merge[@T] : (@T * @T -> bool) -> seq of @T -> seq of @T -> seq of @T
static public InsertAt[@T]: nat1 -> @T -> seq of @T -> seq of @T
static public RemoveAt[@T]: nat1 -> seq of @T -> seq of @T
static public RemoveDup[@T] :  seq of @T ->  seq of @T
static public RemoveMember[@T] :  @T -> seq of @T -> seq of @T
static public RemoveMembers[@T] :  seq of @T -> seq of @T -> seq of @T
static public UpdateAt[@T]: nat1 -> @T -> seq of @T -> seq of @T
static public take[@T]: int -> seq of @T -> seq of @T
static public TakeWhile[@T] : (@T -> bool) -> seq of @T ->seq of @T
static public drop[@T]: int -> seq of @T -> seq of @T
static public DropWhile[@T] : (@T -> bool) -> seq of @T ->seq of @T
static public Span[@T] : (@T -> bool) -> seq of @T -> seq of @T * seq of @T
static public SubSeq[@T]: nat -> nat -> seq1 of @T -> seq of @T
static public last[@T]: seq of @T -> @T
static public fmap[@T1,@T2]: (@T1 -> @T2) -> seq of @T1 -> seq of @T2
public Fmap[@elem]: (@elem -> @elem) -> seq of @elem -> seq of @elem
static public filter[@T]: (@T -> bool) -> seq of @T -> seq of @T
static public Foldl[@T1, @T2] : 
static public Foldr[@T1, @T2] : 
static public isMember[@T] : @T -> seq of @T -> bool
static public isAnyMember[@T]:  seq of @T -> seq of @T -> bool
static public Index[@T]: @T -> seq of @T -> int
static IndexAux[@T]: @T -> seq of @T -> int -> int
static public IndexAll2[@T] : @T -> seq of @T -> set of int
static public flatten[@T] : seq of seq of @T -> seq of @T
static public compact[@T] : seq of [@T] -> seq of @T
static public freverse[@T] : seq of @T -> seq of @T
static public Permutations[@T]: seq of @T -> set of seq of @T
static public RestSeq[@T]: seq of @T * nat -> seq of @T
static public Unzip[@T1, @T2] : seq of (@T1 * @T2) -> seq of @T1 * seq of @T2
static lengthUnzip[@T1, @T2] : seq of (@T1 * @T2) +> nat
static public Zip[@T1, @T2] : seq of @T1 * seq of @T2 -> seq of (@T1 * @T2)
static public Zip2[@T1, @T2] : seq of @T1 -> seq of @T2 -> seq of (@T1 * @T2)
end Sequence

~~~{% endraw %}

###SequenceT.vdmpp

{% raw %}
~~~
class SequenceT is subclass of TestDriver
class SequenceT01 is subclass of TestCase
class SequenceT02 is subclass of TestCase
class SequenceT03 is subclass of TestCase
class SequenceT04 is subclass of TestCase
class SequenceT05 is subclass of TestCase
class SequenceT06 is subclass of TestCase
class SequenceT07 is subclass of TestCase
class SequenceT08 is subclass of TestCase
class SequenceT09 is subclass of TestCase
class SequenceT10 is subclass of TestCase
class SequenceT11 is subclass of TestCase
class SequenceT12 is subclass of TestCase
public product : int -> int -> int
public append : seq of char -> char -> seq of char
operations 
public product : int -> int -> int
~~~{% endraw %}

###Set.vdmpp

{% raw %}
~~~
class Set
functions 
static cardinality[@T]: set of @T +> nat
static public hasSameElems[@T] : (seq of @T) * (set of @T) -> bool
static public Combinations[@T] : nat1 -> set of @T -> set of set of @T

static public fmap[@T1,@T2]: (@T1 -> @T2) -> set of @T1 -> set of @T2
static public Sum[@T]: set of @T ->  @T
static SumAux[@T] : set of @T -> @T -> @T
end Set
~~~{% endraw %}

###SetT.vdmpp

{% raw %}
~~~
class SetT is subclass of TestDriver
class SetT02 is subclass of TestCase
class SetT03 is subclass of TestCase
class SetT04 is subclass of TestCase
~~~{% endraw %}

###String.vdmpp

{% raw %}
~~~
class String is subclass of Sequence
functions
--Conversion functions
static private AsIntegerAux : seq of char -> int -> int
static length : seq of char +> nat
static lengthNil : [seq of char] +> nat
--Decision functions
static public isDigits : seq of char -> bool
static public isLetters : seq of char -> bool
static public isLetterOrDigits : seq of char -> bool
static public isSpaces : [seq of char] -> bool
static public LT : seq of char * seq of char -> bool
static public LT2 : seq of char -> seq of char -> bool
static public LE : seq of char * seq of char -> bool
static public LE2 : seq of char -> seq of char -> bool
static public GT : seq of char * seq of char -> bool
static public GT2 : seq of char -> seq of char -> bool
static public GE : seq of char * seq of char -> bool
static public GE2 : seq of char -> seq of char -> bool
static public Index: char -> seq of char -> int
static public indexAll : seq of char * char -> set of int
static public IndexAll2 : char -> seq of char -> set of int
static public isInclude : seq of char -> seq of char -> bool
static public subStr :
static public SubStr : nat -> nat -> seq1 of char -> seq of char
static public GetToken : seq of char * set of char -> seq of char
static public DropToken : seq of char * set of char -> seq of char
static public getLines : seq of char -> seq of seq of char
static public getLinesAux : seq of char -> seq of seq of char -> seq of seq of char
operations
static public subStrFill :
end String
~~~{% endraw %}

###StringT.vdmpp

{% raw %}
~~~
class StringT is subclass of TestDriver 
class StringT01 is subclass of TestCase
class StringT02 is subclass of TestCase
class StringT03 is subclass of TestCase
class StringT04 is subclass of TestCase
class StringT05 is subclass of TestCase
class StringT06 is subclass of TestCase

class StringT07 is subclass of TestCase
class StringT08 is subclass of TestCase
class StringT09 is subclass of TestCase
class StringT10 is subclass of TestCase
class StringT11 is subclass of TestCase
/*
/*
class StringT14 is subclass of TestCase
;

~~~{% endraw %}

###Term.vdmpp

{% raw %}
~~~
class Term
values
instance variables
functions
public EQ : Term -> bool
operations
public getStartTime : () ==> [Time]
public getEndTime : () ==> [Time]
end  Term
~~~{% endraw %}

###TermT.vdmpp

{% raw %}
~~~
class TermT is subclass of TestDriver 
class TermT01 is subclass of TestCase, CalendarDefinition
~~~{% endraw %}

###TestCase.vdmpp

{% raw %}
~~~
class TestCase
instance variables
	public TestName: seq of char := "** anonymous regression test **";
operations
public getTestName: () ==> seq of char
protected test: () ==> bool
protected setUp: () ==> ()
protected tearDown: () ==> ()
end TestCase

~~~{% endraw %}

###TestDriver.vdmpp

{% raw %}
~~~
class TestDriver
functions
--Return all TestCases
--Confirm test result
operations
--Test a TestCase sequence.
end TestDriver

~~~{% endraw %}

###TestLogger.vdmpp

{% raw %}
~~~
--$Id: TestLogger.vpp,v 1.2 2006/04/04 07:03:05 vdmtools Exp $
values
hisotoryFileName = "VDMTESTLOG.TXT"
functions
public Succeeded: TestCase -> bool
public Failed: TestCase -> bool
public succeededInAllTestcases : seq of char -> bool
public notSucceededInAllTestcases :  seq of char -> bool
end TestLogger
~~~{% endraw %}

###Time.vdmpp

{% raw %}
~~~
class Time is subclass of CalendarDefinition
values
types
instance variables
operations
public Time : Calendar * int * int * int ==> Time
public Time : Date ==> Time
--currentDateTimeãæ±ããåä½ãã¹ãç¨é¢æ°ã

--currentDateTimeãæå®ããreadFromFileåä½ãã¹ãç¨é¢æ°ã
--currentDateTimeãreadFromFile
--ã¤ã³ã¹ã¿ã³ã¹å¤æ°æä½
public getDate : () ==> Date
public setDate : Date ==> ()
public getTime : () ==> TimeInMilliSeconds
public setTime : TimeInMilliSeconds ==> ()
public hour: () ==> nat
public setTimeFromNat : nat ==> ()
public minute: () ==> nat
public setMinuteFromNat : nat ==> ()
public second: () ==> nat
public setSecond : nat ==> ()
public milliSecond: () ==> nat
public setMilliSecond : nat ==> ()
functions
--æéããããã®æéã®å±ããæ¦ãæ±ããã
--æéããããã®æéã®å±ããå¹´ãæ±ããã
--æéããããã®æéã®å±ããæãæ±ããã
--æéãããæ¥ãæ±ããã
public getTimeAsNat : () -> nat
----Compare
public LT: Time -> bool
public GT: Time -> bool
public LE: Time -> bool
public GE: Time -> bool
--èªèº«ã¨ä¸ããããæéãEQãå¤å®ããã
--èªèº«ã¨ä¸ããããæéãç­ãããªããå¤å®ããã
--å¤æ
public IntProduct2TimeMillieSeconds : int * int * int * int -> int
public Time2IntProduct : TimeInMilliSeconds -> nat * nat * nat * nat
operations
public print : () ==> seq of char
----calculation
--milliSecondãå ç®ãã
public plussecond : int ==> Time
public plusminute : int ==> Time
public plushour : int ==> Time
public plus: int * int * int * int ==> Time
--milliSecondãæ¸ç®ãã
public minus: int * int * int * int  ==> Time
end Time
~~~{% endraw %}

###TimeT.vdmpp

{% raw %}
~~~
class TimeT is subclass of TestDriver 
class TimeT01 is subclass of TestCase, CalendarDefinition
class TimeT02 is subclass of TestCase, CalendarDefinition
class TimeT03 is subclass of TestCase, CalendarDefinition
class TimeT04 is subclass of TestCase, CalendarDefinition
	;
class TimeT05 is subclass of TestCase, CalendarDefinition
class TimeT06 is subclass of TestCase, CalendarDefinition
~~~{% endraw %}

###UniqueNumber.vdmpp

{% raw %}
~~~
class ï¼µï½ï½ï½ï½ï½ï¼®ï½ï½ï½ï½ï½ is subclass of CommonDefinition
values
instance variables
functions
operations
public initialize : () ==> int
end  ï¼µï½ï½ï½ï½ï½ï¼®ï½ï½ï½ï½ï½
~~~{% endraw %}

###UniqueNumberT.vdmpp

{% raw %}
~~~
class ï¼µï½ï½ï½ï½ï½ï¼®ï½ï½ï½ï½ï½ï¼´ is subclass of TestDriver
class ï¼µï½ï½ï½ï½ï½ï¼®ï½ï½ï½ï½ï½ï¼´01 is subclass of TestCase
~~~{% endraw %}

###UseCalendar.vdmpp

{% raw %}
~~~
class UseCalendar
instance variables
traces
S1 : 
S2:
end UseCalendar
~~~{% endraw %}

###UseReal.vdmpp

{% raw %}
~~~
class UseReal
instance variables
traces
S1: let n in set {0, 1, 9, 10, 99, 199, 0.1, 9.1, 10.1, 10.123} in r.numberOfDigit(n)
end UseReal
~~~{% endraw %}

###UseUniqueNumber.vdmpp

{% raw %}
~~~
class UseUniqueNumber
instance variables
traces
S1 : 
end UseUniqueNumber
~~~{% endraw %}
