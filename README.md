# ECMAScript Shorthand Property Assignment Improvements

This proposal introduces new forms of shorthand assignments for both object literal initializers 
and object assignment patterns, allowing the use of property accessors in addition to identifiers 
for shorthand assignments. 

## Object Initializers

When property accessors are used in an object initializer, the property name of the accessor is 
used as the property name of the object and the value of property accessor is used as the value of 
the property in the object. This is illustrated by the following syntactic conversion:

```js
const a = { o.x };
```

is identical in its behavior to:

```js
const a = { x: o.x };
```

In addition to the dot notation for property access, bracket notation can be used as well. As with 
the dot notation, the expression of the bracket notation is used as the property name of the object. 
This is illustrated by the following syntactic conversion:

```js
const a = { o["x"] };
```

is identical in its behavior to:

```js
const a = { ["x"]: o["x"] };
```

## Destructuring Assignments

When property accessors are used in a destructuring assignment, the property name of the accessor 
is used as the property name for the assignment property, while the property accessor itself is 
used as the assignment target. This is illustrated by the following syntactic conversion:

```js
({ a.x } = o);
```

is identical in its behavior to:

```js
({ x: a.x } = o);
```

In addition to the dot notation, bracket notation can be used here as well. When the bracket 
notation is used, the expression of the bracket notation is evaluated and used as the property name
of the assignment property, while the property accessor itself is used as the assignment target.
This is illustrated by the following syntactic conversion:

```js
({ a["x"] } = o);
```

is identical in its behavior to:
```js
({ ["x"]: a["x"] } = o);
```

# Grammar

```grammarkdown
PropertyDefinition[Yield, Await]:
    MemberExpression[?Yield, ?Await] `.` IdentifierName
    MemberExpression[?Yield, ?Await] `[` Expression `]`
    CallExpression[?Yield, ?Await] `.` IdentifierName
    CallExpression[?Yield, ?Await] `[` Expression `]`
    ...

AssignmentProperty[Yield, Await]:
    MemberExpression[?Yield, ?Await] `.` IdentifierName Initializer[+In, ?Yield, ?Await]?
    MemberExpression[?Yield, ?Await] `[` Expression `]` Initializer[+In, ?Yield, ?Await]?
    CallExpression[?Yield, ?Await] `.` IdentifierName Initializer[+In, ?Yield, ?Await]?
    CallExpression[?Yield, ?Await] `[` Expression `]` Initializer[+In, ?Yield, ?Await]?
```