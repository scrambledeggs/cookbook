# Ruby

- **2 SPACE INDENTATION**
- generally follow community style guide: [bbatsov/ruby-style-guide](https://github.com/bbatsov/ruby-style-guide)
- [rubocop.yml template](rubocop.yml)
- prefer rspec over test-unit

## Lambdas and Procs
- See here: [Procs](https://en.wikibooks.org/wiki/Ruby_Programming/Syntax/Method_Calls#Procs)
- The shorthand for creating `Proc`s is `Kernel.lambda` **BUT:**
  - `lambda`s check the number of parameters passed when called (throws `ArgumentError`)
  - A return from a `Proc` returns from the enclosing method whereas lambdas return control to the caller
- In general, prefer `lambda { }` over `pro`

## Framework specific
- [Rails](ruby/rails.md)
- [RSpec](ruby/rspec.md)
