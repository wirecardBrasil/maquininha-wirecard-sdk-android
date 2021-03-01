# Moip v2 MPos client SDK - Android

O jeito mais simples e rápido de integrar o moip ao seu App usando a Máquina de cartões para venda presencial do Moip 

## Importante
Tivemos que realizar a mudança de adquirente, por esse motivo, todas as versões passadas dessa SDK iram parar de funcionar. A atualização
para a nova versão é obrigatória.


## Instalação

**IMPORTANTE: Como ainda estamos em fase de desenvolvimento, não temos uma versão para disponibilizar via maven.**

**O intuito dessa documentação é mostrar para clientes que já utilizam a sdk em seus aplicativos as mudanças que foram realizadas.**

## Permissões

Para utilizar o SDK é necessário solicitar ao usuário as seguintes permissões:

```java
        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.BLUETOOTH" />
        <uses-permission android:name="android.permission.BLUETOOTH_ADMIN" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

As permissões são necessárias para que seja possível fazer as requisições em nossas APIs e a conexão bluetooth com o POS que efetuará a cobrança.
Lembre-se de pedir as seguinte permissões antes de realizar qualquer chamada na SDK.

```java
    android.permission.ACCESS_FINE_LOCATION
    android.permission.READ_PHONE_STATE
    android.permission.WRITE_EXTERNAL_STORAGE
```  

# Configurando sua autenticação

Autenticando por BasicAuth:
```java
      Authentication authentication = new BasicAuth("SEU_TOKEN", "SEU_KEY");
```

Autenticando por OAuth:
```java
        Authentication authentication = new OAuth("SEU_TOKEN_OAUTH");
```

Após definir o tipo de autenticação, é necessário inicializar o sdk e informar em qual ambiente você quer executar suas ações:


## Iniciando o SDK

```java
MoipMpos.init(activity, MoipMpos.Enviroment.PRODUCTION, authentication, new InitCallback() {
    public void onSuccess() {
        //SUCCESS
    }

    public void onError(MposError e) {
        //ERROR
    }
});
```

## Busca por maquininhas pareadas válidas

```java
MoipMpos.searchPinpads(new PairedPinpadsCallback() {
        @Override
        public void onPinpadsFound(List<BluetoothDevice> pinpads) {
           
        }

        @Override
        public void onActionChanged(MposAction action) {
           
        }

        @Override
        public void onError(MposError error) {
            
        }
});
```

## Checando comunicação com a maquininha

O parâmetro `pinpad` é do tipo `BluetoothDevice` e deve ser uma maquininha que já foi pareada com algum dispositivo.
Logo esse método checa se a maquininha está apta para realizar um pagamento.

```java
MoipMpos.isPinpadConnected(pinpad, new PinpadCallback() {
    @Override
    public void onSuccess() {
        
    }

    @Override
    public void onActionChanged(MposAction action) {
        
    }

    @Override
    public void onError(MposError error) {
        
    }
});
```

### Método depreciado

**IMPORTANTE:** Não daremos mais suporte para esse método de pagamento, e iremos remové-lo nas próximas versões.

```java
MoipMpos.isPinpadConnected(new PinpadCallback() {
    @Override
    public void onActionChanged(MposAction action) {
        
    }

    @Override
    public void onSuccess() {
    
    }

    @Override
    public void onError(MposError error) {
        
    }
});
```

## Definindo informações do seu pedido

```java
ItemRequest item = new ItemRequest
        ("Produto/Servico", // Nome do produto ou serviço
        1, // Quantidade
        "Detalhe", // Detalhe
        101); // Valor

List items = Arrays.asList(item);

MposPaymentRequest mposPaymentRequest = new MposPaymentRequest()
        .installment(1) // Quantidade de parcelas
        .ownId("#{SEU IDENTIFICADOR}") // OPCIONAL Identificador único da sua transação, caso não seja informado um valor aleatório será criado
        .type(MposPaymentRequest.Type.CREDIT) //  Tipo da transação: `MposPaymentRequest.Type.CREDIT` e `MposPaymentRequest.Type.DEBIT`
        .items(items); // Lista de itens criada

```


## Criando um pagamento

Agora temos que passar como parâmetro a maquininha que desejamos realizar o pagamento. 
Essa maquininha já deve estar pareada com seu dispositivo.

```java
MoipMpos.charge(activity, pinpad, mposPaymentRequest, new MposCallback() {
        @Override
        public void onSuccess(MposPaymentResponse mposPaymentResponse) {
           //SUCCESS
        }

        @Override
        public void onActionChanged(MposAction action) {
           //Status em que sua transação se encontra, você pode criar um dialog para exibir uma mensagem como preferir
        }

        @Override
        public void onError(MposError error) {
            //ERROR
        }
});
```

### Método depreciado

**IMPORTANTE:** Não daremos mais suporte para esse método de pagamento, e iremos remové-lo nas próximas versões.

```java
MoipMpos.charge(activity, mposPaymentRequest, new MposCallback() {
    @Override
    public void onActionChanged(MposAction action) {
        //Status em que sua transação se encontra, você pode criar um dialog para exibir uma mensagem como preferir
    }

    @Override
    public void onSuccess(MposPaymentResponse mposPaymentResponse) {
        //SUCCESS
    }

    public void onError(MposError e) {
        //ERROR
    }
});

```

**IMPORTANTE**: Ao criar o pagamento você receberá o objeto *MposPaymentResponse* como retorno, com ele em mãos é possível checar se o pagamento foi autorizado ou não através de seu status.

## Status da transação
Conforme a transação é realizada você receberá uma mensagem exibindo o status atual da transação, idealmente você deve exibir essa mensagem (ou uma similar) na tela do celular, para que o comprador e o vendedor possam se orientar.

No momento do pagamento o método `onActionChanged` lança vários eventos que estão listados abaixo:

| Status                           |   Descrição      
|:---------------------------------|:--------------------------------------------------:|
| MposAction.GETTING_USER_LOCATION | Pegando localização do usuário                     | 
| MposAction.LOCATION_RETURNED     | Geolocalização obtida com sucesso                  | 
| MposAction.CREATING_ORDER        | Criando pedido                                     | 
| MposAction.ORDER_CREATED         | Pedido criado                                      | 
| MposAction.CREATING_PAYMENT      | Criando pagamento                                  | 
| MposAction.PAYMENT_CREATED       | Pagamento criado                                   | 
| MposAction.WAITING_CARD          | Insira o cartão                                    | 
| MposAction.INSERTED_CARD         | Cartão inserido                                    | 
| MposAction.REMOVED_CARD          | Cartão removido                                    | 
| MposAction.WAITING_PASSWORD      | Insira a senha do cartão                           | 
| MposAction.PROCESSING            | Processando transação                              |
| MposAction.WAITING_CARD_REMOVAL  | Remova o cartão                                    |
| MposAction.COMPLETED_SALE        | Venda finalizada                                   |


## Tratamento de erros
Quando ocorre algum erro no processo de pagamento, você deve utilizar o método onError() que retornará um `MposError`.

No momento do pagamento podem ocorrer os possiveis erros:
| Código  |   Descrição      
|:--------|:--------------------------------------------:|
| POS-005 | Erro de conexão com a Maquininha             |
| POS-010 | Valor deve ser maior ou igual a R$ 1,00 real |
| POS-038 | Ocorreu um erro ao realizar a transação      |
| POS-039 | Operação cancelada                           |
| POS-040 | Transação negada pelo host                   |
| POS-041 | Cartão inválido ou vencido                   |

## Adicionando um recebedor secundário
Para casos de marketplaces ou aplicações com mais de um recebedor envolvido é possível definir recebedores secundários nos pedidos, para isso basta utilizar o objeto *ReceiverRequest* e adicioná-lo em seu OrderRequest.

Ex:
```java
        ReceiverRequest receiverRequest = new ReceiverRequest().secondary("MPA-123", new AmountRequest().fixed(100));
        mposPaymentRequest.setReceivers(Arrays.asList(receiverRequest));
```
Para maiores informações sobre a utilização de recebedores secundários, acesse nossa documentação sobre [Split de Pagamentos](https://dev.wirecard.com.br/docs/split-de-pagamento) ou a [referência API](https://dev.wirecard.com.br/reference)


# Métodos utiltários
**Obs: Todos os métodos acima que recebem como parâmetro um `pinpad`, devem estar com ele pareado com o dispositivo que deseja realizar transações**

Temos esses métodos utilitários que podem ajudar no pareamento de novas maquininhas.

## Buscando maquininhas
Esse método irá retornar maquininhas que não estão estão pareadas com seu dispositivo.

```java
DevicePairingManager.searchDevices(activity, new PinpadSearchCallback() {
    @Override
    public void onPinpadFound(BluetoothDevice pinpad) {
        //retorna os pinpads encontrados
    }

    @Override
    public void onError(MposError error) {

    }
});
```
## Para a busca por maquininhas não pareadas

```java
DevicePairingManager.stopSearch(activity);
```

## Parear com uma maquininha
- Parâmetro `pinpad` significa com qual maquininha deveja parear
- O método `onStateChange` retorna os seguintes estados
`DevicePairingManager.State.PAIRING`: Está realizando o pareamento, com esse estado pode-se mostrar um feedback para o usuário avisando que o pareamento está sendo realizado.
`DevicePairingManager.State.PAIRED`: Pareamento realizado com sucesso.
`DevicePairingManager.State.NOT_PAIRED`: Ocorreu algum erro e o pareamento não foi realizado.


```java
DevicePairingManager.pairDevice(activity, pinpad, new PinpadPairingCallback() {
    @Override
    public void onStateChange(DevicePairingManager.State state) {
        
    }

    @Override
    public void onError(MposError e) {

    }
});
```