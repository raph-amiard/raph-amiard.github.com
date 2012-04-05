Google Summer of Code : Pluggable backend infrastructure for ClojureScript, and development of a new backend
---------------------------------------------------------------------------------------------

### Abstract

ClojureScript's compiler in it's current form is monolithic. The different phases of the compilation process are all coupled to each other and hosted in the same compilation unit. However, the analyze and parse phases of the compiler are not Javascript specific.

In fact, projects have already forked ClojureScript to target different languages, like [clojure-scheme][cljsch] for example.

This projects aims at decoupling and modularizing the backend (code emitter, target specific run-time parts) from the front end (parser and analyzer), and at providing a clear external interface for people wanting to implement their own backend, so that ideally, people wanting to target other languages could do it by creating an independent external module, rather than by forking ClojureScript itself.

### Who am I

Hi, my name is Raphaël Amiard, i am a student in Software Development at the Pierre and Marie Curie University in Paris. Although the specialty i am studying is Distributed Systems and Applications, i'm very interested in compiler design. That's why the master's project I chose is to implement a compiler using the [VmKit Framework][1]. This compiler, named [Z3][6], compiles Ocaml bytecode to LLVM. Together with the people i'm working with, we decided to make this project open source, and eventually to turn it into a full fledged ocaml runtime able to run real applications with decent performance.

I have been working on various side projects, the most significant being [Open Radio Admin][2]. Although the current version is being developped with Python/Django, the original version was developped in Clojure. It was already quite a massive project, but the youth of the Clojure web development ecosystem, together with my lack of experience regarding good web development practices, made it an easier path to redevelop the whole system using an established web development framework.

Another of my side projects has been to try and develop a Python to Lua compiler, so i have a bit of experience with source to source compilers, as ClojureScript is. However, this project died because of limitations of my approach.Most notably the differences in semantics between Python and Lua number representation made it impossible to reach decent performance.

I also have a bit of professionnal experience, mainly in the field of Web Development with Python and Django. Combining my different professionnal projects, i must have worked around 6 months full time on quite large code bases, like thoses of [Paperblog][7] (4 months) and [eloue][8] (1 month). While this is not directly relevant to this application, it means i am able to work and navigate large code bases.

The code of the Clojure version of Open Radio Admin, as well as the code for the python to lua compiler have not been open sourced, mainly because i consider them to be of not good enough quality, but i can make them disponible to you if you want to take a look !

Links:

- [My (very unfinished) homepage][3]
- [My github account][4]

### My proposal : Decoupling ClojureScript's compiler backend

I'm very interrested in the proposal named "Pluggable backend". I also consider that the realisation of this proposal should be paired with the realisation of new backend for Clojurescript. In fact, i think that most generally, it is hard to assess the quality of a modularized software system until you are effectively taking advantage of said modularity.

To go even further, i would say that the design of the backend system should be partly derived from a concrete implementation of a new backend, in the spirit of bottom up design.

I have two ideas of new possible backends for ClojureScript, an easy one and a hard one, that i'm gonna expose in more detail. I'm interested in your feedback regarding which one would be the best fit for ClojureScript, and for the scope of a GSOC project.

#### ClojureScript to Lua compiler

This is probably the easiest of the two. The positives aspects of this idea :

- Lua is a dynamic language, which is quite similar in semantics to Javascript. It is a prototype based dynamic language. This should make the port of the backend and associated runtime quite straightforward.
- Lua runtime is small and quite fast for an interpreter. It would enable a way to run Clojure scripts in a straightforward way.
- Lua has one of the most advanced JIT in existence for a dynamic language, surpassing even V8 in performance. This would possibly enable good performance. Moreover, LuaJIT, in the spirit of lua, is very lean, which would make deployment of clojurescript application in constrained environnments possible.

There are no real downsides to this alternative, except that it's maybe too easy :)

#### ClojureScript to LLVM compiler

This is a much harder alternative. I'm gonna enumerate the negative sides of this approach first because they are in some ways more relevant than the positives :

- LLVM is only a portable assembler representation, with no runtime associated to it. Realisation of this alternative would imply the development/reuse of a runtime, which most important part is the garbage collector.
- LLVM is statically typed. While the realisation of a straightforward non optimizing compiler for it might be relatively simple, the performance of such a compiler would probably be not very good, and most certainly slower than a Javascript or Lua backend, which can benefit of runtimes already optimized for dynamic languages.
- Such a backend would not emit text, but LLVM IR. This would need the use of Clojure bindings to the LLVM API.

With that said, the upsides of such an approach are quite interesting too :

- For most of the reasons cited above (Very different target language, different way of emitting code), it would also be a better test of the effectivity of the modularisation of the backend, by being very different from the Javascript backend.
- Such a backend could provide a way to run *natively compiled* Clojure code, thus also providing a concrete solution for the "Native Clojure" project.

### My vision of how i'm gonna realize this project

I have already done a preliminary analysis of the ClojureScript compiler source code. In fact i have been asking about the possibility of realizing a Lua backend for ClojureScript on Clojure's irc channel a few monthes ago, at which time i already took a look at the source code to assess the rfeasability of such a thing.

From what i have gathered, all of what should be considered the backend of the compiler is contained in the emit multimethod and related functions.

An easy way, if maybe not optimal, of decoupling the backend, would be to make this part of the compiler parametrizable, putting the existing functions and methods in a dedicated namespace, and make the backend switchable in the compiler, using the Javascript/Closure backend by default.

There may be a better decoupling possible, but i think the proper approach would be to first decouple in this way, begin development of the new backend, and then see if a common structure can be derived and factored.

The compiler infrastructure is also using Google Closure library for various things not directly related to code generation (like library/module resolution). This part would need to be redone for the selected backend, probably from scratch, though it looks like it is properly decoupled from the rest of the compiler, using said compiler externally.

#### Specifics to the LLVM alternative

Choosing the LLVM alternative would require the integration/development of a runtime. If this path were to be chosen, i'd consider using the VMKit framework, which uses the very performant MMTK garbage collector. My familiarity with this project would help the development of this project.

### Why i think i am the right person for this task

- I am very motivated to conduct this project. While i may not be the most experienced Clojure developer around, i have a good familiarity with the language, and some of the idioms that are considered good development practice. I can read it quite fluently.
- I have a good familiarity with compiler design, especially the part that is important for this project, that is source to source transformation. I have very recently realized such a compiler, in shape of the [Z3 project][6}, which while incomplete, is already capable of executing complex Ocaml programs.
- I have developped a (small, thoroughly incomplete, very partly functional) Python to Lua compiler, so i am also familiar with compilation where both the source and the target language are dynamic general purpose languages, which will be usefull in case the Lua Backend alternative is chosen.
- I have followed a course with Christian Queinnec, author of the [Lisp in small pieces][5] book, in which we hacked and modified a simple but full-featured lispy language compiler and interpreter. 
- The [Z3 project][6] is using LLVM and VMKit, and has given me skills with those which could be very usefull if the LLVM Backend alternative is chosen.
- To conclude, i am thoroughly passionate about compiler design, which i intend to make my field of work, so i am quite serious about this.

### Other points

##### To what extent are you familiar with the software you're proposing to work with?

I played around with ClojureScript, and developped one large application in Clojure, in the 1.0 era. Like i said previously, i'm probably not the most experienced Clojure developer around, but i know the language, and i'm very motivated to deepen my knowledge of it.

I'm quite familiar with the Lua language, as well as with the LLVM and VMKit projects.

##### How many hours are you going to work on this a week? 10? 20? 30? 40?

I intend to make this my full time project for the span of the GSOC. This probably means that i will work on it as long as i'm efficient, so between 30 and 40 hours a week.

##### Do you have other commitments that we should know about? If so, please suggest a way to compensate if it will take much time away from Summer of Code.

No other commitments

[1]: http://vmkit.llvm.org/
[2]: http://www.github.com/raph-amiard/OpenRadioAdmin/
[3]: http://link_to_my_page/
[4]: http://www.github.com/raph-amiard/
[5]: http://pagesperso-systeme.lip6.fr/Christian.Queinnec/WWW/LiSP.html
[6]: http://www.github.com/raph-amiard/Z3/
[7]: http://www.paperblog.com/
[8]: http://www.e-loue.com/
[cljsc]: https://github.com/takeoutweight/clojure-scheme
