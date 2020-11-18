# Parte 0: Introdução

Decidi embarcar em uma jornada de construção de compiladores. No passado, escrevi alguns [*assemblers*](https://github.com/DoctorWkt/pdp7-unix/blob/master/tools/as7) e um [*compilador simples*](https://github.com/DoctorWkt/h-compiler) para uma linguagem não tipada. No entanto, nunca escrevi um compilador capaz de se auto-compilar. É isso que pretendo fazer nessa jornada.

Como parte do processo, vou descrever meu trabalho para que outros possam acompanhá-lo. Isso também me ajudará a esclarecer meus pensamentos e ideias. Espero que você e eu achemos isso útil! 

## Objetivos da Jornada

Aqui estão meus objetivos, e não objetivos, para a jornada:

 + **Escrever um compilador auto-compilável.** Acho que se o compilador pode compilar a si mesmo, ele pode ser considerado um compilador real.
 + **Atingir pelo menos uma plataforma de hardware real.** Já vi alguns compiladores que geram código para máquinas hipotéticas. Quero que o meu funcione em hardware real. Além disso, se possível, quero que ele suporte vários *back-ends* para diferentes plataformas de hardware.
 + **Praticar antes de pesquisar.** Há muita pesquisa na área de compiladores. Eu quero começar do zero absoluto nesta jornada, então tentarei seguir uma abordagem prática e sem muita carga teórica. Dito isso, haverá momentos que precisarei apresentar (e implementar) algumas coisas baseadas em teoria.
 + **Seguir o *KISS principle: keep it simple, stupid!*** Definitivamente vou usar o princípio de Ken Thompson aqui: *"Na dúvida, use a força bruta."*
 + **Dar um monte de passos pequenos para chegar ao objetivo final.** Dividirei a jornada em várias etapas simples, em vez de dar grandes saltos. Isso tornará cada nova adição ao compilador uma coisa pequena e facilmente administrável.

## Linguagem Alvo

É difícil escolher uma linguagem alvo. Se eu escolher uma linguagem de alto nível, como Python, Go etc., terei que implementar uma pilha inteira de bibliotecas e classes, já que elas são integradas à linguagem.

Eu poderia escrever um compilador para uma linguagem como Lisp, mas isso pode ser [feito facilmente](ftp://publications.ai.mit.edu/ai-publications/pdf/AIM-039.pdf).

Em vez disso, vou escrever um compilador para um *subset* de C, que seja capaz de se auto-compilar.

C está há apenas um nível de abstração acima de Assembly (em se tratando de algum *subset* de C, não de [C18](https://en.wikipedia.org/wiki/C18_(C_standard_revision))) e isso ajudará a tornar a tarefa de compilar o código C até Assembly um pouco mais fácil. Ah, o mais importante, eu também gosto de C.

## O Básico do Trabalho de um Compilador

O trabalho de um compilador é **traduzir** um programa escrito em uma linguagem (usualmente de alto-nível) para outra linguagem (normalmente de nível mais baixo que a de entrada). As principais etapas são:

![](Figs/parsing_steps.png)

 + Fazer a [análise léxica](https://en.wikipedia.org/wiki/Lexical_analysis), para reconhecer os elementos léxicos. Em várias linguagens, `=` é diferente de `==`, então você não pode apenas ler um único `=` sem se importar com o que vem depois dele. Chamamos esses elementos léxicos de **tokens**.

 + Realizar a [análise sintática](https://en.wikipedia.org/wiki/Parsing) de uma sequência de entrada, para verificar se sua estrutura está de acordo com a **gramática** da linguagem. Por exemplo, sua linguagem pode ter essa estrutura de tomada de decisão:

```Java
    if (x < 42) {
        print("x is smaller than 42\n");
}
```

> Mas em outra linguagem você pode escrever:

```Python
    if (x < 42):
        print("x is smaller than 42\n")
```

> Este também é o lugar onde o compilador pode detectar *erros de sintaxe*, como se o `;` estivesse faltando no final do primeiro *print()*, por exemplo.

 + Fazer a [análise semântica](https://en.wikipedia.org/wiki/Semantic_analysis_(compilers)) da entrada, ou seja, entender o **significado** dela. Na verdade, isso é bem diferente das etapas anteriores. Por exemplo, em Inglês, uma frase pode ter a forma `<subject> <verb> <adjective> <object>`. As duas frases a seguir têm a mesma estrutura, mas significados completamente diferentes:

```
    David ate lovely bananas.
    Jennifer hates green tomatoes.
```

 + Finalmente [traduzir](https://en.wikipedia.org/wiki/Code_generation_(compiler)), uma parte de cada vez, a entrada para uma linguagem diferente, de nível mais baixo.
  
## Recursos

Existe muito material sobre compiladores na internet. Seguem os que usarei:

### Recursos de Aprendizagem

Se você quer começar com alguns livros, papers e ferramentas, recomendo essa lista:

 + [Curated list of awesome resources on Compilers, Interpreters and Runtimes](https://github.com/aalhour/awesome-compilers) by Ahmad Alhour

### Compiladores Existentes

Enquanto construo meu próprio compilador, pretendo procurar ideias em outros compiladores e provavelmente também pegar emprestado algum código deles. Aqui estão os que olharei:

  + [SubC](http://www.t3x.org/subc/) by Nils M Holm
  + [Swieros C Compiler](https://github.com/rswier/swieros/blob/master/root/bin/c.c) by Robert Swierczek
  + [fbcc](https://github.com/DoctorWkt/fbcc) by Fabrice Bellard
  + [tcc](https://bellard.org/tcc/), also by Fabrice Bellard and others
  + [catc](https://github.com/yui0/catc) by Yuichiro Nakada
  + [amacc](https://github.com/jserv/amacc) by Jim Huang
  + [Small C](https://en.wikipedia.org/wiki/Small-C) by Ron Cain,
    James E. Hendrix, derivatives by others

Em particular, usarei muitas das ideias e parte do código do compilador *SubC*.

## Preparando o Ambiente de Desenvolvimento

Supondo que você queira me acompanhar nessa jornada, aqui está o necessário. Usarei um ambiente de desenvolvimento Linux, então faça o download e configure seu sistema Linux favorito: vou de Lubuntu 18.04.

Vou ter como alvo duas plataformas de hardware:
  - Intel x86-64, usando um PC com Lubuntu 18.04.
  - ARM 32-bit, com um Raspberry Pi rodando Raspbian.

Na plataforma Intel, vamos precisar de um compilador de C já pronto. Então, instale este pacote (via Ubuntu/Debian commands):

```
    $ sudo apt-get install build-essential
```

Se houver mais ferramentas necessárias para um sistema Linux vanilla, me avise.

Finalmente, *clone* este repositório para sua máquina local.

## Próximo Passo

Na próxima parte, começaremos com o código para avaliar nosso arquivo de entrada e encontrar os **tokens**, que são os elementos léxicos da linguagem.
