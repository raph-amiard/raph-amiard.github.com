Google Summer of Code : Native Clojure
--------------------------------------

### Who am I

Hi, my name is Raphaël Amiard, i am a student in Software Development at the Pierre and Marie Curie University in Paris. Although the specialty i am studying is Distributed Systems and Applications, i'm very interested in compiler design. That's why the master's project I chose is to implement a compiler using the [VmKit Framework][1]. This compiler, named [Z3][6], compiles Ocaml bytecode to LLVM. Together with the people i'm working with, we decided to make this project open source, and eventually to turn it into a full fledged ocaml runtime able to run real applications with decent performance.

I have been working on various side projects, the most significant being [Open Radio Admin][2]. Although the current version is being developped with Python/Django, the original version was developped in Clojure. It was already quite a massive project, but the youth of the Clojure web development ecosystem, together with my lack of experience regarding good web development practices, made it an easier path to redevelop the whole system using an established web development framework.

Another of my side projects has been to try and develop a Python to Lua compiler, so i have a bit of experience with source to source compilers, as ClojureScript is. However, this project died because of limitations of my approach.Most notably the differences in semantics between Python and Lua number representation made it impossible to reach decent performance.

I also have a bit of professionnal experience, mainly in the field of Web Development with Python and Django. Combining my different professionnal projects, i must have worked around 6 months full time on quite large code bases, like thoses of [Paperblog][7] (4 months) and [eloue][8] (1 month). While this is not directly relevant to this application, it means i am able to work and navigate large code bases.

The code of the Clojure version of Open Radio Admin, as well as the code for the python to lua compiler have not been open sourced, mainly because i consider them to be of not good enough quality, but i can make them disponible to you if you want to take a look !

Links:

- [My (very unfinished) homepage][3]
- [My github account][4]

### My proposal : Compiling Clojure to native code with LLVM and VMKit

The basic idea behind my proposal is to compile Clojure to native code using the VMKit infrastructure. The benefits of this approach are numerous :

- Possibility of creating AOT compiled clojure executables
- Reliability and quality of the components used : VMKit has a solid JVM implementation, LLVM is used by industrial grade projects, and MMTK, VMKit's garbage collector, implements some of the most cutting edge collection algorithms to date.

Two approaches are possible, with different drawbacks :

#### Compiling Clojure java implementation with J3

The first one is compiling clojure 'as-is' with J3, VMKit's JVM implementation. This is the easiest way. It would require fiddling with VMKit, as in it's current state, it is not totally well suited for ahead of time compilation. With that said, the project has the possibility of compiling large projects statically, although thoses capacities are not properly exposed.

#### Compiling Clojure directly to LLVM, using VMKit as a runtime

This approach would require creating a new VMKit virtual machine from scratch, as it has already been done for the Z3 project, and adapting the clojure compiler and runtime to run on it. This is a much more ambitious approach. An option would be adapting the ClojureScript compiler, which would probably be an easier path. The details of this approach are more thoroughly detailed in [my other GSOC application][9]

### Why i think i am the right person for this task

- I am very motivated to conduct this project. While i may not be the most experienced Clojure developer around, i have a good familiarity with the language, and some of the idioms that are considered good development practice. I can read it quite fluently.
- I have a good familiarity with compiler design, especially the part that is important for this project, that is source to source transformation. I have very recently realized such a compiler, in shape of the [Z3 project][6}, which while incomplete, is already capable of executing complex Ocaml programs.
- I have followed a course with Christian Queinnec, author of the [Lisp in small pieces][5] book, in which we hacked and modified a simple but full-featured lispy language compiler and interpreter. 
- The [Z3 project][6] is using LLVM and VMKit. This project is not finished, and i will continue to work on it during the next months. The knowledge i'm gaining about LLVM and VMKit is a key asset in the realisation of this GSOC project.
- To conclude, i am thoroughly passionate about compiler design, which i intend to make my field of work, so i am quite serious about this.

### Other points

##### To what extent are you familiar with the software you're proposing to work with?

I developped one modestly large application in Clojure in the 1.0 era, and played around with ClojureScript. Like i said previously, i'm probably not the most experienced Clojure developer around, but i know the language, and i'm very motivated to deepen my knowledge of it.

I'm familiar with LLVM and VMKit. In the course of the Z3 project, we had to modify VMKit, and we probably will have to do it again, which is giving me a quite good understanding about it's internals.

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
