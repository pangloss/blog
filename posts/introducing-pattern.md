What is [Pattern](https://github.com/pangloss/pattern)? 
It is several interrelated things. 
Interrelated, but not complected. 
Pattern is...

### ...an extensible pattern matcher for structured data

### ...a rule substitution engine

### ...an extensible rule combinator system

### ...a tool to build amazingly powerful macros

### ...a nanopass-style tool for building full-fledged compilers

### ...highly expressive and concise

## Backstory and Motivation

About a year and a half ago, the book [Software Design for Flexibility](https://mitpress.mit.edu/books/software-design-flexibility) by Chris Hanson and Gerald Sussman was released. 
I don't usually jump on software development books, but this one was one I was really looking forward to.
I had recently spent a little bit of time digging into [SICMUtils](https://github.com/sicmutils/sicmutils), the Clojure port of Dr. Sussman's incredibly powerful "system for math and physics investigations".
Here was some truly powerful software written in a very unusual style.
The hope was that this book would explain some of the thinking behind that project.

As I worked my way through SDF, I implemented most parts of it, porting it to Clojure as I went.
The result was the early basis of [Pattern](https://github.com/pangloss/pattern).

After finishing with the initial implementation, I encountered [Andy Keep's talk at Clojure Conj 2013](https://www.youtube.com/watch?v=Os7FE3J-U5Q), where he shows the Scheme to C compiler he implemented in a few days in a few thousand lines of code.

The Nanopass library he used to do this had a lot of similarities to the rule-based pattern matching in SDF. 
Unlike SDF's combinator-based approach, Nanopass is a very complex system of Scheme macros, and it took a lot of careful work to understand how it works.

I was constantly encouraged by the fact that despite its high code complexity, it is currently being used as the official compiler for both Chez Scheme and Racket, both very full-featured and relatively high performance systems.
Also, that complexity is a relative thing. It seems to me that it is still several orders of magnitude simpler than LLVM.

Funny story. I ran a compiler development team recently that used LLVM. 
We interviewed a candidate who claimed to have written a full LLVM pass on his own. 
My colleague who interviewed this candidate did not beleive that such a claim was reasonable or realistic.

Yet above is a video demonstration of a complete compiler with 22 passes written over just a few days!

My goal eventually became this: replicate the power and expressiveness of the Nanopass compiler, and prove it by porting a selection of passes and implementing my own Scheme-to-C compiler.

That goal has been achieved.

The Nanopass incremental dialect system is a thing of beauty and I have replicated it quite clonely.
The pass definition and matcher system had some very powerful patterns, which I extracted into composable rule combinators.
I also replicated the matcher shorthand defined within the dialect system that allows terse pattern definitions to match dialect-specific syntax.
Compiler passes sometimes need evaluation environment or other information, so I extended the entire rule combinator system to support passing, modifying and returning a contextual environment map.

The result is that compiler passes ported from the Nanopass compiler are shorter and clearer. 

Unlike the Nanopass framework, Pattern uses 100% immutable persistent data structures.
Also, I built my substitution system to exactly mirror the pattern system, enabling any matched rule to be easily rebuilt
Finally, I take advantage of Clojure's unique support for metadata, so every part of Pattern is metadata-preserving.
That allows my compilers to easily track information like original soure, and even to keep track of every transformation applied and the provenance of every form at each stage of the compilation process.

You may wonder why I'm doing this.
I'm a co-founder of Untether.ai.
We make AI Accelerator chips on a unique spatial at-memory architecture.
Writing a compiler with the standard tools like LLVM is really hard. 
Perhaps it doesn't need to be so hard?

## Further reading and getting started

With the backstory and motivation out of the way, I'll get into the pattern matcher system next.
