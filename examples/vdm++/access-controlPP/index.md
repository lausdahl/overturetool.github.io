---
layout: default
title: access-control
---

~~~

This specification describes access control and policies for restricting this.
2. A Formal Approach to Dependable Evolution of Access Control Policies in 

#******************************************************
~~~
###Env.vdmpp

{% raw %}
~~~
class Env
instance variables 
senv : map FExp`Id to FExp`SType;
operations
public Env: map FExp`Id to FExp`SType * map FExp`Id to FExp`Val ==> Env
public GetSenv: () ==> map FExp`Id to FExp`SType
public GetDenv: () ==> map FExp`Id to FExp`Val
public GetVal: FExp`Id ==> FExp`Val
public GetAVal:FExp`Id * FExp`Id ==> FExp`Val
public GetSType: FExp`Id ==> FExp`SType
public GetSAType: FExp`Id ==> FExp`AType
public GetAType:FExp`Id * FExp`Id ==> FExp`SType
end Env

~~~{% endraw %}

###Evaluater.vdmpp

{% raw %}
~~~
class Evaluator
-- Evaluator is the "top level" class.  It receives an access control
instance variables
pdp : PDP;       -- an object of class PDP --- Policy Decision Point
types 
Inst :: map FExp`UnId to FExp`Id
values
requester : FExp`UnId = mk_FExp`UnId(<requester>);
operations
public Evaluator: Request * PDP * Env ==> Evaluator
-- evaluate operates on the request that is part of Evaluator state. 
public evaluate: () ==> PDP`Effect
evaluatePDPDenyOverrides : () ==> PDP`Effect 
evaluatePDPPermitOverrides : () ==> PDP`Effect 
evaluateRule : PDP`Rule ==> PDP`Effect 
evaluatePol : PDP`Policy ==> PDP`Effect
evaluateRulesDenyOverrides : set of PDP`Rule ==> PDP`Effect
evaluateRulesPermitOverrides : set of PDP`Rule ==> PDP`Effect
-- targetmatch has been adapted.  If any of the sets in the target of 
targetmatch : PDP`Target ==> bool

end Evaluator
~~~{% endraw %}

###FExp.vdmpp

{% raw %}
~~~
class FExp
instance variables
fexp : Expr; 
operations
public FExp : Expr ==> FExp
public GetExp: () ==> Expr
types
public AtomicVal = bool | int | <Indet>;


public Expr = Id | UnId | BoolExpr | ArithExpr | ArrayLookup;
public Id = token;
public UnId :: <requester>|<resource>; --<action>;
-- Expressions returning true or false
public RelExpr :: left  : Expr
public Unary :: op   : <NOT>
public Infix :: left  : Expr
public Equal ::  left  : Expr
public boolLiteral :: <TRUE>|<FALSE>;
-- Expression returning integer
public intLiteral :: <ZERO>|<ONE>|<TWO>|<THREE>|<FOUR>|<FIVE>|<SIX>|<SEVEN>|<EIGHT>|<NINE>|<TEN>;

public ArrayLookup :: aname : Id | UnId	
operations
-- binds the unbound variables within an expression.
public BindExpr: Expr * Request ==> FExp`Expr 
-- Evaluate returns the meaning of an expression,  wrt an environment 
---------------------------------------
public EvaluateBind : Request * Env ==> Val
Evaluate : Expr * Env ==> Val
private MId : Id * Env ==> Val
MRelExpr : RelExpr * Env ==> Val
EvaluateLT: Expr * Expr * Env ==> Val
EvaluateGT: Expr * Expr * Env ==> Val
MUnary : Unary * Env ==> Val
MInfix : Infix * Env ==> Val
EvaluateAND: Expr * Expr * Env ==> Val
MEqual : Equal * Env ==> Val
-- order of evaluation is left to right, as stipulated by XACML standard
EvaluateOR: Expr * Expr * Env ==> Val
MArrayLookup : ArrayLookup * Env ==> Val
functions
 MLiteral : boolLiteral | intLiteral -> Val

--------------------------------------
-- types for TP and WF judgements
types
public SType = <B>|<I>|<U>|AType|<Err>;
public AType = map Id to <B>|<I>|<U>;
operations -- for TP and WF judgements
public wfExpr : Env ==> bool
private exprTp : Expr * Env ==> SType
private wfInfix : Infix * Env ==> SType

private wfUnary : Unary * Env ==> SType
private wfRelExpr : RelExpr * Env ==> SType
private wfLiteral : boolLiteral | intLiteral ==> SType
private wfEqual: Equal * Env ==> SType
private wfId: Id * Env ==> SType
wfUnId : UnId  ==> SType
wfArrayLookup : ArrayLookup * Env ==> SType 

end FExp
~~~{% endraw %}

###PDP.vdmpp

{% raw %}
~~~
class PDP 
instance variables 
policies : set of Policy;
operations
public PDP: set of Policy * CombAlg  ==> PDP
types
public Permit = token;
public CombAlg = <denyOverrides> | <permitOverrides>;
public Policy :: target : [Target]
public Rule :: target : [Target]  
public Effect = <Permit> | <Deny> | <Indeterminate> | <NotApplicable>;   
public Target :: subjects : set of Subject
public Action = FExp`Id;
operations
public GetpolicyCombAlg: () ==> CombAlg
public Getpolicies: () ==> set of Policy
public GetEffect: Rule ==> Effect
end PDP
~~~{% endraw %}

###Request.vdmpp

{% raw %}
~~~
class Request
instance variables
-- Request targets must have a single subject AND a single resource.   
subject  : PDP`Subject;
types
Inst :: map token to FExp`Id;
 operations
public Request: PDP`Subject * PDP`Resource * set of PDP`Action ==> Request
public GetSubject: () ==> PDP`Subject
public GetResource: () ==> PDP`Resource
public GetActions: () ==> set of PDP`Action
end Request
~~~{% endraw %}

###Test.vdmpp

{% raw %}
~~~
class Test
requester : FExp`UnId = mk_FExp`UnId(<requester>);
Anne : FExp`Id = (mk_token("Anne"));
write : FExp`Id = (mk_token("write"));
lab_results_signed : FExp`Id =
results_analysis_signed : FExp`Id =
Project1 : set of PDP`Subject = {Anne,Bob}; 
Company2 : set of PDP`Subject = {Eric, Fred}; 
lab_results : FExp`Id = (mk_token("lab_results"));
doc1 : FExp`Id = (mk_token("doc1"));
signed : FExp`Id = (mk_token("signed"));
con_nt : FExp = new FExp(mk_FExp`Unary(<NOT>,mk_FExp`boolLiteral(<TRUE>)));
-- on Project One, lab results can be created only by a lab
project1_rule_nf : PDP`Rule = 
project1_rule_nt : PDP`Rule = 
project1_rule_no : PDP`Rule = 
project1_rule2 : PDP`Rule = 

-- The project policy for the lab_results document is the combination
lab_results_project_policy_no : PDP`Policy = 

lab_results_project_policy_nt : PDP`Policy = 

lab_results_project_policy_nf : PDP`Policy = 
-- New policy 
lab_results_rule1 : PDP`Rule =
-- Also, after signoff, no one can write to the lab_results file. 
lab_results_rule2 : PDP`Rule =
lab_results_creator_policy : PDP`Policy = 
-- New policy
scale_assess_write : PDP`Rule = 
-- From company 2, anyone can read the scale assessment.
scale_assess_read : PDP`Rule = 
scale_assess_policy : PDP`Policy = 
gold_policy_no : PDP = 
gold_policy_nt : PDP = 
gold_policy_nf : PDP = 
gold_policy_project_results : PDP = 
gold_policy_results_scale : PDP = 



	operations
  public Run: () ==> PDP`Effect
end Test
~~~{% endraw %}
