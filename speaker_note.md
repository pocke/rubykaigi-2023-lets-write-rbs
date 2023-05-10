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
~And I'll also introduce other tools existing before RBS 3.1. I'll use them in the second part, it's a demonstration.~

Second, I'll demonstrate how to develop a Ruby application using RBS. 
Currently, we can develop Ruby applications using RBS. So I'd like to show you the development experience and the tools we use.

## The new features of RBS 3.1

So, let's talk about the new features of RBS 3.1.

### rbs subtract

The first feature is `rbs subtract`.
This is a tool to remove duplicate definitions from RBS files.

For example, suppose you have two RBS files, `a.rbs` and `b.rbs`.
In this case, you can remove duplicate definitions from `a.rbs` by running the first command. It's `rbs subtract a.rbs b.rbs`. It displays the result on the standard output.

The second command does the same thing, but it overwrites the result to `a.rbs`.

#### rbs subtract behavior

Here is an example of the result of `rbs subtract`.
The `a.rbs` file contains two method definitions, `C#foo` and `C#bar`. And the `b.rbs` file contains one method definition, `C#foo`.
The result of `rbs subtract` only contains the definition of `C#bar`. Because `C#foo` is already defined in `b.rbs`, so it is removed from the result.

TOOD: demo


#### why rbs subtract is necessary

Next, I'd like to talk about why `rbs subtract` is necessary.
It is necessary to modify auto-generated RBS files in a maintainable way.

#### Why necessary: example situation

Think about this situation.
You are developing a large Ruby application, and you want to introduce RBS to the application.
In this case, you may want to use an RBS generator, such as `rbs prototype`, to generate RBS files from your codebase. Because it is hard to write RBS files from scratch for the entire application.

#### Why necessary: generated RBS

But the generated file is not perfect. This is an example of the generated RBS.

(NOTE: If some talks mention more smart RBS generators, I'll mention it here)

The generated RBS contains many "untyped" definitions. For example, the `foo` method looks receives an Integer and returns a String, but the generated RBS does not contain this information. It only shows they are "untyped".
So, you want to clarify the "untyped" definitions in the generated RBS by adding type annotations to them.

But editing auto-generated files is not a good idea.
Because the next time you run the RBS generator, your changes will be overwritten by the generator. So we want to manage separately auto-generated files and hand-written files.

#### Why necessary: problem

But if you just write a method definition to a separate file, you will get a duplicate definition error. Because RBS does not allow duplicate method definitions.
So you need to remove the duplication from the auto-generated files.

In this case, the `foo` method is duplicated. Then it raises the error.

#### Why necessary: Solution

The solution is `rbs subtract`.
`rbs subtract` removes duplication from RBS files. So you can use it for auto-generated files.

This tool is especially useful when you develop a large application.

In this example, it removes `foo` method definition from `subtracted.rbs`. So it does not raise the error when you use hand-written.rbs and subtracted.rbs together.

#### example workflow

This is a simple example workflow for introducing RBS to a large application.
The first command generates RBS files from the application codebase using `rbs prototype rb`. This command generates RBS files in the `sig/prototype/` directory from `app` and `lib` directories.

Then, write type definitions under `sig/hand-written/` directory. You can write type definitions to the directory without worrying about duplication.

After that, run `rbs subtract` to remove duplication from the generated RBS files. This command removes duplication from the generated RBS files and writes the result to the `sig/prototype/` directory.
Finally, the generated RBS and the hand-written RBS are available. You can type-check your application using these RBS files.

### rbs parse

The second feature is `rbs parse`.

#### what is rbs parse

`rbs parse` is a command to parse an RBS file and display syntax errors if any.

This is not a new feature of RBS 3.1. It was introduced in the first release of RBS. But I'd like to introduce it because I added new options to this command.

#### what's new

I added new two kinds of options, `-e`, `--type`, and `--method-type`.
`-e` is the same as the `-e` option of the Ruby interpreter. You can use it to parse an RBS string from the command line instead of saving a file.

`--type` and `--method-type` specify the context to parse RBS. You can use them to parse a type signature without writing a dummy class or method definition.

Let's see examples of these options.

#### Example: -e

This is an example of the `-e` option.
Previously, you had to save the RBS string to a file and run `rbs parse` command to parse it. It is a little cumbersome.
But now you can directly parse the RBS string from the command line using the `-e` option.

#### --type and --method-type

They are examples of the `--type` and `--method-type` options. These options are useful with the `-e` option.
Previously, you had to write a dummy class or method definition to parse a type signature. Because RBS does not allow type or method signatures outside of a class or method definitions.

In the first example, without these options, `rbs parse` command parses the code as a whole of RBS. So it needs `class C` and method `f` definition.

In the second example, with `--method-type` option,  `rbs parse` command parses the code as a method signature. So, you don't need `class C` definition. It just needs the content of the method definition.

In the third example, with `--type` option, `rbs parse` command parses the code as a type signature. This example parses a record type.

## Other tools

In this section, I'll introduce other tools existing before RBS 3.1 and rbs related tools.


### RBS Collection

First, I'll introduce `rbs collection`.

`rbs collection` is a command to manage dependent libraries' RBSs.
In short, it is a Bundler for RBS.

For more information, please see my talk at RubyKaigi Takeout 2021 (two thousand twenty-one). I described `rbs collection` in detail in the talk.

### rbs prototype

The second tool is `rbs prototype`.

`rbs prototype` is a command to generate RBS files from Ruby code.
I'll use this command in the demonstration later. 

### Other tools

In this demonstration, I'll use these tools too. They are Steep and RBS Rails.
I do not describe them in detail in this talk. See the GitHub repository for more information.

## Editor integration

In this section, I'll introduce editor integration.

RBS is available in many editors. In the today demonstration, I use VS Code and the these two extensions, RBS Syntax and Steep.
But RBS is also available in Vim, Emacs, and other editors if it supports LSP.

This link is a list of useful extensions for RBS in many editors. I recommend you install them if you use RBS.

## DEMO

I have two demos today.
The first demo shows how to start using RBS on an existing application.
The second demo shows the development experience using RBS.

### Demo 1: Start using RBS

#### Preparation

In this demo, I'll show you how to start using RBS on an existing application.
I use ruby/rubyci repository for this demo. It is a small Rails application.

So, let's start the demo.
This is the repository. I already git cloned the repository and installed dependencies.

First, add the gems related to RBS to the Gemfile and run `bundle install`.
Open `Gemfile`, and add rbs, rbs_rails, and steep. Do not forget add `require: false`. And run `bundle install`.
Ok, let's git commit the changes.

Next, initialize `rbs collection`. Run `bundle exec rbs collection init`, and `install`. It installs dependent libraries' RBSs.
I also need to ignore `.gem_rbs_collection/` directory. Open gitignore and ignore it.
Ok, commit them.

After that, initialize Steep.
Run `bundle exec steep init`. It creates Steepfile so open Steepfile.
let's comment-in the setting. This setting checks app/ directory with RBSs in sig/ directory.
then commit them.

Then, add a rake task to prepare RBS files. I'm copying the code to lib/tasks/rbs.rake.
This is rbs.rake file.
`clean` task cleans generated RBS files.
`collection` task runs `rbs collection install`, I executed it before.
`prototype` task runs `rbs prototype` command. It generates RBS files from `app` directory to sig/prototype directory.
`subtract` task runs `rbs subtract` command. It removes the duplication.
And `setup` task runs the above tasks.
Let's commit them.

`rbs:setup` rake task is available now. Run `bin/rake rbs:setup`. It setup the environment.
It generates RBS files, so let's add them to gitignore.

The preparation is done. Let's start writing RBS.

#### Writing RBS

First, try to execute steep. We can execute "restart steep" command on VS Code. If you want to use it on command line, you can also use `steep check` command.
Yeah, it displays many errors. But today, I do not fix them. I only focus on using `rbs subtract`. 

Today I edit `report.rb` file. I'd like to focus on `fetch_recent` method.
This method calls `get_reports_rubyci_s3` method. But as you can see by the hover, the argument and returned types are untyped. It means this method does not check types.
So, let's add types to this method.

First, find the auto-generated RBS of this method. We can find the RBS by the "Go to Definition" feature of LSP.
Then, copy the definition to `sig/app/models/report.rbs` to avoid editing auto-generated RBS.

But it causes an duplication error. So, let's execute `rbs subtract` command. We can execute `bin/rake rbs:subtract` command.
After that, the duplication has been removed. And the error has been fixed.
So correct the copied type. The argument is Aws::S3::Resource and Server, and the returned type is an optional Integer.

Then, this method is well typed. We can confirm it by the hover feature of VS Code.

### Demo 2: Development experience

The second demo shows the development experience using RBS on rbs repository itself.


## After Events

Let me advertise two events related to me and RubyKaigi.

### BaySide Tech Nite

The first event is "BaySide Tech Nite". Shippio and Money Forward, which is my company, will hold this event on May 19th in Tokyo.
The topic of this event is Ruby and RubyKaigi, and talking in English.
Feel free to join this event if you are interested in it.

But unfortunately, I can't attend this event.

### Contribute to OSS

The second event is "OSSへのコントリビュート". Timee holds this event on May 25th in Tokyo. It has an online option too.
In this event, I, mame-san who is yesterday's speaker, and sinsoku-san will talk about how to contribute to OSS.
I'll talk about how to start contributing to OSS from RBS. If you are interested in contributing to OSS as the Kaigi-Effect, please join this event.

## Conclusion

To conclude my presentation, I'd like to sum up now.

First, I shared new features of RBS 3.1. They are `rbs subtract` and `rbs parse`. I hope you will use them.

Second, I demonstrated how to introduce RBS to an application.

That's all for my talk. I hope you enjoyed it.
Thanks for listening to my talk! Enjoy the closing keynote by soutaro-san!
