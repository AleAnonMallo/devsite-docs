---
sites_supported:
  - mlb
  - mla
---

# Customizações

> INDEX
>
> Nesta página
>
>
>
> [Esquema redirect](https://www.mercadopago.com.ar/developers/es/guides/payments/web-payment-checkout/customizations#bookmark_esquema_modal)
>
> [Cores](https://www.mercadopago.com.ar/developers/es/guides/payments/web-payment-checkout/customizations#bookmark_colores)
>
> [Botões](https://www.mercadopago.com.ar/developers/es/guides/payments/web-payment-checkout/customizations#bookmark_botones)

## Esquema redirect

O Web Checkout redirect permite alterar seu esquema de abertura.

Os esquemas disponíveis atualmente são:

* **Redirect**: abre o Web Checkout em uma nova janela.
* **Modal**: abre o Web Checkout no seu site.

![Checkout-Modal](/images/web-payment-checkout/CHO_modal_pt.png)

Para integrar o esquema redirect, substitua o botão de pagamento que fez na integração básica pelo seguinte snippet:

[[[
```php
===
Redirige al 'init_point' de la preferencia
===
<!doctype html>
<html>
  <head>
    <title>Pagar</title>
  </head>
  <body>
    <a href="<?php echo $preference->init_point; ?>">Pagar con Mercado Pago</a>
  </body>
</html>
```
```node
===
Redirige al 'init_point' de la preferencia
===
<!doctype html>
<html>
  <head>
    <title>Mi sitio</title>
  </head>
  <body>
    <a href="$$init_point$$" target="_blank">Pagar</a>
  </body>
</html>
```
```java
===
Redirige al 'init_point' de la preferencia
===
<!doctype html>
<html>
  <head>
    <title>Pagar</title>
  </head>
  <body>
    <a href="${preference.initPoint}">Pagar con Mercado Pago</a>
  </body>
</html>
```
```ruby
===
Redirige al 'init_point' de la preferencia
===
<!doctype html>
<html>
  <head>
    <title>Mi sitio</title>
  </head>
  <body>
    <a href="<%= @init_point %>" target="_blank">Pagar</a>
  </body>
</html>
```
```csharp
===
Redirige al 'init_point' de la preferencia
===
<!doctype html>
<html>
  <head>
    <title>Pagar</title>
  </head>
  <body>
    <a href="@Html.DisplayFor(model => model.InitPoint)">Pagar con Mercado Pago</a>
  </body>
</html>
```
]]]


## Cores

> NOTE
>
> Nota
>
> válido somente para o esquema modal.

### Elementos

Os elementos que podem ser customizados são:

* Botões
* Campos para inserir dados
* Elementos de transição: spinners e barras de progresso
* Bordas

Você pode mudar a cor desses elementos adicionando o atributo `data-elements-color` no código HTML.
O valor do atributo deve estar em formato hexadecimal. Por exemplo:


```html
data-header-color="#c0392b"
```

![Custom-Component](/images/web-payment-checkout/custom_components.gif)
</p><br/>

#### Textos

A cor do texto dos botões será determinado automaticamente dependendo do contraste da cor definida.
Para uma cor de elemento claro, a color do texto será preta ou #000. Por exemplo:


```html
data-elements-color="#81ecec" <!-- Cor clara -->
```

![Light Color Button](/images/web-payment-checkout/light_color_button.png)

<br/>Para uma cor de elemento escuro, a cor do texto será branca ou #fff. Por exemplo:

```html
data-elements-color="#8e44ad" <!-- Cor escuro -->
```

![Dark Color Button](/images/web-payment-checkout/dark_color_button.png)

## Botões

### Texto

Por padrão, o botão mostra o texto “Pagar”. Você pode alterar o texto do botão adicionando o atributo `data-button-label` no código HTML. Por exemplo:

```html
data-button-label="Comprar"
```

### Por padrão: 

![Default Label Button](/images/web-payment-checkout/default_label_button.png)<br/>

### Customizado:

![Custom Label Button](/images/web-payment-checkout/custom_label_button.png)<br/><br/>

### Prossimos passos

<div>
<a href="http://www.mercadopago.com.br/developers/pt/guides/payments/web-payment-checkout/configurations/" style="text-decoration:none;color:inherit">
<blockquote class="next-step-card next-step-card-left">
<p class="card-note-title">Outras funcionalidades</p>
 <p>Configure seus pagamentos e adapte o Web Checkout ao seu negócio.</p>
</blockquote>
</a>
<a href="http://www.mercadopago.com.ar/developers/es/guides/payments/web-payment-checkout/advanced-integration/" style="text-decoration:none;color:inherit">       
<blockquote class="next-step-card next-step-card-right">
<p class="card-note-title">Integração avançada<span class="card-status-tag card-status-tag-recommended">RECOMENDADO</span></p>
 <p>Otimize sua integração e melhore o gerenciamento de suas vendas.</p>
</blockquote>
</a>   
</div>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
