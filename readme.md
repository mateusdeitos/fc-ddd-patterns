

# Entidades

Expressividade nos métodos, exemplo:

- Usar `changeName` ao invés de `set name` para alterar o nome de um usuário.

> Normalmente os getters/setters são os vilões

## Princípio da auto validação

- A entidade deve ser capaz de se validar, exemplo:

```ts
class User {
  name: string;
  email: string;

  constructor(name: string, email: string) {
	this.name = name;
	this.email = email;
	this.validate();
  }

  changeName(name: string) {
	this.name = name;
	this.validate();
  }

  validate() {
	if (!this.name) {
	  throw new Error('Name is required');
	}

	if (!this.email) {
	  throw new Error('Email is required');
	}
  }
}
```

## Entidade vs ORM

A ideia é ter 2 entidades:
- Uma focada em negócio
- Outra focada em persistência

São classes de mesmo nome mas inseridas em contextos diferentes.

## Value Objects

- São objetos que representam um valor, exemplo: `Email`, `CPF`, `CNPJ`, `Password`

Exemplo:

```ts
export default class Customer {
	private _id: string;
	private _name: string = "";
	private _address!: Address; // Value object
}
```

