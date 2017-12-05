# Guides

Guides for getting things done, programming well, and programming in style.
<!-- TOC -->

- [Best Practices](#best-practices)
    - [General](#general)
    - [Object-Oriented Design](#object-oriented-design)
    - [Languages](#languages)
        - [Ruby](#ruby)
        - [JavaScript](#javascript)
    - [Web](#web)
        - [HTML](#html)
        - [CSS](#css)
        - [Browsers](#browsers)
    - [Shell](#shell)
        - [Bash](#bash)
- [Style](#style)
    - [Formatting](#formatting)
    - [Naming](#naming)
    - [Organization](#organization)
    - [JavaScript](#javascript-1)
    - [Ruby](#ruby-1)
    - [Shell](#shell-1)
    - [Git](#git)
    - [HTML](#html-1)
- [Security](#security)
    - [Think](#think)
    - [Secure Employee Access and Communication](#secure-employee-access-and-communication)
        - [Using Passwords](#using-passwords)
        - [Encryption](#encryption)
    - [Physical Security](#physical-security)
    - [Application Security](#application-security)
        - [Transmitting Information](#transmitting-information)
        - [Storing Information](#storing-information)
- [Code Review](#code-review)
    - [Everyone](#everyone)
    - [Having Your Code Reviewed](#having-your-code-reviewed)
    - [Reviewing Code](#reviewing-code)
- [Credits](#credits)

<!-- /TOC -->

## Best Practices

A guide for programming well.

### General

- These are not to be blindly followed; strive to understand these and ask when in doubt
- Don't duplicate the functionality of a built-in library
- Don't swallow exceptions or "fail silently"
- Don't write code that guesses at future functionality
- Exceptions should be exceptional
- Keep the code simple

### Object-Oriented Design

- Avoid global variables
- Avoid long parameter lists
- Limit collaborators of an object (entities an object depends on)
- Limit an object's dependencies (entities that depend on an object)
- Prefer composition over inheritance
- Prefer small methods Between one and five lines is best
- Prefer small classes with a single, well-defined responsibility. When a class exceeds 100 lines, it may be doing too many things.
- [Tell, don't ask]

[Tell, don't ask]: https://robots.thoughtbot.com/tell-dont-ask

### Languages

#### Ruby

- Avoid optional parameters. Does the method do too much?
- Avoid monkey-patching
- Generate necessary [Bundler binstubs] for the project, such as `rake` and `rspec`, and add them to version control
- Prefer classes to modules when designing functionality that is shared by multiple models
- Prefer `private` when indicating scope. Use `protected` only with comparison methods like `def ==(other)`, `def <(other)`, and `def >(other)`
- Declare dependencies in the `<PROJECT_NAME>.gemspec` file
- Reference the `gemspec` in the `Gemfile`
- Use [Bundler] to manage the gem's dependencies
- Use [Travis CI] for Continuous Integration, indicators showing whether GitHub pull requests can be merged, and to test against multiple Ruby versions
- Specify the [Ruby version] to be used on the project in the `Gemfile`

[Bundler binstubs]: https://github.com/sstephenson/rbenv/wiki/Understanding-binstubs
[Bundler]: http://bundler.io
[Travis CI]: http://travis-ci.org
[Ruby version]: http://bundler.io/v1.3/gemfile_ruby.html

[exact version]: http://robots.thoughtbot.com/post/35717411108/a-healthy-bundle
[pessimistic version]: http://robots.thoughtbot.com/post/35717411108/a-healthy-bundle
[versionless]: http://robots.thoughtbot.com/post/35717411108/a-healthy-bundle

#### JavaScript

- Use the latest stable JavaScript syntax with a transpiler, such as [babel]
- Include a `to_param` or `href` attribute when serializing ActiveRecord models, and use that when constructing URLs client side, rather than the ID
- Prefer `data-*` attributes over `id` and `class` attributes when targeting HTML elements
- Avoid targeting HTML elements using classes intended for styling purposes

[babel]: https://babeljs.io/

### Web

- Avoid a Flash of Unstyled Text, even when no cache is available
- Avoid rendering delays caused by synchronous loading
- Use https instead of http when linking to assets

#### HTML

- Use `<button>` tags over `<a>` tags for actions

#### CSS

- Document the project's CSS architecture (the README, component library or style guide are good places to do this), including things such as:
    - Organization of stylesheet directories and Sass partials
    - Selector naming convention
    - Code linting tools and configuration
    - Browser support
- Use Sass or [Stylus][stylus]
- Use [Autoprefixer][autoprefixer] to generate vendor prefixes based on the project-specific browser support that is needed
- Prefer `overflow: auto` to `overflow: scroll`, because `scroll` will alwaysdisplay scrollbars outside of macOS, even when content fits in the container

[autoprefixer]: https://github.com/postcss/autoprefixer
[stylus]: http://stylus-lang.com

#### Browsers

- Avoid supporting versions of Internet Explorer before IE11

### Shell

- Don't parse the output of `ls`. See [here][parsingls] for details and alternatives
- Don't use `cat` to provide a file on `stdin` to a process that accepts file arguments itself
- Don't use `echo` with options, escapes, or variables (use `printf` for those cases)
- Don't use a `/bin/sh` [shebang][] unless you plan to test and run your script on at least: Actual Sh, Dash in POSIX-compatible mode (as it will be run on Debian), and Bash in POSIX-compatible mode (as it will be run on OSX).
- Don't use any non-POSIX [features][bashisms] when using a `/bin/sh` [shebang][].
- If calling `cd`, have code to handle a failure to change directories
- If calling `rm` with a variable, ensure the variable is not empty
- Prefer "$@" over "$\*" unless you know exactly what you're doing
- Prefer `awk '/re/ { ... }'` to `grep re | awk '{ ... }'`
- Prefer `find -exec {} +` to `find -print0 | xargs -0`
- Prefer `for` loops over `while read` loops
- Prefer `grep -c` to `grep | wc -l`
- Prefer `mktemp` over using `$$` to "uniquely" name a temporary file
- Prefer `sed '/re/!d; s//.../'` to `grep re | sed 's/re/.../'`
- Prefer `sed 'cmd; cmd'` to `sed -e 'cmd' -e 'cmd'`
- Prefer checking exit statuses over output in `if` statements (`if grep -q ...;`, not `if [ -n "$(grep ...)" ];`)
- Prefer reading environment variables over process output (`$TTY` not `$(tty)`, `$PWD` not `$(pwd)`, etc).
- Use `$( ... )`, not backticks for capturing command output
- Use `$(( ... ))`, not `expr` for executing arithmetic expressions
- Use `1` and `0`, not `true` and `false` to represent boolean variables.
- Use `find -print0 | xargs -0`, not `find | xargs`
- Use quotes around every `"$variable"` and `"$( ... )"` expression unless you want them to be word-split and/or interpreted as globs
- Use the `local` keyword with function-scoped variables
- Identify common problems with [shellcheck][]

[parsingls]: http://mywiki.wooledge.org/ParsingLs
[shebang]: http://en.wikipedia.org/wiki/Shebang_(Unix)
[bashisms]: http://mywiki.wooledge.org/Bashism
[shellcheck]: http://www.shellcheck.net/

#### Bash

- Prefer `${var,,}` and `${var^^}` over `tr` for changing case
- Prefer `${var//from/to}` over `sed` for simple string replacements
- Prefer `[[` over `test` or `[`
- Prefer process substitution over a pipe in `while read` loops
- Use `((` or `let`, not `$((` when you don't need the result

## Style

### Formatting

- Avoid inline comments
- Break long lines after 80 characters
- Delete trailing whitespace
- Don't include spaces after `(`, `[` or before `]`, `)`
- Don't misspell
- Don't vertically align tokens on consecutive lines
- If you break up a hash, keep the elements on their own lines and closing curly
  brace on its own line
- Indent continued lines two spaces
- Indent private methods equal to public methods
- If you break up a chain of method invocations, keep each method invocation on its own line. Place the `.` at the end of each line, except the last. [Example][dot guideline example].
- Use 2 space indentation (no tabs)
- Use an empty line between methods
- Use empty lines around multi-line blocks
- Use spaces around operators, except for unary operators, such as `!`
- Use spaces after commas, after colons and semicolons, around `{` and before `}`
- Use [Unix-style line endings][newline explanation] (`\n`)
- Use [uppercase for SQL key words and lowercase for SQL identifiers]

[dot guideline example]: /style/ruby/sample.rb#L11
[uppercase for SQL key words and lowercase for SQL identifiers]: http://www.postgresql.org/docs/9.2/static/sql-syntax-lexical.html#SQL-SYNTAX-IDENTIFIERS
[newline explanation]: http://unix.stackexchange.com/questions/23903/should-i-end-my-text-script-files-with-a-newline

### Naming

- Avoid abbreviations
- Avoid object types in names (`user_array`, `email_method` `CalculatorClass`, `ReportModule`)
- Prefer naming classes after domain concepts rather than patterns they   implement (e.g. `Guest` vs `NullUser`, `CachedRequest` vs `RequestDecorator`).
- Name the enumeration parameter the singular of the collection
- Name variables created by a factory after the factory (`user_factory`   creates `user`)
- Name variables, methods, and classes to reveal intent
- Treat acronyms as words in names (`XmlHttpRequest` not `XMLHTTPRequest`),   even if the acronym is the entire name (`class Html` not `class HTML`)
- Suffix variables holding a factory with `_factory` (`user_factory`)

### Organization

- Order methods so that caller methods are earlier in the file than the methods they call
- Order methods so that methods are as close as possible to other methods they call

### JavaScript

```javascript
object = { spacing: true };

class Cat {
  canBark() {
    return false;
  }
}

const somePerson = {
  name: 'Ralph',
  company: 'Zaibatsu Inc',
};
```

- Use linters, i.e. [jshint]
- Prefer ES6 classes over prototypes
- Use strict equality checks (`===` and `!==`) except when comparing against (`null` or `undefined`)
- Prefer [arrow functions] `=>`, over the `function` keyword except when defining classes or methods
- Use semicolons at the end of each statement
- Prefer single quotes
- Use `PascalCase` for classes, `lowerCamelCase` for variables and functions, `SCREAMING_SNAKE_CASE` for constants, `_singleLeadingUnderscore` for private variables and functions
- Prefer [template strings] over string concatenation
- Prefer promises over callbacks
- Prefer array functions like `map` and `forEach` over `for` loops
- Use `const` for declaring variables that will never be re-assigned, and `let` otherwise
- Avoid `var` to declare variables

[jshint]: http://jshint.com/install/
[template strings]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/template_strings
[arrow functions]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions

### Ruby

- Use [rubocop]
- Avoid conditional modifiers (lines that end with conditionals)
- Avoid multiple assignments per line (`one, two = 1, 2`)
- Avoid organizational comments (`# Validations`)
- Avoid ternary operators (`boolean ? true : false`). Use multi-line `if` instead to emphasize code branches
- Avoid explicit return statements
- Avoid using semicolons
- Avoid bang (!) method names. Prefer descriptive names.
- Don't use `self` explicitly anywhere except class methods (`def self.method`) and assignments (`self.attribute =`)
- Prefer nested class and module definitions over the shorthand version
- Prefer `detect` over `find`
- Prefer `select` over `find_all`
- Prefer `map` over `collect`
- Prefer `reduce` over `inject`
- Prefer double quotes for strings
- Prefer `&&` and `||` over `and` and `or`
- Prefer `!` over `not`
- Prefer `&:method_name` to `{ |item| item.method_name }` for simple method calls
- Prefer `if` over `unless`
- Use `_` for unused block parameters
- Prefix unused variables or parameters with underscore (`_`)
- Use a leading underscore when defining instance variables for memoization
- Use `%{}` for single-line strings needing interpolation and double-quotes
- Use `{...}` for single-line blocks. Use `do..end` for multi-line blocks
- Use `?` suffix for predicate methods
- Use `CamelCase` for classes and modules, `snake_case` for variables and methods, `SCREAMING_SNAKE_CASE` for constants
- Use `def self.method`, not `def Class.method` or `class << self`
- Use `def` with parentheses when there are arguments
- Don't use spaces after required keyword arguments
- Use `each`, not `for`, for iteration
- Use a trailing comma after each item in a multi-line list, including the last item
- Use heredocs for multi-line strings
- Prefer `private` over `protected` for non-public `attr_reader`s, `attr_writer`s, and `attr_accessor`s
- Order class methods above instance methods
- Prefer method invocation over instance variables

[rubocop]: https://github.com/bbatsov/rubocop
### Shell

- Use [shellcheck]
- Break long lines on `|`, `&&`, or `||` and indent the continuations
- Don't add an extension to executable shell scripts
- Don't put a line break before `then` or `do`, use `if ...; then` and `while ...; do`
- Use `for x; do`, not `for x in "$@"; do`
- Use `snake_case` for variable names and `ALLCAPS` for environment variables
- Use single quotes for strings that don't contain escapes or variables
- Use two-space indentation

[shellcheck]: http://www.shellcheck.net/
### Git

- Squash multiple trivial commits into a single commit
- Write a [good commit message]

[good commit message]: http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html

### HTML

- Prefer double quotes for attributes

## Security

A guide for practicing safe web.

### Think

Security is important, and you can't practice these guidelines without understanding them. Make sure you understand each guideline, why it exists, and how to follow it.
Failing to follow these guidelines will likely put you, your team, and your deployed services at risk of compromise or loss of privacy.

### Secure Employee Access and Communication

The following guidelines apply to how you as an individual secure access to your systems (laptop, accounts, etc.)
and communication (email, etc.).

#### Using Passwords

- Use a unique password for every account you create.
- Use a tool like [pwgen](https://github.com/jbernard/pwgen) or [1password](https://1password.com) to generate random passwords.
- Use a tool like GnuPG to encrypt passwords if you need to share them with somebody.

#### Encryption

- Ensure [disk encryption][disk] on your laptop
- Use a PGP signature in an email if you want somebody to trust that you wrote it
- Use PGP to check email signatures if you want to know who wrote it
- Use PGP to encrypt emails if you want to be sure nobody but the recipient is   reading it
- Use ultimate trust for your own keys
- Use full trust for keys you have verified in person or via a secure video chat
- Don't share your private key with anyone, including services like Keybase
- Keep at least one backup of your private key and revocation certificate in a secure location, such as a thumb drive

[disk]: https://theintercept.com/2015/04/27/encrypting-laptop-like-mean/

### Physical Security

The following guidelines apply to how we physically secure our laptops and mobile devices that may contain customer or user data.

- Lock your device when you are away from it
- Don't leave your devices unattended in an unsecured area
- Install a device tracking and remote data wipe tool such as [Prey]

[Prey]: https://www.preyproject.com/

### Application Security

The following guidelines apply to how we develop software on behalf of ourselves and clients

#### Transmitting Information

- Don't accept passwords or session tokens over HTTP
- Use HTTPS for all web traffic
- Use HTTPS in the beginning; it's harder to introduce later
- Use HTTPS redirects for HTTP traffic
- Use [HSTS](http://tools.ietf.org/html/rfc6797) headers to enforce HTTPS   traffic.
- Use secure cookies
- Avoid protocol-relative URLs

#### Storing Information

- Don't log passwords
- Don't store passwords in plain text
- Don't hash passwords using a reversible cipher
- Don't hash passwords using a broken cipher, such as MD5 or SHA1

## Code Review

A guide for reviewing code and having your code reviewed.

### Everyone

- Accept that many programming decisions are opinions. Discuss tradeoffs, which you prefer, and reach a resolution quickly.
- Ask good questions; don't make demands. _"What do you think about naming this `:user_id`?"_
- Good questions avoid judgment and avoid assumptions about the author's perspective.
- Ask for clarification. _"I didn't understand. Can you clarify?"_
- Avoid selective ownership of code. _"mine", "not mine", "yours"_
- Avoid using terms that could be seen as referring to personal traits. _"dumb", "stupid"_. Assume everyone is intelligent and well-meaning.
- Be explicit. Remember people don't always understand your intentions online.
- Be humble. _"I'm not sure - let's look it up."_
- Don't use hyperbole. _"always", "never", "endlessly", "nothing"_
- Don't use sarcasm.
- Talk synchronously (e.g. chat, screensharing, in person) if there are too many "I didn't understand" or "Alternative solution:" comments. Post a follow-up comment summarizing the discussion.

### Having Your Code Reviewed

- Be grateful for the reviewer's suggestions. _"Good call. I'll make that change."_
- A common axiom is "Don't take it personally. The review is of the code, not you." We used to include this, but now prefer to say what we mean: Be aware of [how hard it is to convey emotion online] and how easy it is to misinterpret feedback. If a review seems aggressive or angry or otherwise personal, consider if it is intended to be read that way and ask the person for clarification of intent, in person if possible.
- Keeping the previous point in mind: assume the best intention from the reviewer's comments.
- Explain why the code exists. _"It's like that because of these reasons. Would it be more clear if I rename this class/file/method/variable?"_
- Extract some changes and refactorings into future tickets/stories.
- Link to the code review from the ticket/story
- Push commits based on earlier rounds of feedback as isolated commits to the branch. Do not squash until the branch is ready to merge. Reviewers should be able to read individual updates based on their earlier feedback.
- Seek to understand the reviewer's perspective
- Try to respond to every comment
- Wait to merge the branch until Continuous Integration (Bamboo, TravisCI, etc.) tells you the test suite is green in the branch
- Merge once you feel confident in the code and its impact on the project

[how hard it is to convey emotion online]: https://www.fastcodesign.com/3036748/why-its-so-hard-to-detect-emotion-in-emails-and-texts

### Reviewing Code

Understand why the change is necessary (fixes a bug, improves the user experience, refactors the existing code). Then:

- Communicate which ideas you feel strongly about and those you don't
- Identify ways to simplify the code while still solving the problem
- If discussions turn too philosophical or academic, move the discussion offline. In the meantime, let the author make the final decision on alternative implementations.
- Offer alternative implementations, but assume the author already considered them. _"What do you think about using a custom validator here?"_
- Seek to understand the author's perspective

## Credits

Guides is forked from [thoughtbot/guides](https://github.com/thoughtbot/guides).
