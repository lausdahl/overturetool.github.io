---
layout: default
title: trackerproofPP
---

## trackerproofPP
Author: John Fitzgerald



This VDM++ model is a direct transformation from the 
VDM-SL model presented in the Fitzgerald&Larsen98 book 
on VDM-SL. The tracker takes care of monitoring and 
controlling the nuclear material in a plant that takes 
care of processing such waste material. 


| Properties | Values          |
| :------------ | :---------- |
|Language Version:| vdm10|


### tracker.vdmpp

{% raw %}
~~~
class TRACKER
-- The tracker example

types

 public Tracker :: containers : ContainerInfo
             phases     : PhaseInfo
  inv mk_Tracker(containers,phases) ==
    Consistent(containers,phases) and
    PhasesDistinguished(phases) and
    MaterialSafe(containers,phases);

 public ContainerInfo = map ContainerId to Container;

 public PhaseInfo = map PhaseId to Phase;

 public Container :: fiss_mass : real
               material  : Material;

 public Phase :: contents           : set of ContainerId
           expected_materials : set of Material
           capacity           : nat
  inv p == card p.contents <= p.capacity and
           p.expected_materials <> {};

 public ContainerId = token;

 public PhaseId = token;

 public Material = token

functions

  -- introduce a new container to the plant (map union)

  Introduce : Tracker * ContainerId * real * Material -> Tracker
  Introduce(trk, cid, quan, mat) == 
     mk_Tracker(trk.containers munion 
                {cid |-> mk_Container(quan, mat)},
                trk.phases)
  pre cid not in set dom trk.containers;

  -- permission to move (simple Boolean function)

  Permission: Tracker * ContainerId * PhaseId  ->  bool
  Permission(mk_Tracker(containers, phases), cid, dest) == 
    cid in set dom containers and
    dest in set dom phases and 
    card phases(dest).contents < phases(dest).capacity and
    containers(cid).material in set phases(dest).expected_materials;

  -- move a known container between two phases

  Move : Tracker * ContainerId * PhaseId * PhaseId -> Tracker
  Move(trk, cid, ptoid,pfromid) ==
    let pha = mk_Phase(trk.phases(ptoid).contents union {cid},
                      trk.phases(ptoid).expected_materials,
                      trk.phases(ptoid).capacity)
    in
      mk_Tracker(trk.containers, 
	         Remove(trk,cid,pfromid).phases ++ {ptoid |-> pha})
  pre Permission(trk, cid, ptoid) and pre_Remove(trk,cid,pfromid);

  -- remove a container from the contents of a phase

  Remove: Tracker * ContainerId * PhaseId -> Tracker
  Remove(mk_Tracker(containers, phases), cid, source) ==
    let pha = mk_Phase(phases(source).contents \ {cid},
                     phases(source).expected_materials,
                     phases(source).capacity)
    in
      mk_Tracker(containers, phases ++ {source |-> pha})
  pre source in set dom phases and 
    cid in set phases(source).contents;

  -- delete a container from the plant

  Delete: Tracker * ContainerId * PhaseId  ->  Tracker
  Delete(tkr, cid, source) ==
    mk_Tracker({cid} <-: tkr.containers,
               Remove(tkr, cid, source).phases)
  pre pre_Remove(tkr,cid,source);
    
  -- Auxiliary functions defined for inv-Tracker
  
Consistent: ContainerInfo * PhaseInfo -> bool
  Consistent(containers, phases) ==
    forall ph in set rng phases & 
      ph.contents subset dom containers;

  PhasesDistinguished: PhaseInfo -> bool
  PhasesDistinguished(phases) ==
    not exists p1, p2 in set dom phases &
      p1 <> p2 and 
      phases(p1).contents inter phases(p2).contents <> {}; 
	
 MaterialSafe: ContainerInfo * PhaseInfo -> bool
  MaterialSafe(containers, phases) ==                
     forall ph in set rng phases & 
        forall cid in set ph.contents &
           cid in set dom containers and
           containers(cid).material in set ph.expected_materials;


end TRACKER
~~~
{% endraw %}

