# Golang PagSeguro v4 client

[![Build Status](https://github.com/deepakvashist/go-pagseguro/workflows/release/badge.svg)](https://github.com/deepakvashist/go-pagseguro/actions)
[![Go Report Card](https://goreportcard.com/badge/github.com/deepakvashist/go-pagseguro)](https://goreportcard.com/report/github.com/deepakvashist/go-pagseguro)
[![go.dev reference](https://img.shields.io/badge/go.dev-reference-007d9c?logo=go&logoColor=white&style=flat-square)](https://pkg.go.dev/github.com/deepakvashist/go-pagseguro)

PagSeguro v4 API golang client: [docs](https://dev.pagseguro.uol.com.br/v4.0/reference/nova-plataforma)

PagSeguro's dashboards:

- [Sandbox dashboard](https://sandbox.pagseguro.uol.com.br/)
- [Production dashboard](https://pagseguro.uol.com.br/)

## Usage

```golang
client := pags.NewClient(<baseURL>, <token>, <retryCount>, <timeout>, <retryWait>, <retryMaxWait>)
```

### Credit card

#### Authorize transaction

```golang
charge := pags.Charge{
    ReferenceID: "sample-reference-id",
    Description: "Sample description",
    Amount: pags.Amount{
        Value:    17000,
        Currency: "BRL",
    },
    PaymentMethod: &pags.PaymentMethod{
        Type:         "CREDIT_CARD",
        Installments: 1,
        Capture:      false,
        Card: pags.Card{
            Number:       "4111111111111111",
            ExpMonth:     "03",
            ExpYear:      "2026",
            SecurityCode: "123",
            Holder: &pags.Holder{
                Name: "Neil Armstrong",
            },
        },
    },
}

response, err := client.Charge(&charge)
```

#### Capture transaction

```golang
charge := pags.Charge{
    Amount: pags.Amount{
        Value: 17000,
    },
}

response, err := client.Capture(<tid>, &charge)
```

#### Cancel transaction

```golang
charge := pags.Charge{
    Amount: pags.Amount{
        Value: 15000,
    },
}

response, err := client.Cancel(<tid>, &charge)
```

### Boleto

```golang
charge := pags.Charge{
    ReferenceID: "sample-reference-id",
    Description: "Sample charge",
    Amount: pags.Amount{
        Value:    17000,
        Currency: "BRL",
    },
    PaymentMethod: pags.PaymentMethod{
        Type: "BOLETO",
        Boleto: &pags.Boleto{
            DueDate: "1969-07-20",
            InstructionLines: pags.InstructionLines{
                Line1: "Sample line 1",
                Line2: "Sample line 2",
            },
            Holder: pags.Holder{
                Name:  "Neil Armstrong",
                TaxID: "46274361499",
                Email: "neil.armstrong@nasa.dev",
                Address: &pags.Address{
                    Country:    "Brazil",
                    Region:     "Rio Grande do Sul",
                    RegionCode: "RS",
                    City:       "Uruguaiana",
                    PostalCode: "97504000",
                    Street:     "Rua Marechal Floriano",
                    Number:     "835",
                    Locality:   "Rio Branco",
                },
            },
        },
    },
    NotificationUrls: []string{"https://sample.com"},
}

response, err := client.Charge(&charge)
```
