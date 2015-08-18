# State of the Haskell ecosystem - August 2015

In this post I will describe the current state of the Haskell ecosystem to the
best of my knowledge and its suitability for various programming domains and
tasks.  The purpose of this post is to discuss both the good and the bad by
advertising where Haskell shines while highlighting where I believe there is
room for improvement.

This post is grouped into two sections: the first section covers Haskell's
suitability for particular programming application domains (i.e. servers,
games, or data science) and the second section covers Haskell's suitability
for common general-purpose programming needs (such as testing, IDEs, or
concurrency).

The topics are roughly sorted from greatest strengths to greatest weaknesses.
Each programming area will also be summarized by a single rating of either:

* **Best in class**: the best experience in any language
* **Mature**: suitable for most programmers
* **Immature**: only acceptable for early-adopters
* **Bad**: pretty unusable

The more positive the rating the more I will support the rating with
success stories in the wild.  The more negative the rating the more I will
offer constructive advice for how to improve things.

**Disclaimer #1:** I obviously don't know everything about the Haskell
ecosystem, so whenever I am unsure I will make a ballpark guess and clearly
state my uncertainty in order to solicit opinions from others who have more
experience.  I keep tabs on the Haskell ecosystem pretty well, but even this
post is stretching my knowledge.  If you believe any of my ratings are
incorrect, I am more than happy to accept corrections (both upwards and
downwards)

**Disclaimer #2:** There are some "Educational resource" sections below which
are remarkably devoid of books, since I am not as familiar with
textbook-related resources.  If you have suggestions for textbooks to add,
please let me know.

**Disclaimer #3:** I am very obviously a Haskell fanboy if you haven't guessed
from the name of my blog and I am also an author of several libraries mentioned
below, so I'm highly biased.  I've made a sincere effort to honestly appraise
the language, but please challenge my ratings if you believe that my bias is
blinding me!  I've also clearly marked Haskell sales pitches as "Propaganda"
in my external link sections.  :)

## Table of Contents

* [Application Domains](#application-domains)
  * [Compilers](#compilers)
  * [Server-side web programming](#server-side-web-programming)
  * [Scripting / Command-line applications](#scripting--command-line-applications)
  * [Numerical programming](#numerical-programming)
  * [Client-side web programming](#client-side-web-programming)
  * [Distributed programming](#distributed-programming)
  * [Standalone GUI applications](#standalone-gui-applications)
  * [Machine learning](#machine-learning)
  * [Data science](#data-science)
  * [Game programming](#game-programming)
  * [Systems / embedded programming](#systems--embedded-programming)
  * [Mobile apps](#mobile-apps)
* [Common Programming Needs](#common-programming-needs)
  * [Refactoring](#refactoring)
  * [Type system](#type-system)
  * [Domain-specific languages (DSLs)](#domain-specific-languages-dsls)
  * [Testing](#testing)
  * [Concurrency](#concurrency)
  * [Data structures and algorithms](#data-structures-and-alogirthms)
  * [Benchmarking](#benchmarking)
  * [Unicode](#unicode)
  * [Parsing / Pretty-printing](#parsing--pretty-printing)
  * [Stream programming](#stream-programming)
  * [Serialization / Deserialization](#serialization--deserialization)
  * [Support for file formats](#support-for-file-formats)
  * [Package management](#package-management)
  * [Logging](#logging)
  * [Databases and data stores](#databases-and-data-stores)
  * [IDE support](#ide-support)

# Application Domains
<br>

## Compilers

**Rating:** Best in class

Haskell is an amazing language for writing your own compiler.  If you are
writing a compiler in another language you should genuinely consider switching.

Haskell originated in academia, and most languages of academic origin (such as
the ML family of languages) excel at compiler-related tasks for obvious
reasons.  As a result the language has a rich ecosystem of libraries dedicated
to compiler-related tasks, such as parsing, pretty-printing, unification,
bound variables, syntax tree manipulations, and optimization.

Anybody who has ever written a compiler knows how difficult they are to
implement because by necessity they manipulate very weakly typed data
structures (trees and maps of strings and integers).  Consequently, there is a
huge margin for error in everything a compiler does, from type-checking to
optimization, to code generation.  Haskell knocks this out of the park, though,
with a really powerful type system with many extensions that can eliminate
large classes of errors at compile time.

I also believe that there are many excellent educational resources for compiler
writers, both papers and books.  I'm not the best person to summarize all the
educational resources available, but the ones that I have read have been very
high quality.

Finally, there are a large number of parsers and pretty-printers for other
languages which you can use to write compilers to or from these languages.

**Notable libraries:**

* [`parsec`](https://hackage.haskell.org/package/parsec) / [`attoparsec`](https://hackage.haskell.org/package/attoparsec) / [`trifecta`](https://hackage.haskell.org/package/trifecta) / [`alex`](https://hackage.haskell.org/package/alex)+[`happy`](https://hackage.haskell.org/package/happy) - parsing libraries
* [`bound`](https://hackage.haskell.org/package/bound) - manipulating bound variables
* [`hoopl`](https://hackage.haskell.org/package/hoopl) - optimization
* [`wl-pprint`](https://hackage.haskell.org/package/wl-pprint) / [`ansi-wl-pprint`](https://hackage.haskell.org/package/ansi-wl-pprint) - pretty-printing
* [`llvm-general`](https://hackage.haskell.org/package/llvm-general) - LLVM API
* `language-`{[`javascript`](https://hackage.haskell.org/package/language-javascript)|[`python`](https://hackage.haskell.org/package/language-python)|[`c-quote`](https://hackage.haskell.org/package/language-c-quote)|[`lua`](https://hackage.haskell.org/package/language-lua)|[`java`](https://hackage.haskell.org/package/language-java)|[`objc`](https://hackage.haskell.org/package/language-objc)} - parsers and
   pretty-printers for other languages

**Some compilers written in Haskell:**

* [`Elm`](http://elm-lang.org)
* [`Purescript`](http://www.purescript.org)
* [`Idris`](http://www.idris-lang.org)
* [`Agda`](http://wiki.portal.chalmers.se/agda/pmwiki.php)
* [`Pugs`](http://www.perlfoundation.org/perl6/index.cgi?pugs) (the first Perl 6 implementation)
* [`ghc`](https://www.haskell.org/ghc/) (self-hosting)
* [`frege`](https://github.com/Frege/frege) (very similar to Haskell, also self-hosting)

**Educational resources:**

* [Write you a Haskell](http://dev.stephendiehl.com/fun/)
* [A Tutorial Implementation of a Dependently Typed Lambda Calculus](http://www.andres-loeh.de/LambdaPi/)

<br>

## Server-side web programming

**Rating:** Mature

Haskell's second biggest strength is the back-end, both for web applications
and services.  The main features that the language brings to the table are:

* Server stability
* Performance
* Ease of concurrent programming
* Excellent support for web standards

The strong type system and polished runtime greatly improve server stability
and simplify maintenance.  This is the greatest differentiator of Haskell from
other backend languages, because it significantly reduces the
total-cost-of-ownership.  You should expect that you can maintain Haskell-based
services with significantly fewer programmers than other languages, even when
compared to other statically typed languages.

However, the greatest weakness of server stability is space leaks.  The most
common solution that I know of is to use `ekg` (a process monitor) to examine
a server's memory stability before deploying to production.  The second most
common solution is to learn to detect and prevent space leaks with experience,
which is not as hard as people think.

Haskell's performance is excellent and currently comparable to Java.  Both
languages give roughly the same performance in beginner or expert hands,
although for different reasons.

Where Haskell shines in usability is the runtime support for the following
three features:

* lightweight threads enhanced (which differentiate Haskell from the JVM)
* software transactional memory (which differentiate Haskell from Go)
* garbage collection (which differentiate Haskell from Rust)

Many languages support two of the above three features, but Haskell is the only
one that I know of that supports all three.

If you have never tried out Haskell's software transactional memory you should
really, really, really give it a try, since it eliminates a large number of
concurrency logic bugs.  STM is far and away the most underestimated feature
of the Haskell runtime.

**Notable libraries:**

* [`warp`](https://hackage.haskell.org/package/warp) / [`wai`](https://hackage.haskell.org/package/wai) - the low-level server and API that all server libraries share, with the exception of `snap`
* [`scotty`](https://hackage.haskell.org/package/scotty) - A beginner-friendly server framework analogous to Ruby's Sinatra
* [`yesod`](https://hackage.haskell.org/package/yesod) / [`yesod-*`](https://hackage.haskell.org/packages/search?terms=yesod) / [`snap`](https://hackage.haskell.org/package/snap) / [`snap-*`](https://hackage.haskell.org/packages/search?terms=snap) - "Enterprise" server frameworks with all the bells and whistles
* [`servant`](https://hackage.haskell.org/package/servant) / [`servant-*`](https://hackage.haskell.org/packages/search?terms=servant) - This server framework might blow your mind
* [`authenticate`](https://hackage.haskell.org/package/authenticate) / [`authenticate-*`](https://hackage.haskell.org/packages/search?terms=authenticate) - Shared authentication libraries
* [`ekg`](https://hackage.haskell.org/package/ekg) / [`ekg-*`](https://hackage.haskell.org/packages/search?terms=ekg) - Haskell service monitoring
* [`stm`](https://hackage.haskell.org/package/stm) - Software-transactional memory

**Some web sites and services powered by Haskell:**

* Facebook's spam filter: Sigma
* [elm-lang.org](http://elm-lang.org/)
* [glot.io](http://glot.io/)
* [The Perry Bible Fellowship](http://pbfcomics.com/)
* [Silk](https://www.silk.co)
* [Shellcheck](http://www.shellcheck.net/)
* [instantwatcher.com](http://instantwatcher.com/)

**Propaganda:**

* [Fighting spam with Haskell - Haskell in production, at scale, at Facebook](https://code.facebook.com/posts/745068642270222/fighting-spam-with-haskell/)
* [Mio: A High-Performance Multicore IO Manager for GHC](http://haskell.cs.yale.edu/wp-content/uploads/2013/08/hask035-voellmy.pdf)
* [The Performance of Open Source Applications - Warp](http://www.aosabook.org/en/posa/warp.html)
* [Optimising Garbage Collection Overhead in Sigma](https://simonmar.github.io/posts/2015-07-28-optimising-garbage-collection-overhead-in-sigma.html)
* instantwatcher.com author comments on rewrite from Ruby to Haskell - [\[1\]](http://www.reddit.com/r/haskell/comments/3am3qu/should_i_put_a_powered_by_haskell_tag_w_haskell/csef8eq)
[\[2\]](http://www.reddit.com/r/haskell/comments/3e10ea/til_instantwatcher_is_made_with_haskell/ctavz2m)

**Educational resources:**

* [Making a Website With Haskell](http://adit.io/posts/2013-04-15-making-a-website-with-haskell.html)
* [Beautiful concurrency](https://www.fpcomplete.com/school/advanced-haskell/beautiful-concurrency) - a software-transactional memory tutorial
* [The Yesod book](http://www.yesodweb.com/book)
* [The Servant tutorial](http://haskell-servant.github.io/tutorial/)

<br>

## Scripting / Command-line applications

**Rating:** Mature

Haskell's biggest advantage as a scripting language is that Haskell is the
most widely adopted language that support global type inference.  Many
languages support local type inference (such as Rust, Go, Java, C#), which
means that function argument types and interfaces must be declared but
everything else can be inferred.  In Haskell, you can omit everything: all
types and interfaces are completely inferred by the compiler (with some
caveats, but they are minor).

Global type inference gives Haskell the feel of a scripting language while
still providing static assurances of safety.  Script type safety matters in
particular for enterprise environments where glue scripts running with elevated
privileges are one of the weakest points in these software architectures.

The second benefit of Haskell's type safety is ease of script maintenance.
Many scripts grow out of control as they accrete arcane requirements and once
they begin to exceed 1000 LOC they become difficult to maintain in a
dynamically typed language.  People rarely budget sufficient time to create a
sufficiently extensive test suite that exercises every code path for each and
every one of their scripts.  Having a strong type system is like getting a
large number of auto-generated tests for free that exercise all script code
paths.  Moreover, the type system is more resilient to refactoring than a test
suite.

However, the main reason I mark Haskell as mature because the language is also
usable even for simple one-off disposable scripts.  These Haskell scripts are
comparable in size and simplicity to their equivalent Bash or Python scripts.
This lets you easily start small and finish big.

Haskell has one advantage over many dynamic scripting languages, which is that
Haskell can be compiled into a native and statically linked binary for
distribution to others.

Haskell's scripting libraries are feature complete and provide all the
niceties that you would expect from scripting in Python or Ruby, including
features such as:

* rich suite of Unix-like utilities
* advanced sub-process management
* POSIX support
* light-weight idioms for exception safety and automatic resource disposal

**Notable libraries:**

* [`shelly`](https://hackage.haskell.org/package/shelly) / [`turtle`](https://hackage.haskell.org/package/turtle) - scripting libraries (Full disclosure: I authored `turtle`)
* [`optparse-applicative`](https://hackage.haskell.org/package/optparse-applicative) / [`cmdargs`](https://hackage.haskell.org/package/cmdargs) - command-line argument parsing
* [`haskeline`](https://hackage.haskell.org/package/haskeline) - a complete Haskell implementation of `readline` for console
  building
* [`process`](https://hackage.haskell.org/package/process) - low-level library for sub-process management

**Some command-line tools written in Haskell:**

* [`pandoc`](https://hackage.haskell.org/package/pandoc)
* [`git-annex`](https://hackage.haskell.org/package/git-annex)

**Educational resources:**

* [Shelly: Write your shell scripts in Haskell](http://www.yesodweb.com/blog/2012/03/shelly-for-shell-scripts)
* [Use Haskell for shell scripting](http://www.haskellforall.com/2015/01/use-haskell-for-shell-scripting.html)

<br>

## Numerical programming

**Rating:** Immature? (Uncertain)

Haskell's numerical programming story is not ready, but steadily improving.

My main experience in this area was from a few years ago doing numerical
programming for bioinformatics that involved a lot of vector and matrix
manipulation and my rating is largely colored by that experience.

The biggest issues that the ecosystem faces are:

* Really clunky matrix library APIs
* Fickle rewrite-rule-based optimizations

When the optimizations work they are amazing and produce code competitive with
C.  However, small changes to your code can cause the optimizations to
suddenly not trigger and then performance drops off a cliff.

There is one Haskell library that avoids this problem entirely which I believe
holds a lot of promise: `accelerate` generates LLVM and CUDA code at runtime
and does not rely on Haskell's optimizer for code generation, which side-steps
the problem.  `accelerate` has a large set of supported algorithms that you
can find by just checking the library's reverse dependencies:

* [Reverse dependencies of `accelerate`](http://packdeps.haskellers.com/reverse/accelerate)

However, I don't have enough experience with `accelerate` or enough familiarity
with numerical programming success stories in Haskell to vouch for this just
yet.  If somebody has more experience then me in this regard and can provide
evidence that the ecosystem is mature then I might consider revising my rating
upward.

**Notable libraries:**

* [`accelerate`](https://hackage.haskell.org/package/accelerate) / [`accelerate-*`](https://hackage.haskell.org/packages/search?terms=accelerate) - GPU programming
* [`vector`](https://hackage.haskell.org/package/vector) - high-performance arrays
* [`repa`](https://hackage.haskell.org/package/repa) / [`repa-*`](https://hackage.haskell.org/packages/search?terms=repa) - parallel shape-polymorphic arrays
* [`hmatrix`](https://hackage.haskell.org/package/hmatrix) / [`hmatrix-*`](https://hackage.haskell.org/packages/search?terms=hmatrix) - Haskell's BLAS / LAPACK wrapper
* [`ad`](https://hackage.haskell.org/package/ad) - automatic differentiation

**Propaganda:**

* [Exploiting vector instructions with generalized stream fusion](http://research.microsoft.com/en-us/um/people/simonpj/papers/ndp/haskell-beats-C.pdf)
* [Type-safe Runtime Code Generation: Accelerate to LLVM](http://www.cse.unsw.edu.au/~chak/papers/acc-llvm.pdf)

**Educational Resources:**

* [Parallel and concurrent programming in Haskell](http://chimera.labs.oreilly.com/books/1230000000929/index.html)

<br>

## Client-side web programming

**Rating:** Immature

This boils down to Haskell's ability to compile to Javascript.  `ghcjs` is the
front-runner, but for a while setting up `ghcjs` was non-trivial.  However,
`ghcjs` appears to be very close to having a polished setup story now that
`ghc-7.10.2` is out
([Source](https://github.com/commercialhaskell/stack/issues/337#issuecomment-114876426)).

One of the distinctive features of `ghcjs` compared to other competing
Haskell-to-Javascript compilers is that a huge number of Haskell libraries work
out of the box with `ghcjs` because it supports most Haskell primitive
operations.

I would also like to mention that there are two Haskell-like languages that
you should also try out for front-end programming: `elm` and `purescript`.
These are both used in production today and have equally active maintainers and
communities of their own.

**Areas for improvement:**

* There needs to be a clear story for smooth integration with existing
  Javascript projects
* There need to be many more educational resources targeted at non-experts
  explaining how to translate existing front-end programming idioms to Haskell
* There need to be several well-maintained and polished Haskell libraries for
  front-end programming

**Notable Haskell-to-Javascript compilers:**

* [`ghcjs`](https://github.com/ghcjs/ghcjs)
* [`haste`](https://hackage.haskell.org/package/haste)

**Notable libraries:**

* [reflex-dom](https://hackage.haskell.org/package/reflex-dom) - Functional reactive programming library for DOM manipulation

<br>

## Distributed programming

**Rating:** Immature

This is sort of a broad area since I'm using this topic to refer to both
distributed computation (for analytics) and distributed service architectures.
However, in both regards Haskell is lagging behind its peers.

The JVM, Go, and Erlang have much better support for this sort of things,
particularly in terms of libraries.

There has been a lot of work in replicating Erlang-like functionality in
Haskell through the Cloud Haskell project, not just in creating the low-level
primitives for code distribution / networking / transport, but also in
assembling a Haskell analog of Erlang's OTP.  I'm not that familiar with how
far progress is in this area, but people who love Erlang should check out
Cloud Haskell.

**Areas for improvement:**

* We need more analytics libraries.  Haskell has no analog of `scalding` or
  `spark`.  The most we have is just a Haskell wrapper around `hadoop`
* We need a polished consensus library (i.e. a high quality Raft
  implementation in Haskell)

**Notable libraries:**

* [`distributed-process`](https://hackage.haskell.org/package/distributed-process) / [`distributed-process-*`](https://hackage.haskell.org/packages/search?terms=distributed-process) - Haskell analog to Erlang
* [`hadron`](https://github.com/Soostone/hadron) - Haskell wrapper around `hadoop`
* [`aws`](https://hackage.haskell.org/package/aws) / [`aws-*`](https://hackage.haskell.org/packages/search?terms=aws) - Amazon web services libraries

<br>

## Standalone GUI applications

**Rating:** Immature

Haskell really lags behind the C# and F# ecosystem in this area.

My experience on this is based on several private GUI projects I wrote several
years back.  Things may have improved since then so if you think my assessment
is too negative just let me know.

All Haskell GUI libraries are wrappers around toolkits written in other
languages (such as GTK+ or Qt).  The last time I checked the `gtk` bindings
were the most comprehensive, best maintained, and had the best documentation.

However, the Haskell bindings to GTK+ have a strongly imperative feel to them.
The way you do everything is communicating between callbacks by mutating
`IORef`s.  Also, you can't take extensive advantage of Haskell's awesome
threading features because the GTK+ runtime is picky about what needs to happen
on certain threads.  I haven't really seen a Haskell library that takes this
imperative GTK+ interface and wraps it in a more idiomatic Haskell API.

My impression is that most Haskell programmers interested in applications
programming have collectively decided to concentrate their efforts on improving
Haskell web applications instead of standalone GUI applications.  Honestly,
that's probably the right decision in the long run.

Another post that goes into more detail about this topic is this post written
by Keera Studios:

* [On the state of GUI programming in Haskell](http://keera.co.uk/blog/2014/05/23/state-gui-programming-haskell/)

**Areas for improvement:**

* A GUI toolkit binding that is maintained, comprehensive, and easy to use
* Polished GUI interface builders

**Notable libraries:**

* [`gtk`](https://hackage.haskell.org/package/gtk) / [`glib`](https://hackage.haskell.org/package/glib) / [`cairo`](https://hackage.haskell.org/package/cairo) / [`pango`](https://hackage.haskell.org/package/pango) - The GTK+ suite of libraries
* [`wx`](https://hackage.haskell.org/package/wx) - wxWidgets bindings
* [`X11`](https://hackage.haskell.org/package/X11) - X11 bindings
* [`threepenny-gui`](https://hackage.haskell.org/package/threepenny-gui) - Framework for local apps that use the web browser as the
  interface
* [`hsqml`](http://hackage.haskell.org/package/hsqml) - A Haskell binding for Qt Quick, a cross-platform framework for creating graphical user interfaces.
* [`fltkhs`](http://hackage.haskell.org/package/fltkhs) - A Haskell binding to FLTK. Easy install/use, cross-platform, self-contained executables.

**Some example applications:**

* [`xmonad`](http://xmonad.org)

**Educational resources:**

* [Haskell port of the GTK tutorial](http://code.haskell.org/gtk2hs/docs/tutorial/Tutorial_Port/)
* [Building pragmatic user interfaces in Haskell with HsQML](https://www.youtube.com/watch?v=JCSxWfUvi6o)

<br>

## Machine learning

**Rating:** Immature? (Uncertain)

This area has been pioneered almost single-handedly by one person: Mike
Izbicki.  He maintains the `HLearn` suite of libraries for machine learning
in Haskell.

I have essentially no experience in this area, so I can't really rate it that
well.  However, I'm pretty certain that I would not rate it mature because I'm
not aware of any company successfully using machine learning in Haskell.

For the same reason, I can't really offer constructive advice for areas for
improvement.

If you would like to learn more about this area the best place to begin is the
Github page for the `HLearn` project:

* [Github repository for `HLearn`](https://github.com/mikeizbicki/HLearn)

**Notable libraries:**
* [`HLearn-*`](https://hackage.haskell.org/packages/search?terms=HLearn)

<br>

## Data science

**Rating:** Immature

Haskell really lags behind Python and R in this area.  Haskell is somewhat
usable for data science, but probably not ready for expert use under deadline
pressure.

I'll primarily compare Haskell to Python since that's the data science
ecosystem that I'm more familiar with.  Specifically, I'll compare to the
`scipy` suite of libraries:

The Haskell analog of `NumPy` is the `hmatrix` library, which provides Haskell
bindings to BLAS, LAPACK.  `hmatrix`'s main limitation is that the API is a bit
clunky, but all the tools are there.

Haskell's charting story is "okay", but still not good.  All of the Haskell
libraries for charting that I've tried have been heavyweight and poorly
documented.  To pick on one library as an example, the `Chart` library does not
have a single minimal end-to-end code example that I can find anywhere in the
Hackage documentation.  I was eventually able to chart something once I figured
out the right incantation but it should not have taken so long.

Fortunately, Haskell does integrate into IPython so you can use Haskell within
an IPython shell or an online notebook.  For example, there is an online
"IHaskell" notebook that you can use right now located here:

* [IHaskell notebook](https://try.jupyter.org/) - Click on "Welcome to Haskell.ipynb"

If you want to learn more about how to setup your own IHaskell notebook, visit
this project:

* [IHaskell Github repository](https://github.com/gibiansky/IHaskell)

The closest thing to Python's `pandas` is the `frames` library.  I haven't used
it that much personally so I won't comment on it much other than to link to
some tutorials in the Educational Resources section.

I'm not aware of a Haskall analog to `SciPy` (the library) or `sympy`.  If
you know of an equivalent Haskell library then let me know.

One Haskell library that deserves honorable mention here is the `diagrams`
library which lets you produce complex data visualizations very easily if
you want something a little bit fancier than a chart.  Check out the `diagrams`
project if you have time:

* [The Diagrams project](http://projects.haskell.org/diagrams/)
* [Gallery of example diagrams](http://projects.haskell.org/diagrams/gallery.html)

**Areas for improvement:**

* Smooth user experience and integration across all of these libraries
* Simple types and APIs.  The data science programmers I know dislike overly
  complex or verbose APIs
* Beautiful data visualizations with very little investment

**Notable libraries:**

* [`cassava`](https://hackage.haskell.org/package/cassava) - CSV encoding and decoding
* [`hmatrix`](https://hackage.haskell.org/package/hmatrix) - BLAS / LAPACK wrapper
* [`Frames`](https://hackage.haskell.org/package/Frames) - Haskell data analysis tool analogous to Python's `pandas`
* [`statistics`](https://hackage.haskell.org/package/statistics) - Statistics (duh!)
* [`Chart`](https://hackage.haskell.org/package/Chart) / [`Chart-*`](https://hackage.haskell.org/packages/search?terms=Chart) - Charting library
* [`diagrams`](https://hackage.haskell.org/package/diagrams) / [`diagrams-*`](https://hackage.haskell.org/packages/search?terms=diagrams) - Vector graphics library
* [`ihaskell`](https://hackage.haskell.org/package/ihaskell) - Haskell backend to IPython

<br>

## Game programming

**Rating:** Immature? / Bad?

Haskell has SDL and OpenGL bindings, which are actually quite good, but that's
about it.  You're on your own from that point onward.  There is not a rich
ecosystem of higher-level libraries built on top of those bindings.  There is
some work in this area, but I'm not aware of anything production quality.

There is also one really fundamental issue with the language, which is garbage
collection, which runs the risk of introducing perceptible pauses in gameplay
if your heap grows too large.

For this reason I don't see Haskell ever being used for AAA game programming.
I suppose you could use Haskell for simpler games that don't require keeping a
lot of resources in memory.

Haskell could maybe be used for the scripting layer of a game or to power
the backend for an online game, but for rendering or updating an extremely
large graph of objects you should probably stick to another language.

The company that has been doing the most to push the envelope for game
programming in Haskell is Keera Studios, so if this is an area that interests
you then you should follow their blog:

* [Keera Studios Blog](http://keera.co.uk/blog/)

**Areas for improvement:**

* Improve the garbage collector and benchmark performance with large heap sizes
* Provide higher-level game engines
* Improve distribution of Haskell games on proprietary game platforms

**Notable libraries:**

* [`Helm`](http://helm-engine.org/)
* [`gl`](https://hackage.haskell.org/package/gl)
* [`SDL`](https://hackage.haskell.org/package/SDL) / [`SDL-*`](https://hackage.haskell.org/packages/search?terms=SDL)
* [`SFML`](https://hackage.haskell.org/package/SFML)
* [`quine`](https://github.com/ekmett/quine) (Github-only project)

<br>

## Systems / embedded programming

**Rating:** Bad / Immature (?) (See description)

Since systems programming is an abused word, I will clarify that I mean
programs where speed, memory layout, and latency really matter.

Haskell fares really poorly in this area because:

* The language is garbage collected, so there are no latency guarantees
* Executable sizes are large
* Memory usage is difficult to constrain (thanks to space leaks)
* Haskell has a large and unavoidable runtime, which means you cannot easily
  embed Haskell within larger programs
* You can't easily predict what machine code that Haskell code will compile to

Typically people approach this problem from the opposite direction: they write
the low-level parts in C or Rust and then write Haskell bindings to the
low-level code.

It's worth noting that there is an alternative approach which is Haskell DSLs
that are strongly typed that generate low-level code at runtime.  This is the
approach championed by the company Galois.

**Notable libraries:**

* [`atom`](https://hackage.haskell.org/package/atom) / [`ivory`](https://hackage.haskell.org/package/ivory) - DSL for generating embedded programs
* [`copilot`](https://hackage.haskell.org/package/copilot) - Stream DSL that generates C code
* [`improve`](https://hackage.haskell.org/package/improve) - High-assurance DSL for embedded code that generates C and Ada

<br>

## Mobile apps

**Rating:** Immature? / Bad? (Uncertain)

This greatly lags behind using the language that is natively supported by the
mobile platform (i.e. Java for Android or Objective-C / Swift for iOS).

I don't know a whole lot about this area, but I'm definitely sure it is far
from mature.  All I can do is link to the resources I know of for Android and
iPhone development using Haskell.

I also can't really suggest improvements because I'm pretty out of touch with
this branch of the Haskell ecosystem.

**Educational resources:**

* [Android development in Haskell](https://wiki.haskell.org/Android)
* [iPhone development in Haskell](https://wiki.haskell.org/IPhone)

<br>
<br>

# Common Programming Needs

## Refactoring

**Rating:** Best in class

Haskell is unbelievably awesome for refactoring code.  There's nothing that I
can say that will fully convey how nice it is to refactor Haskell code.  You
can only appreciate this through experience.

When I say that Haskell is easy to refactor, I mean that you can easily
approach a large Haskell code base written by somebody else and make sweeping
architectural changes to the project without breaking the code.

You'll often hear people say: "if it compiles, it works".  I think that is a
bit of an exaggeration, but a more accurate statement is: "if you refactor and
it compiles, it works".  This lets you move fast without breaking things.

Most statically typed languages are easy to refactor, but Haskell is on its
own level for the following reasons:

* Strong types
* Global type inference
* Type classes
* Laziness

The latter two features are what differentiate Haskell from other statically
typed languages.

If you've ever refactored code in other languages you know that usually your
test suite breaks the moment you make large changes to your code base and you
have to spend a significant amount of effort keeping your test suite up to date
with your changes.  However, Haskell has a very powerful type system that lets
you transform tests into invariants that are enforced by the types so that you
can statically eliminate entire classes of errors at compile time.  These
types are much more flexible than tests when refactoring and types require much
less upkeep as you make large changes.

The Haskell community and ecosystem use the type system heavily to "test" their
applications, more so than other programming language communities.  That's not
to say that Haskell programmers don't write tests (they do), but rather we
prefer types over tests when they have the option.

Global type inference means that you don't have to update types and interfaces
as you refactor.  Whenever I do a large refactor the first thing I do is delete
all type signatures and let the compiler infer the types and interfaces for me
as I refactor.  When I'm done refactoring I just insert back the type
signatures that the compiler infers as machine-checked documentation.

Type classes also assist refactoring because the compiler automatically
infers type class constraints (analogous to interfaces in other languages) so
that you don't need to explicitly annotate interfaces.  This is a huge time
saver.

Laziness deserves special mention because many outsiders do not appreciate how
laziness simplifies refactoring.  Many languages require tight coupling between
producers and consumers of data structures in order to avoid wasteful
evaluation, but laziness avoids this problem by only evaluating data structures
on demand.  This means that if your refactoring process changes the order in
which data structures are consumed or even stops referencing them altogether
you don't need to reorder or delete those data structures.  They will just
sit around patiently waiting until they are actually needed, if ever, before
they are evaluated.

## Type system

**Rating:** Mature

I wasn't sure whether or not to give this a "Best in class" rating or not so
I conservatively chose "Mature" instead.  Haskell definitely does not have the
most advanced type system (not even close if you count research languages) but
out of all languages that are actually used in production Haskell is probably
at the top.  Idris is probably the closest thing to a type system more powerful
than Haskell that has a realistic chance of use in production in the
foreseeable future.

The killer features of Haskell's type system are:

* Type classes
* Global type and type class inference
* Light-weight type syntax

Haskell's type system really does not get in your way at all.  You (almost)
never need to annotate the type of anything.  As a result, the language feels
light-weight to use like a dynamic language, but you get all the assurances of
a static language.

The compiler really infers **everything**.  You don't even need to annotate
function arguments or interfaces.  The compiler deduces all of those for you.
The optional type signatures are for the benefit of the programmer, not the
compiler.

This really benefits projects where you need to prototype quickly but refactor
painlessly when you realize you are on the wrong track.  You can leave out all
type signatures while prototyping but the types are still there even if you
don't see them.  Then when you dramatically change course those strong and
silent types step in and keep large refactors painless.

## Domain-specific languages (DSLs)

**Rating:** Mature

Haskell rocks at DSL-building.  While not as flexible as a Lisp language I
would venture that Haskell is the most flexible of the non-Lisp languages.
You can overload a large amount of built-in syntax for your custom DSL.

The most popular example of overloaded syntax is `do` notation, which you can
overload to work with any type that implements the `Monad` interface.  This
syntactic sugar for `Monad`s in turn led to a huge overabundance of `Monad`
tutorials.

However, there are lesser known but equally important things that you can
overload, such as:

* numeric and string literals
* `if`/`then`/`else` expressions
* list comprehensions
* numeric operators

**Educational resources:**

* [You could have invented monads](http://blog.sigfpe.com/2006/08/you-could-have-invented-monads-and.html)
* [Rebindable syntax](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/syntax-extns.html#rebindable-syntax)
* [Monad comprehensions](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/syntax-extns.html#monad-comprehensions)
* [Overloaded strings](https://www.fpcomplete.com/school/to-infinity-and-beyond/pick-of-the-week/guide-to-ghc-extensions/basic-syntax-extensions#overloadedstrings)

## Testing

**Rating:** Mature

There are a few places where Haskell is the clear leader among all languages:

* property-based testing
* mocking / dependency injection

Haskell's `QuickCheck` is the gold standard which all other property-based
testing libraries are measured against.  The reason `QuickCheck` works so
smoothly in Haskell is due to Haskell's type class system and purity.  The type
class system simplifies automatic generation of random data from the input type
of the property test.  Purity means that any failing test result can be
automatically minimized by rerunning the check on smaller and smaller inputs
until `QuickCheck` identifies the corner case that triggers the failure.

Mocking is another area where Haskell shines because you can overload almost
all built-in syntax, including:

* `do` notation
* `if` statements
* numeric literals
* string literals

Haskell programmers overload this syntax (particularly `do` notation) to write
code that looks like it is doing real work:

```haskell
example = do str <- readLine
             putLine str
```

... and the code will actually evaluate to a pure syntax tree that you can use
to mock in external inputs and outputs:

```haskell
example = ReadLine (\str -> PutStrLn str (Pure ()))
```

Haskell also supports most testing functionality that you expect from other
languages, including:

* standard package interfaces for testing
* unit testing libraries
* test result summaries and visualization

**Notable libraries:**

* [`QuickCheck`](https://hackage.haskell.org/package/QuickCheck) - property-based testing
* [`doctest`](https://hackage.haskell.org/package/doctest) - tests embedded directly within documentation
* [`free`](https://hackage.haskell.org/package/free) - Haskell's abstract version of "dependency injection"
* [`hspec`](https://hackage.haskell.org/package/hspec) - Testing library analogous to Ruby's RSpec
* [`HUnit`](https://hackage.haskell.org/package/HUnit) - Testing library analogous to Java's JUnit
* [`tasty`](https://hackage.haskell.org/package/tasty) - Combination unit / regression / property testing library

**Educational resources:**

* [Why free monads matter](http://www.haskellforall.com/2012/06/you-could-have-invented-free-monads.html)
* [Purify code using free monads](http://www.haskellforall.com/2012/07/purify-code-using-free-monads.html)
* [Up-front Unit Testing in Haskell](https://github.com/kazu-yamamoto/unit-test-example/blob/master/markdown/en/tutorial.md)

## Concurrency

**Rating:** Mature

Haskell's concurrency runtime performs very well and is very easy to use.  The
best explanation of Haskell's threading module is the documentation in
`Control.Concurrent`:

> Concurrency is "lightweight", which means that both thread creation and
> context switching overheads are extremely low. Scheduling of Haskell threads
> is done internally in the Haskell runtime system, and doesn't make use of any
> operating system-supplied thread packages.

The best way to explain the performance of Haskell's threaded runtime is to
give hard numbers:

* The Haskell thread scheduler can easily handle millions of threads
* Each thread requires 1 kb of memory, so the hard limitation to thread count
  is memory (1 GB per million threads).
* Haskell channel overhead for the standard library (using `TQueue`) is on the
  order of one microsecond per message and degrades linearly with increasing
  contention
* Haskell channel overhead using the `unagi-chan` library is on the order of
  100 nanoseconds (even under contention)

Haskell also provides software-transactional memory, which allows programmers
build composable and atomic memory transactions.  Haskell's version of STM has
one important advantage over competing implementations: the type system
enforces that only memory operations can occur in STM transactions.  This
guarantees at compile time that rolling back or repeating failed transactions
is always safe.

Notable libraries:

* [`stm`](https://hackage.haskell.org/package/stm) - Software transactional memory
* [`unagi-chan`](https://hackage.haskell.org/package/unagi-chan) - High performance channels
* [`async`](https://hackage.haskell.org/package/async) - Futures library

Educational resources:

* [Parallel and Concurrent Programming in Haskell](http://chimera.labs.oreilly.com/books/1230000000929)
* [Parallel and Concurrent Programming in Haskell - Software transactional
memory](http://chimera.labs.oreilly.com/books/1230000000929/ch10.html#sec_stm-async)
* [Beautiful concurrency](https://www.fpcomplete.com/school/advanced-haskell/beautiful-concurrency) - a software-transactional memory tutorial

## Data structures and algorithms

**Rating:** Mature

Haskell primarily uses persistent data structures, meaning that when you
"update" a persistent data structure you just create a new data structure and
you can keep the old one around (thus the name: persistent).  Haskell data
structures are immutable, so you don't actually create a deep copy of the data
structure when updating; any new structure will reuse as much of the original
data structure as possible.

The **Notable libraries** sections contains links to Haskell collections
libraries that are heavily tuned.  You should realistically expect these
libraries to compete with tuned Java code.  However, you should not expect
Haskell to match expertly tuned C++ code.

The selection of algorithms is not as broad as in Java or C++ but it is still
pretty good and diverse enough to cover the majority of use cases.

**Notable libraries:**

* [`vector`](https://hackage.haskell.org/package/vector) - High-performance arrays
* [`containers`](https://hackage.haskell.org/package/containers) - High-performance `Map`s, `Set`s, `Tree`s, `Graph`s, `Seq`s
* [`unordered-containers`](https://hackage.haskell.org/package/unordered-containers) - High-performance `HashMap`s, HashSets
* [`accelerate`](https://hackage.haskell.org/package/accelerate) / [`accelerate-*`](https://hackage.haskell.org/packages/search?terms=accelerate) - GPU programming
* [`repa`](https://hackage.haskell.org/package/repa) / [`repa-*`](https://hackage.haskell.org/packages/search?terms=repa) - parallel shape-polymorphic arrays

## Benchmarking

**Rating:** Mature

This boils down exclusively to the `criterion` library, which was done so well
that nobody bothered to write a competing library.  Notable `criterion`
features include:

* Detailed statistical analysis of timing data
* Beautiful graph output: ([Example](http://www.serpentine.com/criterion/report.html))
* High-resolution analysis (accurate down to nanoseconds)
* Customizable HTML/CSV/JSON output
* Garbage collection insensitivity

**Notable libraries:**

* [`criterion`](https://hackage.haskell.org/package/criterion)

**Educational resources:**

* [The `criterion` tutorial](http://www.serpentine.com/criterion/tutorial.html)

## Unicode

**Rating:** Mature

Haskell's Unicode support is excellent.  Just use the `text` and `text-icu`
libraries, which provide a high-performance, space-efficient, and easy-to-use
API for Unicode-aware text operations.

**Notable libraries:**

* [`text`](https://hackage.haskell.org/package/text)
* [`text-icu`](https://hackage.haskell.org/package/text-icu)

## Parsing / Pretty-printing

**Rating:** Mature

Haskell is amazing at parsing.  Recursive descent parser combinators are
far-and-away the most popular parsing paradigm within the Haskell ecosystem, so
much so that people use them even in place of regular expressions.  I strongly
recommend reading the "Monadic Parsing in Haskell" functional pearl linked
below if you want to get a feel for why parser combinators are so dominant in
the Haskell landscape.

If you're not sure what library to pick, I generally recommend the `parsec`
library as a default well-rounded choice because it strikes a decent balance
between ease-of-use, performance, good error messages, and small dependencies
(since it ships with GHC).

`attoparsec` deserves special mention as an extremely fast backtracking parsing
library.  The speed and simplicity of this library will blow you away.  The
main deficiency of `attoparsec` is the poor error messages.

The pretty-printing front is also excellent.  Academic researchers just really
love writing pretty-printing libraries in Haskell for some reason.

**Notable libraries:**

* [`parsec`](https://hackage.haskell.org/package/parsec) - best overall "value"
* [`attoparsec`](https://hackage.haskell.org/package/attoparsec) - Extremely fast backtracking parser
* [`trifecta`](https://hackage.haskell.org/package/trifecta) - Best error messages (`clang`-style)
* [`alex`](https://hackage.haskell.org/package/alex) / [`happy`](https://hackage.haskell.org/package/happy) - Like `lexx` / `yacc` but with Haskell integration
* [`ansi-wl-pprint`](https://hackage.haskell.org/package/ansi-wl-pprint) - Pretty-printing library
* [`text-format`](https://hackage.haskell.org/package/text-format) - High-performance string formatting

**Educational resources:**

* [Monadic Parsing in Haskell](https://www.cs.nott.ac.uk/~gmh/pearl.pdf)

**Propaganda:**

* [A major upgrade to attoparsec: more speed, more power](http://www.serpentine.com/blog/2014/05/31/attoparsec/)

## Stream programming

**Rating:** Mature

Haskell's streaming ecosystem is mature.  Probably the biggest issue is that
there are too many good choices (and a lot of ecosystem fragmentation as a
result), but each of the streaming libraries listed below has a sufficiently
rich ecosystem including common streaming tasks like:

* Network transmissions
* Compression
* External process pipes
* High-performance streaming aggregation
* Concurrent streams
* Incremental parsing

**Notable libraries:**

* [`conduit`](https://hackage.haskell.org/package/conduit) / [`io-streams`](https://hackage.haskell.org/package/io-streams) / [`pipes`](https://hackage.haskell.org/package/pipes) - Stream programming libraries (Full disclosure: I authored `pipes` and wrote the official `io-streams` tutorial)
* [`machines`](https://hackage.haskell.org/package/machines) - Networked stream transducers library

**Educational resources:**

* [The official `conduit` tutorial](https://www.fpcomplete.com/school/to-infinity-and-beyond/pick-of-the-week/conduit-overview)
* [The official `pipes` tutorial](http://hackage.haskell.org/package/pipes-4.1.6/docs/Pipes-Tutorial.html)
* [The official `io-streams` tutorial](http://hackage.haskell.org/package/io-streams-1.3.2.0/docs/System-IO-Streams-Tutorial.html)

## Serialization / Deserialization

**Rating:** Mature

Haskell's serialization libraries are reasonably efficient and very easy to
use.  You can easily automatically derive serializers/deserializers for
user-defined data types and it's very easy to encode/decode values.

Haskell's serialization does not suffer from any of the gotchas that
object-oriented languages deal with (particularly Java/Scala).  Haskell data
types don't have associated methods or state to deal with so
serialization/deserialization is straightforward and obvious.  That's also
why you can automatically derive correct serializers/deserializers.

Serialization performance is pretty good.  You should expect to serialize data
at a rate between 100 Mb/s to 1 Gb/s with careful tuning.  Serialization
performance still has about 3x-5x room for improvement by multiple independent
estimates.  See the "Faster binary serialization" link below for details of the
ongoing work to improve the serialization speed of existing libraries.

**Notable libraries:**

* [`binary`](https://hackage.haskell.org/package/binary) / [`cereal`](https://hackage.haskell.org/package/cereal) - serialization / deserialization libraries

**Educational resources:**

* [Faster binary serialization](http://code.haskell.org/~duncan/binary-experiment/binary.pdf)

## Support for file formats

**Rating:** Mature

Haskell supports all the common domain-independent serialization formats (i.e.
XML/JSON/YAML/CSV).  For more exotic formats Haskell won't be as good as, say,
Python (which is notorious for supporting a huge number of file formats) but
it's so easy to write your own quick and dirty parser in Haskell that this is
not much of an issue.

**Notable libraries:**

* [`aeson`](https://hackage.haskell.org/package/aeson) - JSON encoding/decoding
* [`cassava`](https://hackage.haskell.org/package/cassava) - CSV encoding/decoding
* [`yaml`](https://hackage.haskell.org/package/yaml) - YAML encoding/decoding
* [`xml`](https://hackage.haskell.org/package/xml) - XML encoding/decoding

## Package management

**Rating:** Mature

If you had asked me a few months back I would have rated Haskell immature in
this area.  This rating is based entirely on the recent release of the `stack`
package tool by FPComplete which greatly simplifies package installation and
dependency management.  This tool was created in response to a broad survey of
existing Haskell users and potential users where `cabal-install` was identified
as the single greatest issue for professional Haskell development.

The `stack` tool is not just good by Haskell standards but excellent even
compared to other language package managers.  Key features include:

* Excellent project isolation (including compiler isolation)
* Global caching of shared dependencies to avoid wasteful rebuilds
* Easily add local repositories or remote Github repositories as dependencies

`stack` is also powered by Stackage, which is a very large Hackage mono-build
that ensures that a large subset of Hackage builds correctly against each
other and automatically notifies package authors to fix or update libraries
when they break the mono-build.  Periodically this package set is frozen as a
Stackage LTS release which you can supply to the `stack` tool in order to
select dependencies that are guaranteed to build correctly with each other.
Also, if all your projects use the same or similar LTS releases they will
benefit heavily from the shared global cache.

**Educational resources:**

* [The `stack` project](https://github.com/commercialhaskell/stack)

**Propaganda:**

* [`stack` announcement](https://www.fpcomplete.com/blog/2015/06/stack-0-1-release)

## Logging

Haskell has decent logging support.  That's pretty much all there is to say.

**Rating:** Mature

* [`fast-logger`](https://hackage.haskell.org/package/fast-logger) - High-performance multicore logging system
* [`hslogger`](https://hackage.haskell.org/package/hslogger) - Logging library analogous to Python's `ConfigParser` library

## Databases and data stores

**Rating:** Immature

This is is not one of my areas of expertise, but what I do know is that Haskell
has bindings to most of the open source databases and datastores such as MySQL,
Postgres, SQLite, Cassandra, Redis, DynamoDB and MongoDB.  However, I haven't really
evaluated the quality of these bindings other than the `postgresql-simple`
library, which is the only one I've personally used and was decent as far as I
could tell.

The "Immature" ranking is based on the recommendation of Stephen Diehl who
notes:

> Raw bindings are mature, but the higher level ORM tooling is a lot less mature
> than its Java, Scala, Python counterparts [Source](https://twitter.com/smdiehl/status/633262465938157569)

However, Haskell appears to be deficient in bindings to commercial databases
like Microsoft SQL server and Oracle.  So whether or not Haskell is right for
you probably depends heavily on whether there are bindings to the specific data
store you use.

**Notable libraries:**

* [`mysql-simple`](https://hackage.haskell.org/package/mysql-simple) - MySQL bindings
* [`postgresql-simple`](https://hackage.haskell.org/package/postgresql-simple) - Postgres bindings
* [`persistent`](https://hackage.haskell.org/package/persistent) - Database-agnostic ORM that supports automatic migrations
* [`esqueleto`](https://hackage.haskell.org/package/esqueleto) / [`relational-record`](https://hackage.haskell.org/package/relational-record) / [`opaleye`](https://hackage.haskell.org/package/opaleye) - type-safe APIs for building well-formed SQL queries
* [`acid-state`](https://hackage.haskell.org/package/acid-state) - Simple ACID data store that saves Haskell data types natively
* [`aws`](https://hackage.haskell.org/package/aws) - Bindings to Amazon DynamoDB 
* [`hedis`](https://hackage.haskell.org/package/hedis) - Bindings to Redis

## IDE support

**Rating:** Immature

I am not the best person to review this area since I do not use an IDE myself.
I'm basing this "Immature" rating purely on what I have heard from others.

The impression I get is that the biggest pain point is that Haskell IDEs,
IDE plugins, and low-level IDE tools keep breaking with every new GHC release.

Most of the Haskell early adopters have been `vi`/`vim` or `emacs` users so
those editors have gotten the most love.  Support for more traditional IDEs
has improved recently with Haskell plugins for IntelliJ and Eclipse and also
the Haskell-native `leksah` IDE.

FPComplete has also released a web IDE for Haskell programming that is also
worth checking out which is reasonably polished but cannot be used offline.

**Notable tools:**

* [`hoogle`](https://www.haskell.org/hoogle/) - Type-based function search
* [`hlint`](https://hackage.haskell.org/package/hlint) - Code linter
* [`ghc-mod`](https://hackage.haskell.org/package/ghc-mod) - editor agnostic tool that powers many IDE-like features
* [`ghcid`](https://hackage.haskell.org/package/ghcid) - lightweight background type-checker that triggers on code changes
* [`haskell-mode`](https://github.com/haskell/haskell-mode) - Umbrella project for Haskell `emacs` support
* [`structured-haskell-mode`](https://github.com/chrisdone/structured-haskell-mode) - structural editing based on Haskell syntax for `emacs`
* [`codex`](https://hackage.haskell.org/package/codex) - Tags file generator for cabal project dependencies.
* [`hdevtools`](https://hackage.haskell.org/package/hdevtools) - Persistent GHC-powered background server for development tools
* [`ghc-imported-from`](https://hackage.haskell.org/package/ghc-imported-from) - editor agnostic tool that finds Haddock documentation page for a symbol

**IDE plugins**:

* IntelliJ (the official plugin or Haskforce)
* Eclipse (the EclipseFP plugin)
* Atom (the IDE-Haskell plugin)

**IDEs**:

* FPComplete Center
* [`leksah`](https://hackage.haskell.org/package/leksah)

**Educational resources:**

* [Survey: Which Haskell development tools are you using that make you a more
   productive Haskell programmer?](https://www.reddit.com/r/haskell/comments/3bqy5h/survey_which_haskell_development_tools_are_you/)
* [FPComplete Center](https://www.fpcomplete.com/business/haskell-center/overview/) - A web-based Haskell IDE

# Contributors

* Aaron Levin
* Ben Kovach
* Edward Cho
* Liam O'Connor-Davis
* Oliver Charles
* Stephen Diehl
* Tran Ma
