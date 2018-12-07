# ANTLR4 Calculator Example
ANTLR4 calculator example and explanation

# How do I set this up?
* The project uses Java 11, but you can change this in `pom.xml` if need be
* Install packages using Maven (see `pom.xml`)
* This automatically generates
 the required classes through ANTLR4
* For manually updating/regenerating the required classes
 based on the grammar, you can let Maven execute the
 antlr4:antlr4 command

# How to start?
Run the `ListenerMain.main()` or `VisitorMain.main()`  
and start typing your calculations.
Leave the calculation blank or type `exit` to exit the program.

# What is going on?
ANTLR4 generates Parser, Visitor and Listener
classes based on the `Calculator.g4` grammar
found in `src/main/antlr4/parser`.
These classes contain context and methods needed
for visiting the nodes in the parse tree or
for listening to events when entering or exiting
the nodes in a parse tree.

The `CalculationVisitor` and `CalculationListener` 
are custom classes that extend their respective 
base classes generated by ANTLR4. In this custom class,
we can overwrite the steps taken when visiting each node
in a certain expression.

# How does the Visitor work?
The Visitor visits each node in the given parse tree.
This principle is based on the
[visitor pattern](https://refactoring.guru/design-patterns/visitor).

If the node is an expression, it tries to evaluate the expression
until terminals (in this grammar: *numbers*) are found.
For non-terminals (in this grammar: *operations*),
How the operations should be applied
is defined in the visitor's methods. If a node is visited that is
a number, it is converted to Double and returned.

In our grammar, numbers are our only terminals. They are
the only tokens that need to be converted into a Java datatype
(Double, for convenience's sake). Operations, if any, are then applied
to the returned values.

The order of operations is specified in the grammar.
This is a feature of ANTLR4 through the order
of possible matches for a certain production rule.
Labels have been added to the grammar to make it easier
to identify a rule and certain tokens by name, considerably
simplifying the work the Visitor has to do.

# How does the Listener work?
The Listener listens to `enter` and `exit` events
for each rule and token matched.
These events are emitted when traversing the parse tree.
This mechanism is comparable to the 
[Observer pattern](https://refactoring.guru/design-patterns/observer)

An `enter` event occurs before traversing a node's children,
an `exit` event occurs after traversing a node's children.
Before dealing with the result of a non-terminal, its children
should be traversed first. 

In this example, a [stack-based](https://en.wikipedia.org/wiki/Stack_(abstract_data_type)) 
listener is implemented.
The stack records the last number seen according to Last In First Out.
For each number traversed a Double is pushed
to the stack. 
For each operation performed, the right number is popped off,
the left number is popped off and the result of the operation
is pushed back on the stack. 

# Where are all the classes?
ANTLR4 generates these class files
in the target directory. You can use the
maven commands defined in the `pom.xml`.

The generated classes are output to the `target/generated-sources/antlr4` directory
and reside in the project's `parser` package.
This is due to the standard configuration of ANTLR4,
matching the location of the grammar file (`src/antlr4/parser`).
No further configuration required.

# Author
Alex Rothuis: [@arothuis](https://twitter.com/arothuis)

[arothuis.nl](http://arothuis.nl)
