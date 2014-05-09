---
layout: listing
title: ADT
---

~~~

This work specifies a number of abstract data types (ADT) in
VDM-SL. This includes single-linked lists, double-linked lists,
queues, stacks and binary trees. In addition this work is initiating a
translation from VDM-SL to Modula-2 so system addressing and dynamic
memory handing is specified in VDM-SL. This work was done by Matthew
Suderman when he was a student at Trinity Western University in the
Spring Semester, 1997.


#******************************************************
#  AUTOMATED TEST SETTINGS
#------------------------------------------------------
#AUTHOR= Matthew Suderman
#LANGUAGE_VERSION=classic
#INV_CHECKS=true
#POST_CHECKS=true
#PRE_CHECKS=true
#DYNAMIC_TYPE_CHECKS=true
#SUPPRESS_WARNINGS=false
#ENTRY_POINT=DEFAULT`TestTrees()
#EXPECTED_RESULT=NO_ERROR_TYPE_CHECK
#******************************************************

~~~
###adt.vdmsl

~~~
% actual VDM-SL specification
\setcounter{section}{1}
\section{VDM-SL Specification of Data Structures}
\subsection{Address Model}
The underlying type for modelling memory addresses is the natural numbers.  The {\tt ADDRESS} value {\tt NIL} models the address to  `nowhere'.  In Modula-2 terms, {\tt ADDRESS} models the corresponding Modula-2 type with the same name found the pseudo-module SYSTEM.   {\tt NIL} likewise models the corresponding Modula-2 value with the same name.\\       
\\
\begin{vdm_al}
types 
     ADDRESS = nat;

values
     NIL: ADDRESS = 0;   
\end{vdm_al}

\subsection{Node Definitions}
This section specifies the node types that will be used by the abstract data types.  The {\tt Nodes\_SingleLink} type will be used in the singly-linked list; the {\tt Nodes\_DoubleLink} type will be used in the doubly-linked list; and the {\tt Nodes\_BinaryTree} type will be used in the binary tree.  The queue and stack specifications will be based on the singly-linked list so they do not require node definitions.\\
\\
There is a somewhat generic node type, {\tt Nodes\_Node}, which can be the type of any one of the three specific node types.  Because of this, the functions for handling node manipulation and retrieval are generic with respect to node type.  For example, the function {\tt Nodes\_GetData} can receive as input any one of the three node types and return its data field.

The exported data types include:
\begin{itemize}
\item {\tt Nodes\_Data}
\item {\tt Nodes\_SingleLink}
\item {\tt Nodes\_DoubleLink}
\item {\tt Nodes\_BinaryTree}
\item {\tt Nodes\_Node}
\item {\tt Nodes\_NodePtr}
\end{itemize}
The exported functions include:
\begin{itemize}
\item {\tt Nodes\_MkSingleLink}
\item {\tt Nodes\_MkDoubleLink}
\item {\tt Nodes\_MkBinaryTree}
\item {\tt Nodes\_GetData}
\item {\tt Nodes\_SetData}
\item {\tt Nodes\_GetNext}
\item {\tt Nodes\_SetNext}
\item {\tt Nodes\_GetPrev}
\item {\tt Nodes\_SetPrev}
\item {\tt Nodes\_GetRight}
\item {\tt Nodes\_SetLeft}
\item {\tt Nodes\_GetParent}
\item {\tt Nodes\_SetParent}
\end{itemize}
\begin{vdm_al}
types
     Nodes_Data = Data;
\end{vdm_al}
The type {\tt Nodes\_Data} acts a generic parameter for the {\tt Nodes}  `module'.  The type {\tt Data} is defined in the testing section of this specification (2.11).  \\
\begin{vdm_al}
types

Nodes_SingleLink ::
     data: Nodes_Data
     next: Nodes_NodePtr;
     
Nodes_DoubleLink ::
     data: Nodes_Data
     next: Nodes_NodePtr 
     prev: Nodes_NodePtr;

Nodes_BinaryTree ::

     data: Nodes_Data
     right: Nodes_NodePtr
     left: Nodes_NodePtr
     parent: Nodes_NodePtr;

Nodes_Node = Nodes_SingleLink | Nodes_DoubleLink | Nodes_BinaryTree;
Nodes_NodePtr = ADDRESS;

functions

Nodes_MkSingleLink: Nodes_Data * Nodes_NodePtr -> Nodes_Node
Nodes_MkSingleLink(data, ptr) == mk_Nodes_SingleLink(data, ptr);

Nodes_MkDoubleLink: Nodes_Data * Nodes_NodePtr * Nodes_NodePtr -> Nodes_Node
Nodes_MkDoubleLink(data, next, prev) == mk_Nodes_DoubleLink(data, next, prev);

Nodes_MkBinaryTree: Nodes_Data * Nodes_NodePtr * Nodes_NodePtr * Nodes_NodePtr 
               -> Nodes_Node
Nodes_MkBinaryTree(data, right, left, parent) == 
     mk_Nodes_BinaryTree(data, right, left, parent);

\end{vdm_al}
These three {\tt Nodes\_Mk...} functions return a node whose fields match the values of the function's arguments.  In other words, these are node constructor functions.\\
\\
\begin{vdm_al}

Nodes_GetData: Nodes_Node -> Nodes_Data
Nodes_GetData(node) == 
     if is_Nodes_SingleLink(node) then
          let mk_Nodes_SingleLink(data, -) = node in data
     elseif  is_Nodes_DoubleLink(node) then
          let mk_Nodes_DoubleLink(data, -, -) = node in data
     else 
          let mk_Nodes_BinaryTree(data, -, -, -) = node in data
pre is_Nodes_SingleLink(node) 
     or is_Nodes_DoubleLink(node) 
     or is_Nodes_BinaryTree(node); 

Nodes_SetData: Nodes_Node * Nodes_Data -> Nodes_Node
Nodes_SetData(node, data) ==
     if is_Nodes_SingleLink(node) then
          let mk_Nodes_SingleLink(-, next) = node in 
               mk_Nodes_SingleLink(data, next)
     elseif  is_Nodes_DoubleLink(node) then
          let mk_Nodes_DoubleLink(-, next, prev) = node in 

               mk_Nodes_DoubleLink(data, next, prev)
     else 
          let mk_Nodes_BinaryTree(-, right, left, parent) = node in
               mk_Nodes_BinaryTree(data, right, left, parent)
pre is_Nodes_SingleLink(node) 
     or is_Nodes_DoubleLink(node) 
     or is_Nodes_BinaryTree(node); 

Nodes_GetNext: Nodes_Node -> Nodes_NodePtr
Nodes_GetNext(node) ==
     if is_Nodes_SingleLink(node) then
          let mk_Nodes_SingleLink(-, next) = node in next
     else 
          let mk_Nodes_DoubleLink(-, next, -) = node in next
pre is_Nodes_SingleLink(node) 
     or is_Nodes_DoubleLink(node);       
               
Nodes_SetNext: Nodes_Node * Nodes_NodePtr -> Nodes_Node
Nodes_SetNext(node, next) ==
     if is_Nodes_SingleLink(node) then
          let mk_Nodes_SingleLink(data, -) = node in
               mk_Nodes_SingleLink(data, next) 
     else  
          let mk_Nodes_DoubleLink(data, -, prev) = node in
               mk_Nodes_DoubleLink(data, next, prev)
pre is_Nodes_SingleLink(node) 
     or is_Nodes_DoubleLink(node);
 
Nodes_GetPrev: Nodes_Node -> Nodes_NodePtr
Nodes_GetPrev(node) == 
     let mk_Nodes_DoubleLink(-, -, prev) = node in prev
pre is_Nodes_DoubleLink(node);

Nodes_SetPrev: Nodes_Node * Nodes_NodePtr -> Nodes_Node
Nodes_SetPrev(node, prev) == 
     let mk_Nodes_DoubleLink(data, next, -) = node in
          mk_Nodes_DoubleLink(data, next, prev)
pre  is_Nodes_DoubleLink(node);    

Nodes_GetRight: Nodes_Node -> Nodes_NodePtr
Nodes_GetRight(node) == 
     let mk_Nodes_BinaryTree(-, right, -, -) = node in right
pre is_Nodes_BinaryTree(node);     

Nodes_SetRight: Nodes_Node * Nodes_NodePtr -> Nodes_Node
Nodes_SetRight(node, right) == 

     let mk_Nodes_BinaryTree(data, -, left, parent) = node in
          mk_Nodes_BinaryTree(data, right, left, parent)
pre is_Nodes_BinaryTree(node);     

Nodes_GetLeft: Nodes_Node -> Nodes_NodePtr
Nodes_GetLeft(node) == 
     let mk_Nodes_BinaryTree(-, -, left, -) = node in left
pre is_Nodes_BinaryTree(node);

Nodes_SetLeft: Nodes_Node * Nodes_NodePtr -> Nodes_Node
Nodes_SetLeft(node, left) == 
     let mk_Nodes_BinaryTree(data, right, -, parent) = node in
          mk_Nodes_BinaryTree(data, right, left, parent)
pre is_Nodes_BinaryTree(node);     

Nodes_GetParent: Nodes_Node -> Nodes_NodePtr
Nodes_GetParent(node) == 
     let mk_Nodes_BinaryTree(-, -, -, parent) = node in parent
pre is_Nodes_BinaryTree(node);

Nodes_SetParent: Nodes_Node * Nodes_NodePtr -> Nodes_Node
Nodes_SetParent(node, parent) == 
     let mk_Nodes_BinaryTree(data, right, left, -) = node in
          mk_Nodes_BinaryTree(data, right, left, parent)
pre is_Nodes_BinaryTree(node);     
\end{vdm_al}
Each {\tt Nodes\_Get...} function has a matching {\tt Nodes\_Set...} function.  The `get' functions return one of the input node's fields.  The `set' functions return the input node with the correct field set to the input value.

\subsection{Minimal System Heap Model}
In Modula-2, memory found in the system heap can be dynamically allocated and deallocated. Allocated memory is accessed by variables of type {\tt ADDRESS}.  Memory is allocated by the {\tt NEW} procedure and deallocated by the {\tt DISPOSE} procedure.  The purpose of this {\tt Heaps}  `module' is to model Modula-2 dynamic memory as closely as possible.  \\
\\
The heap is modelled as a sequence of locations.  Locations are referenced by variables of type {\tt ADDRESS}.  Each location consists of stored data and a boolean stating whether the location has been allocated or not. \\
\\
This heap model is limited in that it supports storage of only one type of data.  In this case, the only data type is {\tt Nodes\_Node}. \\
\\
{\tt Heaps} exports the following data types:
\begin{itemize}
\item {\tt Heaps\_Size}
\item {\tt Heaps\_Heap}
\end{itemize}
The following functions are exported:
\begin{itemize}
\item {\tt Heaps\_Init}
\item {\tt Heaps\_AmountUsed}
\item {\tt Heaps\_Available}
\item {\tt Heaps\_Retrieve}
\item {\tt Heaps\_Modify}
\end{itemize}
\begin{vdm_al}
types Heaps_Data = [Nodes_Node];
values Heaps_Size = 20;
\end{vdm_al}
{\tt Heaps\_Data} and {\tt Heaps\_Size} act as generic parameters to the {\tt Heaps}  `module'.  {\tt Heaps\_Data} determines the type of data which may be stored in the heap.  {\tt Heaps\_Size} determines the size of the heap; that is, it equals the maximum number of storage locations for variables of type {\tt Heaps\_Data}.\\
\\
\begin{vdm_al}
types
     
Heaps_Location :: 
     data: [Heaps_Data]
     allocated: bool
inv mk_Heaps_Location(d, a) == (a = true) <=> (d <> nil);
\end{vdm_al}
The invariant for {\tt Heaps\_Location} states that if the location is allocated, then the {\tt data} must contain something other than {\tt nil}\footnote{In VDM-SL, {\tt [Type]} is equivalent to {\tt Type | nil} which means that a variable of that type may have a value in {\tt Type} or may be {\tt nil}.}.  On the other hand, if the location is unallocated then the {\tt data} field must be equal to
{\tt nil}.\\
\\
\begin{vdm_al}

Heaps_Heap ::
     storage: seq of Heaps_Location
inv mk_Heaps_Heap(s) == len s = Heaps_Size
\end{vdm_al}
This invariant states that the sequence of locations must contain {\tt Heaps\_Size} locations.\\
\\
\begin{vdm_al}

functions
     
Heaps_InitSequence: nat1 -> seq of Heaps_Location
Heaps_InitSequence(length) == 
     if length > 1 then
          [mk_Heaps_Location(nil, false)]
               ^Heaps_InitSequence(length - 1)
     else
          [mk_Heaps_Location(nil, false)];
\end{vdm_al}
{\tt Heaps\_InitSequence} returns a sequence containing {\tt length} locations.  It is used by {\tt Heaps\_Init} to initialize the system heap.
\\
\begin{vdm_al}

Heaps_Init: () -> Heaps_Heap
Heaps_Init() == mk_Heaps_Heap(Heaps_InitSequence(Heaps_Size));

Heaps_AmountUsed: Heaps_Heap -> nat
Heaps_AmountUsed(heap) ==
     let store = heap.storage in
          len [store(i) | i in set inds store 
               & store(i).allocated = true];
\end{vdm_al}
{\tt Heaps\_AmountUsed} returns the number of locations allocated in the system heap.  The maximum this function can return is {\tt Heaps\_Size}.
\\
\begin{vdm_al}

Heaps_Available: Heaps_Heap -> bool
Heaps_Available(heap) == 
     Heaps_AmountUsed(heap) < len heap.storage;
\end{vdm_al}
{\tt Heaps\_Available} returns true if the heap contains unallocated locations and false otherwise.
\\
\begin{vdm_al}

Heaps_ModifyLoc: Heaps_Heap * ADDRESS * Heaps_Location -> Heaps_Heap
Heaps_ModifyLoc(heap, address, location) ==
     mk_Heaps_Heap(heap.storage++{address|->location})
pre address in set inds heap.storage;
\end{vdm_al}
{\tt Heaps\_ModifyLoc} modifies the location in the heap corresponding to the {\tt address} with new  {\tt location} value.  
\\
\begin{vdm_al}
     
Heaps_Modify: Heaps_Heap * ADDRESS * Heaps_Data -> Heaps_Heap
Heaps_Modify(heap, address, data) ==

     Heaps_ModifyLoc(heap, address, mk_Heaps_Location(data, true))
pre let store = heap.storage in
     address in set inds store
     and store(address).allocated = true;
\end{vdm_al}
{\tt Heaps\_Modify} modifies the {\tt data} field of the allocated location in the heap corresponding to {\tt address}. 
\\
\begin{vdm_al}

Heaps_Retrieve: Heaps_Heap * ADDRESS -> [Heaps_Data]
Heaps_Retrieve(heap, address) ==
     heap.storage(address).data
pre 
     let store = heap.storage in
          address in set inds store     
          and store(address).allocated = true;
\end{vdm_al}
{\tt Heaps\_Retrieve} retreives the {\tt data} field of the allocated location in the heap corresponding to {\tt address}.
\\
\begin{vdm_al}
     
Heaps_UnallocatedAddresses: Heaps_Heap -> set of ADDRESS
Heaps_UnallocatedAddresses(heap) ==
     let store = heap.storage in
          {i|i in set inds store & store(i).allocated = false};
\end{vdm_al}
{\tt Heaps\_UnallocatedAddresses} returns the set of addresses corresponding to unallocated locations.
\\
\begin{vdm_al}

Heaps_UnallocatedAddress: Heaps_Heap -> ADDRESS
Heaps_UnallocatedAddress(heap) ==
     iota new in set Heaps_UnallocatedAddresses(heap) & 
          (forall i in set Heaps_UnallocatedAddresses(heap) & new <= i) 
pre Heaps_Available(heap); 
\end{vdm_al}
{\tt Heaps\_UnallocatedAddress} returns the lowest address in the set of unallocated addresses.

\subsection{Pointer Manipulation}

This section does not really correspond to a `module'.  The following operation specifications are simply meant to simulate pointer allocation, deallocation, data retrieval and data modification facilities that are available as part of the Modula-2 language.  All of the following operations assume that the specification contains a system heap and that its name is {\tt heap}. They also assume that the type {\tt Heap\_Data} is defined and is the type of the data being stored in the heap. \\
\\
\begin{vdm_al}

operations
     
NEW: Heaps_Data ==> ADDRESS
NEW(data) == 
(    def newAddress = Heaps_UnallocatedAddress(heap) in 
	 def newLoc = mk_Heaps_Location(data, true) in
     (    heap := Heaps_ModifyLoc(heap, newAddress, newLoc);
          return newAddress;
     )
)
pre Heaps_Available(heap);    
\end{vdm_al}
The operation {\tt NEW} models the regular procedure in Modula-2 having the same name.  If the heap is not completely allocated then it allocates a new location and returns its address; otherwise, it returns {\tt NIL}.  {\tt NEW} does not exactly model its counterpart in Modula-2 because in Modula-2 a {\tt NEW} call would look like the following:\\
\hspace*{7mm}{\tt NEW(ptr);}\\
whereas in this specification it would be done as follows:\\
\hspace*{7mm}{\tt ptr := NEW(initializationData);}\\
In other words, this model of {\tt NEW} requires initialization data for the location and returns the allocated location.  In Modula-2, however, {\tt ptr} is simply a variable parameter modified by  {\tt NEW}.\\
\\
\begin{vdm_al}

DISPOSE: ADDRESS ==> ()
DISPOSE(address) == 
     heap := Heaps_ModifyLoc(heap, address, mk_Heaps_Location(nil, false))
pre pre_Heaps_ModifyLoc(heap, address, mk_Heaps_Location(nil, false));
\end{vdm_al}
{\tt DISPOSE} models the procedure in Modula-2 having the same name.  If {\tt address} matches an allocated location in the heap, then that location is deallocated.  Note that the pre-condition catches addresses that do not match. \\
\\
\begin{vdm_al}

SET_DATA: ADDRESS * Data ==> ()
SET_DATA(ptr, data) == 
     heap := Heaps_Modify(heap, ptr, 
               Nodes_SetData(Heaps_Retrieve(heap, ptr), data));

SET_NEXT: ADDRESS * ADDRESS ==> ()

SET_NEXT(ptr, next) == 
     heap := Heaps_Modify(heap, ptr, 
               Nodes_SetNext(Heaps_Retrieve(heap, ptr), next));

SET_LEFT: ADDRESS * ADDRESS ==> ()
SET_LEFT(ptr, left) ==
     heap := Heaps_Modify(heap, ptr, 
               Nodes_SetLeft(Heaps_Retrieve(heap, ptr), left));

SET_RIGHT: ADDRESS * ADDRESS ==> ()
SET_RIGHT(ptr, right) ==
     heap := Heaps_Modify(heap, ptr, 
               Nodes_SetRight(Heaps_Retrieve(heap, ptr), right));

SET_PREV: ADDRESS * ADDRESS ==> ()
SET_PREV(ptr, prev) == 
     heap := Heaps_Modify(heap, ptr, 
               Nodes_SetPrev(Heaps_Retrieve(heap, ptr), prev));  

SET_PARENT: ADDRESS * ADDRESS ==> ()
SET_PARENT(ptr, parent) ==
     heap := Heaps_Modify(heap, ptr, 
               Nodes_SetParent(Heaps_Retrieve(heap, ptr), parent));

DATA: ADDRESS ==> Data
DATA(ptr) ==
     return Nodes_GetData(Heaps_Retrieve(heap, ptr));
 
NEXT: ADDRESS ==> ADDRESS
NEXT(ptr) ==
     return Nodes_GetNext(Heaps_Retrieve(heap, ptr));

LEFT: ADDRESS ==> ADDRESS
LEFT(ptr) == 
     return Nodes_GetLeft(Heaps_Retrieve(heap, ptr));

RIGHT: ADDRESS ==> ADDRESS
RIGHT(ptr) ==
     return Nodes_GetRight(Heaps_Retrieve(heap, ptr));

PARENT: ADDRESS ==> ADDRESS
PARENT(ptr) ==
     return Nodes_GetParent(Heaps_Retrieve(heap, ptr));

\end{vdm_al}

The {\tt SET\_...} operations model dynamic memory modification for pointers to {\tt Nodes\_Node}.  For example, \\
\hspace*{7mm}{\tt SET\_DATA(ptr, modificationData);}\\
models the Modula-2 statement, \\
\hspace*{7mm}{\tt ptr\char94.data := modificationData;}\\
Likewise, the {\tt GET\_...} operations are meant to model Modula-2 pointer dereferencing and retrieval of a field.  For example, \\
\hspace*{7mm}{\tt data := GET\_DATA(ptr);}\\
models the Modula-2 statement, \\
\hspace*{7mm}{\tt data := ptr\char94.data;}.\\


\subsection{Singly-Linked List}
This section specifies a singly-linked list.  It is specified so as to allow an almost automatic translation into Modula-2 \footnote{The singly-linked list is completely translated into Generic Modula-2 in Appendix A.1}.  Its operations' post-conditions checking generally test two things: that the list operation and a corresponding sequence operation are equivalent, and that memory allocation and deallocation is performed correctly.  The first test, equivalence, is achieved by translating the linked list before and after the list operation into two VDM-SL sequences, `old' and `new'.  The function {\tt SList\_Seq} performs this translation.  If the list and sequence operation are equivalent, then the result of the sequence operation on the `old' sequence should be equal to the `new' sequence.  The second test concerning memory allocation is found in the pre-conditions and post-conditions of all operations that use dynamic memory.  If an operation will attempt to allocate memory!
, th
en the pre-condition checks that memory is available using the {\tt Heaps\_Available} function.  If an operation allocates or deallocates memory, then the post-condition checks that it is done correctly.  For example, if one node was to have been deallocated then the heap should have one more location available for allocation.  The function {\tt Heaps\_AmountUsed} is used to check this.\\
\\
The following data types are exported:
\begin{itemize}
\item{\tt SList\_List}
\item{\tt SList\_Data}
\end{itemize}
The following functions are exported:      
\begin{itemize}
\item{\tt SList\_Init}
\item{\tt SList\_Seq}
\end{itemize}
The following operations are exported:
\begin{itemize}
\item{\tt SList\_Insert}
\item{\tt SList\_Delete}
\item{\tt SList\_Update}
\item{\tt SList\_Append}
\item{\tt SList\_Empty}
\item{\tt SList\_Element}
\item{\tt SList\_Length}
\item{\tt SList\_Traverse}
\end{itemize}  

\begin{vdm_al}
types SList_Data = Data;
\end{vdm_al}
{\tt SList\_Data} acts a generic parameter for the {\tt SList} `module'.    It determines the type of the elements found in the list.\\
\begin{vdm_al}

types SList_List = Nodes_NodePtr;

functions

SList_IsEmpty: SList_List -> bool
SList_IsEmpty(list) == list = NIL;

SList_Seq: Heaps_Heap * SList_List -> seq of SList_Data
SList_Seq(heap, list) == 
     if not SList_IsEmpty(list) then    
          let node = Heaps_Retrieve(heap, list) in
          let data = Nodes_GetData(node) in
          let tail = Nodes_GetNext(node) in 
               [data]^SList_Seq(heap, tail)
     else
          [];  
\end{vdm_al}
{\tt SList\_Seq} translates a {\tt SList\_List} into a sequence of {\tt SList\_Data} elements.  This is the function that appears in the post conditions of the list operations in order to compare the results of these operations with corresponding sequence operations \footnote{Sequence operations are built into {\tt VDM-SL}.}. \\
\begin{vdm_al}

SList_Lengthf: Heaps_Heap * SList_List -> nat
SList_Lengthf(heap, list) == 
     if not SList_IsEmpty(list) then
          let tail = Nodes_GetNext(Heaps_Retrieve(heap, list)) in 
               1 + SList_Lengthf(heap, tail)
     else
          0
post RESULT = len SList_Seq(heap, list);
\end{vdm_al}
{\tt SList\_Lengthf} returns the number of elements in the list.  The `f' differentiates this {\em function} from the length {\em operation} defined later.\\
\\
\begin{vdm_al}

SList_PtrToNode: Heaps_Heap * SList_List * nat1 -> Nodes_NodePtr

SList_PtrToNode(heap, list, position) ==
     let tail = Nodes_GetNext(Heaps_Retrieve(heap, list)) in
          if position > 1 then 
               SList_PtrToNode(heap, tail, position - 1) 
          else list
pre position <= SList_Lengthf(heap, list)
post 
     let data = Nodes_GetData(Heaps_Retrieve(heap, RESULT)), 
          listSeq = SList_Seq(heap, list) in
               data = listSeq(position);
\end{vdm_al}
{\tt SList\_PtrToNode} returns the pointer to the element at position {\tt position} in the list.
\\
\begin{vdm_al}

SList_Init: () -> SList_List
SList_Init() == NIL;
\end{vdm_al}
{\tt SList\_Init} returns an empty list.
\\
\begin{vdm_al}

operations

SList_InsertAtBeginning: SList_List * SList_Data ==> SList_List
SList_InsertAtBeginning(list, data) ==
     return NEW(Nodes_MkSingleLink(data, list))
post [data]^SList_Seq(heap~, list) = SList_Seq(heap, RESULT);
\end{vdm_al}
{\tt SList\_InsertAtBeginning} returns the list with {\tt data} as the first element.\\
\\
\begin{vdm_al}

SList_InsertAfter: SList_List * Nodes_NodePtr * SList_Data ==> SList_List
SList_InsertAfter(list, ptr, data) ==
(    dcl new: Nodes_NodePtr := NEW(Nodes_MkSingleLink(data, NEXT(ptr)));
     SET_NEXT(ptr, new);
     return list;
)
post 
     let old = SList_Seq(heap~, ptr) in
          [old(1)]^[data]^tl old = SList_Seq(heap, ptr);
\end{vdm_al}
{\tt SList\_InsertAfter} returns the list with {\tt data} inserted after the element corresponding to {\tt ptr}.
\\

\begin{vdm_al}

SList_Insert: SList_List * SList_Data * nat1 ==> SList_List
SList_Insert(list, data, position) ==
     if position = 1 then 
          return SList_InsertAtBeginning(list, data)
     else
          return SList_InsertAfter(list, 
               SList_PtrToNode(heap, list, position - 1), data)
pre position <= SList_Lengthf(heap, list) + 1 and Heaps_Available(heap)
post let new = SList_Seq(heap, RESULT) in
     SList_Seq(heap~, list) 
          = [new(i) | i in set inds new & i <> position]
     and new(position) = data
     and Heaps_AmountUsed(heap~) + 1 = Heaps_AmountUsed(heap);
\end{vdm_al}
{\tt SList\_Insert} returns the list with  {\tt data} at position {\tt position}.
\\
\begin{vdm_al}
     
SList_Append: SList_List * SList_Data ==> SList_List
SList_Append(list, data) ==
(    dcl ptr: Nodes_NodePtr := list;
     if ptr = NIL then
          return SList_InsertAtBeginning(list, data)
     else
     (    while (NEXT(ptr) <> NIL) do ptr := NEXT(ptr);
          return SList_InsertAfter(list, ptr, data);
     )
 )     
pre Heaps_Available(heap)
post SList_Seq(heap~, list)^[data] = SList_Seq(heap, RESULT)
     and Heaps_AmountUsed(heap~) + 1 = Heaps_AmountUsed(heap);
\end{vdm_al}
{\tt SList\_Append} returns the list with {\tt data} as its last element.
\\
\begin{vdm_al}

SList_Update: SList_List * SList_Data * nat1 ==> SList_List
SList_Update(list, data, position) ==
(    dcl ptr: Nodes_NodePtr := SList_PtrToNode(heap, list, position);
     SET_DATA(ptr, data);
     return list; 
)
pre position <= SList_Lengthf(heap, list)
post SList_Seq(heap~, list)++{position |-> data} = SList_Seq(heap, RESULT)

     and Heaps_AmountUsed(heap~) = Heaps_AmountUsed(heap);
\end{vdm_al}
{\tt SList\_Update} returns the list with {\tt data} replacing the previous element at position {\tt position}. \\
\begin{vdm_al}

SList_DeleteAtBeginning: SList_List ==> SList_List
SList_DeleteAtBeginning(list) ==
(    dcl  temp: Nodes_NodePtr := list,
          newlist: SList_List := NEXT(list);
     DISPOSE(temp);
     return newlist;
)
post tl SList_Seq(heap~, list) = SList_Seq(heap, RESULT);
\end{vdm_al}
{\tt SList\_DeleteAtBeginning} returns the list without the first element.\\
\begin{vdm_al}

SList_DeleteAfter: SList_List * Nodes_NodePtr ==> SList_List
SList_DeleteAfter(list, ptr) ==
(    dcl temp: Nodes_NodePtr;
     temp := NEXT(ptr);
     SET_NEXT(ptr, NEXT(temp));
     DISPOSE(temp);
     return list;
)
post let old = SList_Seq(heap~, ptr) in
     [old(1)]^tl (tl old) = SList_Seq(heap, ptr);
\end{vdm_al}
{\tt SList\_DeleteAfter} returns the list without the element pointed to by {\tt ptr}.\\
\begin{vdm_al}

SList_Delete: SList_List * nat1 ==> SList_List
SList_Delete(list, position) ==
(    if position = 1 then return SList_DeleteAtBeginning(list)
     else 
          return SList_DeleteAfter(list, 
                    SList_PtrToNode(heap, list, position - 1))
)
pre position <= SList_Lengthf(heap, list)
post let old = SList_Seq(heap~, list) in
     [old(i) | i in set inds old & i <> position] 
          = SList_Seq(heap, RESULT)

     and Heaps_AmountUsed(heap~) = Heaps_AmountUsed(heap) + 1;
\end{vdm_al}
{\tt SList\_Delete} returns the list without the element at position {\tt position}.
\\
\begin{vdm_al}

SList_Traverse:  SList_List * (SList_Data -> SList_Data) ==> SList_List
SList_Traverse(list, traversal) ==
(    dcl ptr: Nodes_NodePtr := list;
     while (ptr <> NIL) do
     (    SET_DATA(ptr, traversal(DATA(ptr)));
          ptr := NEXT(ptr);
     );
     return list;
)
post 
     let old = SList_Seq(heap~, list) in
          old <> [] => [traversal(old(i)) | i in set inds old] 
               = SList_Seq(heap, RESULT);
\end{vdm_al}
{\tt SList\_Traverse} returns the list with {\tt traversal} applied to each element.
\\
\begin{vdm_al}

SList_Length: SList_List ==> nat
SList_Length(list) == return SList_Lengthf(heap, list); 

SList_Empty: SList_List ==> bool
SList_Empty(list) == return SList_IsEmpty(list);

SList_Element: SList_List * nat1 ==> SList_Data
SList_Element(list, position) == DATA(SList_PtrToNode(heap, list, position));
\end{vdm_al}
{\tt SList\_Element} returns the element at position {\tt position} in the list.

\subsection{Doubly Linked List}
This specification specifies a doubly-linked list that is based on the singly-linked list; thus, it modifies only the insert, delete and append operations.  In addition, the function, {\tt DList\_IsList}, is added to determine if a list is a valid doubly-linked list (see its specification for more details).  Comments are added in this section only if the specification differs significantly from the singly-linked list specification; otherwise, the comments there apply here as well.\\     
\\ 
The following data types are exported:
\begin{itemize}
\item{\tt DList\_List}
\item{\tt DList\_Data}
\end{itemize}
The following functions are exported:      
\begin{itemize}
\item{\tt DList\_Init}
\item{\tt DList\_IsList}
\end{itemize}
The following operations are exported:
\begin{itemize}
\item{\tt DList\_Insert}
\item{\tt DList\_Delete}
\item{\tt DList\_Update}
\item{\tt DList\_Append}
\item{\tt DList\_Empty}
\item{\tt DList\_Element}
\item{\tt DList\_Length}
\item{\tt DList\_Traverse}
\end{itemize} 

\begin{vdm_al}

types
DList_Data = Data;
\end{vdm_al}
{\tt DList\_Data} acts as the generic parameter for the `module' {\tt DList}.
\begin{vdm_al}

types
DList_List = Nodes_NodePtr;

functions
     
DList_LastNode: Heaps_Heap * DList_List -> Nodes_NodePtr
DList_LastNode(heap, list) ==
     let next = Nodes_GetNext(Heaps_Retrieve(heap, list)) in
          if next <> NIL then
               DList_LastNode(heap, next)
          else list;
\end{vdm_al}
{\tt DList\_LastNode} returns the pointer to the last element in the list.
\\
\begin{vdm_al}

DList_Forward: Heaps_Heap * DList_List -> seq of DList_Data
DList_Forward(heap, list) == SList_Seq(heap, list);
\end{vdm_al}

{\tt DList\_Forward} returns the sequence corresponding to the list by following the {\tt next} links in the list from the beginning to the end.
\\
\begin{vdm_al}

DList_Backward: Heaps_Heap * DList_List -> seq of DList_Data
DList_Backward(heap, list) ==
     if list <> NIL then
          let prev = Nodes_GetPrev(Heaps_Retrieve(heap, list)) in
          let data = Nodes_GetData(Heaps_Retrieve(heap, list)) in
               DList_Backward(heap, prev)^[data]
     else [];
\end{vdm_al}
{\tt DList\_Backward} returns the sequence corresponding to the list by following the {\tt prev} links in the list from the end to the beginning.
\\
\begin{vdm_al}
               
DList_IsList: Heaps_Heap * DList_List -> bool
DList_IsList(heap, list) == 
     if list <> NIL then
          DList_Forward(heap, list) 
               = DList_Backward(heap, DList_LastNode(heap, list))
     else true;
\end{vdm_al}
{\tt DList\_IsList} decides whether a list is valid on the basis that the sequence derived by moving from beginning to end must be similar to the sequence derived by moving from end to beginning.
\\
\begin{vdm_al}

DList_Init: () -> DList_List
DList_Init() == NIL;                         

operations

DList_InsertAtBeginning: DList_List * DList_Data ==> DList_List
DList_InsertAtBeginning(list, data) ==
(    dcl new: Nodes_NodePtr := NEW(Nodes_MkDoubleLink(data, list, NIL));
     if list <> NIL then SET_PREV(list, new);
     return new;
)
     post [data]^SList_Seq(heap~, list) = SList_Seq(heap, RESULT);

DList_InsertAfter: DList_List * Nodes_NodePtr * DList_Data ==> DList_List
DList_InsertAfter(list, ptr, data) ==
(    dcl new: Nodes_NodePtr := NEW(Nodes_MkDoubleLink(data, NEXT(ptr), ptr));
     if NEXT(ptr) <> NIL then SET_PREV(NEXT(ptr), new);

     SET_NEXT(ptr, new);
     return list;
)
post 
     let old = SList_Seq(heap~, ptr) in
          [old(1)]^[data]^tl old = SList_Seq(heap, ptr);

DList_Insert: DList_List * DList_Data * nat1 ==> DList_List
DList_Insert(list, data, position) ==
     if position = 1 then 
          return DList_InsertAtBeginning(list, data)
     else
          return DList_InsertAfter(list, 
                    SList_PtrToNode(heap, list, position - 1), data)
pre position <= SList_Lengthf(heap, list) + 1 and 
     Heaps_Available(heap) and DList_IsList(heap, list)
post DList_IsList(heap, RESULT) and
     let new = SList_Seq(heap, RESULT) in
          SList_Seq(heap~, list) 
               = [new(i) | i in set inds new & i <> position]
          and new(position) = data;          

DList_DeleteAtBeginning: DList_List ==> DList_List
DList_DeleteAtBeginning(list) ==
(    dcl  temp: Nodes_NodePtr := list, 
          newlist: DList_List := NEXT(list);
        if newlist <> NIL then SET_PREV(newlist, NIL);
     DISPOSE(temp);
     return newlist;
)
post tl SList_Seq(heap~, list) = SList_Seq(heap, RESULT);

DList_DeleteAfter: DList_List * Nodes_NodePtr ==> DList_List
DList_DeleteAfter(list, ptr) ==
(    dcl temp: Nodes_NodePtr, nextPtr: Nodes_NodePtr;
     temp := NEXT(ptr);
        nextPtr := NEXT(temp);
     SET_NEXT(ptr, nextPtr);
     if nextPtr <> NIL then SET_PREV(nextPtr, ptr);
     DISPOSE(temp);
     return list;
)
post let old = SList_Seq(heap~, ptr) in
     [old(1)]^tl (tl old) = SList_Seq(heap, ptr);

DList_Delete: DList_List * nat1 ==> DList_List

DList_Delete(list, position) ==
(    if position = 1 then return DList_DeleteAtBeginning(list)
     else return DList_DeleteAfter(list, 
               SList_PtrToNode(heap, list, position - 1))
)
pre position <= SList_Lengthf(heap, list) and DList_IsList(heap, list)
post DList_IsList(heap, RESULT) and
     let old = SList_Seq(heap~, list) in
          [old(i) | i in set inds old & i <> position] 
               = SList_Seq(heap, RESULT)
          and Heaps_AmountUsed(heap~) = Heaps_AmountUsed(heap) + 1;

DList_Append: DList_List * DList_Data ==> DList_List
DList_Append(list, data) ==
(    dcl ptr: Nodes_NodePtr := list;
     if ptr = NIL then
          return DList_InsertAtBeginning(list, data)
     else
     (    while (NEXT(ptr) <> NIL) do ptr := NEXT(ptr);
          return DList_InsertAfter(list, ptr, data);
     )
)     
pre Heaps_Available(heap) and DList_IsList(heap, list)
post SList_Seq(heap~, list)^[data] = SList_Seq(heap, RESULT)
     and Heaps_AmountUsed(heap~) + 1 = Heaps_AmountUsed(heap)
     and DList_IsList(heap, RESULT);

DList_Empty: DList_List ==> bool
DList_Empty(list) == SList_Empty(list);

DList_Element: DList_List * nat1 ==> DList_Data
DList_Element(list, position) == SList_Element(list, position);

DList_Length: DList_List ==> nat
DList_Length(list) == SList_Length(list);

DList_Traverse: DList_List * (DList_Data -> DList_Data) ==> DList_List
DList_Traverse(list, traversal) == SList_Traverse(list, traversal);
     
DList_Update: DList_List * DList_Data * nat1 ==> DList_List
DList_Update(list, data, position) == SList_Update(list, data, position);
\end{vdm_al}

\subsection{Queues}

This queue specification is based on the singly-linked list.  Note the different style of specification from the singly and doubly linked list.  Specifically note that the preconditions are if-statements inside the functions and if a pre-condition fails the function returns `error'.   \\
\\
This `module' exports everything it defines.
\\
\begin{vdm_al}
types Queues_Data = Data;
\end{vdm_al}
{\tt Queues\_Data} acts as the generic parameter of the `module' {\tt Queues}.\\
\\
\begin{vdm_al}

types Queues_Queue = SList_List;

functions

Queues_Init: () -> Queues_Queue
Queues_Init() == SList_Init();

operations
     
Queues_Enqueue: Queues_Queue * Queues_Data ==> Queues_Queue
Queues_Enqueue(queue, data) == return SList_Append(queue, data);

Queues_Dequeue: Queues_Queue ==> Queues_Queue * Queues_Data
Queues_Dequeue(queue) ==
(    if not SList_Empty(queue) then
     (    dcl data: Queues_Data := Queues_Head(queue);
          return mk_(SList_Delete(queue, 1), data);    
     )
     else error;
);

Queues_Head: Queues_Queue ==> Queues_Data
Queues_Head(queue) == 
     if not SList_Empty(queue) then SList_Element(queue, 1)
     else error;


\end{vdm_al}

\subsection{Stacks}
Like the queue, the stack is also based on the singly-linked list.  \\
\\
This `module' exports everything it defines.
\\
\begin{vdm_al}
types Stacks_Data = Data;
\end{vdm_al}
{\tt Stacks\_Data} acts as the generic parameter of the `module' {\tt Stacks}.\\
\begin{vdm_al}

types Stacks_Stack = SList_List;

functions
Stacks_Init: () -> Stacks_Stack
Stacks_Init() == SList_Init();

operations
     
Stacks_Push: Stacks_Stack * Stacks_Data ==> Stacks_Stack
Stacks_Push(stack, data) == return SList_Insert(stack, data, 1);

Stacks_Top: Stacks_Stack ==> Stacks_Data
Stacks_Top(stack) == 
     if not SList_Empty(stack) then SList_Element(stack, 1)
     else error;

Stacks_Pop: Stacks_Stack ==> Stacks_Stack * Stacks_Data
Stacks_Pop(stack) == 
(    if not SList_Empty(stack) then
     (    dcl data: Stacks_Data := Stacks_Top(stack);
          return mk_(SList_Delete(stack, 1), data);
     )
     else error
);

\end{vdm_al}


\subsection{Binary Tree Based on Sets}
This binary tree will be used in the post-conditions of the binary tree in the next section.  This tree  is modelled as a set containing nodes where each node consists of a data field and a position field.  The node's {\tt position} field determines its position in the tree.  Position numbering begins with 1 at the root and then is incremented by 1 as one moves from left to right in a tree level.  At the end of a level, incrementing continues at the leftmost node in the next level.  A tree viewed in terms of  positions would appear as follows:\\
\begin{quote}
{\bf Tree Position Numbering}
\end{quote}
\begin{quote}
\begin{tabular}{|r|*{15}{c}|}
\hline
$Levels$&$Positions$&&&&&&&&&&&&&&\\
\hline
1&&&&&&&&1&&&&&&&\\
2&&&&2&&&&&&&&3&&&\\
3&&4&&&&5&&&&6&&&&7&\\
4&8&&9&&10&&11&&12&&13&&14&&15\\
n&...&&&&&&&&&&&&&&\\
\hline
\end{tabular}
\end{quote}
This scheme implies the following formulas for calculating various positions within the tree: 
\begin{itemize}
\item Given position $p$, the position of the right child is $2p + 1$ and the position of the left child is $2p$.  These formulas can be verified by referencing the table above.  Notice that a left child position is even and a right child position is odd (except for the root which is definitely not a child).
\item The parent position of $p$ is $\frac{p}{2}$.
\item Let $n$ be a level in the tree.  The leftmost node in that level has the position $2^n$.  The maximum number of nodes in level $n$ is $2^n$.  Therefore, the position of the rightmost node in that level is $2^n + 2^n - 1$ or $2^{n + 1} - 1$.   
\item The situation is somewhat more complex when calculating for a subtree rather than a tree.  Call the subtree $S$ and the position of the subtree root $r$.  Consider the positions in the subtree at a level that is $d$ levels below the subtree root.  The leftmost  position amongst these is $r2^d$.  The maximum number of positions is $2^d$;  therefore, the rightmost position is $r2^d + 2^d - 1$ or $(r + 1)2^d - 1$.
\item An obvious conclusion from the above formulas is that if $p$ is a position in $S$ then $\exists e \in {\bf N } \cdot r2^e \leq p \leq (r + 1)2^e - 1$.  If such an $e$ exists then it will denote the number of levels $p$ is below the root of $S$.  Another way to state this expression is $\exists e \in {\bf N } \cdot r2^e \leq p < (r + 1)2^e$.  Notice that $(r + 1)2^e$ is just the leftmost positions of the subtree whose root is at the position immediatly to the right of  the root of $S$.     
\end{itemize}
The following data types are exported:
\begin{itemize}
\item {\tt STrees\_Direction}
\item {\tt STrees\_Node}
\item {\tt STrees\_Tree}
\item {\tt STrees\_Info}
\item {\tt STrees\_Data}
\end{itemize}
The following functions are exported:
\begin{itemize}
\item {\tt STrees\_Delete}
\item {\tt STrees\_ExistsData}
\item {\tt STrees\_ExistsDirection}
\item {\tt STrees\_ExistsNode}
\item {\tt STrees\_GetCurrentData}
\item {\tt STrees\_GetCurrentNode}
\item {\tt STrees\_GetData}
\item {\tt STrees\_GetTree}
\item {\tt STrees\_Init}
\item {\tt STrees\_Insert}
\item {\tt STrees\_MkNode}
\item {\tt STrees\_MkTree}
\item {\tt STrees\_MkInfo}
\item {\tt STrees\_MoveInDir}
\item {\tt STrees\_MoveToAnscestor}
\item {\tt STrees\_MoveToNode}
\item {\tt STrees\_MoveToParent}
\item {\tt STrees\_SetCurrentNode}
\item {\tt STrees\_Size}
\item {\tt STrees\_StoreCurrentData}
\item {\tt STrees\_Traverse}
\end{itemize}
\begin{vdm_al}

types
STrees_Data = Data;
\end{vdm_al}
{\tt STrees\_Data} acts as the generic parameter of the `module' {\tt STrees}.  It is the data which will be inserted, deleted and updated. 
\\
\begin{vdm_al}

types 
STrees_Direction = <ToRoot> | <ToLeft> | <ToRight>
\end{vdm_al}
{\tt STrees\_Direction} is used to describe direction in the tree.  \textsc{ToRoot} refers to the root node, \textsc{ToLeft} refers to the left child of the `current node'  (`current node' is described later) and \textsc{ToRight} refers to the right child of the `current node'.   
\\
\begin{vdm_al}

types
STrees_Node ::
     data: STrees_Data
     position: nat1;
\end{vdm_al}
{\tt STrees\_Node} is the type of all of the nodes in the tree.  As the earlier discussion indicated, each node consists of a position and a data field.
\\
\begin{vdm_al}

STrees_Tree = set of STrees_Node
inv tree == 
     (forall node in set tree & 
          not STrees_IsRoot(tree, node) <=> STrees_IsChild(tree, node)
          and STrees_IsUnique(tree, node)) 
     and (tree <> {} <=> exists1 node in set tree & 
                                 STrees_IsRoot(tree, node));
\end{vdm_al}
{\tt STrees\_Tree} is the actual binary tree.  It is a set of nodes.  The invariant means that each node is either a child or the root but not both.  The only node not a child is the root; all other nodes must have a parent.   In addition, each node is unique with respect to position; that is, at most one node can occupy a single position.  Finally, if the tree is empty then there is no root; however, if it is not empty, then there is exactly one root.  Note that if the root's position is 1, then the second part of the invariant is unnecessary; however, if the tree is a subtree then the root's position is greater than 1 and, hence, it is possible for there to be multiple roots or no roots at all.  In that case, the second part of the invariant is necessary.    
\\
\begin{vdm_al}

STrees_Info ::
     tree: STrees_Tree
     current: [STrees_Node]
inv mk_STrees_Info(t, c) == 
     (c = nil <=> t = {}) and
     (c <> nil <=> (c in set t and let r = STrees_Root(t) in r.position = 1));
\end{vdm_al}
{\tt STrees\_Info} stores both the tree and the current node.  The current node is a node which enables the tree user to move around the tree.  Functions for this are specified later.  The invariant states that the current node has a node value if and only if the tree is non-empty.  The second part of the invariant states that, if and only  if the tree is not empty then, the current node is a node in the tree and the position of the root is 1.  Notice that this second part prevents the tree from being a subtree.\\
\\
\begin{vdm_al}

functions

STrees_GetTree: STrees_Info -> STrees_Tree

STrees_GetTree(mk_STrees_Info(tree, -)) == tree;
\end{vdm_al}
{\tt STrees\_GetTree} returns the set of all the nodes in the tree.
\\
\begin{vdm_al}
 
STrees_MkNode: STrees_Data * nat1 -> STrees_Node
STrees_MkNode(data, position) ==
     mk_STrees_Node(data, position);

STrees_MkTree: set of STrees_Node -> STrees_Tree
STrees_MkTree(tree) == tree
pre inv_STrees_Tree(tree);

STrees_MkInfo: STrees_Tree * STrees_Node -> STrees_Info
STrees_MkInfo(tree, current) ==
     mk_STrees_Info(tree, current)
pre inv_STrees_Info(mk_STrees_Info(tree, current));
\end{vdm_al}
The {\tt STrees\_Mk...} functions construct one of the binary tree data structures defined above.
\\
\begin{vdm_al}

STrees_Init: () -> STrees_Info
STrees_Init() == mk_STrees_Info({}, nil);
\end{vdm_al}
Initially a tree is empty.  Hence, this intialized tree is an empty set whose current node does not contain a node value.
\\
\begin{vdm_al}

STrees_IsRoot: set of STrees_Node * STrees_Node -> bool
STrees_IsRoot(tree, mk_STrees_Node(dr, pr)) == 
     not STrees_IsChild(tree, mk_STrees_Node(dr, pr))
pre mk_STrees_Node(dr, pr) in set tree;
\end{vdm_al}
{\tt STrees\_IsRoot} returns true if the input node is the root of the input tree or subtree.  Another way of saying `is a root' is to say `is not a child'.
\\
\begin{vdm_al}

STrees_IsParent: STrees_Tree * STrees_Node -> bool
STrees_IsParent(tree, node) ==
     exists child in set tree & STrees_IsParentOf(tree, node, child)
pre node in set tree;

STrees_IsChild: set of STrees_Node * STrees_Node -> bool
STrees_IsChild(tree, node) == 
     (exists parent in set tree & STrees_IsParentOf(tree, parent, node))
     and (exists1 parent in set tree & STrees_IsParentOf(tree, parent, node))
pre node in set tree;

STrees_IsUnique: set of STrees_Node * STrees_Node -> bool
STrees_IsUnique(tree, mk_STrees_Node(data, position)) ==
     (mk_STrees_Node(data, position) in set tree) and
     (exists1 node in set tree & node.position = position);
\end{vdm_al}
{\tt STrees\_IsUnique} returns true if there exists exactly one node at the node's position; that is, a set of nodes is unique if each has a unique position.  Recall that this function was used in the invariant of {\tt STrees\_Tree}.
\\
\begin{vdm_al}

STrees_IsParentOf: set of STrees_Node * STrees_Node * STrees_Node -> bool
STrees_IsParentOf(tree, node1, node2) == 
     STrees_IsRightChildOf(tree, node2, node1) 
          or STrees_IsLeftChildOf(tree, node2, node1)
pre node1 in set tree and node2 in set tree; 

STrees_IsRightChildOf: set of STrees_Node * STrees_Node * STrees_Node -> bool
STrees_IsRightChildOf(tree, node1, node2) ==
     let mk_STrees_Node(-, position1) = node1 in
     let mk_STrees_Node(-, position2) = node2 in
          (position1 = 2*position2 + 1)
pre node1 in set tree and node2 in set tree;

STrees_IsLeftChildOf: set of STrees_Node * STrees_Node * STrees_Node -> bool
STrees_IsLeftChildOf(tree, node1, node2) ==
     let mk_STrees_Node(-, position1) = node1 in
     let mk_STrees_Node(-, position2) = node2 in
          (position1 = 2*position2)
pre node1 in set tree and node2 in set tree;
\end{vdm_al}
The {\tt STrees\_Is...Of} returns true of the first node is the ... of the second node in the tree.
\\
\begin{vdm_al}

STrees_Insert: STrees_Info * STrees_Data * STrees_Direction -> STrees_Info
STrees_Insert(mk_STrees_Info(tree, current), data, direction) == 
     cases mk_(current, direction):
          mk_(nil, <ToRoot>)  -> STrees_InsertRoot(data),
          mk_(-, <ToLeft>)    -> STrees_InsertLeft(tree, current, data),

          mk_(-, <ToRight>)   -> STrees_InsertRight(tree, current, data)
     end
pre 
     (direction = <ToRoot> => tree = {}) and
     (direction = <ToLeft> => not STrees_HasLeftChild(tree, current)) and 
     (direction = <ToRight> => not STrees_HasRightChild(tree, current))
post inv_STrees_Info(RESULT);
\end{vdm_al}
{\tt STrees\_Insert} inserts a node containing the data into the tree.  Note that a node can be inserted as the root only if the tree is empty; otherwise, the insertion must be to the right or left of the tree's current node.  Following the insert, the current node becomes the node just inserted.
\\
\begin{vdm_al}

STrees_InsertRoot: STrees_Data -> STrees_Info
STrees_InsertRoot(data) == 
     let root = mk_STrees_Node(data, 1) in
          mk_STrees_Info({root}, root);

STrees_InsertLeft: STrees_Tree * STrees_Node * STrees_Data -> STrees_Info
STrees_InsertLeft(tree, current, data) ==
     let mk_STrees_Node(-, position) = current in 
     let new = mk_STrees_Node(data, 2*position) in
          mk_STrees_Info(tree union {new}, new)
pre not STrees_HasLeftChild(tree, current);

STrees_InsertRight: STrees_Tree * STrees_Node * STrees_Data -> STrees_Info
STrees_InsertRight(tree, current, data) ==
     let mk_STrees_Node(-, position) = current in 
     let new = mk_STrees_Node(data, 2*position + 1) in
          mk_STrees_Info(tree union {new}, new)
pre not STrees_HasRightChild(tree, current);

STrees_Traverse: STrees_Info * (STrees_Data -> STrees_Data) -> STrees_Info
STrees_Traverse(treeinfo, traversal) ==
     let mk_STrees_Info(tree, current) = treeinfo in
          if current <> nil then
               let mk_STrees_Node(data, position) = current in
               let newtree = {mk_STrees_Node(traversal(data), position) | 
                              mk_STrees_Node(data, position) in set tree} in
                    mk_STrees_Info(newtree, 
                         mk_STrees_Node(traversal(data), position))
          else
               treeinfo;
\end{vdm_al}
{\tt STrees\_Traverse} applies the function {\tt traversal} to the data of each node in the tree.\\
\begin{vdm_al}

STrees_MoveInDir: STrees_Info * STrees_Direction -> STrees_Info
STrees_MoveInDir(mk_STrees_Info(tree, current), direction) ==
     cases direction:
          <ToRoot>  -> mk_STrees_Info(tree, STrees_Root(tree)),
          <ToLeft>  -> mk_STrees_Info(tree, STrees_LeftChild(tree, current)),
          <ToRight> ->  mk_STrees_Info(tree, STrees_RightChild(tree, current))
 end
pre STrees_ExistsDirection(mk_STrees_Info(tree, current), direction);

\end{vdm_al}
{\tt STrees\_MoveInDir} moves the current node in the direction specified.  Movement can be to the root or to the right or left of the current node.  Note that there must be a node in the direction the current node is moving.\\
\begin{vdm_al}

STrees_MoveToNode: STrees_Info * nat1 -> STrees_Info
STrees_MoveToNode(mk_STrees_Info(tree, current), position) ==
     mk_STrees_Info(tree, STrees_GetNode(tree, position))
pre STrees_ExistsNode(mk_STrees_Info(tree, current), position);
\end{vdm_al}
{\tt STrees\_MoveToNode} moves the current node to the node at position {\tt position}.  Of course, the node at the position must already exist in the tree.
\\
\begin{vdm_al}

STrees_MoveToParent: STrees_Info -> STrees_Info
STrees_MoveToParent(mk_STrees_Info(tree, current)) ==
     mk_STrees_Info(tree, STrees_Parent(tree, current))
pre not STrees_IsRoot(tree, current);
\end{vdm_al}
{\tt STrees\_MoveToParent} moves the current node to the parent of the current node.  Of course, the current node must not already be at the root.
\\
\begin{vdm_al}

STrees_MoveToAnscestor: STrees_Info * nat1 -> STrees_Info 
STrees_MoveToAnscestor(treeinfo, pathlength) ==
  if pathlength > 1 
  then STrees_MoveToAnscestor(STrees_MoveToParent(treeinfo), pathlength - 1)
  else STrees_MoveToParent(treeinfo)
pre pre_STrees_MoveToParent(treeinfo);

\end{vdm_al}
{\tt STrees\_MoveToAnscestor} basically calls {\tt MoveToParent} {\tt pathlength} times.
\\
\begin{vdm_al}

STrees_Root: STrees_Tree -> STrees_Node
STrees_Root(tree) ==
  iota root in set tree & STrees_IsRoot(tree, root)
pre tree <> {};

STrees_Parent: STrees_Tree * STrees_Node -> STrees_Node
STrees_Parent(tree, node) == 
  iota parent in set tree 
       & STrees_IsParentOf(tree, parent, node)
pre node in set tree and not STrees_IsRoot(tree, node);

STrees_LeftChild: STrees_Tree * STrees_Node -> STrees_Node
STrees_LeftChild(tree, parent) ==
  iota leftchild in set tree & 
       STrees_IsLeftChildOf(tree, leftchild, parent)
pre parent in set tree and STrees_HasLeftChild(tree, parent);

STrees_RightChild: STrees_Tree * STrees_Node -> STrees_Node
STrees_RightChild(tree, parent) ==
  iota rightchild in set tree & 
       STrees_IsRightChildOf(tree, rightchild, parent)
pre parent in set tree and STrees_HasRightChild(tree, parent); 

STrees_GetNode: STrees_Tree * nat1 -> STrees_Node
STrees_GetNode(tree, position) ==
  iota node in set tree & node.position = position
pre exists node in set tree & node.position = position; 
\end{vdm_al}
{\tt STrees\_GetNode} returns the node at position {\tt position} in the tree.
\\
\begin{vdm_al}

STrees_GetData: STrees_Info * nat1 -> STrees_Data
STrees_GetData(mk_STrees_Info(tree, current), position) ==
     let mk_STrees_Node(data, -) = STrees_GetNode(tree, position) in 
          data
pre STrees_ExistsNode(mk_STrees_Info(tree, current), position);
\end{vdm_al}
{\tt STrees\_GetData} returns the data stored in the node at the {\tt position}.
\\
\begin{vdm_al}

STrees_StoreCurrentData: STrees_Info * STrees_Data -> STrees_Info

STrees_StoreCurrentData(mk_STrees_Info(tree, current), data) ==
     let mk_STrees_Node(-, position) = current in
     let newcurrent = mk_STrees_Node(data, position) in 
          mk_STrees_Info((tree\{current}) union {newcurrent}, newcurrent)
pre current <> nil;
\end{vdm_al}
{\tt STrees\_StoreCurrentData} updates the data stored by the current node.
\\
\begin{vdm_al}

STrees_GetCurrentData: STrees_Info -> STrees_Data
STrees_GetCurrentData(mk_STrees_Info(tree, mk_STrees_Node(data, -))) == data
pre tree <> {};
\end{vdm_al}
{\tt STrees\_GetCurrentData} retrieves the data stored by the current node. 
\\
\begin{vdm_al}

STrees_Size: STrees_Info -> nat
STrees_Size(mk_STrees_Info(tree, -)) == card tree;
\end{vdm_al}
{\tt STrees\_Size} returns the number of nodes in the tree.
\\
\begin{vdm_al}

STrees_GetCurrentNode: STrees_Info -> STrees_Node
STrees_GetCurrentNode(mk_STrees_Info(-, current)) == current;

STrees_SetCurrentNode: STrees_Info * STrees_Node -> STrees_Info
STrees_SetCurrentNode(mk_STrees_Info(tree, -), newcurrent) == 
     mk_STrees_Info(tree, newcurrent)
pre newcurrent in set tree;
\end{vdm_al}
The {\tt STrees\_...CurrentNode} functions return the current node and supply a new current node respectively.  "Setting the current node" means to move the current node to a another node in the tree.
\\
\begin{vdm_al}

STrees_HasLeftChild: STrees_Tree * STrees_Node -> bool
STrees_HasLeftChild(tree, parent) ==
     exists1 child in set tree 
          & STrees_IsLeftChildOf(tree, child, parent)
pre parent in set tree;

STrees_HasRightChild: STrees_Tree * STrees_Node -> bool
STrees_HasRightChild(tree, parent) ==

     exists1 child in set tree
          & STrees_IsRightChildOf(tree, child, parent)
pre parent in set tree;
\end{vdm_al}
The {\tt STrees\_Has...Child} functions return true if the given node has the corresponding child (left or right child) in the tree; otherwise, they return false.  
\\
\begin{vdm_al}

STrees_InOrderPredecessor: STrees_Tree * STrees_Node -> STrees_Node
STrees_InOrderPredecessor(tree, node) ==
     let leftchild = STrees_LeftChild(tree, node) in
     let left = STrees_Subtree(tree, leftchild) in 
     let rightpath = {n | n in set left 
             & (exists p in set {0, ..., card left} 
               & n.position 
			= (leftchild.position + 1)*2**p - 1)} in
          iota pred in set rightpath 
                    & (forall n in set rightpath & n.position <= pred.position)
pre node in set tree and STrees_HasLeftChild(tree, node);
\end{vdm_al}
{\tt STrees\_InOrderPredecessor} returns the in-order predecessor of the input node.  In other words, it returns the rightmost node in the left subtree of the input node.  The method by which the predecessor position is found is described at the beginning of this section.  From that description, notice that $p$, when it is found, will indicate the number of levels the node $pred$ is below the node $leftchild$.  Also recall that in the earlier discussion $p$ was said to be a natural number.  Here, however, $p$ is said only to be in the set $\{0, ..., card\ left\}$.  The reason for this difference is that the toolbox interpreter cannot search infinite sets such as the natural numbers; hence, we must supply a finite set.  This finite set is sufficient because the number of levels in any subtree is less than or equal to the number of elements in the subtree.  Thus, necessarily $p \leq card\ left$.
\\
\begin{vdm_al}

STrees_Delete: STrees_Info -> STrees_Info
STrees_Delete(mk_STrees_Info(tree, current)) ==
  let old = STrees_Subtree(tree, current) 
  in
   if STrees_HasRightChild(tree, current) and 
      STrees_HasLeftChild(tree, current) 
   then let leftchild = STrees_LeftChild(tree, current),
            rightchild = STrees_RightChild(tree, current),
            left = STrees_Subtree(old, leftchild), 
            mk_STrees_Node(-, position) = 
                STrees_InOrderPredecessor(old, current), 
            newright = STrees_MoveSubtree(old, rightchild, 
                                          2*position + 1), 
            newleft = left union newright,
            new = STrees_MoveSubtree(newleft, STrees_Root(newleft), 
                                     current.position) 
        in
          mk_STrees_Info((tree \ old) union new, STrees_Root(new))
   elseif STrees_HasLeftChild(tree, current) 
   then let leftchild = STrees_LeftChild(tree, current),
            new = STrees_MoveSubtree(old, leftchild, current.position) 
        in
          mk_STrees_Info((tree \ old) union new, STrees_Root(new)) 
   elseif STrees_HasRightChild(tree, current) 
   then let rightchild = STrees_RightChild(tree, current),
            new = STrees_MoveSubtree(old, rightchild, current.position)
        in
          mk_STrees_Info((tree \ old) union new, STrees_Root(new))

     else 
          mk_STrees_Info(tree \ {current}, STrees_Parent(tree, current))
pre current <> nil
post inv_STrees_Info(RESULT);
\end{vdm_al}
{\tt Delete} removes the current node from the tree.  Removal can be viewed in four different cases.  The first case is when the current node has both children.  In that case the current node is removed and the left child takes its place.  The right child becomes the right child of the current node's in-order predecessor.  The second case is when the current node has only the left child.  In that case, the current node is replaced by the left child.  The third case is when the current node has only the right child.  In that case, the current node is replaced by the right child.  The last case is when the current node has no children in which case the current node is removed and the new current node becomes its parent. Note that when the left child is moved, the entire left subtree must be moved.  The same is true for the right child.
\\
\begin{vdm_al}

STrees_Subtree: STrees_Tree * STrees_Node -> STrees_Tree
STrees_Subtree(tree, mk_STrees_Node(rootdata, rootpos)) ==
     {mk_STrees_Node(d, p) | mk_STrees_Node(d, p) in set tree 
          & (exists1 n in set {0, ..., card tree} 
               & p >= rootpos*2**n and p < (rootpos + 1)*2**n)}
pre mk_STrees_Node(rootdata, rootpos) in set tree;
\end{vdm_al}
{\tt STrees\_Subtree} returns the subtree of the input node.  To understand the formulas, refer to the beginning of this section.  To understand why $n$ is said be in the set $\{0, ..., card\ tree\}$ refer to previous discussion for {\tt STrees\_InOrderPredecessor}.
\\
\begin{vdm_al}

STrees_MoveSubtree:  STrees_Tree * STrees_Node * nat1 -> STrees_Tree
STrees_MoveSubtree(tree, subtreeRoot, newRootPos) ==
     let subtree = STrees_Subtree(tree, subtreeRoot), 
               mk_STrees_Node(-, oldRootPos) = subtreeRoot in
          {STrees_MoveNode(tree, node, oldRootPos, newRootPos) 
          | node in set subtree} 
pre subtreeRoot in set tree;
\end{vdm_al}
{\tt STrees\_MoveSubtree} moves the subtree found in {\tt tree} whose root is {\tt subtreeRoot}.  The subtree is moved so that the new position of its root is {\tt newRootPos}.  \\
NOTE:  This function is specified specifically to be used in {\tt STrees\_Delete}.  In the interests of simplicity in {\tt STrees\_Delete}, this function does not return a modified {\tt tree}.  Instead, it returns \emph{only} the transplanted subtree.
\\
\begin{vdm_al}

STrees_MoveNode: STrees_Tree * STrees_Node * nat1 * nat1 -> STrees_Node
STrees_MoveNode(tree, mk_STrees_Node(d, p), oldRootPos, newRootPos) ==

     let n = (iota n in set {0, ..., card tree} 
               & p >= oldRootPos*2**n and p < (oldRootPos + 1)*2**n) in
          mk_STrees_Node(d, newRootPos*2**n + p - oldRootPos*2**n);
\end{vdm_al}
{\tt STrees\_MoveNode} moves the input node in such a way that its new position has the same relationship to {\tt newRootPos} as its old position had to {\tt oldRootPos}.  To understand why $n$ is said to be in the finite set $\{0, ..., card\ tree\}$ refer to the previous discussion about {\tt STrees\_InOrderPredecessor}.
\\
\begin{vdm_al}

STrees_ExistsData: STrees_Info * STrees_Data -> bool
STrees_ExistsData(mk_STrees_Info(tree, -), data) ==
     exists node in set tree & node.data = data;
\end{vdm_al}
{\tt STrees\_ExistsData} returns true if there is a node in the tree which stores {\tt data}; otherwise it returns false. 
\\
\begin{vdm_al}

STrees_ExistsNode: STrees_Info * nat1 -> bool
STrees_ExistsNode(mk_STrees_Info(tree, -), position) ==
     exists node in set tree & node.position = position;
\end{vdm_al}
{\tt STrees\_ExistsNode} returns true if there is node at position {\tt position} in the tree; otherwise it returns false.
\\
\begin{vdm_al}

STrees_ExistsDirection: STrees_Info * STrees_Direction -> bool
STrees_ExistsDirection(mk_STrees_Info(tree, current), direction) ==
     cases direction: 
          <ToRoot>  -> (tree <> {}),
          <ToLeft>  -> 
               if current <> nil then 
                    STrees_HasLeftChild(tree, current) 
               else false,
          <ToRight> -> 
               if current <> nil then 
                    STrees_HasRightChild(tree, current) 
               else false
     end;
\end{vdm_al}
{\tt STrees\_ExistsDirection} returns true if a node exists in the indicated direction.\\

\subsection{Binary Tree Based on Links}

This section specifies a binary tree much like one would in Modula-2.   In other words, in contrast to the other binary tree, this tree uses dynamic links and memory allocation and deallocation.  Thus, just as the singly and doubly linked list operations contained checking for correct memory allocation and deallocation in their post-conditions, so do the operations in this section.  The purpose for the first binary tree specification is to provide additional post-condition checking for the operations in this section.  Similar to the way the lists were translated into sequences, the binary tree in this section will be translated into the binary tree based on sets and the results compared. \\

The following data types are exported:
\begin{itemize}
\item {\tt Trees\_Direction}
\item {\tt Trees\_Tree}
\item {\tt Trees\_Data}
\end{itemize}
The following functions are exported:
\begin{itemize}
\item {\tt Trees\_Set}
\item {\tt Trees\_Init}
\end{itemize}
The following operations are exported:
\begin{itemize}
\item {\tt Trees\_Delete}
\item {\tt Trees\_ExistsData}
\item {\tt Trees\_ExistsDirection}
\item {\tt Trees\_GetCurrentData}
\item {\tt Trees\_Insert}
\item {\tt Trees\_MoveInDir}
\item {\tt Trees\_MoveToParent}
\item {\tt Trees\_Size}
\item {\tt Trees\_StoreCurrentData}
\item {\tt Trees\_Traverse}
\end{itemize}
\begin{vdm_al}
types Trees_Data = Data
\end{vdm_al}
{\tt Trees\_Data} acts as the generic parameter for `module' {\tt Trees}.  
\\
\begin{vdm_al}

types
Trees_Direction = STrees_Direction;

Trees_Tree ::
     treePtr: Nodes_NodePtr
     current: Nodes_NodePtr
\end{vdm_al}
{\tt Trees\_Tree} consists of a pointer to the root, {\tt treePtr}, and a pointer to the current node, {\tt current}.
\\
\begin{vdm_al}


functions
Trees_Position: Heaps_Heap * Nodes_NodePtr -> nat1
Trees_Position(heap, child) == 
     let parent = Nodes_GetParent(Heaps_Retrieve(heap, child)) in
          if parent = NIL then
               1
          elseif Trees_IsRightChildOf(heap, child, parent) then
               2*Trees_Position(heap, parent) + 1
          else 
               2*Trees_Position(heap, parent)
pre child <> NIL; 
\end{vdm_al}
{\tt Trees\_Position} returns the position of the node pointed to by {\tt child}.  This is necessary for translating the `linked tree' to the `set tree'.
\\
\begin{vdm_al}

Trees_Set: Heaps_Heap * Trees_Tree -> STrees_Info
Trees_Set(heap, mk_Trees_Tree(treePtr, current)) == 

     if treePtr <> NIL then
          let treeset = STrees_MkTree(Trees_SubtreeToSet(heap, treePtr, 1)) in
          let data = Nodes_GetData(Heaps_Retrieve(heap, current)) in
          let position = Trees_Position(heap, current) in
          let currentnode = STrees_MkNode(data, position) in
          STrees_MkInfo(treeset, currentnode)
     else STrees_Init();
\end{vdm_al}
{\tt Trees\_Set} translates the `linked tree' to the `set tree' to aid in post-condition checking later on.  It begins by creating the set of nodes using {\tt Trees\_SubtreeToSet} and then constructing the current node from the current pointer.  It finishes by returning a `set tree'. \\
\\
The argument `1' in the call to {\tt Trees\_SubtreeToSet} at the beginning of {\tt Trees\_Set} means that the root position is `1'.  {\tt Trees\_SubtreeToSet} uses this number to construct all of the other node positions.
\\
\begin{vdm_al}

Trees_SubtreeToSet: Heaps_Heap * Nodes_NodePtr * nat1 -> set of STrees_Node
Trees_SubtreeToSet(heap, subtree, position) ==
  if subtree <> NIL 
  then {STrees_MkNode(Nodes_GetData(Heaps_Retrieve(heap, subtree)), position)}
       union Trees_SubtreeToSet(heap, 
       Nodes_GetLeft(
			Heaps_Retrieve(heap, subtree)), 2*position)
       union Trees_SubtreeToSet(heap, 
               Nodes_GetRight(
		          Heaps_Retrieve(heap, subtree)), 2*position + 1)
     else {};
\end{vdm_al}
{\tt Trees\_SubtreeToSet} creates a set of nodes from the input subtree.  It does this recursively first by adding the subtree root to the set and then by adding the left and right subtrees to the set.  The input value {\tt position} refers to the position of the subtree root.  Thus the positions of its children are $2position$ and $2position + 1$ respectively. 
\\
\begin{vdm_al}

Trees_HasLeftChild: Heaps_Heap * Nodes_NodePtr -> bool
Trees_HasLeftChild(heap, ptr) ==
     if ptr <> NIL then 
          Nodes_GetLeft(Heaps_Retrieve(heap, ptr)) <> NIL
     else false
pre ptr <> NIL => pre_Heaps_Retrieve(heap, ptr);

Trees_HasRightChild: Heaps_Heap * Nodes_NodePtr -> bool
Trees_HasRightChild(heap, ptr) ==
     if ptr <> NIL then
          Nodes_GetRight(Heaps_Retrieve(heap, ptr)) <> NIL
     else false

pre ptr <> NIL => pre_Heaps_Retrieve(heap, ptr);

Trees_IsRightChildOf: Heaps_Heap * Nodes_NodePtr * Nodes_NodePtr -> bool
Trees_IsRightChildOf(heap, child, parent) ==
     child = Nodes_GetRight(Heaps_Retrieve(heap, parent))
     and parent = Nodes_GetParent(Heaps_Retrieve(heap, child))
pre pre_Heaps_Retrieve(heap, parent) 
	and pre_Heaps_Retrieve(heap, child);

Trees_IsLeftChildOf: Heaps_Heap * Nodes_NodePtr * Nodes_NodePtr -> bool
Trees_IsLeftChildOf(heap, child, parent) ==
     child = Nodes_GetLeft(Heaps_Retrieve(heap, parent))
     and parent = Nodes_GetParent(Heaps_Retrieve(heap, child))
pre pre_Heaps_Retrieve(heap, parent) 
	and pre_Heaps_Retrieve(heap, child);

Trees_IsRoot: Heaps_Heap * Nodes_NodePtr -> bool
Trees_IsRoot(heap, ptr) == Nodes_GetParent(Heaps_Retrieve(heap, ptr)) = NIL
pre ptr <> NIL;

Trees_Init: () -> Trees_Tree
Trees_Init() == mk_Trees_Tree(NIL, NIL);

operations
Trees_Insert: Trees_Tree * Trees_Data * Trees_Direction ==> Trees_Tree
Trees_Insert(tree, data, direction) ==
 cases direction: 
   <ToRoot>  -> return  Trees_InsertRoot(data), 
   <ToLeft>  -> return  Trees_InsertLeft(tree, data),
   <ToRight> -> return  Trees_InsertRight(tree, data),
   others -> error
end
pre Heaps_Available(heap) and
    let mk_Trees_Tree(treePtr, current) = tree 
    in 
      (direction = <ToRoot> => treePtr = NIL) and
      (direction = <ToRight> => not Trees_HasRightChild(heap, current)) and
      (direction = <ToLeft> => not Trees_HasLeftChild(heap, current))
post Heaps_AmountUsed(heap~) + 1 = Heaps_AmountUsed(heap) and 
     let old = Trees_Set(heap~, tree) in
     	STrees_Insert(old, data, direction) = Trees_Set(heap, RESULT);

Trees_InsertRoot: Trees_Data ==> Trees_Tree
Trees_InsertRoot(data) ==
(dcl newTreePtr: Nodes_NodePtr := NEW(Nodes_MkBinaryTree(data, NIL, NIL, NIL)); 
 return mk_Trees_Tree(newTreePtr, newTreePtr);
);

Trees_InsertLeft: Trees_Tree * Trees_Data ==> Trees_Tree
Trees_InsertLeft(mk_Trees_Tree(treePtr, current), data) ==
(dcl new: Nodes_NodePtr := NEW(Nodes_MkBinaryTree(data, NIL, NIL, current)); 
 SET_LEFT(current, new);
 return mk_Trees_Tree(treePtr, new);
);

Trees_InsertRight: Trees_Tree * Trees_Data ==> Trees_Tree
Trees_InsertRight(mk_Trees_Tree(treePtr, current), data) ==
(dcl new: Nodes_NodePtr := NEW(Nodes_MkBinaryTree(data, NIL, NIL, current));
 SET_RIGHT(current, new);
 return mk_Trees_Tree(treePtr, new);
);

Trees_InOrderPredecessor: Nodes_NodePtr ==> Nodes_NodePtr
Trees_InOrderPredecessor(ptr) ==
(dcl pred: Nodes_NodePtr := LEFT(ptr);
 while Trees_HasRightChild(heap, pred) do
    pred := RIGHT(pred);
 return pred;   
)
pre Trees_HasLeftChild(heap, ptr);

Trees_Delete: Trees_Tree  ==> Trees_Tree
Trees_Delete(mk_Trees_Tree(treePtr, current)) ==
(    dcl  hasLeftChild: bool := Trees_HasLeftChild(heap, current),
          hasRightChild: bool := Trees_HasRightChild(heap, current),
          newcurrent: Nodes_NodePtr,
          newtree: Nodes_NodePtr := treePtr,
          parent: Nodes_NodePtr := PARENT(current),
          newchild: Nodes_NodePtr;

     if hasLeftChild or hasRightChild then 
     (    if hasLeftChild then
          (    newcurrent := LEFT(current);
               if hasRightChild then
               (    dcl right: Nodes_NodePtr := RIGHT(current),
                     pred :Nodes_NodePtr := Trees_InOrderPredecessor(current);
                    SET_PARENT(right, pred);
                    SET_RIGHT(pred, right);
               );
          )
          else 
               newcurrent := RIGHT(current);

          newchild := newcurrent;
          SET_PARENT(newcurrent, parent);
     )
     else
     (    newcurrent := parent;
          newchild := NIL;
     );

     if Trees_IsRoot(heap, current) then 
          newtree := newchild
     elseif Trees_IsRightChildOf(heap, current, parent) then
          SET_RIGHT(parent, newchild)
     else
          SET_LEFT(parent, newchild);

     DISPOSE(current);
     return mk_Trees_Tree(newtree, newcurrent);
)
pre treePtr <> NIL
post Heaps_AmountUsed(heap~) = Heaps_AmountUsed(heap) + 1 and
     let old = Trees_Set(heap~, mk_Trees_Tree(treePtr, current)) in
          STrees_Delete(old) = Trees_Set(heap, RESULT);
\end{vdm_al}
{\tt Trees\_Delete} is significantly different from its counterpart in the previous section.  Here, a deletion is divided into 12 cases.  First there are the four cases based on the current node's children.  These are the same four cases used in the previous section's delete.  Then, because the this tree is based on links, each of the four cases must be divided three further ways.  The first is when the current node is the root, the second when the current node is a right child and the third when the current node is a left child.  In the previous section, the delete did not have to be concerned about the current node's relationship to its parent because moving a child around did not affect the parent in any way.  In this case, because the parent has links to its children, they must be modified when the child is removed and/or replaced.
\\
\begin{vdm_al}

Trees_SearchSubtree: Nodes_NodePtr * Trees_Data ==> bool
Trees_SearchSubtree(subtree, dataToFind) ==
(    
     if subtree <> NIL then
     (    def data = DATA(subtree) in
          if data = dataToFind then
               return true
          else 
               return Trees_SearchSubtree(RIGHT(subtree), dataToFind)         
                    or Trees_SearchSubtree(LEFT(subtree), dataToFind)
     )

     else return false
);
\end{vdm_al}
{\tt Trees\_SearchSubtree} is used by {\tt Trees\_ExistData} to recursively search for data in the tree.
\\
\begin{vdm_al}
           
Trees_ExistsData: Trees_Tree * Trees_Data ==> bool
Trees_ExistsData(tree, data) == Trees_SearchSubtree(tree.treePtr, data)
post STrees_ExistsData(Trees_Set(heap, tree), data) = RESULT;

Trees_ExistsDirection: Trees_Tree * Trees_Direction ==> bool
Trees_ExistsDirection(tree, direction) == 
     cases direction:
          <ToRoot>  -> return tree.treePtr <> NIL,
          <ToLeft>  -> 
               return tree.current <> NIL 
                    and Trees_HasLeftChild(heap, tree.current),
          <ToRight> -> 
               return tree.current <> NIL 
                    and Trees_HasRightChild(heap, tree.current),
        others -> error
     end
post STrees_ExistsDirection(Trees_Set(heap, tree), direction) 
		= RESULT;


Trees_GetCurrentData: Trees_Tree ==> Trees_Data
Trees_GetCurrentData(tree) == DATA(tree.current)
pre tree.treePtr <> NIL
post STrees_GetCurrentData(Trees_Set(heap, tree)) = RESULT;

Trees_StoreCurrentData: Trees_Tree * Trees_Data ==> Trees_Tree
Trees_StoreCurrentData(tree, data) == 
(    SET_DATA(tree.current, data);
     return tree;
)
pre tree.treePtr <> NIL
post STrees_StoreCurrentData(Trees_Set(heap~, tree), data)
     = Trees_Set(heap, RESULT);

Trees_MoveInDir: Trees_Tree * Trees_Direction ==> Trees_Tree
Trees_MoveInDir(tree, direction) == 
     cases direction:
          <ToRoot> -> return mk_Trees_Tree(tree.treePtr, tree.treePtr),
          <ToLeft> -> return mk_Trees_Tree(tree.treePtr, LEFT(tree.current)),
          <ToRight> -> return mk_Trees_Tree(tree.treePtr, RIGHT(tree.current)),
          others -> error
     end
pre tree.treePtr <> NIL 
     and (direction = <ToLeft> => Trees_HasLeftChild(heap, tree.current)) 
     and (direction = <ToRight> => Trees_HasRightChild(heap, tree.current))
post STrees_MoveInDir(Trees_Set(heap~, tree), direction)
     = Trees_Set(heap, RESULT); 
     

Trees_MoveToParent: Trees_Tree ==> Trees_Tree
Trees_MoveToParent(tree) ==
     return mk_Trees_Tree(tree.treePtr, PARENT(tree.current))
pre not Trees_IsRoot(heap, tree.current)
post STrees_MoveToParent(Trees_Set(heap~, tree)) 
     = Trees_Set(heap, RESULT); 

Trees_Size: Trees_Tree ==> nat
Trees_Size(tree) == Trees_SubtreeSize(tree.treePtr)
post STrees_Size(Trees_Set(heap~, tree)) = RESULT;

Trees_SubtreeSize: Nodes_NodePtr ==> nat
Trees_SubtreeSize(subtree) ==
     if subtree <> NIL then
          return 
               1 
               + Trees_SubtreeSize(RIGHT(subtree)) 
               + Trees_SubtreeSize(LEFT(subtree))
     else return 0;

\end{vdm_al}
{\tt Trees\_SubtreeSize} is used by {\tt Trees\_Size} to recursively discover how many nodes are in the tree.
\\
\begin{vdm_al}

Trees_Traverse: Trees_Tree * (Trees_Data -> Trees_Data) ==> Trees_Tree
Trees_Traverse(tree, traversal) == 
(    Trees_TraverseSubtree(tree.treePtr, traversal);
     return tree;
)
post STrees_Traverse(Trees_Set(heap~, tree), traversal)
     = Trees_Set(heap, RESULT);

Trees_TraverseSubtree: Nodes_NodePtr * (Trees_Data -> Trees_Data) ==> ()
Trees_TraverseSubtree(subtree, traversal) ==
     if subtree <> NIL then
     (    SET_DATA(subtree, traversal(DATA(subtree)));

          Trees_TraverseSubtree(LEFT(subtree), traversal);
          Trees_TraverseSubtree(RIGHT(subtree), traversal);
     );
\end{vdm_al}
{\tt Trees\_TraverseSubtree} is used by {\tt Trees\_Traverse} to recursively apply the function {\tt traversal} to each node's data.\\

\subsection{Hard Hat Area Only! Heavy Duty Testing in Progress}
This section tests the singly-linked list, the doubly linked list, the `set tree' and the `link tree'. \\
\\
The following is  the global state for the entire specification.  It contains a system heap, a singly-linked list, a queue, a stack, a doubly-linked list, a `set tree' and a `link tree'.  All of these are necessary for testing.\\
\\
\begin{vdm_al}
     state SystemState of
          heap: Heaps_Heap
          slist: SList_List
          charQueue: Queues_Queue
          charStack: Stacks_Stack
          dlist: DList_List
          charTree: Trees_Tree
          stree: STrees_Info
     init s == s = mk_SystemState(
               Heaps_Init(), 
               SList_Init(), 
               Queues_Init(), 
               Stacks_Init(), 
               DList_Init(), 
               Trees_Init(), 
               STrees_Init()) 
     end

 
types Data = char;
\end{vdm_al}
{\tt Data} is the type used to test all of the data structures.  Recall that each data structure set its generic data parameter to {\tt Data}. 
\\
\begin{vdm_al}

operations

TestSList: () ==> bool

TestSList() ==
(
     slist := SList_Insert(slist, 'b', 1);
     slist := SList_Update(slist, 'a', 1);
     slist := SList_Delete(slist, 1);   
          
     slist := SList_Insert(slist, 'a', 1);
     slist := SList_Insert(slist, 'b', 2);
     slist := SList_Update(slist, 'c', 2);
     slist := SList_Delete(slist, 2);

     slist := SList_Insert(slist, 'b', 2);
     slist := SList_Insert(slist, 'c', 3);
     slist := SList_Update(slist, 'd', 2);
     slist := SList_Update(slist, 'b', 3);
     slist := SList_Delete(slist, 2);

     slist := SList_Append(slist, 'c');
     slist := SList_Delete(slist, 1);
     slist := SList_Insert(slist, 'a', 1);

     slist := SList_Insert(slist, 'f', 4);
     slist := SList_Insert(slist, 'd', 4);
     slist := SList_Append(slist, 'g');
     slist := SList_Insert(slist, 'e', 5);
     slist := SList_Append(slist, 'h');
     slist := SList_Append(slist, 'i');
     slist := SList_Append(slist, 'j');
     slist := SList_Delete(slist, 10);
     return [SList_Element(slist, i)| i in set {1, ..., 9}] = "abcdefghi";
);
\end{vdm_al}
{\tt TestSList} tests much of the singly-linked list.  When it is done, if the resulting list translated into a sequence is equal to the predicted result ``abcdefghi'', then it returns true; otherwise, it returns false.  
\\
\begin{vdm_al}

TestDList: () ==> bool
TestDList() ==
(
     dlist := DList_Insert(dlist, 'b', 1);
     dlist := DList_Update(dlist, 'a', 1);
     dlist := DList_Delete(dlist, 1);   

     dlist := DList_Insert(dlist, 'a', 1);

     dlist := DList_Insert(dlist, 'b', 2);
     dlist := DList_Update(dlist, 'c', 2);
     dlist := DList_Delete(dlist, 2);

     dlist := DList_Insert(dlist, 'b', 2);
     dlist := DList_Insert(dlist, 'c', 3);
     dlist := DList_Update(dlist, 'd', 2);
     dlist := DList_Update(dlist, 'b', 3);
     dlist := DList_Delete(dlist, 2);

     dlist := DList_Append(dlist, 'c');
     dlist := DList_Delete(dlist, 1);
     dlist := DList_Insert(dlist, 'a', 1);

     dlist := DList_Insert(dlist, 'f', 4);
     dlist := DList_Insert(dlist, 'd', 4);
     dlist := DList_Append(dlist, 'g');
     dlist := DList_Insert(dlist, 'e', 5);
     dlist := DList_Append(dlist, 'h');
     dlist := DList_Append(dlist, 'i');
     dlist := DList_Append(dlist, 'j');
     dlist := DList_Delete(dlist, 10);
     return [DList_Element(dlist, i)| i in set {1, ..., 9}] = "abcdefghi";
);
\end{vdm_al}
{\tt TestDList} tests the doubly-linked list exactly the same way {\tt TestSList} tests the singly-linked list.\\
\\
The following definitions are temporary predicted results for the testing applied to the `set binary tree'.  These three predicted tree states are constructed as constant values and compared to the actual tree states during the testing.
\\
\begin{vdm_al}

values
 
InsertResult = STrees_MkTree(
          { 
                    STrees_MkNode('a', 1), 
                    STrees_MkNode('b', 2),
                    STrees_MkNode('c', 3),
                    STrees_MkNode('d', 4),
                    STrees_MkNode('e', 5),
                    STrees_MkNode('f', 6),
                    STrees_MkNode('g', 7),
                    STrees_MkNode('h', 8),

                    STrees_MkNode('i', 9),
                    STrees_MkNode('j', 10),
                    STrees_MkNode('k', 11),
                    STrees_MkNode('l', 12),
                    STrees_MkNode('m', 13),
                    STrees_MkNode('n', 14),
                    STrees_MkNode('o', 15),
                    STrees_MkNode('p', 16),
                    STrees_MkNode('q', 17),
                    STrees_MkNode('r', 18),
                    STrees_MkNode('s', 19),
                    STrees_MkNode('t', 20),
                    STrees_MkNode('u', 21),
                    STrees_MkNode('v', 22),
                    STrees_MkNode('w', 23),
                    STrees_MkNode('x', 24),
                    STrees_MkNode('y', 25),
                    STrees_MkNode('z', 26)
          });

DeleteResult = STrees_MkTree(
          { 
               STrees_MkNode('a', 1), 
                    STrees_MkNode('d', 2),
                    STrees_MkNode('c', 3),
                    STrees_MkNode('h', 4),
                    STrees_MkNode('i', 5),
                    STrees_MkNode('f', 6),
                    STrees_MkNode('o', 7),
                    STrees_MkNode('p', 8),
                    STrees_MkNode('r', 10),
                    STrees_MkNode('s', 11),
                    STrees_MkNode('l', 12),
                    STrees_MkNode('z', 13),
                    STrees_MkNode('j', 23),
                    STrees_MkNode('x', 24),
                    STrees_MkNode('y', 25),
                    STrees_MkNode('t', 46),
                    STrees_MkNode('u', 47),
                    STrees_MkNode('k', 95),
                    STrees_MkNode('v', 190),
                    STrees_MkNode('w', 191)
          });

TraverseResult = STrees_MkTree(
          { 

               STrees_MkNode('a', 1), 
                    STrees_MkNode('a', 2),
                    STrees_MkNode('a', 3),
                    STrees_MkNode('a', 4),
                    STrees_MkNode('a', 5),
                    STrees_MkNode('a', 6),
                    STrees_MkNode('b', 7),
                    STrees_MkNode('b', 8),
                    STrees_MkNode('b', 10),
                    STrees_MkNode('b', 11),
                    STrees_MkNode('b', 12),
                    STrees_MkNode('b', 13),
                    STrees_MkNode('a', 23),
                    STrees_MkNode('b', 24),
                    STrees_MkNode('b', 25),
                    STrees_MkNode('b', 46),
                    STrees_MkNode('b', 47),
                    STrees_MkNode('b', 95),
                    STrees_MkNode('b', 190),
                    STrees_MkNode('b', 191)
          });

AlphabetSubset = {'a','b','c','d','e','f','g','h','i','j'};
\end{vdm_al}
{\tt Alphabet Subset} is a constant value used by {\tt Traversal} to test tree traversal.  
\\
\begin{vdm_al}

functions
Traversal: char -> char
Traversal(ch) ==
     if ch in set AlphabetSubset then 'a' else 'b';
\end{vdm_al}
{\tt Traversal} is used to test tree traversal.  When applied to node data it returns either `a' or `b', depending on whether or not the node data value is in {\tt AlphabetSubset} or not.
\\
\begin{vdm_al}

operations

TestSTreesInsert: () ==> bool
TestSTreesInsert() ==
(
     stree := STrees_Insert(stree, 'a', <ToRoot>);
  
     stree := STrees_Insert(stree, 'b', <ToLeft>); 

   
     stree := STrees_MoveToParent(stree);
     stree := STrees_Insert(stree, 'c', <ToRight>); 

     stree := STrees_MoveToParent(stree); 
     stree := STrees_MoveInDir(stree, <ToLeft>);
     stree := STrees_Insert(stree, 'd', <ToLeft>); 

     stree := STrees_MoveToParent(stree);
     stree := STrees_Insert(stree, 'e', <ToRight>); 

     stree := STrees_MoveInDir(stree, <ToRoot>); 
     stree := STrees_MoveInDir(stree, <ToRight>);
     stree := STrees_Insert(stree, 'f', <ToLeft>); 

     stree := STrees_MoveToParent(stree);
     stree := STrees_Insert(stree, 'g', <ToRight>); 

     stree := STrees_MoveInDir(stree, <ToRoot>); 
     stree := STrees_MoveInDir(stree, <ToLeft>); 
     stree := STrees_MoveInDir(stree, <ToLeft>);
     stree := STrees_Insert(stree, 'h', <ToLeft>); 

     stree := STrees_MoveToParent(stree);
     stree := STrees_Insert(stree, 'i', <ToRight>);

     stree := STrees_MoveToAnscestor(stree, 2); 
     stree := STrees_MoveInDir(stree, <ToRight>);
     stree := STrees_Insert(stree, 'j', <ToLeft>);

     stree := STrees_MoveToParent(stree);
     stree := STrees_Insert(stree, 'k', <ToRight>);

     stree := STrees_MoveInDir(stree, <ToRoot>); 
     stree := STrees_MoveInDir(stree, <ToRight>); 
     stree := STrees_MoveInDir(stree, <ToLeft>);
     stree := STrees_Insert(stree, 'l', <ToLeft>);

     stree := STrees_MoveToParent(stree);
     stree := STrees_Insert(stree, 'm', <ToRight>);

     stree := STrees_MoveToAnscestor(stree, 2); 
     stree := STrees_MoveInDir(stree, <ToRight>);
     stree := STrees_Insert(stree, 'n', <ToLeft>);

     stree := STrees_MoveToParent(stree);

     stree := STrees_Insert(stree, 'o', <ToRight>);

     stree := STrees_MoveInDir(stree, <ToRoot>); 
     stree := STrees_MoveInDir(stree, <ToLeft>); 
     stree := STrees_MoveInDir(stree, <ToLeft>); 
     stree := STrees_MoveInDir(stree, <ToLeft>);
     stree := STrees_Insert(stree, 'p', <ToLeft>);

     stree := STrees_MoveToParent(stree);
     stree := STrees_Insert(stree, 'q', <ToRight>);

     stree := STrees_MoveToAnscestor(stree, 2); 
     stree := STrees_MoveInDir(stree, <ToRight>);
     stree := STrees_Insert(stree, 'r', <ToLeft>);

     stree := STrees_MoveToParent(stree);
     stree := STrees_Insert(stree, 's', <ToRight>);

     stree := STrees_MoveToAnscestor(stree, 3); 
     stree := STrees_MoveInDir(stree, <ToRight>); 
     stree := STrees_MoveInDir(stree, <ToLeft>);
     stree := STrees_Insert(stree, 't', <ToLeft>);

     stree := STrees_MoveToParent(stree);
     stree := STrees_Insert(stree, 'u', <ToRight>);

     stree := STrees_MoveToAnscestor(stree, 2); 
     stree := STrees_MoveInDir(stree, <ToRight>);
     stree := STrees_Insert(stree, 'v', <ToLeft>);

     stree := STrees_MoveToParent(stree);
     stree := STrees_Insert(stree, 'w', <ToRight>);

     stree := STrees_MoveInDir(stree, <ToRoot>); 
     stree := STrees_MoveInDir(stree, <ToRight>); 
     stree := STrees_MoveInDir(stree, <ToLeft>); 
     stree := STrees_MoveInDir(stree, <ToLeft>);
     stree := STrees_Insert(stree, 'x', <ToLeft>);

     stree := STrees_MoveToParent(stree);
     stree := STrees_Insert(stree, 'y', <ToRight>);

     stree := STrees_MoveToAnscestor(stree, 2); 
     stree := STrees_MoveInDir(stree, <ToRight>);
     stree := STrees_Insert(stree, 'z', <ToLeft>);


     return STrees_GetTree(stree) = InsertResult;
);

TestSTreesDelete: () ==> bool
TestSTreesDelete() ==
(
     stree := STrees_MoveToNode(stree, 14);
     stree := STrees_Delete(stree);

     stree := STrees_MoveToNode(stree, 17);
     stree := STrees_Delete(stree);

     stree := STrees_MoveToNode(stree, 13);
     stree := STrees_Delete(stree);

     stree := STrees_MoveToNode(stree, 7);
     stree := STrees_Delete(stree);

     stree := STrees_MoveToNode(stree, 5);
     stree := STrees_Delete(stree);

     stree := STrees_MoveToNode(stree, 2);
     stree := STrees_Delete(stree);
     return STrees_GetTree(stree) = DeleteResult;
);
\end{vdm_al}
The previous two operations,  {\tt TestSTreesInsert} and {\tt TestSTreesDelete} should not be called individually; otherwise, they will not return correct results \footnote{{\tt TestSTreesInsert} assumes that the list is empty and {\tt TestSTreesDelete} assumes that {\tt TestSTreesInsert} was successful.}.  Instead, they should only be called through the operation {\tt TestSTrees} below.
\\
\begin{vdm_al}

TestSTrees: () ==> bool
TestSTrees() ==
(
     if STrees_ExistsDirection(stree, <ToLeft>) then return false;

     if STrees_ExistsDirection(stree, <ToRight>) then return false;

     if not TestSTreesInsert() then return false;

     if not TestSTreesDelete() then return false;

     if not STrees_ExistsData(stree, 'c') then return false;


     if not STrees_ExistsNode(stree, 3) then return false;

     stree := STrees_MoveToNode(stree, 3);
     if not STrees_ExistsDirection(stree, <ToLeft>) then return false;

     if not STrees_ExistsDirection(stree, <ToRight>) then return false;

     if not STrees_ExistsDirection(stree, <ToRoot>) then return false;

     if 'z' <> STrees_GetData(stree, 13) then return false;

     stree := STrees_SetCurrentNode(stree, STrees_MkNode('z', 13));

     if STrees_MkNode('z', 13) <> STrees_GetCurrentNode(stree) 
     then return false;

     stree := STrees_StoreCurrentData(stree, 'Z');
     if 'Z' <> STrees_GetCurrentData(stree) then return false;

     if STrees_Size(stree) <> 20 then return false;

     stree := STrees_Traverse(stree, Traversal);

     return STrees_GetTree(stree) = TraverseResult;
);


\end{vdm_al}
The next three operations are used to test the `link tree'.  Because it's post-conditions rely heavily on the `set tree' operations, if the `set tree' has already been tested then it is not necessary to construct predicted results for the `link tree' test.  In other words, if the `set tree' is free of errors and the following test operations run successfully then we can be reasonably sure that the `link tree' is also free of errors.
\\
\begin{vdm_al}

operations

TestTreesInsert: () ==> ()
TestTreesInsert() ==
(
     charTree := Trees_Insert(charTree, 'a', <ToRoot>);

     charTree := Trees_Insert(charTree, 'b', <ToLeft>); 
 
     charTree := Trees_MoveToParent(charTree);

     charTree := Trees_Insert(charTree, 'c', <ToRight>); 

     charTree := Trees_MoveToParent(charTree); 
     charTree := Trees_MoveInDir(charTree, <ToLeft>);
     charTree := Trees_Insert(charTree, 'd', <ToLeft>); 

     charTree := Trees_MoveToParent(charTree);
     charTree := Trees_Insert(charTree, 'e', <ToRight>); 

     charTree := Trees_MoveInDir(charTree, <ToRoot>); 
     charTree := Trees_MoveInDir(charTree, <ToRight>);
     charTree := Trees_Insert(charTree, 'f', <ToLeft>); 

     charTree := Trees_MoveToParent(charTree);
     charTree := Trees_Insert(charTree, 'g', <ToRight>); 

     charTree := Trees_MoveInDir(charTree, <ToRoot>);
     charTree := Trees_MoveInDir(charTree, <ToLeft>);
     charTree := Trees_MoveInDir(charTree, <ToLeft>);
     charTree := Trees_Insert(charTree, 'h', <ToLeft>);

     charTree := Trees_MoveToParent(charTree);
     charTree := Trees_Insert(charTree, 'i', <ToRight>);

     charTree := Trees_MoveToParent(charTree);
     charTree := Trees_MoveToParent(charTree);
     charTree := Trees_MoveInDir(charTree, <ToRight>);
     charTree := Trees_Insert(charTree, 'j', <ToLeft>);

     charTree := Trees_MoveToParent(charTree);
     charTree := Trees_Insert(charTree, 'k', <ToRight>);

     charTree := Trees_MoveInDir(charTree, <ToRoot>); 
     charTree := Trees_MoveInDir(charTree, <ToRight>); 
     charTree := Trees_MoveInDir(charTree, <ToLeft>);
     charTree := Trees_Insert(charTree, 'l', <ToLeft>);

     charTree := Trees_MoveToParent(charTree);
     charTree := Trees_Insert(charTree, 'm', <ToRight>);

     charTree := Trees_MoveToParent(charTree);
     charTree := Trees_MoveToParent(charTree);
     charTree := Trees_MoveInDir(charTree, <ToRight>);
     charTree := Trees_Insert(charTree, 'n', <ToLeft>);

     charTree := Trees_MoveToParent(charTree);

     charTree := Trees_Insert(charTree, 'o', <ToRight>);
);

TestTreesDelete: () ==> ()
TestTreesDelete() ==
(
     charTree := Trees_MoveInDir(charTree, <ToRoot>);
     charTree := Trees_MoveInDir(charTree, <ToRight>);
     charTree := Trees_MoveInDir(charTree, <ToRight>);
     charTree := Trees_MoveInDir(charTree, <ToRight>);
     charTree := Trees_Delete(charTree);

     charTree := Trees_Delete(charTree);

     charTree := Trees_MoveInDir(charTree, <ToRoot>);
     charTree := Trees_MoveInDir(charTree, <ToLeft>);
     charTree := Trees_MoveInDir(charTree, <ToLeft>);
     charTree := Trees_MoveInDir(charTree, <ToLeft>);
     charTree := Trees_Delete(charTree);

     charTree := Trees_Delete(charTree);

     charTree := Trees_MoveInDir(charTree, <ToRoot>);
     charTree := Trees_MoveInDir(charTree, <ToLeft>);
     charTree := Trees_Delete(charTree);

     charTree := Trees_MoveInDir(charTree, <ToRoot>);
     charTree := Trees_MoveInDir(charTree, <ToRight>);
     charTree := Trees_Delete(charTree);

     charTree := Trees_MoveInDir(charTree, <ToRoot>);
     charTree := Trees_Delete(charTree);
);


TestTrees: () ==> bool
TestTrees() ==
(
     if Trees_ExistsDirection(charTree, <ToLeft>) then return false;

     if Trees_ExistsDirection(charTree, <ToRight>) then return false;      
     TestTreesInsert();
     TestTreesDelete();

     if not Trees_ExistsData(charTree, 'i') then return false;


     if Trees_ExistsDirection(charTree, <ToLeft>) then return false;

     if not Trees_ExistsDirection(charTree, <ToRight>) then return false;

     if not Trees_ExistsDirection(charTree, <ToRoot>) then return false;

     charTree := Trees_StoreCurrentData(charTree, 'Z');
     if 'Z' <> Trees_GetCurrentData(charTree) then return false;

     if Trees_Size(charTree) <> 8 then return false;

     charTree := Trees_Traverse(charTree, Traversal);
     return true;
);

\end{vdm_al}
~~~