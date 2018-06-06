# Ruby

- **2 SPACE INDENTATION**
- generally follow community style guide: [bbatsov/ruby-style-guide](https://github.com/bbatsov/ruby-style-guide)
- [rubocop.yml template](rubocop.yml)
- prefer rspec over test-unit

## Lambdas and Procs
- See here: [Procs](https://en.wikibooks.org/wiki/Ruby_Programming/Syntax/Method_Calls#Procs)
- The shorthand for creating Procs is `Kernel.lambda` **BUT WITH SOME SLIGHT DIFFERENCES:**
  - lambdas check the number of parameters passed when called (throws `ArgumentError`)
  - A return from a `Proc` returns from the enclosing method whereas lambdas return control to the caller
- In general, prefer `lambda { }` over `Proc.new` to avoid surprising behavior.

## Framework specific
- [Rails](ruby/rails.md)
- [RSpec](ruby/rspec.md)
