# 1. Write a PROLOG program to implement the family tree and demonstrate the family relationship.

```
% Facts
male(john).
male(paul).
male(mike).
female(mary).
female(lisa).

parent(john, paul).
parent(mary, paul).
parent(paul, mike).
parent(lisa, mike).

% Rules
father(X, Y) :- parent(X, Y), male(X).
mother(X, Y) :- parent(X, Y), female(X).

sibling(X, Y) :-
    parent(Z, X),
    parent(Z, Y),
    X \= Y.

grandparent(X, Y) :-
    parent(X, Z),
    parent(Z, Y).
```
```
?- father(john, paul).
?- mother(mary, paul).
?- sibling(paul, mike).
?- grandparent(john, mike).
```

# 2. Write a PROLOG program to implement conc(L1, L2, L3) where L.2 is the list to be appended with L1 to get the resulted list L3.
```
conc([], L, L).

conc([H|T], L2, [H|R]) :-
    conc(T, L2, R).
```

```
?- conc([1,2], [3,4], R).
```

# 3. Write a PROLOG program to implement reverse(L, R) where List L is original and List R is reversed list.
```
my_reverse([], []).

my_reverse([H|T], R) :-
    my_reverse(T, RT),
    conc(RT, [H], R).

conc([], L, L).
conc([H|T], L, [H|R]) :-
    conc(T, L, R).
```
```
?- my_reverse([1,2,3,4], R).

```

# 4. Write a PROLOG program to calculate the sum of two numbers.
```
sum(X, Y, S) :-
    S is X + Y.
```
```
?- sum(10, 20, S).
```
# 5. Write a PROLOG program to implement max(X, Y, M) so that M is the maximum of two numbers X and Y.
```
   max(X, Y, X) :-
    X >= Y.

max(X, Y, Y) :-
    X < Y.

```
```
?- max(10, 20, M).

```

# 6. Write a program in PROLOG to implement factorial (N, F) where F represents the factorial of a number N.
```
factorial(0, 1).

factorial(N, F) :-
    N > 0,
    N1 is N - 1,
    factorial(N1, F1),
    F is N * F1.

```

```
?- factorial(5, F).
```

# 7. Write a program in PROLOG to implement generate fib(N,T) where T represents the Nth term of the Fibonacci series.
```
fib(0, 0).
fib(1, 1).

fib(N, T) :-
    N > 1,
    N1 is N - 1,
    N2 is N - 2,
    fib(N1, T1),
    fib(N2, T2),
    T is T1 + T2.

```
```
?- fib(6, T).
```
# 8. Write a PROLOG program to implement power (Num, Pow, Ans) where Num is raised to the power Pow to get Ans.
```
power(_, 0, 1).

power(Num, Pow, Ans) :-
    Pow > 0,
    P1 is Pow - 1,
    power(Num, P1, A1),
    Ans is Num * A1.
```
```
?- power(2, 3, Ans).

```
# 9. PROLOG program to implement multi (N1, N2, R) where N1 and N2 denotes the numbers to be multiplied and R represents the result.
```
multi(N1, N2, R) :-
    R is N1 * N2.

```
```
?- multi(5, 4, R).
```
# 10 . Write a PROLOG program to implement memb(X, L): to check whether X is a member of L or not.
```
memb(X, [X|_]).

memb(X, [_|T]) :-
    memb(X, T).
```
```
?- memb(3, [1,2,3,4]).
```
# 11. Write a PROLOG program to implement sumlist(L, S) so that S is the sum of a given list L.
```
sumlist([], 0).

sumlist([H|T], S) :-
    sumlist(T, S1),
    S is H + S1.
```

```
?- sumlist([1,2,3,4,5], S).
```
# 12. Write a PROLOG program to implement two predicates evenlength(List) and oddlength(List) so that they are true if their argument is a list of even or odd length respectively.
```
evenlength([]).

evenlength([_,_|T]) :-
    evenlength(T).

oddlength([_]).

oddlength([_,_|T]) :-
    oddlength(T).
```
```
?- evenlength([1,2,3,4]).
?- oddlength([1,2,3]).
```

# 13. Write a PROLOG program to implement maxlist(L, M) so that M is the maximum number in the list.
```
maxlist([X], X).

maxlist([H|T], M) :-
    maxlist(T, M1),
    H >= M1,
    M is H.

maxlist([H|T], M) :-
    maxlist(T, M1),
    H < M1,
    M is M1.
```

```
?- maxlist([10,20,5,40,30], M).
```
# 14. Write a PROLOG program to implement insert(I, N, L, R) that inserts an item I into Nth position of list L to generate a list R
```
insert(I, 1, L, [I|L]).

insert(I, N, [H|T], [H|R]) :-
    N > 1,
    N1 is N - 1,
    insert(I, N1, T, R).
```

```
?- insert(50, 2, [10,20,30], R).

```
# 15. Write a PROLOG program to implement delete(N, L, R) that removes the element on Nth position from a list L to generate a list R.
```
delete_n(1, [_|T], T).

delete_n(N, [H|T], [H|R]) :-
    N > 1,
    N1 is N - 1,
    delete_n(N1, T, R).
```
```
?- delete_n(2, [10,20,30,40], R).
```
