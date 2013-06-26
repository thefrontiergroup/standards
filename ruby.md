## Table of Contents

* [Source Code Layout](#source-code-layout)
* [Syntax](#syntax)
* [Naming](#naming)
* [Comments](#comments)
    * [Comment Annotations](#comment-annotations)
* [Classes](#classes--modules)
* [Exceptions](#exceptions)
* [Collections](#collections)
* [Strings](#strings)
* [Regular Expressions](#regular-expressions)
* [Percent Literals](#percent-literals)
* [Metaprogramming](#metaprogramming)
* [Misc](#misc)
* [Tools](#tools)

## Source Code Layout

* Use two **spaces** per indentation level. No hard tabs.

    ```Ruby
    # bad - four spaces
    def some_method
        do_something
    end

    # good
    def some_method
      do_something
    end
    ```

* Don't use `;` to separate statements and expressions. As a
  corollary - use one expression per line.

    ```Ruby
    # bad
    puts 'foobar'; # superfluous semicolon

    puts 'foo'; puts 'bar' # two expression on the same line

    # good
    puts 'foobar'

    puts 'foo'
    puts 'bar'

    puts 'foo', 'bar' # this applies to puts in particular
    ```

* Use spaces around operators, after commas, colons and semicolons, around `{`
  and before `}`. Whitespace might be (mostly) irrelevant to the Ruby
  interpreter, but its proper use is the key to writing easily
  readable code.

    ```Ruby
    sum = 1 + 2
    a, b = 1, 2
    1 > 2 ? true : false; puts 'Hi'
    [1, 2, 3].each { |e| puts e }
    ```

    The only exception, regarding operators, is the exponent operator:

    ```Ruby
    # bad
    e = M * c ** 2

    # good
    e = M * c**2
    ```

    `{` and `}` deserve a bit of clarification, since they are used
    for block and hash literals, as well as embedded expressions in
    strings. For hash literals two styles are considered acceptable.

    ```Ruby
    # bad
    { one: 1, two: 2 }

    # good - no space after { and before }
    {one: 1, two: 2}
    ```
    The second variant has
    the advantage of adding visual difference between block and hash
    literals.

    As far as embedded expressions go, there are also two acceptable
    options:

    ```Ruby
    # good - no spaces
    "string#{expr}"

    # ok - arguably more readable
    "string#{ expr }"
    ```

    The first style is extremely more popular and you're generally
    advised to stick with it. The second, on the other hand, is
    (arguably) a bit more readable. As with hashes - pick one style
    and apply it consistently.

* No spaces after `(`, `[` or before `]`, `)`.

    ```Ruby
    some(arg).other
    [1, 2, 3].length
    ```

* Indent `when` as deep as `case`. I know that many would disagree
  with this one, but it's the style established in both "The Ruby
  Programming Language" and "Programming Ruby".

    ```Ruby
    case
    when song.name == 'Misty'
      puts 'Not again!'
    when song.duration > 120
      puts 'Too long!'
    when Time.now.hour > 21
      puts "It's too late"
    else
      song.play
    end

    kind = case year
           when 1850..1889 then 'Blues'
           when 1890..1909 then 'Ragtime'
           when 1910..1929 then 'New Orleans Jazz'
           when 1930..1939 then 'Swing'
           when 1940..1950 then 'Bebop'
           else 'Jazz'
           end
    ```

* Use empty lines between `def`s and to break up a method into logical
  paragraphs.

    ```Ruby
    def some_method
      data = initialize(options)

      data.manipulate!

      data.result
    end

    def some_method
      result
    end
    ```

* Don't use spaces around the `=` operator when assigning default values to method parameters:

    ```Ruby
    # good
    def some_method(arg1=:default, arg2=nil, arg3=[])
      # do something...
    end

    # bad
    def some_method(arg1 = :default, arg2 = nil, arg3 = [])
      # do something...
    end
    ```

* Align the parameters of a method call if they span more than one line.

    ```Ruby
    # starting point (line is too long)
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com', from: 'us@example.com', subject: 'Important message', body: source.text)
    end

    # bad (normal indent)
    def send_mail(source)
      Mailer.deliver(
        to: 'bob@example.com',
        from: 'us@example.com',
        subject: 'Important message',
        body: source.text)
    end

    # good
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com',
                     from: 'us@example.com',
                     subject: 'Important message',
                     body: source.text)
    end
    ```

* Limit lines to 80 characters.
* Avoid trailing whitespace.

## Syntax

* Use `def` with parentheses when there are arguments. Omit the
  parentheses when the method doesn't accept any arguments.

     ```Ruby
     # bad
     def some_method()
       # body omitted
     end

     # good
     def some_method
       # body omitted
     end

     # bad
     def some_method_with_arguments arg1, arg2
       # body omitted
     end

     # good
     def some_method_with_arguments(arg1, arg2)
       # body omitted
     end
     ```

* Never use `for`, unless you know exactly why. Most of the time iterators
  should be used instead. `for` is implemented in terms of `each` (so
  you're adding a level of indirection), but with a twist - `for`
  doesn't introduce a new scope (unlike `each`) and variables defined
  in its block will be visible outside it.

    ```Ruby
    arr = [1, 2, 3]

    # bad
    for elem in arr do
      puts elem
    end

    # good
    arr.each { |elem| puts elem }
    ```

* Never use `then` for multi-line `if/unless`.

    ```Ruby
    # bad
    if some_condition then
      # body omitted
    end

    # good
    if some_condition
      # body omitted
    end
    ```

* Favor the ternary operator(`?:`) over `if/then/else/end` constructs.
  It's more common and obviously more concise.

    ```Ruby
    # bad
    result = if some_condition then something else something_else end

    # good
    result = some_condition ? something : something_else
    ```

* Use one expression per branch in a ternary operator. This
  also means that ternary operators must not be nested. Prefer
  `if/else` constructs in these cases.

    ```Ruby
    # bad
    some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

    # good
    if some_condition
      nested_condition ? nested_something : nested_something_else
    else
      something_else
    end
    ```

* Use `!` instead of `not`.

    ```Ruby
    # bad - braces are required because of op precedence
    x = (not something)

    # good
    x = !something
    ```

* The `and` and `or` keywords are banned. It's just not worth
  it. Always use `&&` and `||` instead.

    ```Ruby
    # bad
    # boolean expression
    if some_condition and some_other_condition
      do_something
    end

    # control flow
    document.saved? or document.save!

    # good
    # boolean expression
    if some_condition && some_other_condition
      do_something
    end

    # control flow
    document.saved? || document.save!
    ```

* Favor modifier `if/unless` usage when you have a single-line
  body. Another good alternative is the usage of control flow `&&/||`.

    ```Ruby
    # bad
    if some_condition
      do_something
    end

    # good
    do_something if some_condition

    # another good option
    some_condition && do_something
    ```

* Favor `unless` over `if` for negative conditions (or control
  flow `||`).

    ```Ruby
    # bad
    do_something if !some_condition

    # bad
    do_something if not some_condition

    # good
    do_something unless some_condition

    # another good option
    some_condition || do_something
    ```

* Never use `unless` with `else`. Rewrite these with the positive case first.

    ```Ruby
    # bad
    unless success?
      puts 'failure'
    else
      puts 'success'
    end

    # good
    if success?
      puts 'success'
    else
      puts 'failure'
    end
    ```

* Omit parentheses around parameters for methods that are part of an
  internal DSL (e.g. Rake, Rails, RSpec), methods that have
  "keyword" status in Ruby (e.g. `attr_reader`) and attribute
  access methods. Use parentheses around the arguments of all other
  method invocations.

    ```Ruby
    class Person
      attr_reader :name, :age

      # omitted
    end

    temperance = Person.new('Temperance', 30)
    temperance.name

    x = Math.sin(y)
    array.delete(e)

    bowling.score.should == 0
    ```

* Omit parentheses for method calls with no arguments.

    ```Ruby
    # bad
    Kernel.exit!()
    2.even?()
    fork()
    'test'.upcase()

    # good
    Kernel.exit!
    2.even?
    fork
    'test'.upcase
    ```

* Prefer `{...}` over `do...end` for single-line blocks.  Avoid using
  `{...}` for multi-line blocks (multiline chaining is always
  ugly). Always use `do...end` for "control flow" and "method
  definitions" (e.g. in Rakefiles and certain DSLs).  Avoid `do...end`
  when chaining.

    ```Ruby
    names = ['Bozhidar', 'Steve', 'Sarah']

    # bad
    names.each do |name|
      puts name
    end

    # good
    names.each { |name| puts name }

    # bad
    names.select do |name|
      name.start_with?('S')
    end.map { |name| name.upcase }

    # good
    names.select { |name| name.start_with?('S') }.map { |name| name.upcase }
    ```

    Some will argue that multiline chaining would look OK with the use of {...}, but they should
    ask themselves - is this code really readable and can the blocks' contents be extracted into
    nifty methods?

* Avoid `return` where not required for flow of control.

    ```Ruby
    # bad
    def some_method(some_arr)
      return some_arr.size
    end

    # good
    def some_method(some_arr)
      some_arr.size
    end
    ```

* Avoid `self` where not required. (It is only required when calling a self write accessor.)

    ```Ruby
    # bad
    def ready?
      if self.last_reviewed_at > self.last_updated_at
        self.worker.update(self.content, self.options)
        self.status = :in_progress
      end
      self.status == :verified
    end

    # good
    def ready?
      if last_reviewed_at > last_updated_at
        worker.update(content, options)
        self.status = :in_progress
      end
      status == :verified
    end
    ```

* Never put a space between a method name and the opening parenthesis.

    ```Ruby
    # bad
    f (3 + 2) + 1

    # good
    f(3 + 2) + 1
    ```

* If the first argument to a method begins with an open parenthesis,
  always use parentheses in the method invocation. For example, write
`f((3 + 2) + 1)`.

### DEBATABLE

* Use the new lambda literal syntax for single line body blocks. Use the
  `lambda` method for multi-line blocks.

    ```Ruby
    # bad
    l = lambda { |a, b| a + b }
    l.call(1, 2)

    # correct, but looks extremely awkward
    l = ->(a, b) do
      tmp = a * 7
      tmp * b / 50
    end

    # good
    l = ->(a, b) { a + b }
    l.call(1, 2)

    l = lambda do |a, b|
      tmp = a * 7
      tmp * b / 50
    end
    ```

* Use `_` for unused block parameters.

    ```Ruby
    # bad
    result = hash.map { |k, v| v + 1 }

    # good
    result = hash.map { |_, v| v + 1 }
    ```

* Use ranges instead of complex comparison logic when possible.

    ```Ruby
    # bad
    do_something if x >= 1000 && x < 2000

    # good
    do_something if (1000...2000).include?(x)
    ```

## Naming

* Use `snake_case` for symbols, methods and variables.

    ```Ruby
    # bad
    :'some symbol'
    :SomeSymbol
    :someSymbol

    someVar = 5

    def someMethod
      ...
    end

    def SomeMethod
     ...
    end

    # good
    :some_symbol

    def some_method
      ...
    end
    ```

* Use `CamelCase` for classes and modules.  (Keep acronyms like HTTP,
  RFC, XML uppercase.)

    ```Ruby
    # bad
    class Someclass
      ...
    end

    class Some_Class
      ...
    end

    class SomeXml
      ...
    end

    # good
    class SomeClass
      ...
    end

    class SomeXML
      ...
    end
    ```

* Use `SCREAMING_SNAKE_CASE` for other constants.

    ```Ruby
    # bad
    SomeConst = 5

    # good
    SOME_CONST = 5
    ```

* The names of predicate methods (methods that return a boolean value)
  should end in a question mark.
  (i.e. `Array#empty?`).

* The names of potentially *dangerous* methods (i.e. methods that
  modify `self` or the arguments, `exit!` (doesn't run the finalizers
  like `exit` does), etc.) should end with an exclamation mark if
  there exists a safe version of that *dangerous* method.

    ```Ruby
    # bad - there is not matching 'safe' method
    class Person
      def update!
      end
    end

    # good
    class Person
      def update
      end
    end

    # good
    class Person
      def update!
      end

      def update
      end
    end
    ```

* Define the non-bang (safe) method in terms of the bang (dangerous)
  one if possible.

    ```Ruby
    class Array
      def flatten_once!
        res = []

        each do |e|
          [*e].each { |f| res << f }
        end

        replace(res)
      end

      def flatten_once
        dup.flatten_once!
      end
    end
    ```

## Comments

> Good code is its own best documentation. As you're about to add a
> comment, ask yourself, "How can I improve the code so that this
> comment isn't needed?" Improve the code and then document it to make
> it even clearer. <br/>
> -- Steve McConnell

* Write self-documenting code and ignore the rest of this section. Seriously!
* Use one space between the leading `#` character of the comment and the text
  of the comment.
* Comments longer than a word are capitalized and use punctuation. Use [one
  space](http://en.wikipedia.org/wiki/Sentence_spacing) after periods.
* Avoid superfluous comments.

    ```Ruby
    # bad
    counter += 1 # increments counter by one
    ```

* Keep existing comments up-to-date. An outdated comment is worse than no comment
at all.

* Avoid writing comments to explain bad code. Refactor the code to
  make it self-explanatory.

## Classes & Modules

* Use a consistent structure in your class definitions.

    ```Ruby
    class Person
      # extend and include go first
      extend SomeModule
      include AnotherModule

      # constants are next
      SOME_CONSTANT = 20

      # afterwards we have attribute macros
      attr_reader :name

      # followed by other macros (if any)
      validates :name

      # public class methods are next in line
      def self.some_method
      end

      # followed by public instance methods
      def some_method
      end

      # protected and private methods are grouped near the end
      protected

      def some_protected_method
      end

      private

      def some_private_method
      end
    end
    ```

* Always supply a proper `to_s` method for classes that represent
  domain objects.

    ```Ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def to_s
        "#{@first_name} #{@last_name}"
      end
    end
    ```

* Use the `attr` family of functions to define trivial accessors or
mutators.

    ```Ruby
    # bad
    class Person
      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def first_name
        @first_name
      end

      def last_name
        @last_name
      end
    end

    # good
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end
    ```

* Consider using `Struct.new`, which defines the trivial accessors,
constructor and comparison operators for you.

    ```Ruby
    # good
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end

    # better
    Person = Struct.new(:first_name, :last_name) do
    end
    ````

* Assign proper visibility levels to methods (`private`, `protected`)
in accordance with their intended usage. Don't go off leaving
everything `public` (which is the default). After all we're coding
in *Ruby* now, not in *Python*.

### DEBATABLE

* Indent the `public`, `protected`, and `private` methods as much the
  method definitions they apply to. Leave one blank line above the
  visibility modifier
  and one blank line below in order to emphasize that it applies to all
  methods below it.

    ```Ruby
    class SomeClass
      def public_method
        # ...
      end

      private

      def private_method
        # ...
      end

      def another_private_method
        # ...
      end
    end
    ```

## Exceptions

* Use *implicit begin blocks* where possible.

    ```Ruby
    # bad
    def foo
      begin
        # main logic goes here
      rescue
        # failure handling goes here
      end
    end

    # good
    def foo
      # main logic goes here
    rescue
      # failure handling goes here
    end
    ```

* Avoid rescuing the `Exception` class.  This will trap signals and calls to
  `exit`, requiring you to `kill -9` the process.

    ```Ruby
    # bad
    begin
      # calls to exit and kill signals will be caught (except kill -9)
      exit
    rescue Exception
      puts "you didn't really want to exit, right?"
      # exception handling
    end

    # good
    begin
      # a blind rescue rescues from StandardError, not Exception as many
      # programmers assume.
    rescue => e
      # exception handling
    end

    # also good
    begin
      # an exception occurs here

    rescue StandardError => e
      # exception handling
    end

    ```

* Put more specific exceptions higher up the rescue chain, otherwise
  they'll never be rescued from.

    ```Ruby
    # bad
    begin
      # some code
    rescue Exception => e
      # some handling
    rescue StandardError => e
      # some handling
    end

    # good
    begin
      # some code
    rescue StandardError => e
      # some handling
    rescue Exception => e
      # some handling
    end
    ```

* Release external resources obtained by your program in an ensure
block.

    ```Ruby
    f = File.open('testfile')
    begin
      # .. process
    rescue
      # .. handle error
    ensure
      f.close unless f.nil?
    end
    ```

* Favor the use of exceptions for the standard library over
introducing new exception classes.

## Collections

* Use `Set` instead of `Array` when dealing with unique elements. `Set`
  implements a collection of unordered values with no duplicates. This
  is a hybrid of `Array`'s intuitive inter-operation facilities and
  `Hash`'s fast lookup.

* Prefer symbols instead of strings as hash keys.

    ```Ruby
    # bad
    hash = { 'one' => 1, 'two' => 2, 'three' => 3 }

    # good
    hash = { one: 1, two: 2, three: 3 }
    ```

* Use the hash literal syntax when your hash keys are symbols.

    ```Ruby
    # bad
    hash = { :one => 1, :two => 2, :three => 3 }

    # good
    hash = { one: 1, two: 2, three: 3 }
    ```

* Rely on the fact that as of Ruby 1.9 hashes are ordered.
* Never modify a collection while traversing it.

## Strings

* Prefer string interpolation instead of string concatenation:

    ```Ruby
    # bad
    email_with_name = user.name + ' <' + user.email + '>'

    # good
    email_with_name = "#{user.name} <#{user.email}>"
    ```

* Consider padding string interpolation code with space. It more clearly sets the
  code apart from the string.

    ```Ruby
    "#{ user.last_name }, #{ user.first_name }"
    ```

* Avoid using `String#+` when you need to construct large data chunks.
  Instead, use `String#<<`. Concatenation mutates the string instance in-place
  and is always faster than `String#+`, which creates a bunch of new string objects.

    ```Ruby
    # good and also fast
    html = ''
    html << '<h1>Page title</h1>'

    paragraphs.each do |paragraph|
      html << "<p>#{paragraph}</p>"
    end
    ```

## Regular Expressions

* Don't use regular expressions if you just need plain text search in string:
  `string['text']`
* For simple constructions you can use regexp directly through string index.

    ```Ruby
    match = string[/regexp/]             # get content of matched regexp
    first_group = string[/text(grp)/, 1] # get content of captured group
    string[/text (grp)/, 1] = 'replace'  # string => 'text replace'
    ```

* Use non-capturing groups when you don't use captured result of parentheses.

    ```Ruby
    /(first|second)/   # bad
    /(?:first|second)/ # good
    ```

* Avoid using $1-9 as it can be hard to track what they contain. Named groups
  can be used instead.

    ```Ruby
    # bad
    /(regexp)/ =~ string
    ...
    process $1

    # good
    /(?<meaningful_var>regexp)/ =~ string
    ...
    process meaningful_var
    ```

* Character classes have only a few special characters you should care about:
  `^`, `-`, `\`, `]`, so don't escape `.` or brackets in `[]`.

* Be careful with `^` and `$` as they match start/end of line, not string endings.
  If you want to match the whole string use: `\A` and `\z` (not to be
  confused with `\Z` which is the equivalent of `/\n?\z/`).

    ```Ruby
    string = "some injection\nusername"
    string[/^username$/]   # matches
    string[/\Ausername\z/] # don't match
    ```

* Use `x` modifier for complex regexps. This makes them more readable and you
  can add some useful comments. Just be careful as spaces are ignored.

    ```Ruby
    regexp = %r{
      start         # some text
      \s            # white space char
      (group)       # first group
      (?:alt1|alt2) # some alternation
      end
    }x
    ```

* For complex replacements `sub`/`gsub` can be used with block or hash.

## Metaprogramming

* Avoid needless metaprogramming.

* Do not mess around in core classes when writing libraries.
  (Do not monkey-patch them.)

* `define_method` is preferable to `class_eval{ def ... }`

* When using `class_eval` (or other `eval`) with string interpolation, add a comment block showing its appearance if interpolated (a practice I learned from the Rails code):

    ```ruby
    # from activesupport/lib/active_support/core_ext/string/output_safety.rb
    UNSAFE_STRING_METHODS.each do |unsafe_method|
      if 'String'.respond_to?(unsafe_method)
        class_eval <<-EOT, __FILE__, __LINE__ + 1
          def #{unsafe_method}(*args, &block)       # def capitalize(*args, &block)
            to_str.#{unsafe_method}(*args, &block)  #   to_str.capitalize(*args, &block)
          end                                       # end

          def #{unsafe_method}!(*args)              # def capitalize!(*args)
            @dirty = true                           #   @dirty = true
            super                                   #   super
          end                                       # end
        EOT
      end
    end
    ```

* Avoid using `method_missing` for metaprogramming because backtraces become messy, the behavior is not listed in `#methods`, and misspelled method calls might silently work, e.g. `nukes.launch_state = false`. Consider using delegation, proxy, or `define_method` instead. If you must use `method_missing`:

  - Be sure to [also define `respond_to_missing?`](http://blog.marc-andre.ca/2010/11/methodmissing-politely.html)
  - Only catch methods with a well-defined prefix, such as `find_by_*` -- make your code as assertive as possible.
  - Call `super` at the end of your statement
  - Delegate to assertive, non-magical methods:

    ```ruby
    # bad
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        # ... lots of code to do a find_by
      else
        super
      end
    end

    # good
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        find_by(prop, *args, &block)
      else
        super
      end
    end

    # best of all, though, would to define_method as each findable attribute is declared
    ```

## Misc

* Avoid hashes as optional parameters. Does the method do too much? (Object initializers are exceptions for this rule).
* Avoid methods longer than 10 LOC (lines of code). Ideally, most methods will be shorter than
  5 LOC. Empty lines do not contribute to the relevant LOC.
* Avoid parameter lists longer than three or four parameters.
* If you really need "global" methods, add them to Kernel
  and make them private.
* Use module instance variables instead of global variables.

    ```Ruby
    # bad
    $foo_bar = 1

    #good
    module Foo
      class << self
        attr_accessor :bar
      end
    end

    Foo.bar = 1
    ```

* Avoid `alias` when `alias_method` will do.
* Code in a functional way, avoiding mutation when that makes sense.
* Do not mutate arguments unless that is the purpose of the method.
* Avoid more than three levels of block nesting.
* Be consistent. In an ideal world, be consistent with these guidelines.