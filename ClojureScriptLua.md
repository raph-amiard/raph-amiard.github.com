Google Summer of Code : Pluggable backend infrastructure for ClojureScript, and development of a new backend
---------------------------------------------------------------------------

### Abstract

ClojureScript's compiler in it's current form is monolithic. There are two main ways the ClojureScript compiler is tied to the javascript language at the moment: 

- The different phases of the compilation process are all coupled to each other and hosted in the same compilation unit. More specifically, the part of the code that handles code generation is hosted in the same namespace as the part of the code that handles parsing and analysis, and some helper functions and macro are directly tying them together.
- The core compiler (cljs.compiler) is organized as a library, with some helpers function to use it in a REPL. The user-facing compiler is provided by the cljs.closure module, using google Closure library to organize the compilation services provided by the cljs.compiler namespace into a full-fledged compiler. That part is also totally javascript specific.

Projects have already forked ClojureScript to target different languages, like [clojure-scheme][cljsc] for example. However, this approach is clearly not the most optimal if you want to maintain the different backends easily.

This projects aims at decoupling and modularizing the backend (code emitter, target specific run-time parts) from the front end (parser and analyzer), and at providing a clear external interface for people wanting to implement their own backend, so that ideally, people wanting to target other languages could do it by creating an independent external module, rather than by forking ClojureScript itself.

This project would thus provide a fully modularized JavaScript/Closure backend for ClojureScript, using the newly developped API to interface with the core of the ClojureScript compiler.

Additionally, this project would deliver a new target for the ClojureScript compiler, as a proof of concept of the efficiency of the modularization process. The development of the new target would be paired with the developpement of a new user-facing compiler module (equivalent to the cljs.closure module for javascript). In the process of developing this module, attempts would be made to factor the common parts of a user facing ClojureScript compiler module into a separate module, eventually providing an interface and helpers to people wanting to develop their own user facing compilers for their new backends.

The newly developped backend would target the Lua language.  Lua has been chosen for the following reasons :

- Lua is a dynamic language, which is quite similar in semantics to Javascript. It is a prototype based dynamic language. This should make the port of the backend and associated runtime quite straightforward.
- Lua runtime is small and quite fast for an interpreter. It would enable a way to run Clojure scripts in a straightforward way.
- Lua is extremely small and portable, opening the possibility of running Clojure code on a [very large number of platforms][luadistribs]
- Lua has one of the most advanced JIT in existence for a dynamic language, surpassing even V8 in performance. This would possibly enable good performance for the resulting backend. Moreover, LuaJIT, in the spirit of lua, is very lean, which would make deployment of clojurescript applications in constrained environnments possible.

### Implementation plan

The project has the following main phases:

1. Modularize the backend (code generation) of the cljs.compiler module), and create an interface to which backends must conform.
2. Move the Javascript code generation into a separate module that conforms to the defined interface. Move the js-specific helper functions of the cljs.compiler module into their own module.
3. Create the new ClojureScript to Lua backend as a module conforming to the defined interface.
4. Create helpers and a user facing compiler module for the lua backend offering similar functionnality as exists for Javascript.
5. If possible and interresting, decouple target agnostic functionnality of the user facing compiler module into a separate module.

### Deliverables

- A fork of the master branch of the ClojureScript compiler in which the two first phases of the Implementation plan, and eventually the fifth, are realized.
- A new backend developed living in a separate repository, compiling ClojureScript to Lua according to phases 3 and 4.

### Timeline

**April 20 to May 21**

Familiarize further with ClojureScript's code. Set up a good development workflow (version control, tools, etc.).Make myself known to the various people working on ClojureScript and related projects. Begin prototyping ideas and testing approaches as to how the various parts of the project could be implemented.

**May 21 to June 6**

Define the interface ClojureScript's backends have to conform to. Create the interface, and modularize the javascript code generator, by putting it into a separate module and making it conform to the defined interface.

**June 6 to June 12**

Test and document the created code and interfaces. Create a module that enables the use of the JS backend on an repl in the same fashion than possible now. Adapt the cljs.closure user facing compiler to the refactored core compiler.

**June 13 to June 30**

Develop the lua backend as a module conforming to the defined interface. Adapt said interfaces if unforeseen needs arises, and adapt the JS backend accordingly.

**June 30 to July 13**

Test and document the lua backend. Create facilities so that it can be used on a clojure repl.

**July 14 to July 21**

Create a user facing compiler module. Define the protocol by which clojure namespaces and files maps to Lua concepts. Document the eventual Lua libraries dependencies added by the user facing compiler.

**July 21 to July 30**

Decouple target agnostic parts of the user facing compiler into a separate module, if possible. Document the created module. If not possible/interresting, document the process by which a new user facing compiler module is created.

**July 31 to August 13**

Create code examples and demos for the Lua backend, document it's use. Eventually create small bindings/helpers for a lua library (Mike Pall's LuaJIT ffi library would be particularly interresting).
Further refine and document existing code. Take time to take further input from all the communities involved (Clojure, ClojureScript, Lua) regarding the created code. 

### Related projects

- [clojure-scheme][cljsc] is a project that targets scheme from the original ClojureScript compiler. It modified the appropriate code directly though, without trying to modularize code generation.
- [CinC][cinc] (Clojure in Clojure) is a project leveraging the ClojureScript compiler to create a full Clojure compiler in Clojure, targeting the JVM. Modular backends is a goal too. It is a similar but more ambitious project than the one presented here.
- [clojure-py][cljpy] is a project aiming to compile Clojure to python. It doesn't use the ClojureScript compiler though, a full compiler has been reimplemented in Python.

[cljsc]: https://github.com/takeoutweight/clojure-scheme
[luadistribs]: http://lua-users.org/wiki/LuaDistributions
[cinc]: https://github.com/remleduff/CinC
[cljpy]: https://github.com/halgari/clojure-py/

### About me

#### General Information

- Raphaël Amiard
- Lives in Paris, France
- Full contact information here
- Student in Software Development at the Pierre and Marie Curie University - Paris
- Specialized in Distributed Systems and Applications

#### Personal Projects

- [My github page][github]
- [Z3][z3], a Ocaml bytecode to [LLVM][llvm] compiler, leveraging the [VMKit Framework][vmkit]
- [RAD|io][rad] Radio Administration Deck, amongst other things a web interface to the [liquidsoap project][liquidsoap] enabling the complete administration of the sound stream of a web radio.
- [RadioZeroZero][rzz], a public french web radio powered by RAD|io

[z3]: http://www.github.com/raph-amiard/Z3/
[rad]: http://www.github.com/raph-amiard/RAD-io/
[rzz]: http://www.radiozerozero.com/
[github]: http://www.github.com/raph-amiard/
[llvm]: http://www.llvm.org/

#### Relevant professionnal experience

I worked for approximately 6 months as a full time Django/Python developper for the following web sites:

- [Paperblog][ppb]
- [Eloue][eloue]

[ppb]: http://www.paperblog.com/
[eloue]: http://www.e-loue.com/

#### Why are you the right person for this task?

- I am very motivated to conduct this project. While i may not be the most experienced Clojure developer around, i have a good familiarity with the language, and some of the idioms that are considered good development practice. I can read it quite fluently.
- I have a good familiarity with compiler design, especially the part that is important for this project, that is source to source transformation. I have very recently realized such a compiler, in shape of the [Z3 project][z3], which while incomplete, is already capable of executing complex Ocaml programs.
- I have developped a (small, thoroughly incomplete, very partly functional) Python to Lua compiler, so i am also familiar with compilation where both the source and the target language are dynamic general purpose languages, which will be usefull in case the Lua Backend alternative is chosen.
- I have followed a course with Christian Queinnec, author of the [Lisp in small pieces][lisp] book, in which we hacked and modified a simple but full-featured lispy language compiler and interpreter. 
- To conclude, i am thoroughly passionate about compiler design, which i intend to make my field of work, so i am quite serious about this.

[lisp]: http://pagesperso-systeme.lip6.fr/Christian.Queinnec/WWW/LiSP.html

#### To what extent are you familiar with the software you're proposing to work with? Have you used it? Have you read the source? Have you modified the source?

I played around with ClojureScript, and developped one large application in Clojure, in the 1.0 era. Like i said previously, i'm probably not the most experienced Clojure developer around, but i know the language, and i'm very motivated to deepen my knowledge of it.

I read a good part of the source of the ClojureScript compiler.

I'm quite familiar with the Lua language.

#### How many hours are you going to work on this a week? 10? 20? 30? 40?

I intend to make this my full time project for the span of the GSOC. This probably means that i will work on it as long as i'm efficient, so between 30 and 40 hours a week.

#### Do you have other commitments that we should know about? If so, please suggest a way to compensate if it will take much time away from Summer of Code.

No other commitments

#### Where do you live, and can we assign a mentor who is local to you so you can meet in a coffee shop for lunch?

I live in France, so probably not. Virtual video meetings would be possible though.

#### Are you comfortable working independently under a supervisor or mentor who is several thousand miles away, not to mention 12 time zones away? How will you work with your mentor to track your work? Have you worked in this style before?

I already worked remotely as part of my professionnal projects, using tools such as version control systems, instant messaging, distant code reviews, etc. The time difference between France and USA is manageable, and since GSOC would be my main summer project, i could adapt to my mentor's schedule.

#### If your native language is not English, are you comfortable working closely with a supervisor whose native language is English? What is your native language, as that may help us find a mentor who has the same native language?

Although most of this work has been conducted in french, i'm very comfortable communicating in english. And i'm doing so on a daily basis.

