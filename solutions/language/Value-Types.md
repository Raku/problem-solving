# Basic Value Type Structures In Raku

## Preface

This resolution is based upon discussion in 
[problem-solving#135](https://github.com/Raku/problem-solving/issues/135) and,
accordingly, provides a solution to the issue.

The issue was opened as a followup to unification of behaviors of `List` and
`Map` structures which resulted in both types preserving containerization of
their elements. This resulted in both types becoming non-value types,
effectively leaving Raku without value type positional and associative
structures.

## Solution

Two new types are to be added to the Raku language:

- `ValueList` to represent immutable positional value type
- `ValueMap` to represent immutable associative value type

Both types are characterized by forcible scalar de-containerization of any
object, stored in them. For example, here is how it must work with lists:

    my $val = 42;
    my $l := List.new($val);
    say $l[0].VAR.^name; # Scalar
    my $vl := ValueList.new($val);
    say $vl[0].VAR.^name; # Int

It also means that for any `Value*`-type structures instances of any non-value
type classes, including but not limited to `Array` and `Hash`, are considered
the same value if they're the same object, not if their content is identical.

## Reasoning 

Provide means for operators like `===`, types like `QuantHash`-es, and other
value-type based language entities to easily operate on positional and
associative type structures.

## Language Version Eligibility

Both types are to be available no earlier than in Raku v6.e.
