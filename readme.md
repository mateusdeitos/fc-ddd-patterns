

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

## Agregados (Aggregate)

É um conjunto de objetos associados que são tratados como uma única unidade.

Exemplo:

```ts

class Customer {
  private _id: string;
  private _name: string = "";
  private _address!: Address; // Value object
  private _orders: Order[] = []; // Agregado
}

class Order {
  private _id: string;
  private _customer: Customer;
  private _items: OrderItem[] = [];
}

class OrderItem {
  private _id: string;
  private _order: Order;
  private _product: Product;
  private _quantity: number;
  private _price: number;
}

class Product {
  private _id: string;
  private _name: string;
  private _price: number;
}
```

## Domain Services

- São serviços que não são parte do domínio, mas que são necessários para o domínio.

Exemplo, realizar uma ação em lote em várias entidades:

```ts

class OrderService {
  public calculateTotalOfOrders(orders: Order[]): number {
	let total = 0;
	for (const order of orders) {
	  total += order.total;
	}

	return total;
  }
}
```

### Cuidados

- Quando um service possuir muitos métodos, provavelmente os agregados estão anêmicos, ou seja, eles somente são getters/setters

- Services são stateless, ou seja, não possuem estado