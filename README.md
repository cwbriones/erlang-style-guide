Erlang Style Guide
==================

Much of this style guide was cobbled together from various sources, including http://www.erlang.se/doc/programming_rules.shtml. Somewhat altered from the styleguide we use at http://www.github.com/whisperapp.

## Coding Style
* Each exported method should be on its own line for easy adding and removal.
  * An exception to this is a block of exports to implement a behavior should be on the same line if possible.
* Use comments to differentiate between public exports and functions which should only be called from within in the module. 
* Write a spec attribute for all public functions.

```erlang
%%% This is my module.
%%% It does cool things.
-module(my_module)

%% Implementing a behaviour
-behaivour(some_behaviour).
-export([my_func1, my_func2, my_func3]).

%% Public exports
-export([
  func1,
  func2,
  func3
]).
```

* Use soft-tabs with a 2 space indentation.
* Do not use smart indentation to align with operators such as lists.
* Use spaces around operators, between elements of lists, elements of tuples, and arguments to functions.
* Do not use spaces after `[`, `{`, `(` or before `]`, `}`, `)` 

```erlang
Sum = 1 + 2,
Tuple = {tuple, 1, 2, 3},
List = [list, 1, 2, 3],
some_module:func(ArgOne, ArgTwo, ArgThree).
```

* When matching to a case statement, place the case statement on the same line with the following cases only being indented 1 level. The only exception to this is when the right hand side is too long, in which it should be placed on the following line, at the same indentation level as the assignment.

```erlang
SomeVariable = case SomeResult of
  result_one -> one;
  result_two -> two;
  result_three -> three
end.
```

* Avoid writing if expressions (use case instead).
* Pattern match as explicitly as possible, avoid underscore and match on list elements instead of boolean results if possible.

```erlang
%% Don't
case length(MyList) > 0 of
  true  -> do_something(hd(MyList));
  false -> do_something_else()
end,

%% Do
case MyList of
  [Head| _] -> do_something(Head);
  [] -> do_something_else()
end,
```

* Place patterns and results on one line in case statements for "short lines" (use your own judgment).

* Functions of varying arity (for default arguments) should call their higher-arity counterparts instead of having an implementation themselves.

```erlang
%% Don't
%% my_function() -> lager:info("~p", ["Hello, World!"]).
%% Do
my_function() -> my_function("Hello World!").

my_function(Msg) -> lager:info("~p", [Msg]).
```

## Naming
* Always use `snake_case` for atoms.
* Whenever possible, use `_<Description>` for unused fields in pattern matches for the sake of clarity.
* Never use `_` for any other reason in variable names.
* Use so-called `SCREAMING_SNAKE_CASE` for macro names.

## Comments
* Module level comments should start with `%%%` and be at the top of the file
* Function level comments should start with `%%` and be at the top of the function
* Code comments should start with `%` and be above the line they are referencing


## Exception Handling
* Avoid writing try/catch expressions.

