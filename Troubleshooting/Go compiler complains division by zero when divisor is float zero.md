## Symptom

```go
package main

import (
	"testing"
)

func TestDivByZero(t *testing.T) {
	zero := float64(0)
	t.Log(1 / zero)
}
```

```console
% go test -v ./main_test.go
=== RUN   TestDivByZero
    main_test.go:9: +Inf
--- PASS: TestDivByZero (0.00s)
PASS
ok      command-line-arguments  0.773s
```

```go
package main

import (
	"testing"
)

func TestDivByZero(t *testing.T) {
	t.Log(1 / float64(0))
}
```

```console
$  go test -v ./main_test.go
# command-line-arguments [command-line-arguments.test]
./main_test.go:8:12: invalid operation: division by zero
FAIL    command-line-arguments [build failed]
FAIL
```

## Spec

The divisor of a constant division or remainder operation must not be zero:

```go
3.14 / 0.0   // illegal: division by zero
```

https://github.com/golang/go/issues/81723
https://go.dev/ref/spec#Constant_expressions
