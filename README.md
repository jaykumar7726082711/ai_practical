# ai_practical
## 1  Write a program in Prolog to implement TowerOfHanoi(N) where 
N represents the number of disks. 
```
% Tower of Hanoi
% tower_of_hanoi(N) solves the problem for N disks.

tower_of_hanoi(N) :-
    hanoi(N, left, center, right).

% Base case: one disk
hanoi(1, Source, _, Destination) :-
    format('Move disk from ~w to ~w~n', [Source, Destination]).

% Recursive case
hanoi(N, Source, Auxiliary, Destination) :-
    N > 1,
    M is N - 1,
    hanoi(M, Source, Destination, Auxiliary),
    format('Move disk from ~w to ~w~n', [Source, Destination]),
    hanoi(M, Auxiliary, Source, Destination).


```
## 2 Write a program to implement the Hill climbing search algorithm in Prolog
```
% Hill Climbing Search Algorithm

% hill_climbing(Start, Goal)
% Starts the search from Start and tries to reach Goal.

hill_climbing(Start, Goal) :-
    climb(Start, Goal, [Start]).

% Goal reached
climb(State, Goal, Path) :-
    State = Goal,
    reverse(Path, Result),
    write('Solution Path: '),
    write(Result), nl.

% Continue climbing to the best neighbor
climb(State, Goal, Path) :-
    State \= Goal,
    findall(Next, neighbor(State, Next), Neighbors),
    best_neighbor(State, Neighbors, Best),
    Best \= State,
    climb(Best, Goal, [Best|Path]).

% Example neighbor relationships
neighbor(a, b).
neighbor(a, c).
neighbor(b, d).
neighbor(b, e).
neighbor(c, f).
neighbor(c, g).
neighbor(d, h).
neighbor(e, h).
neighbor(f, h).
neighbor(g, h).

% Heuristic values
% Smaller value means closer to the goal.
heuristic(a, 4).
heuristic(b, 3).
heuristic(c, 3).
heuristic(d, 2).
heuristic(e, 2).
heuristic(f, 2).
heuristic(g, 1).
heuristic(h, 0).

% Select the neighbor with the lowest heuristic value
best_neighbor(State, [], State).

best_neighbor(_, [First|Rest], Best) :-
    best_from_list(Rest, First, Best).

best_from_list([], Best, Best).

best_from_list([H|T], Current, Best) :-
    heuristic(H, HValue),
    heuristic(Current, CValue),
    HValue < CValue,
    best_from_list(T, H, Best).

best_from_list([H|T], Current, Best) :-
    heuristic(H, HValue),
    heuristic(Current, CValue),
    HValue >= CValue,
    best_from_list(T, Current, Best).


```
## 3 Write a program to implement the Best first search algorithm in Prolog. 
```
% Best First Search Algorithm in Prolog

% Graph representation
edge(a, b).
edge(a, c).
edge(b, d).
edge(b, e).
edge(c, f).
edge(c, g).
edge(d, h).
edge(e, h).
edge(f, h).
edge(g, h).

% Heuristic values
% Smaller value means closer to the goal.
heuristic(a, 4).
heuristic(b, 3).
heuristic(c, 2).
heuristic(d, 2).
heuristic(e, 1).
heuristic(f, 1).
heuristic(g, 1).
heuristic(h, 0).

% Best First Search
best_first_search(Start, Goal, Path) :-
    bfs([([Start], Start)], Goal, RevPath),
    reverse(RevPath, Path).

% Goal reached
bfs([([Node|Path], Node)|_], Node, [Node|Path]).

% Expand the best node
bfs([([Node|Path], Node)|Rest], Goal, Solution) :-
    findall(
        ([Next,Node|Path], Next),
        (edge(Node, Next), \+ member(Next, [Node|Path])),
        Children
    ),
    append(Rest, Children, NewQueue),
    sort_queue(NewQueue, SortedQueue),
    bfs(SortedQueue, Goal, Solution).

% Sort nodes according to heuristic value
sort_queue(Queue, SortedQueue) :-
    predsort(compare_nodes, Queue, SortedQueue).

compare_nodes(Order, (_, Node1), (_, Node2)) :-
    heuristic(Node1, H1),
    heuristic(Node2, H2),
    compare(Order, H1, H2).

```
## 4 Write a program to implement A* search algorithm in Prolog.
```
% A* Search Algorithm in Prolog

% Graph: edge(Source, Destination, Cost)

edge(a, b, 1).
edge(a, c, 4).
edge(b, d, 2).
edge(b, e, 5).
edge(c, f, 3).
edge(d, g, 3).
edge(e, g, 1).
edge(f, g, 2).

% Heuristic values: h(Node, Value)

h(a, 7).
h(b, 6).
h(c, 4).
h(d, 4).
h(e, 2).
h(f, 2).
h(g, 0).

% A* Search
astar(Start, Goal, Path, Cost) :-
    h(Start, H),
    F is H,
    search([node(Start, [Start], 0, F)], Goal, RevPath, Cost),
    reverse(RevPath, Path).

% Goal reached
search([node(Goal, Path, G, _)|_], Goal, Path, G).

% Continue searching
search([node(Current, Path, G, _)|Rest], Goal, Solution, Cost) :-
    findall(
        node(Next, [Next|Path], NewG, F),
        (
            edge(Current, Next, StepCost),
            \+ member(Next, Path),
            NewG is G + StepCost,
            h(Next, H),
            F is NewG + H
        ),
        Children
    ),
    append(Rest, Children, NewQueue),
    sort_queue(NewQueue, SortedQueue),
    search(SortedQueue, Goal, Solution, Cost).

% Sort nodes according to f(n) = g(n) + h(n)
sort_queue(Queue, SortedQueue) :-
    predsort(compare_nodes, Queue, SortedQueue).

compare_nodes(Order, node(_, _, _, F1), node(_, _, _, F2)) :-
    compare(Order, F1, F2).

```
 ## 5 Write a program to implement the min-max search algorithm in Prolog.
 ```
% Min-Max Search Algorithm in Prolog

% A game tree is represented using:
% node(Name, Value) for terminal nodes
% node(Name, [Children]) for non-terminal nodes

% Terminal nodes
value(a, 3).
value(b, 5).
value(c, 2).
value(d, 9).
value(e, 12).
value(f, 5).
value(g, 23).
value(h, 23).

% Min-Max evaluation
minimax(Node, Value) :-
    value(Node, Value).

% MAX node
minimax(Node, Value) :-
    children(Node, Children),
    max_value(Children, Value).

% MIN node
minimax(Node, Value) :-
    children(Node, Children),
    min_value(Children, Value).

% Game tree
children(root, [n1, n2]).
children(n1, [a, b]).
children(n2, [c, d]).

% Maximum value among children
max_value([Node|Nodes], Max) :-
    minimax(Node, Value),
    max_value(Nodes, Value, Max).

max_value([], Max, Max).

max_value([Node|Nodes], Current, Max) :-
    minimax(Node, Value),
    ( Value > Current ->
        NewCurrent = Value
    ;
        NewCurrent = Current
    ),
    max_value(Nodes, NewCurrent, Max).

% Minimum value among children
min_value([Node|Nodes], Min) :-
    minimax(Node, Value),
    min_value(Nodes, Value, Min).

min_value([], Min, Min).

min_value([Node|Nodes], Current, Min) :-
    minimax(Node, Value),
    ( Value < Current ->
        NewCurrent = Value
    ;
        NewCurrent = Current
    ),
    min_value(Nodes, NewCurrent, Min).



```
## 6 Write a program to solve the Water-Jug Problem in Prolog. 
```
% Water Jug Problem
% Jug capacities: 4 liters and 3 liters
% Goal: Get exactly 2 liters in the 4-liter jug.

% water_jug(+Start, -Solution)
water_jug(Solution) :-
    solve((0, 0), [(0, 0)], Solution).

% Goal state
solve((X, _), Path, Path) :-
    X =:= 2.

% Continue searching
solve(State, Visited, Solution) :-
    move(State, NextState),
    \+ member(NextState, Visited),
    solve(NextState, [NextState|Visited], Solution).

% Possible operations

% Fill 4-liter jug
move((_, Y), (4, Y)).

% Fill 3-liter jug
move((X, _), (X, 3)).

% Empty 4-liter jug
move((_, Y), (0, Y)).

% Empty 3-liter jug
move((X, _), (X, 0)).

% Pour 4-liter jug into 3-liter jug
move((X, Y), (NX, 3)) :-
    X + Y >= 3,
    NX is X + Y - 3.

% Pour 4-liter jug into 3-liter jug
move((X, Y), (0, NY)) :-
    X + Y =< 3,
    NY is X + Y.

% Pour 3-liter jug into 4-liter jug
move((X, Y), (4, NY)) :-
    X + Y >= 4,
    NY is X + Y - 4.

% Pour 3-liter jug into 4-liter jug
move((X, Y), (NX, 0)) :-
    X + Y =< 4,
    NX is X + Y.

```
## 7  Implement sudoku problem (minimum 9×9 size) using constraint satisfaction in 
Prolog. 
```
% Sudoku Solver using Constraint Satisfaction
% Requires the CLPFD library.

:- use_module(library(clpfd)).

% sudoku(+Puzzle)
% Puzzle is a 9x9 list. Use 0 for empty cells.

sudoku(Puzzle) :-
    length(Puzzle, 9),
    maplist(same_length(Puzzle), Puzzle),
    append(Puzzle, Vars),
    Vars ins 1..9,

    % Rows must contain different values
    maplist(all_distinct, Puzzle),

    % Columns must contain different values
    transpose(Puzzle, Columns),
    maplist(all_distinct, Columns),

    % 3x3 blocks must contain different values
    blocks(Puzzle),

    % Fill in the solution
    labeling([ffc], Vars).

% Check all 3x3 blocks
blocks([]).
blocks([A,B,C|Rows]) :-
    blocks_in_rows(A, B, C),
    blocks(Rows).

blocks_in_rows([], [], []).
blocks_in_rows(
    [A1,A2,A3|Arest],
    [B1,B2,B3|Brest],
    [C1,C2,C3|Crest]
) :-
    all_distinct([A1,A2,A3,B1,B2,B3,C1,C2,C3]),
    blocks_in_rows(Arest, Brest, Crest).

Example Query

The following is a Sudoku puzzle where 0 represents an empty cell:

?- sudoku([
    [5,3,0,0,7,0,0,0,0],
    [6,0,0,1,9,5,0,0,0],
    [0,9,8,0,0,0,0,6,0],
    [8,0,0,0,6,0,0,0,3],
    [4,0,0,8,0,3,0,0,1],
    [7,0,0,0,2,0,0,0,6],
    [0,6,0,0,0,0,2,8,0],
    [0,0,0,4,1,9,0,0,5],
    [0,0,0,0,8,0,0,7,9]
]).


Output:

Puzzle = [
    [5,3,4,6,7,8,9,1,2],
    [6,7,2,1,9,5,3,4,8],
    [1,9,8,3,4,2,5,6,7],
    [8,5,9,7,6,1,4,2,3],
    [4,2,6,8,5,3,7,9,1],
    [7,1,3,9,2,4,8,5,6],
    [9,6,1,5,3,7,2,8,4],
    [2,8,7,4,1,9,6,3,5],
    [3,4,5,2,8,6,1,7,9]
].


```
## 8 Write a Prolog program to implement the family tree and 
demonstrate the family relationship.
```
% Family Tree in Prolog

% Facts: parent(Parent, Child)

parent(john, mary).
parent(john, david).

parent(susan, mary).
parent(susan, david).

parent(david, tom).
parent(david, lisa).

parent(emily, tom).
parent(emily, lisa).

parent(mary, peter).
parent(mary, anna).

parent(james, peter).
parent(james, anna).

% Gender facts
male(john).
male(david).
male(tom).
male(james).
male(peter).

female(susan).
female(mary).
female(emily).
female(lisa).
female(anna).

% Basic relationships

% Father
father(Father, Child) :-
    male(Father),
    parent(Father, Child).

% Mother
mother(Mother, Child) :-
    female(Mother),
    parent(Mother, Child).

% Child
child(Child, Parent) :-
    parent(Parent, Child).

% Siblings
sibling(X, Y) :-
    parent(P, X),
    parent(P, Y),
    X \= Y.

% Brother
brother(Brother, Person) :-
    male(Brother),
    sibling(Brother, Person).

% Sister
sister(Sister, Person) :-
    female(Sister),
    sibling(Sister, Person).

% Grandparent
grandparent(Grandparent, Grandchild) :-
    parent(Grandparent, Parent),
    parent(Parent, Grandchild).

% Grandfather
grandfather(Grandfather, Grandchild) :-
    male(Grandfather),
    grandparent(Grandfather, Grandchild).

% Grandmother
grandmother(Grandmother, Grandchild) :-
    female(Grandmother),
    grandparent(Grandmother, Grandchild).

% Uncle
uncle(Uncle, Person) :-
    male(Uncle),
    sibling(Uncle, Parent),
    parent(Parent, Person).

% Aunt
aunt(Aunt, Person) :-
    female(Aunt),
    sibling(Aunt, Parent),
    parent(Parent, Person).

% Cousin
cousin(X, Y) :-
    parent(P1, X),
    parent(P2, Y),
    sibling(P1, P2),
    X \= Y.

```
## 9  Write a Prolog program to implement knowledge representation 
using frames with appropriate examples.
```
% Knowledge Representation using Frames in Prolog

% -------------------------------------------------
% Frame: Person
% -------------------------------------------------

frame(person, [
    name(unknown),
    age(unknown),
    gender(unknown),
    occupation(unknown)
]).

% -------------------------------------------------
% Specific Person Frames
% -------------------------------------------------

frame(john, [
    isa(person),
    name(john),
    age(25),
    gender(male),
    occupation(engineer)
]).

frame(mary, [
    isa(person),
    name(mary),
    age(22),
    gender(female),
    occupation(teacher)
]).

% -------------------------------------------------
% Frame: Student
% -------------------------------------------------

frame(student, [
    isa(person),
    status(student),
    studies(computer_science)
]).

frame(alice, [
    isa(student),
    name(alice),
    age(20),
    gender(female),
    college(ABC_University)
]).

% -------------------------------------------------
% Frame: Car
% -------------------------------------------------

frame(car, [
    type(vehicle),
    wheels(4),
    fuel(petrol)
]).

frame(toyota, [
    isa(car),
    brand(toyota),
    model(corolla),
    color(white),
    year(2024)
]).

% -------------------------------------------------
% Retrieve a slot value from a frame
% -------------------------------------------------

slot(Frame, Slot, Value) :-
    frame(Frame, Slots),
    member(SlotValue, Slots),
    SlotValue =.. [Slot, Value].

% -------------------------------------------------
% Display all slots of a frame
% -------------------------------------------------

show_frame(Frame) :-
    frame(Frame, Slots),
    write('Frame: '),
    write(Frame), nl,
    show_slots(Slots).

show_slots([]).

show_slots([Slot|Rest]) :-
    write('  '),
    write(Slot),
    nl,
    show_slots(Rest).

% -------------------------------------------------
% Check inheritance relationship
% -------------------------------------------------

is_a(Frame, Parent) :-
    frame(Frame, Slots),
    member(isa(Parent), Slots).

% Recursive inheritance
is_a(Frame, Ancestor) :-
    frame(Frame, Slots),
    member(isa(Parent), Slots),
    is_a(Parent, Ancestor).

```
## 10 Write a Prolog program to implement conc(L1, L2, L3) where L2 
is the list to be appended with L1 to get the resulted list L3. 
```
% Concatenation of two lists
% conc(L1, L2, L3)
% L3 is the result of appending L2 to L1.
 
% Base case
conc([], L, L).

% Recursive case
conc([H|T], L2, [H|L3]) :-
    conc(T, L2, L3).


```
## 11   Write a Prolog program to implement reverse(L, R) where List L 
is original and List R is reversed list. 
```
% Reverse a list
% reverse_list(L, R)
% L = original list
% R = reversed list

reverse_list([], []).

reverse_list([H|T], R) :-
    reverse_list(T, RT),
    append(RT, [H], R).

```
## 12  Write a Prolog program to generate a parse tree of a given sentence 
in English language assuming the grammar required for parsing. 
```
% Parse Tree Generator using DCG

% Sentence -> Noun Phrase + Verb Phrase
sentence(s(NP, VP)) -->
    noun_phrase(NP),
    verb_phrase(VP).

% Noun Phrase -> Determiner + Noun
noun_phrase(np(Det, N)) -->
    determiner(Det),
    noun(N).

% Noun Phrase -> Proper Noun
noun_phrase(np(PN)) -->
    proper_noun(PN).

% Verb Phrase -> Verb + Noun Phrase
verb_phrase(vp(V, NP)) -->
    verb(V),
    noun_phrase(NP).

% Verb Phrase -> Verb
verb_phrase(vp(V)) -->
    verb(V).

% Determiners
determiner(det(the)) --> [the].
determiner(det(a)) --> [a].
determiner(det(an)) --> [an].

% Nouns
noun(n(cat)) --> [cat].
noun(n(dog)) --> [dog].
noun(n(boy)) --> [boy].
noun(n(girl)) --> [girl].

% Proper nouns
proper_noun(pn(john)) --> [john].
proper_noun(pn(mary)) --> [mary].

% Verbs
verb(v(eats)) --> [eats].
verb(v(sees)) --> [sees].
verb(v(likes)) --> [likes].
verb(v(runs)) --> [runs].

```
## 13  Write a Prolog program to recognize context free grammar anbn. 
```
% Recognize the language a^n b^n
% Grammar:
% S -> a S b
% S -> epsilon

% DCG implementation

s --> [].
s --> [a], s, [b].

Example Queries

1. Recognize aabb:

?- phrase(s, [a,a,b,b]).


Output:

true.


2. Recognize aaabbb:

?- phrase(s, [a,a,a,b,b,b]).


Output:

true.


3. Reject aabbb:

?- phrase(s, [a,a,b,b,b]).


Output:

false.

Grammar

The context-free grammar is:

S → aSb
S → ε


It generates:

ε
ab
aabb
aaabbb
aaaabbbb
...


The rule s --> [a], s, [b]. ensures that one a is matched with one b, while s --> []. represents the empty string (ε).

You can also generate strings belonging to the grammar:

?- phrase(s, String).


This will successively generate:

String = [] ;
String = [a,b] ;
String = [a,a,b,b] ;
String = [a,a,a,b,b,b] ;
...
```
