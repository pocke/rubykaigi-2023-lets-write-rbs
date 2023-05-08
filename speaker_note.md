# Speaker note

## Title

Hello everyone, I'm Masataka Pocke Kuwabara. Today, I'll talk about writing RBS.

## Self-introduction

First, let me introduce myself.

I'm a software engineer at Money Forward.
In Money Forward, I'm working on the development of a Ruby on Rails application and the maintenance of the RBS project.

## For contributors

Before this talk, I'd like to introduce the RBS contributors. They are the contributors to the RBS project. I'm very grateful to them.
Thank you for your contribution! (clap)


## Agenda

This talk is divided into two parts.

First, I'll talk about the features of RBS.
Recently RBS 3.1 was released. RBS 3.1 introduced new features to make it easier to write RBS. So I'd like to introduce them.
And I'll also introduce other tools existing before RBS 3.1. I'll use them in the second part, demonstration.

Second, I'll demonstrate how to develop a Ruby application using RBS. 
Currently, we can develop Ruby applications using RBS. So I'd like to show you the development experience and the tools we use.

## The new features of RBS 3.1

So, let's talk about the new features of RBS 3.1.

### rbs subtract

The first feature is `rbs subtract`.
This is a tool to remove duplicate definitions from RBS files.

For example, suppose you have two RBS files, `a.rbs` and `b.rbs`.
In this case, you can remove duplicate definitions from `a.rbs` by running the first command. It displays the result on the standard output.

The second command does the same thing, but it overwrites the result to `a.rbs`.

(next: example)

Here is an example of the result of `rbs subtract`.
The `a.rbs` file contains two method definitions, `C#foo` and `C#bar`. And the `b.rbs` file contains one method definition, `C#foo`.
The result of `rbs subtract` only contains the definition of `C#bar`. Because `C#foo` is already defined in `b.rbs`, so it is removed from the result.

TOOD: demo

(next: why rbs subtract is necessary)

Next, I'd like to talk about why `rbs subtract` is necessary.
It is necessary to modify auto-generated RBS files in a maintainable way.

Think about this situation.
You are developing a large Ruby application, and you want to introduce RBS to the application.
So you run an RBS generator, such as `rbs prototype`, to generate RBS files from your codebase. Because it is hard to write RBS files from scratch for the entire application.

But the generated file is not perfect. This is an example of the generated RBS.
(NOTE: If some talks mention more smart RBS generators, I'll mention it here)
The generated RBS contains many "untyped" definitions. For example, the `foo` method looks receives an Integer and returns a String, but the returned type in the generated RBS is `untyped`.
So, you want to clarify the "untyped" definitions in the generated RBS by adding type annotations to them.

But editing auto-generated files is not a good idea.
Because the next time you run the RBS generator, your changes will be overwritten by the generator. So we want to manage auto-generated files and hand-written files separately.

But if you just write a method definition to a separate file, you will get a duplicate definition error. Because RBS does not allow duplicate method definitions.
So you need to remove the duplication from the auto-generated files.

`rbs subtract` removes duplicate definitions from RBS files. So you can use it to remove duplicate definitions from auto-generated files.

This tool is especially useful when you develop a large application.

(next: example workflow)

This is a simple example workflow for introducing RBS to a large application.
The first command generates RBS files from the application codebase using `rbs prototype rb`. This command generates RBS files in the `sig/prototype/` directory from `app` and `lib` directories.

Then, write type definitions under `sig/hand-written/` directory. You can write type definitions to the directory without worrying about duplication.

After that, run `rbs subtract` to remove duplication from the generated RBS files. This command removes duplicate definitions from the generated RBS files and writes the result to the `sig/prototype/` directory.
Finally, the generated RBS and the hand-written RBS are available. You can type-check your application using these RBS files.

## rbs parse

The second feature is `rbs parse`.

(next: what is rbs parse)

`rbs parse` is a command to parse an RBS file and display syntax errors if any.

This is not a new feature of RBS 3.1. It was introduced in the first release of RBS. But I'd like to introduce it because I added new options to this command.

(next: what's new)

I added new two kinds of options, `-e`, `--type`, and `--method-type`.
`-e` is the same as the `-e` option of the Ruby interpreter. You can use it to parse an RBS string from the command line instead of saving a file.

`--type` and `--method-type` specify the context to parse RBS. You can use them to parse a type signature without writing a dummy class or method definition.

Let's see examples of these options.

(next: -e)

This is an example of the `-e` option.
Previously, you had to save the RBS string to a file and run `rbs parse` command to parse it. It is a little cumbersome.
But now you can directly parse the RBS string from the command line using the `-e` option.

(next: --type and --method-type)

They are examples of the `--type` and `--method-type` options. These options are useful with the `-e` option.
Previously, you had to write a dummy class or method definition to parse a type signature. Because RBS does not allow type or method signatures outside of a class or method definitions.

In the first example, without these options, `rbs parse` command parses the code as a whole of RBS. So it needs `class C` definition and method `f` definition.

In the second example, with `--method-type` option,  `rbs parse` command parses the code as a method signature. So, you don't need `class C` definition. It just needs the content of the method definition.

In the third example, with `--type` option, `rbs parse` command parses the code as a type signature. This example parses a record type.

## Other tools

In this section, I'll introduce other tools existing before RBS 3.1.


## RBS Collection

First, I'll introduce `rbs collection``.

`rbs collection` is a command to manage dependent libraries' RBSs.
In short, it is a Bundler for RBS.

For more information, please see my talk at RubyKaigi Takeout 2021 (two thousand twenty-one). I described `rbs collection` in detail in the talk.

## rbs prototype

The second tool is `rbs prototype`.

`rbs prototype` is a command to generate RBS files from Ruby code.
I'll use this command in the demonstration later. 

## Other tools

In this demonstration, I'll use these tools too. They are Steep and RBS Rails.
I do not describe them in detail in this talk. See the GitHub repository for more information.

## Editor integration

In this section, I'll introduce editor integration.

RBS is available in many editors. Today I use VS Code and the following two extensions, RBS Syntax and Steep.
But RBS is also available in Vim, Emacs, and other editors if it supports LSP.

This link is a list of useful extensions for RBS. I recommend you install them if you use RBS.

## After Events

I will introduce two events related to me and RubyKaigi.

(next: BaySide Tech Nite)

The first event is "BaySide Tech Nite". Shippio and Money Forward, which is my company, will hold this event on May 19th in Tokyo.
The topic of this event is Ruby and RubyKaigi, and talking in English.
Feel free to join this event if you are interested in it.

But unfortunately, I can't attend this event.

(next: Contribute to OSS)

The second event is "OSSへのコントリビュート". Timee holds this event on May 25th in Tokyo. It has an online option too.
In this event, I, mame-san who is yesterday's speaker, and sinsoku-san will talk about how to contribute to OSS.
I'll talk about how to start contributing to OSS from RBS. If you are interested in contributing to OSS as the Kaigi-Effect, please join this event.

## Conclusion

To conclude my presentation, I'd like to sum up now.

First, I shared new features of RBS 3.1. They are `rbs subtract` and `rbs parse`. I hope you will use them.

Second, I demonstrated how to introduce RBS to an application.

That's all for my talk. I hope you enjoyed it.
Thanks for listening to my talk! Enjoy the closing keynote by soutaro-san!
