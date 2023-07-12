# nEXO Code style #

## Preface ##

We create a software for a **big long-lasting project**. Maintainability and
readability are of a big importance in this case. The use of unified style in
code throughout the entire project is a strong aid on this way. The following
simple rules will help all of us to make it better.

Preordinations:

1. nEXO-offline is based on [SNiPER framework](https://github.com/SNiPER-Framework)
1. source code is in C++ and Python
1. development goes on GitHub (work style is described [elsewhere](https://ntpc.ucllnl.org/nEXO/index.php/Using_Git))
1. code in master is automatically built and tested
1. documentation should be kept with code, dedicated description for our
classes and functions will be generated from it
1. target platform is CentOS7, this is a primary testing place

SNiPER forces a modular approach to processing. We should use it
at full. Calculation type will be determined by modules selection.
Write module that does one task, but does it well.

## Language and encoding ##

We are international collaboration, joint research with institutions from 7
countries and people from many more. Major part is from North America.
So we're almost in `Latin1` encoding, but we shouldn't limit ourselves
with it, special symbols could be handy.

* Language is 'English (US)'
* Encoding is UTF-8
* End-Of-Line is LF ('\n')
* No tabulation, only spaces

Please tune your head and spell checker accordingly.
Documentation, names, output, comments — everything follows this.
Most editors/IDEs have options to set all required for your project.

## Commentary and documentation ##

Comments are for humans. Actually, code is written for humans too.
So be kind when writing both.

> Always code as if the guy who ends up maintaining your code will be
> a violent psychopath who knows where you live. © Rick Osborne

Comments are to help reader understand not _what_ code is doing, that follows
from the code itself, but _why_ it is doing this and exactly this way. Describe
your _intentions_ and _tricky things_.
Comments are communication and communication is vital. Think
about what questions a programmer may have about the code.
Consider your comments a story describing the system.

There is nothing worse than misleading comment. Keep comments in sync with code,
if you change the latter.

Unless you have (you pay to) a dedicated technical writer, documentation should
be kept with code. We will generate description for our classes and functions
automatically. Describe (almost) every function and data field. This will help
anyone (maybe even you) who will read the code later, sometimes a lot later,
when there is nobody to ask about it. We don't know whether it will be Doxygen
or something else, but let's begin forming a habit for it right now.

Examples aren't comments or documentation, but they are also very helpful.
It provides working example of use style and also serves as a testbed for
users' exercises. Add and use examples.

## Naming convention ##

Source code is written by humans and for humans. You write it for everybody,
even for yourself.

> Names are the key to program readability.
> Good names save time and brain cells (especially a year after).

Give as descriptive a name as possible, within reason. Do not worry about saving
horizontal space as it is far more important to make your code immediately
understandable by a new reader. Do not use abbreviations that are ambiguous or
unfamiliar to readers outside your project, and do not abbreviate by deleting
letters within a word. If the name is appropriate everything fits together
naturally, relationships are clear, meaning is derivable, and reasoning from
common human expectations works as expected.

As a rule of a thumb, variable name length should correspond to the variable
scope size in lines of code.

We follow style from ROOT & GEANT4, that in general is Camel Case (e.g. `fVolumeName`).
Also see: [Naming convention](https://en.wikipedia.org/wiki/Naming_convention_\(programming\))

Identifier | Convention | Example
-----------|------------|--------
Classes | Upper camel case | `MyClass`
Simple types | `typedef`s and `struct`s ends with `_t`| `Simple_t`
Enumerations | Begin with E | `EVolumeShape`
Virtual classes | Begin with V | `VBaseClass`
Mixing classes  | Begin with M | `MPrintable`
Local variables | Lower camel case | `aLocalTime`
Class members   | Begin with f | `fPosition`
Static & global | Begin with g | `gSystem`
Static members  | Begin with fg | `TView::fgToken`
Constants       | Begin with k | `kMagenta`
Getters/Setters | `Get…()`, `Set…()`, `Is…()`, `Has…()` | `IsEmpty()`
Common methods  | Use familiar names: `Print()`, `Clone()` and such |

Class covers a set of entities of the same type. It codes common behavior in
methods and keep differences in members. Avoid the temptation of bringing the
name of the class a class derives from into the derived class's name. A class
should stand on its own.

+ Name the class after what it is.

Variable is entity, it names a single object. The core of its name is noun, with
some details if necessary.

+ Name is \<noun> (with adjective)

Function or method performs an action, the core of its name is verb, and the
name should make clear what it does. This will also make functions and data
objects more distinguishable.

+ Name is \<verb> (with additions)

Example:

```C++
thing->makeit(n+na-ii214); // Bad
filter->SetQueueSize(betaEvents+alphaEvents-BiPoEvents); // Good
```

Also see: [Naming 101: Programmer's Guide](https://blog.elpassion.com/naming-101-quick-guide-on-how-to-name-things-a9bcbfd02f02)

## C++ ##

Files for MyClass should be named `MyClass.hh` and `MyClass.cc`.

Indentation should use spaces, steps equal to 2 or 4. No
tabulation is allowed.
We might want to define a style for astyle/clang-format to help
people.
Have a look into ROOT's [conventions](https://root.cern.ch/coding-conventions).

Do use [namespaces](https://en.wikipedia.org/wiki/Namespace_\(programming\)).
This helps a lot to arrange stuff in big projects.

+ **Our namespace is 'nEXO'**

Benefit from modern C++ standards, many good things appeared
after C++98.

+ **We use C++11**

Use `0` for integers, `0.0` for reals, `nullptr` for pointers, and `'\0'` for
chars. Using the correct type makes the code more readable.

Use CLHEP physical units for physical values. It really helps.

```C++
double mass = (1.3*m3)*(2.54*g/cm3);
cout << “mass is “ << (mass/kg) << “ kg” << endl;
```

Warning is a message from a compiler about dangerous stuff. Don't
ignore it. Warnings bother other people and automatic build tools. If
you don't understand how to fix — google it or ask someone.

+ You are responsible for warnings you've created

C++ is a difficult language, but it is widely used, so there is a lot of
information on the Internet. Also see:
[CppCoreGuidelines](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md),
[C++ coding standards](https://isocpp.org/wiki/faq/coding-standards).


## Python ##

Read [PEP 8](https://www.python.org/dev/peps/pep-0008/) — Style Guide for Python
Code (also available in many other languages).

But use our naming conventions.

+ **We use Python3**

Create classes and modules. Each class should have an own file. A module can
consist of several files in one directory.

Make use of NumPy and SciPy, it is a great stuff.

## Advice ##

* Increase your productivity by using an IDE like [NetBeans](https://netbeans.org/), 
[CLion](https://www.jetbrains.com/clion/), [Eclipse](https://www.eclipse.org/downloads/packages/) 
* Names should be clear, unambiguous, and descriptive
* Be sane, think modular
* Comment and document things
* Provide examples
* Improve things, rather than make it worse
* Be cautions, don't break things
* Use [Simulation maillist](https://ntpc.ucllnl.org/nEXO/index.php/Email_lists)
and [Slack channel](https://ntpc.ucllnl.org/nEXO/index.php/Communication_Tools)
* Don't hesitate to raise and discuss difficult questions
