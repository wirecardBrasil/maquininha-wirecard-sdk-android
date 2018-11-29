# Moip v2 MPos client SDK - Android

O jeito mais simples e rápido de integrar o moip ao seu App usando a Máquina de cartões para venda presencial do Moip 
# Instalação

Caso você use o Gradle, você pode adicionar a dependência em seu projeto
```java
        repositories {
            maven { url 'https://oss.sonatype.org/content/repositories/snapshots/' }
            maven { url "https://packagecloud.io/stone/sdk-android/maven2/" }
        }

        packagingOptions {
            exclude 'META-INF/DEPENDENCIES'
        }
```

```java
        implementation 'br.com.moip.mpos:mpos-android-sdk:4.0.8'
```

# Permissões

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


# Iniciando o SDK

```java
        MoipMpos.init(activity, MoipMpos.Enviroment.PRODUCTION,
                            authentication,
                            new InitCallback() {
                                public void onSuccess() {
                                    //SUCCESS
                                }
                                public void onError(MposError e) {
                                    //ERROR
                                }
        });
```

```
# Checando comunicação com a maquininha

```java
        MoipMpos.isPinpadConnected(activity, new PinpadCallback() {
                    @Override
                    public void onSuccess() {
                        //SUCCESS
                    }

                    public void onError(MposError e) {
                        //ERROR
                    }
                });
```

# Definindo informações do seu pedido
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
# Criando um pagamento

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

# Status da transação
Conforme a transação é realizada você recebera uma mensagem exibindo o status atual da transação, idealmente você deve exibir essa mensagem (ou uma similar) na tela do celular, para que o comprador e o vendedor possam se orientar.

# Tratamento de erros
Quando ocorre algum erro no processo de pagamento, você deve utilizar o método onError() que retornará um `MposError`.

# Adicionando um recebedor secundário
Para casos de marketplaces ou aplicações com mais de um recebedor envolvido é possível definir recebedores secundários nos pedidos, para isso basta utilizar o objeto *ReceiverRequest* e adicioná-lo em seu OrderRequest.
Ex:
```java
        ReceiverRequest receiverRequest = new ReceiverRequest().secondary("MPA-123", new AmountRequest().fixed(100));
        mposPaymentRequest.setReceivers(Arrays.asList(receiverRequest));
```
Para maiores informações sobre a utilização de recebedores secundários, acesse nossa documentação:
https://moip.com.br/referencia-api/
