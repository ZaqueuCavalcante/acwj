# Part 0: Introduction

Decidi embarcar em uma jornada de construção de compiladores. No passado, escrevi alguns [*assemblers*](https://github.com/DoctorWkt/pdp7-unix/blob/master/tools/as7) e um [*compilador simples*](https://github.com/DoctorWkt/h-compiler) para uma linguagem não tipada. No entanto, nunca escrevi um compilador capaz de se auto-compilar. É isso que pretendo fazer nessa jornada.

Como parte do processo, vou descrever meu trabalho para que outros possam acompanhá-lo. Isso também me ajudará a esclarecer meus pensamentos e ideias. Espero que você e eu achemos isso útil! 

## Goals of the Journey

Aqui estão meus objetivos, e não objetivos, para a jornada:

 + **Escrever um compilador auto-compilável.** Acho que se o compilador pode compilar a si mesmo, ele pode ser considerado um compilador real.
 + **Atingir pelo menos uma plataforma de hardware real.** Já vi alguns compiladores que geram código para máquinas hipotéticas. Quero que o meu funcione em hardware real. Além disso, se possível, quero que ele suporte vários *back-ends* para diferentes plataformas de hardware.
 + **Praticar antes de pesquisar.** Há muita pesquisa na área de compiladores. Eu quero começar do zero absoluto nesta jornada, então tentarei seguir uma abordagem prática e sem muita carga teórica. Dito isso, haverá momentos que precisarei apresentar (e implementar) algumas coisas baseadas em teoria.
 + **Seguir o *KISS principle: keep it simple, stupid!*** Definitivamente vou usar o princípio de Ken Thompson aqui: *"Na dúvida, use a força bruta."*
 + **Dar um monte de passos pequenos para chegar ao objetivo final.** Dividirei a jornada em várias etapas simples, em vez de dar grandes saltos. Isso tornará cada nova adição ao compilador uma coisa pequena e facilmente administrável.

## Target Language

É difícil escolher uma linguagem alvo. Se eu escolher uma linguagem de alto nível, como Python, Go etc., terei que implementar uma pilha inteira de bibliotecas e classes, já que elas são integradas à linguagem.

Eu poderia escrever um compilador para uma linguagem como Lisp, mas isso pode ser [feito facilmente](ftp://publications.ai.mit.edu/ai-publications/pdf/AIM-039.pdf).

Em vez disso, vou escrever um compilador para um *subset* de C, que seja capaz de se auto-compilar.

C está há apenas um nível de abstração acima de Assembly (em se tratando de algum subset de C, não de [C18](https://en.wikipedia.org/wiki/C18_(C_standard_revision))) e isso ajudará a tornar a tarefa de compilar o código C até Assembly um pouco mais fácil. Ah, o mais importante, eu também gosto de C.

## The Basics of a Compiler's Job

O trabalho de um compilador é **traduzir** um programa escrito em uma linguagem (usualmente de alto-nível) para outra linguagem (normalmente de nível mais baixo que a de entrada). As principais etapas são:

![](Figs/parsing_steps.png)

 + Do [lexical analysis](https://en.wikipedia.org/wiki/Lexical_analysis)
to recognise the lexical elements. In several languages, `=` is different
to `==`, so you can't just read a single `=`. We call these lexical
elements *tokens*.
 + [Parse](https://en.wikipedia.org/wiki/Parsing) the input, i.e. recognise
the syntax and structural elements of the input and ensure that they
conform to the *grammar* of the language. For example, your language
might have this decision-making
structure:

```
      if (x < 23) {
        print("x is smaller than 23\n");
      }
```

> but in another language you might write:

```
      if (x < 23):
        print("x is smaller than 23\n")
```

> This is also the place where the compiler can detect syntax errors, like if
the semicolon was missing on the end of the first *print* statement.

 + Do [semantic analysis](https://en.wikipedia.org/wiki/Semantic_analysis_(compilers))
   of the input, i.e. understand the meaning of the input. This is actually different
   from recognising the syntax and structure. For example, in English, a
   sentence might have the form `<subject> <verb> <adjective> <object>`.
   The following two sentences have the same structure, but completely
   different meaning:

```
          David ate lovely bananas.
          Jennifer hates green tomatoes.
```

 + [Translate](https://en.wikipedia.org/wiki/Code_generation_(compiler))
   the meaning of the input into a different language. Here we
   convert the input, parts at a time, into a lower-level language.
  
## Resources

There's a lot of compiler resources out on the Internet. Here are the ones
I'll be looking at.

### Learning Resources

If you want to start with some books, papers and tools on compilers,
I'd highly recommend this list:

  + [Curated list of awesome resources on Compilers, Interpreters and Runtimes](https://github.com/aalhour/awesome-compilers) by Ahmad Alhour

### Existing Compilers

While I'm going to build my own compiler, I plan on looking at other compilers
for ideas and probably also borrow some of their code. Here are the ones
I'm looking at:

  + [SubC](http://www.t3x.org/subc/) by Nils M Holm
  + [Swieros C Compiler](https://github.com/rswier/swieros/blob/master/root/bin/c.c) by Robert Swierczek
  + [fbcc](https://github.com/DoctorWkt/fbcc) by Fabrice Bellard
  + [tcc](https://bellard.org/tcc/), also by Fabrice Bellard and others
  + [catc](https://github.com/yui0/catc) by Yuichiro Nakada
  + [amacc](https://github.com/jserv/amacc) by Jim Huang
  + [Small C](https://en.wikipedia.org/wiki/Small-C) by Ron Cain,
    James E. Hendrix, derivatives by others

In particular, I'll be using a lot of the ideas, and some of the code,
from the SubC compiler.

## Setting Up the Development Environment

Assuming that you want to come along on this journey, here's what you'll
need. I'm going to use a Linux development environment, so download and
set up your favourite Linux system: I'm using Lubuntu 18.04.

I'm going to target two hardware platforms: Intel x86-64 and 32-bit ARM.
I'll use a PC running Lubuntu 18.04 as the Intel target, and a Raspberry
Pi running Raspbian as the ARM target.

On the Intel platform, we are going to need an existing C compiler.
So, install this package (I give the Ubuntu/Debian commands):

```
  $ sudo apt-get install build-essential
```

If there are any more tools required for a vanilla Linux
system, let me know.

Finally, clone a copy of this Github repository.

## The Next Step

In the next part of our compiler writing journey, we will start with
the code to scan our input file and find the *tokens* that are the
lexical elements of our language.
