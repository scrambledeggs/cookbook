# Go Style guide

## Naming Conventions

- **Variables**: Use `camelCase` for local variables and function parameters.
- **Constants**: Use `ALL_CAPS` for constant names.
- **Functions**: Use `camelCase` for private function names and `UpperCamelCase` for public function names.

## Error Handling

- **Error Checking**: Always check for errors and handle them appropriately.
    - eg. don't skip `err` returns with `_ := someFunction()`.

## Formatting

- Indentation: Use **tabs** for indentation.
- Comments: Use `//` for single-line comments and `/* ... */` for block comments. Add comments to describe the purpose of the package, exported functions, and any complex logic.

## File structure 

Typical **[SAM](https://aws.amazon.com/serverless/sam/)** projects
```
.
├── .gitignore
├── bin/
├── cmd/
├── db/
│   ├── queries/
│   │   └── create_this_resource.sql
│   └── migrations/
│       └── 20240524_migrate_something.sql
├── events/
├── functions/
│   └── [resource]/
│       ├── [get/create/update/delete/action]/
│       │   ├── v1/
│       │   │   ├── main.go
│       │   │   └── main_test.go
│       │   └── v2/
│       │       ├── main.go
│       │       └── main_test.go
│       └── [action]/
│           └── v1/
│               ├── main.go
│               └── main_test.go
├── go.mod
├── go.sum
├── internal/
|   ├── database/
│   │   └── db.go
|   └── helpers/
│       └── strings.go
├── Makefile
├── README.md
├── sam-config.toml
├── sqlc.yml (or depending on used DB interface/compiler)
└── template.yml
```

## Testing

- **Files**: Place test files in the same directory as the code they test. Name test files with `*_test.go` suffix.
- **Framework**: Use Go’s built-in testing framework.
- **Coverage**: Aim for high test coverage. Ensure all critical paths and edge cases are tested.

## Documentation

- **Godoc**: Write clear and concise comments for all exported functions, types, and packages. Use [Godoc](https://go.dev/blog/godoc) conventions.
- **README**: Provide a README file with an overview of the project, setup instructions, and examples.

## Libraries or Packages

- **Standard Library**: Favor the use of Go’s standard library packages when possible.
- **Internal Library**: Favor the use of Go’s standard library packages when possible.
- **Third-party Packages**: Use vetted and well-maintained third-party packages. Lock versions in go.mod.

## Logging

- **Library**: Use our [common](https://github.com/scrambledeggs/booky-go-common) logger 
- **Levels**: Use appropriate log levels (INFO, WARN, ERROR).

## Stylistic

- **Initialism**: Use userID rather than userId. Same for abbreviations; HTTP is preferred over Http or http.
- **Guard Clause**: Return early from a function (or continue through a loop) to make nested conditionals one-dimensional. Instead of using if/else chains, we simply return early from the function at the end of each conditional block:
  - example
  ```go
  func divide(dividend, divisor int) (int, error) {
	if divisor == 0 {
		return 0, errors.New("Can't divide by zero")
	}

	return dividend/divisor, nil
  }
  ```
- **Defining Variables**: Define variables close to usage or consumption (useless func definition, just demo for variable assigning).
  - example
  ```go
  func badCompute(i int) bool {
  	foo := 1
  	bar := 2
  	buzz := 3
  
  	if i + buzz != 100 {
  		return false
  	}

  	result := foo + bar
  
  	return result == 3
  }

  func goodCompute(i int) bool {
  	buzz := 3
  
  	if i + buzz != 100 {
  		return false
  	}

  	foo := 1
  	bar := 2
  	result := foo + bar
  
  	return result == 3
  }
  ```
