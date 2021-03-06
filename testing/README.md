# Testes

> Todo arquivo de testes deve ter o sufixo `_test.go` para o `go test` (ferramenta do go pra executar testes) enxergar o arquivo e suas
funções devem ter a assinatura `func Test...(t *testing.T)` para serem consideradas testes.

# Exemplo

- Função a ser testada:

```go
package testing

import (
	"errors"
)

var errDivisaoInvalida = errors.New("divisão invalida")

func divideInteiros(dividendo, divisor int) (quociente int, resto int, err error) {
	if divisor == 0 {
		err = errDivisaoInvalida
		return
	}
	quociente = dividendo / divisor
	resto = dividendo % divisor
	return
}
```

- Teste:

```go
func TestDivideInteiros(t *testing.T) {
	for _, test := range []struct {
		// Struct que define os dados de entrada e saida necessarios para os testes
		dividendo int
		divisor   int
		quociente int
		resto     int
		err       error
	}{
		// Casos de teste para a função
		{10, 0, 0, 0, errDivisaoInvalida},
		{10, 2, 5, 0, nil},
		{7, 2, 3, 1, nil},
	} {
		// faz interação sobre os casos de testes
		q, r, erro := divideInteiros(test.dividendo, test.divisor)
		if q != test.quociente {
			t.Errorf("Esperava como quociente %d e obiteve %d\n", test.quociente, q)
		}
		if r != test.resto {
			t.Errorf("Esperava como resto %d e obiteve %d\n", test.resto, r)
		}
		if erro != test.err {
			t.Errorf("Esperava como err %v e obiteve %v\n", test.err, erro)
		}
	}
}
```

> Como podemos ver no exemplo, em Go só precisamos descrever nos testes os casos de falha, 
se algum caso de falha for satisfeito o código entrará no if e o teste falhará

## TestMain

> É possivel criar uma função `Main` para nossos testes com isso conseguimos testar recursos globais de nossa aplicação e criar um `setup`e um `teardown`global para nossa base de testes.

## Exemplo

```go
func TestMain(m *testing.M) {
	log.Println("Start tests")
	code := m.Run()
	log.Println("Stop tests")
	os.Exit(code)
}
```

---
[Inicio](../README.md)

[< tratando sinais](../signals/) - [pligin >](../plugin/)