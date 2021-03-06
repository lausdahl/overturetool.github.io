---
layout: default
title: shmemSL
---

## shmemSL
Author: Nick Battle


This example was produced by Nick Battle and it is used in the VDMJ user
manual to illustrate different features of VDMJ. It models the behaviour
of the 32-bit shared memory quadrants of HP-UX, using a record type M to 
represent a block of memory which is either <FREE> or <USED>, and a 
sequence of M records to represent a Quadrant.

The specification output indicates which allocation policy, first-fit or 
best-fit (or neither), produces the most memory fragmentation.


| Properties | Values          |
| :------------ | :---------- |
|Language Version:| classic|
|Entry point     :| M`main(5,100)|


### shmem.vdmsl

{% raw %}
~~~
module M
exports all
definitions
types

Quadrant = seq of M;
--inv Q == 
--  forall a in set elems Q &
--    (not exists b in set elems Q \ {a} &
--           (b.start >= a.start and b.start <= a.stop) or
--           (b.stop  >= a.start and b.stop  <= a.stop));

M :: type : <USED> | <FREE>
     start: nat
     stop : nat
inv mk_M(-, a, b) == (b >= a)


state Memory of
  rseed: nat
  Q3: Quadrant
  Q4: Quadrant
inv mk_Memory(-, q3, q4) == len q3 > 0 and len q4 > 0
init q == 
  q = mk_Memory(87654321,
                [mk_M(<FREE>, 0, MAXMEM-1)],
                [mk_M(<FREE>, 0, MAXMEM-1)])
		
end

values

MAXMEM = 10000;
CHUNK = 100;

functions

sizeof: M -> nat1
sizeof(m) == 
  m.stop - m.start + 1;

least: nat1 * nat1 -> nat1
least(a, b) == 
  if a < b 
  then a 
  else b;
	
spacefor: nat1 * Quadrant -> nat1
spacefor(size, Q) ==
  cases Q:
    []         -> MAXMEM + 1,
    [h] ^ tail -> if h.type = <FREE> and sizeof(h) >= size
                  then sizeof(h) 
                  else spacefor(size, tail)
  end
measure QuadrantLen;
	
QuadrantLen:  nat1 * Quadrant -> nat
QuadrantLen(-,list) == 
  len list;

bestfit: nat1 * Quadrant -> nat1
bestfit(size, Q) ==
  cases Q:
    -- as we're looking for the smallest
    []         -> MAXMEM + 1,	
    [h] ^ tail -> if h.type = <FREE> and sizeof(h) >= size
                  then least(sizeof(h), bestfit(size, tail))
                  else bestfit(size, tail)
  end
measure QuadrantLen;
	
add: nat1 * nat1 * Quadrant -> Quadrant
add(size, hole, Q) ==
  cases Q:
    [h] ^ tail ->  if h.type = <FREE> and sizeof(h) = hole 
                   then	if hole = size 
                        then [mk_M(<USED>, h.start, h.stop)] ^ tail
                        else [mk_M(<USED>, h.start, h.start + size - 1),
                              mk_M(<FREE>, h.start + size, h.stop)] ^ tail
                   else	[h] ^ add(size, hole, tail),
    others -> Q
  end
pre hole >= size
measure QuadrantLen2;

QuadrantLen2: nat1 * nat1 * Quadrant -> nat
QuadrantLen2(-,-,list) == 
  len list;

combine: Quadrant -> Quadrant
combine(Q) ==
  cases Q:
    [h1, h2] ^ tail -> 
       if h1.type = <FREE> and h2.type = <FREE>
       then combine([mk_M(<FREE>, h1.start, h2.stop)] ^ tail)
       else [h1] ^ combine(tl Q),
    others -> Q 
  end
measure QuadrantLen0;

QuadrantLen0: Quadrant -> nat
QuadrantLen0(list) == 
  len list;

delete: M * Quadrant -> Quadrant
delete(item, Q) ==
  if hd Q = item
  then combine([mk_M(<FREE>, item.start, item.stop)] ^ tl Q)
  else [hd Q] ^ delete(item, tl Q)
  measure MQuadrantLen;

MQuadrantLen: M * Quadrant -> nat
MQuadrantLen(-,list) == 
  len list;

fragments: Quadrant -> nat
fragments(Q) ==
  card {x | x in set elems Q & x.type = <FREE>} - 1;

operations

seed: nat1 ==> ()
seed(n) == 
  rseed := n;
	
inc: () ==> ()
inc() ==
  for i = 1 to rseed mod 7 + 3 
  do
    rseed := (rseed * 69069 + 5) mod 4294967296;

rand: nat1 ==> nat1
rand(n) ==
 (inc();
  return rseed mod n + 1;
 );

FirstFit: nat1 ==> bool
FirstFit(size) ==
 (let q4 = spacefor(size, Q4) 
  in
    if q4 <= MAXMEM
    then Q4 := add(size, q4, Q4)
    else let q3 = spacefor(size, Q3) 
         in
           if q3 <= MAXMEM
           then Q3 := add(size, q3, Q3)
           else return false;
  return true;
 );

BestFit: nat1 ==> bool
BestFit(size) ==
 (let q4 = bestfit(size, Q4) 
  in
    if q4 <= MAXMEM
    then Q4 := add(size, q4, Q4)
    else let q3 = bestfit(size, Q3) 
         in
           if q3 <= MAXMEM
           then Q3 := add(size, q3, Q3)
           else return false;
  return true;
 );

Reset: () ==> ()
Reset() ==
 (Q3 := [mk_M(<FREE>, 0, MAXMEM-1)];
  Q4 := [mk_M(<FREE>, 0, MAXMEM-1)];
 );

DeleteOne: () ==> ()
DeleteOne() ==
 (if rand(2) = 1
  then let i = rand(len Q3) 
       in
         if Q3(i).type = <USED>
         then Q3 := delete(Q3(i), Q3)
         else DeleteOne()
  else let i = rand(len Q4) 
       in
         if Q4(i).type = <USED>
         then Q4 := delete(Q4(i), Q4)
         else DeleteOne()
 )
pre (exists m in set elems Q3 & m.type = <USED>) or
    (exists m in set elems Q4 & m.type = <USED>);

TryFirst: nat ==> nat
TryFirst(loops) ==
 (dcl count:int := 0;
	
  while count < loops and FirstFit(rand(CHUNK)) 
  do
    (if count > 50 
     then DeleteOne();
     count := count + 1;
    );

  -- return count;
  return fragments(Q3) + fragments(Q4);
 );

TryBest: nat ==> nat
TryBest(loops) ==
 (dcl count:int := 0;
	
  while count < loops and BestFit(rand(CHUNK)) 
  do
    (if count > 50 
     then DeleteOne();
     count := count + 1;
    );
	
  -- return count;
  return fragments(Q3) + fragments(Q4);
 );

main: nat1 * nat1 ==> seq of (<BEST> | <FIRST> | <SAME>)
main(tries, loops) ==
 (dcl result: seq of (<BEST> | <FIRST> | <SAME>) := [];
	
  for i = 1 to tries 
  do
    (dcl best:int, first:int;
		
     Reset();
     seed(i);
     best := TryBest(loops);
		
     Reset();
     seed(i);
     first := TryFirst(loops);
		
     if best = first
     then result := result ^ [<SAME>]
     elseif best > first 
     then result := result ^ [<BEST>]
     else result := result ^ [<FIRST>];
    );
		
  return result;
 )

end M

~~~
{% endraw %}

