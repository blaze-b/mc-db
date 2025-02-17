# Mclebtec Database

- A project to understand the inner workings of a REPL database

## Commands

```sh

# gcc version check
gcc --version

# Install the ruby

ruby --version
gem --version

gem install bundler

gem install rspec

# Command to test the unit test case

rspec main_spec.rb

```

## Setting up REPL

- REPL stands for Read, Evaluate, Print, and Loop
- A sample database is created mimicing the same structure of the sqlite
- The db created will be similar to the [sqllite](https://www.sqlite.org/zipvfs/doc/trunk/www/howitworks.wiki).
- A query goes through a chain of components in order to retrieve or modify data.
- The front-end consists of the:
  - tokeniser
  - parser
  - code generator
- The input to the front-end is a SQL query. the output is sqlite virtual machine bytecode (essentially a compiled program that can operate on the database).
- The back-end consists of the:
  - virtual machine
    - It takes bytecode generated by the front-end as instructions.
    - It can then perform operations on one or more tables or indexes, each of which is stored in a data structure called a B-tree.
    - The VM is essentially a big switch statement on the type of bytecode instruction.
  - B-tree
    - It consists of many nodes.
    - Each node is one page in length.
    - The B-tree can retrieve a page from disk or save it back to disk by issuing commands to the pager.
  - pager
    - It receives commands to read or write pages of data.
    - It is responsible for reading/writing at appropriate offsets in the database file.
    - It also keeps a cache of recently-accessed pages in memory, and determines when those pages need to be written back to disk.
  - os interface
    - It is the layer that differs depending on which operating system sqlite was compiled for.

## Creating the compiler and virtual machine

- Create the sql command processor
- The “front-end” of sqlite is a SQL compiler that parses a string and outputs an internal representation called bytecode
- The byte code will be passed into the virtual machine

{% include image.html url="/images/arch2.jpg" description="SQLite Architecture (source: <https://www.sqlite.org/arch.html>)" %}

- Breaking things into two steps like this has a couple advantages:
  - Reduces the complexity of each part (e.g. virtual machine does not worry about syntax errors)
  - Allows compiling common queries once and caching the bytecode for improved performance

```sql

insert 1 cstack foo@bar.com

-- Executed.

insert 2 bob bob@example.com

-- Executed.

select

-- (1, cstack, foo@bar.com)
-- (2, bob, bob@example.com)
-- Executed.

insert foo bar 1

-- Syntax error. Could not parse statement.

```

## [First test addition](https://cstack.github.io/db_tutorial/parts/part4.html)

- Use rspec to generate the IT tests.

## Reference link

- [Build your own link](https://github.com/codecrafters-io/build-your-own-x.git)
- [Set up gcc for vscode](https://code.visualstudio.com/docs/cpp/config-mingw)
- [Ruby Installer](https://rubyinstaller.org/downloads/)