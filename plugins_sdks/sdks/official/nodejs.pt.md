# SDK Mercado Pago para Node

* [Instalação](#bookmark_instalação)
* [Configuração](#bookmark_configuração)
* [Modo Sandbox](#bookmark_modo_sandbox)
* [Callbacks e promessas](#bookmark_callbacks_y_promessas)
* [Arquitetura](#bookmark_arquitetura)
* [Uso](#bookmark_uso)
* [Combatibilidade com versões anteriores](#bookmark_combatibilidade_com_versões_anteriores)
* [Validação de esquema](#bookmark_validação_de_esquema)
* [Resposta e erro](#bookmark_Resposta_e_erro)
* [Paginação](#bookmark_paginação)
* [Notificações IPN](#bookmark_notificações_IPN)


## Instalação

```
$ npm install mercadopago
```

### Suporte a Promises e Callbacks

Cada método suporta promises e callbacks. Por exemplo:

```javascript
var at = mp.getAccessToken ();

at.then (
    function (accessToken) {
        console.log (accessToken);
    },
    function (error) {
        console.log (error);
    });
```
é o mesmo que:

```javascript
mp.getAccessToken(function (err, accessToken){
    if (err) {
        console.log (err);
    } else {
        console.log (accessToken);
    }
});
```

Para utilizar callbacks, simplesmente passe uma função como último parâmetro.

## Configuração

Obtenha seu **ACCESS_TOKEN** nos seguintes links:

* Argentina: [https://www.mercadopago.com/mla/account/credentials](https://www.mercadopago.com/mla/account/credentials)
* Brasil: [https://www.mercadopago.com/mlb/account/credentials](https://www.mercadopago.com/mlb/account/credentials)
* México: [https://www.mercadopago.com/mlm/account/credentials](https://www.mercadopago.com/mlm/account/credentials)
* Colombia: [https://www.mercadopago.com/mco/account/credentials](https://www.mercadopago.com/mco/account/credentials)
* Perú: [https://www.mercadopago.com/mpe/account/credentials](https://www.mercadopago.com/mpe/account/credentials)
* Chile: [https://www.mercadopago.com/mlc/account/credentials](https://www.mercadopago.com/mlc/account/credentials)
* Uruguay: [https://www.mercadopago.com/mlu/account/credentials](https://www.mercadopago.com/mlu/account/credentials)

Para configurar o SDK deve-se usar o método **cofigure**. Este método recebe um objeto JSON. As configurações válidas são:

* client_id - String
* client_secret - String
* access_token - String
* sandbox (defaults = false) - Boolean
* show_promise_error (defaults = true) - Boolean

_Configuração de credenciais válidas:_

```javascript
mercadopago.configure({
    access_token: 'ACCESS_TOKEN'
});
```

```javascript
mercadopago.configure({
    sandbox: true,
    access_token: 'ACCESS_TOKEN'
});
```

```javascript
mercadopago.configure({
    client_id: 'CLIENT_ID',
    client_secret: 'CLIENT_SECRET'
});
```

_Configuração de credenciais inválidas:_

```javascript
mercadopago.configure({});
```

> É preciso fornecer um object com as configurações.

```javascript
mercadopago.configure({
    sandbox: true
});
```

> É preciso fornecer um método de autenticação (client_id & client_secret ou access_token).

```javascript
mercadopago.configure({
    client_id: 'CLIENT_ID'
});
```

> É preciso fornecer o client_id e client_secret.

```javascript
mercadopago.configure({
    client_secret: 'CLIENT_SECRET'
});
```

> Não se pode alterar o client_id ou client_secret porque já estão configurados.


## Modo Sandbox

Caso vá utilizar nossa aplicação no modo Sandbox, é preciso informar o access_token.

> Leve em conta que caso forneça o client_id e client_secret, o SDK gerará um access_token produtivo.

```javascript
mercadopago.configure({
    sandbox: true,
    access_token: 'ACCESS_TOKEN'
});
```
**O modo modo Sandbox funciona apenas com o access_token**


## Callbacks e promessas

Suportamos ambas. Para suportar versões de Node.js anteriores a 0.11.13 estamos utilizando internamente [bluebird](https://github.com/petkaantonov/bluebird).

_Callbacks:_

```javascript
library.method(function (err, res) {
    if (err) return console.log(err);
    
    console.log(res);
})
```

_Promessas:_

```javascript
library.method().then(function (res) {
    console.log(res);
}).catch(function (err) {
    console.log(err);
});
```

Pode utilizar o que preferir e adaptá-lo a sua aplicação.

## Arquitetura

O SDK exporta um objeto JSON expondo os recursos ao integrador. Pode-se acessá-los para executar operações válidas. Os recursos são os seguintes:

- payment
- preferences
- preapproval
- customers
- merchant_orders
- money_requests
- connect
- ipn

Pode-se acessá-los pelo SDK:

```javascript
var mercadopago = require('mercadopago');

console.log(mercadopago.payment);

/*
...
create: [Function],
update: [Function],
get: [Function],
search: [Function],
...
*/
```

Isso permite executar operações como, por exemplo, criação de um pagamento:

```javascript
mercadopago.payment.create({
  description: 'Buying a PS4',
  transaction_amount: 10500,
  payment_method_id: 'rapipago',
  payer: {
    email: 'test_user_3931694@testuser.com',
    identification: {
      type: '[FAKER][IDENTIFICATION][TYPE]',
      number: '34123123'
    }
  }
}).then(function (mpResponse) {
  console.log(mpResponse);
}).catch(function (mpError) {
  console.log(err);
});
```

Como resultado obterá:

```javascript
mercadopagoResponse {
  body: 
   { id: 2556382,
     date_created: '2017-01-19T17:00:21.000-04:00',
     date_approved: null,
     date_last_updated: '2017-01-19T17:00:21.000-04:00',
     money_release_date: null,
     operation_type: 'regular_payment',
     issuer_id: null,
     payment_method_id: 'rapipago',
     payment_type_id: 'ticket',
     status: 'pending',
     status_detail: 'pending_waiting_payment',
     currency_id: 'ARS',
     description: 'Descripci&oacute;n',
     live_mode: false,
     sponsor_id: null,
     authorization_code: null,
     collector_id: 239656545,
     payer: 
      { type: 'guest',
        id: null,
        email: 'test_user_3931694@testuser.com',
        identification: [Object],
        phone: [Object],
        first_name: 'Test',
        last_name: 'Test',
        entity_type: null },
     metadata: {},
     order: {},
     external_reference: null,
     transaction_amount: 10,
     transaction_amount_refunded: 0,
     coupon_amount: 0,
     differential_pricing_id: null,
     deduction_schema: null,
     transaction_details: 
      { net_received_amount: 0,
        total_paid_amount: 10,
        overpaid_amount: 0,
        external_resource_url: 'http://www.mercadopago.com/mla/sandbox/payments/ticket/helper?payment_method=rapipago',
        installment_amount: 0,
        financial_institution: null,
        payment_method_reference_id: '2556383' },
     fee_details: [],
     captured: true,
     binary_mode: false,
     call_for_authorize_id: null,
     statement_descriptor: null,
     installments: 1,
     card: {},
     notification_url: null,
     refunds: [] },
  status: 201,
  idempotency: '7fea2db6-fc84-436f-b6f4-457f7f5f151e',
  pagination: undefined
}
```

## Uso

### Como funciona

Basicamente, o SDK é um Cliente REST melhorado com ferramentas poderosas. Cada recurso (Payments, Preference, etc.) têm métodos básicos:

- create (POST)
- update (PUT)
- get (GET)
- remove (DELETE)

Esses métodos são criados dinâmicamente pelo SDK. Cada um têm os seguintes parâmetros.

```javascript
mercadopago.resource.create( ..., configurations, callback );
```

```javascript
mercadopago.resource.create( ..., configurations ).then().catch();
```

#### Configuração de parâmetos

> Este parâmetro é opcional se não está utilizando callbacks. Isso se convertirá automáticamente em um objeto vazio.

A configuração de parâmetros é um objeto JSON que permite os seguintes valores:

- qs:

Esse é um objeto JSON com os parâmetros que deseja enviar através da cadeia de consulta. A seguir um exemplo de como obter todos os pagamentos.

Se vemos a [API de Search](https://www.mercadopago.com.br/developers/pt/api-docs/account/payments/search/), isso será um GET, e devemos enviar payer.id = me na cadeia de consulta. Para isso, somente temos que por:

```javascript
var configurations = {
  qs: {
    'payer.id': 'me'
  }
};

mercadopago.payment.search(configurations, callback);
```

Enviando isso, o qs converterá a url de saída para:

```
https://api.mercadopago.com/v1/payments/search?payer.id=me
```

- idempotência - idempotência uuid personalizada a ser enviada.

Em alguns casos, um problema de conexão podería interromper a operação. Para assegurar-se de que a operação seja completada, pode-se tentar realizá-la novamente. Nesse caso, pode-se duplicá-la.

Para evitar esse comportamente, pode-se utilizar um UUID [(Identificador Único Universal)](https://en.wikipedia.org/wiki/Universally_unique_identifier) que identifica a operação. Esse UUID será um header na idempotencia de chamada de solicitação. Esses headers serão gerados automaticamente pela primeira operação.

Se uma operação falha, o erro terá a chave de idempotência enviada. Pode-se utilizá-la para tentar executá-la novamente. A seguir um exemplo:

```javascript
var payment = {
  description: 'Buying a PS4',
  transaction_amount: 10500,
  payment_method_id: 'rapipago',
  payer: {
    email: 'test_user_3931694@testuser.com',
    identification: {
      type: '[FAKER][IDENTIFICATION][TYPE]',
      number: '34123123'
    }
  }
};

mercadopago.payment.create(payment).then(function (mpResponse) {
  console.log(mpResponse);
}).catch(function (mpError) {
  return mercadopago.payment.create(payment, {
    qs: {
      idempotency: mpError.idempotency
    }
  });
}).then(function(mpResponse){
  console.log(mpResponse);
});
```

A primeira resposta será:

```javascript
mercadopagoError {
  name: 'MercadoPagoError',
  message: 'Lost Connection',
  cause: 'Unknown Cause',
  status: 500,
  idempotency: '7fea2db6-fc84-436f-b6f4-457f7f5f151e'
}
```

Na segunda tentativa, será enviada a mesma idempotencia que identifica a criação do pagamento anterior. Fazendo isso, se assegura de estar evitando que se duplique.

<

- access_token

Se desejar, pode-se anular o `access_token` configurado. Caso esteja utilizando MP Cpnnect, pode anulá-la na operação que **não deseja executar**. Por exemplo:

```javascript
var payment = {
  description: 'Buying a PS4',
  transaction_amount: 10500,
  payment_method_id: 'rapipago',
  payer: {
    email: 'test_user_3931694@testuser.com',
    identification: {
      type: '[FAKER][IDENTIFICATION][TYPE]',
      number: '34123123'
    }
  }
};

mercadopago.payment.create(payment, {
  access_token: 'MY_MERCHANT_ACCESS_TOKEN',
}).then(function (mpResponse) {
});
```

Nesse exemplo, se criará um pagamento utilizando my merchant access_token ('MY_MERCHANT_ACCESS_TOKEN')

#### Parâmetros de callback

O callback é opcional. Caso utilize promessas pode-se evitar enviá-las:

```javascript
mercadopago.resource.create( ..., configurations, callback );

mercadopago.payment.get(1, {}, function(error, response){
  if (error) return console.log('An error ocurred: ' + error.message);

  console.log(response);
});
```

```javascript
mercadopago.resource.create( ... ).then().catch();

mercadopago.payment.get(1).then(function (response) {
  console.log(response);
}).then(function (error) {
  console.log('An error ocurred: ' + error.message);
});
```

### Parâmetros dinâmicos

Já dissemos que os dois últimos parâmetros são _configurations_ e _callback_. Porém, o que ocorre com os primeiros parâmetros? Dependendo do método (POST, GET, etc.) e da rota, os parâmetros serão trocados.

Em relação as rotas, temos diferentes paths:

- /v1/payments - No path variables
- /v1/payments/:id - One path variable
- /v1/customers/:id/cards/:card_id - Two path variables

Isso significa que o método ao que chama necessita obter está variável (id, card_id) de alguma maneira. Vemos alguns métodos.

```javascript
payment.create = requestManager.describe({
  path: '/v1/payments',
  method: 'POST'
});

payment.update = requestManager.describe({
  path: '/v1/payments/:id',
  method: 'PUT'
});

payment.get = requestManager.describe({
  path: '/v1/payments/:id',
  method: 'GET'
});
```

Há uma diferença entre os métodos que recebem o JSON payload e os que não.

- Sem Payload: GET, DELETE
- Com Payload: POST, PUT, PATCH

#### Sem Payload

GET, DELETE: Esses receberão as variáveis nos parâmetros. A seguir um exemplo:

```javascript
payment.get = requestManager.describe({
  path: '/v1/payments/:id',
  method: 'GET'
});

// Calling the get
mercadopago.payment.get(1, {}, function (){});
```

```javascript
customers.cards.get = requestManager.describe({
  path: '/v1/customers/:id/cards/:card_id',
  method: 'GET'
});

// Getting the card 10 from the customer 1
mercadopago.customers.cards.get(1, 10, {}, function ()  {});
```

#### Com Payload
POST, PUT, PATH: Esses farão coincidir as variáveis com os payloads. A seguir um exemplo:

```javascript
payment.update = requestManager.describe({
  path: '/v1/payments/:id',
  method: 'PUT'
});

// Calling the update
mercadopago.payment.update({
  id: 1,
  status: 'cancelled'
}, function (){});
```

```javascript
customers.cards.update = requestManager.describe({
  path: '/v1/customers/:id/cards/:card_id',
  method: 'PUT'
});

// Calling the update
mercadopago.customers.cards.update({
  id: 1,
  card_id: 10,
  expiration_year: 2020
}, function (){});
```

O nome da variável do path deve ser o mesmo que o payload. Todos os nomes são os mesmo que necessitam enviar na atualização.

Um bom exemplo é:

```javascript
mercadolibre.payment.get(1).then(function (mpResponse) {
  // Cancelling a payment
  return mercadolibre.payment.update({
    id: mpResponse.body.id,
    status: 'cancelled'
  });
}).catch(function (error) {
  console.log('An error ocurred updating the payment');
});
```

## Combatibilidade com versões anteriores

Se está implementando o [SDK antigo](https://github.com/mercadopago/sdk-nodejs), não necessita modificar nada. Simplesmente atualize o pacote NPM e receberá todos os métodos anteriores:

- sandboxMode
- getAccessToken
- get
- post
- put
- delete
- createPreference
- updatePreference
- getPreference
- createPreapprovalPayment
- updatePreapprovalPayment
- getPreapprovalPayment
- searchPayment
- getPayment
- getPaymentInfo
- getAuthorizedPayment
- refundPayment
- cancelPayment
- cancelPreapprovalPayment

Quando usá-los, receberá uma advertência no console de que os métodos estão em desuso. Se eliminarão do SDK em futuras versões.

>Lembre-se, estão em desuso, somente registramos para que sua aplicação não caia. É preciso atualizar seu código para futuras atualizações.

## Validação do esquema

Cada vez que realizar uma operação que necessite enviar uma carga útil JSON, como criar um pagamento, um cliente, etc. O payload se validará automáticamente.

Esta funcionalidade permitirá economizar tempo e evitar erros no momenta de integrar. As validaões são as seguintes:

- Validações de tipo (Block)
- Validações do nome do campo (Warning)
- Parâmetros Extra (Warning)

#### Validações de bloqueio

Esse tipo de validação gerará um erro, não te permite executar a operação e evita um erro do servidor.

#### Validações de Advertencia

Esse tipo de validações emitirão uma advertência no console.

## Resposta e erros

### Respostas e erros personalizados.

Implementamos três novos objetos que ajudarão a tornar sua implementação muito mais fácil:

- mercadopagoResponse
- mercadopagoIpnResponse
- mercadopagoError

### mercadopagoResponse

Esse é o objeto de resposta onde receberá todas as respostas dos seus recursos. Está composto pelas seguintes variáveis e funções:

- body - Object: Corpo da resposta
- status - Integer: Estado da resposta
- idempotency - String: Cabeçalho de idempotencia enviado na solicitação
- pagination - Object: Objeto de resposta que provêm do endpoint que tem paginação como Payment Search
- next - Function: Ir a página seguinte
- hasNext - Function: Comprova se há próxima página (Boolean)
- prev - Function: Volta a página anterior
- hasPrev - Function: Comprova se há página anterior (Boolean)

#### Idempotência

A idempotência é um cabeçalho enviado ao MercadoPago para identificar a operação. Encontre mais informações sobre isto, [aqui](#bookmark_uso)

#### Paginação

Encontre toda a informação de paginação [aqui](#bookmark_paginación).

### mercadopagoIpnResponse

Esta é a resposta que provirá do Administrador de IPN. Isto está composto pelas seguintes variáveis:

- body - Object: Corpo resposta
- status - Integer: Estado de resposta
- Id - String: ID recebido do webhook
- Topic - String: topic recebido do Webhook

Encontre mais informações sobre IPN [aqui](#bookmark_notificaciones_IPN).

### mercadopagoError

Este é o objeto erro apresentado quando um recurso falha. Está composto pelas seguintes variáveis e funções:

- name - String: Nome do erro
- message - String: Mensagem recebida da API de MercadoPago
- cause - Array (Object): Causa do erro
- stack - Stacktrace do erro
- status - Integer: Estado da resposta
- idempotency - String: Idempotência Id Header
- retry - Função

#### Tentar novamente

Se as solicitações falham por tempo de espera ou outros erros, expomos um método de tente novamente de chamada para ajudá-lo a executar a mesma operação novamente, reutilizando a idempotência e facilitando-lhe a forma de tentá-lo novamente. Se este método não existe, deverá guardar o request object e obter a idempotência do erro para construir a solicitação novamente. Vejamos uma comparação:

##### Sem tentar novamente

```javascript
var payment = {
  description: 'Descripción',
  transaction_amount: 10,
  payment_method_id: 'rapipago',
  payer: {
    email: 'test_user_3931694@testuser.com',
    identification: {
      type: '[FAKER][IDENTIFICATION][TYPE]',
      number: '34214577'
    }
  }
};

mercadopago.payment.create(payment).then(function (response) {
  console.log(response);
}).catch(function (err) {
  // Manually retry
  mercadopago.payment.create(payment, {
    idempotency: err.idempotency
  }).then(function (response) {
    console.log(response);
  }).catch(function () {
    // Another Error
  });
});
```

##### Com tentar novamente

```javascript
mercadopago.payment.create({
  description: 'Descripción',
  transaction_amount: 10,
  payment_method_id: 'rapipago',
  payer: {
    email: 'test_user_3931694@testuser.com',
    identification: {
      type: '[FAKER][IDENTIFICATION][TYPE]',
      number: '34214577'
    }
  }
}).then(function (response) {
  console.log(response);
}).catch(function (err) {
  err.retry().then(function (response) {
    console.log(response);
  }).catch(function () {
    // Another Error
  });
});
```

## Paginação

### Wrapper Automático de paginação

Há alguns endpoints que devolvem a resposta paginada. Por exemplo, se está procurando todos os pagamentos que se fizeram a partir de sua conta. Vejamos um exemplo:

```javascript
mercadopago.payment.search({
  qs: {
    'collector.id': 'me'
  }
}).then(function (mpResponse) {
  console.log(mpResponse);
});
```

O output será:

```javascript
mercadopagoResponse {
  body: 
   { paging: { total: 110, limit: 30, offset: 0 },
     results: 
      [ [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object],
        [Object] ] },
  status: 200,
  idempotency: undefined,
  pagination: { total: 110, limit: 30, offset: 0 } }
```

Como pode ver, tem 100 pagamentos e 30 resultados por request.

Se não deseja obter a seguinte página, deve executar manualmente a busca desta forma:

```javascript
mercadopago.payment.search({
  qs: {
    'collector.id': 'me',
    pagination: {
      offset: 30
    }
  }
}).then(function (mpResponse) {
  console.log(mpResponse);
});
```

Para resolver isto, inserimos dois métodos à resposta:

- next: devolve uma Promise() e também recebe um callback com a página seguinte
- hasNext: devolve um booleano se existe uma página seguinte
- prev: devolve uma Promise() e também recebe um callback com a página anterior
- hasPrev: devolve um booleano se existe uma página anterior

Com isto, pode-se verificar se os resultados têm uma página seguinte ou anterior e os obter. Vejamos um exemplo com a seguinte página:

```javascript
mercadopago.payment.search({
  qs: {
    'collector.id': 'me'
  }
}).then(function (mpResponse) {
  if (mpResponse.hasNext()) {
    return mpRepsonse.next();
  }
}).then(function (nextPage) {
  console.log(nextPage);
});
```

Também pode-se utilizar callbacks:

```javascript
mercadopago.payment.search({
  qs: {
    'collector.id': 'me'
  }
}, function(err, mpResponse) {
  if (mpResponse.hasNext()) {
    return mpRepsonse.next(function (nextPage) {
      console.log(nextPage);
    });
  }
});
```

## Notificações IPN

A API de MercadoPago envia notificações de pagamentos e assinaturas realizadas. A notificação vem com os seguintes parâmetros:

- topic: Identifica que tipo de recurso é

  - merchant_order
  - payment
  - preapproval
  - authorized_payment

- id: id único da notificação

Quando receber essa notificação, poderá obter a informação do pedido ou do pagamento.

Merchant Order:
```
/merchant_orders/[ID]?access_token=[ACCESS_TOKEN]
```

Payment:
```
/v1/payments/[ID]?access_token=[ACCESS_TOKEN] 
```

### IPN Manager

Para simplificar este fluxo, oferecemos um Administrador de IPN que realizará a solicitação. Para usá-lo, deve passar a solicitação ao IPN Manager. Vamos ver como funciona:

```javascript
mercadopago.ipn.manage(request).then(function (response) {
  console.log(response);
}).then(function (error) {
  console.log(error);
});
```

O output será:

```javascript
mercadopagoIpnResponse {
  body: 
   { id: 2556382,
     date_created: '2017-01-19T17:00:21.000-04:00',
     date_approved: null,
     date_last_updated: '2017-01-19T17:00:21.000-04:00',
     money_release_date: null,
     operation_type: 'regular_payment',
     issuer_id: null,
     payment_method_id: 'rapipago',
     payment_type_id: 'ticket',
     status: 'pending',
     status_detail: 'pending_waiting_payment',
     currency_id: 'ARS',
     description: 'Descripci&oacute;n',
     live_mode: false,
     sponsor_id: null,
     authorization_code: null,
     collector_id: 239656545,
     payer: 
      { type: 'guest',
        id: null,
        email: 'test_user_3931694@testuser.com',
        identification: [Object],
        phone: [Object],
        first_name: 'Test',
        last_name: 'Test',
        entity_type: null },
     metadata: {},
     order: {},
     external_reference: null,
     transaction_amount: 10,
     transaction_amount_refunded: 0,
     coupon_amount: 0,
     differential_pricing_id: null,
     deduction_schema: null,
     transaction_details: 
      { net_received_amount: 0,
        total_paid_amount: 10,
        overpaid_amount: 0,
        external_resource_url: 'http://www.mercadopago.com/mla/sandbox/payments/ticket/helper?payment_method=rapipago',
        installment_amount: 0,
        financial_institution: null,
        payment_method_reference_id: '2556383' },
     fee_details: [],
     captured: true,
     binary_mode: false,
     call_for_authorize_id: null,
     statement_descriptor: null,
     installments: 1,
     card: {},
     notification_url: null,
     refunds: [] },
  status: 200,
  topic: 'payment'
}
```

Dentro da resposta, você encontrará o ** tópico ** recebido na solicitação, para que você não precise obter o valor dele.
