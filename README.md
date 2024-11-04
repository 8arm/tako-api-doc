# Tako API

## Credenciais e Autenticação

Para as credenciais de acesso à Tako, serão disponibilizados através de link seguro contendo os dados que serão utilizados para a autenticação da comunicação com nossa API.

- **organizationId**: Identificador da sua organização.
- **accessKey**: Chave de acesso a API.
- **accessSecret**:  Segredo vinculado a chave de API utilizado para autenticação da comunicação entre cliente e Tako.

Exemplo:

```jsx
{
    "organizationId": "cffa0272-b689-4517-bc71-c93fab6ce0d3",
    "accessKey": "eeb35682-d334-4fd7-9b5f-7fd58930ed01",
    "accessSecret": "2r3CnYtRdqFOkGVWuSrRupwuBdTE1NLN4bDiqjLUKuORql1_xOA8nAOMYKer6yoY49zzMb3JTBhjutfXzvL7HA"
}
```

Utilizando **organizationId**, **accessKey** e **accessSecret** recebidos será possível gerar o JWT utilizado na autenticação e autorização das chamadas na nossa API.

Para recuperar o JWT a seguinte requisição deve ser feita no endpoint de emissão de tokens:

```jsx
POST https://customer-api.8arm.io/api/v1/auth/token

Body: {
  "organizationId": "cffa0272-b689-4517-bc71-c93fab6ce0d3",
  "accessKey": "eeb35682-d334-4fd7-9b5f-7fd58930ed01",
  "accessSecret": "2r3CnYtRdqFOkGVWuSrRupwuBdTE1NLN4bDiqjLUKuORql1_xOA8nAOMYKer6yoY49zzMb3JTBhjutfXzvL7HA"
}
```

Após realizar a autenticação através das credenciais o endpoint retornará os seguintes valores em seu corpo:

```jsx
{
    "token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6ImMyODMzNzgwLWVjZTgtNGNjYy1iODM0LTk1M2U3OTY4NDExOCJ9.eyJpc3MiOiJjdXN0b21lci1hcGkuZGV2Ljhhcm0uaW8iLCJhdWQiOiJjdXN0b21lci1hcGkiLCJzdWIiOiJlZWIzNTY4Mi1kMzM0LTRmZDctOWI1Zi03ZmQ1ODkzMGVkMDEiLCJpYXQiOjE3Mjc0NDI3MTQsImV4cCI6MTcyNzQ0MzYxNCwianRpIjoiMWUyMzc0NDgtYTQ1My00Zjc1LTg3NmQtZDczYjUyYjI0Y2IwIiwib3JnIjoiY2ZmYTAyNzItYjY4OS00NTE3LWJjNzEtYzgyZmFiNmNlMGQzIn0.nuhWfcE4qJcVxYEq-nEOKgtm_CPtZNOTlf9qWYuq4s0N4TT2SXuV8-FrZai1s5wnAViTwzmdgpEsq5dROY5s3lpDLP-KqTX5Dji05Fw_4JrOUP_psejyJexy7akzLgELRb2cZquojwFkuzmvxBHKv7JdYG69-WYSXtBM2SWze7SK6Ra-2AegunNvFwKOLWuG384_lj9L7wGqos4dgbyoEBYIlpXgZiBdD2C3BUikCtmk_g6tgUEamI7c9M7pgghzAMQQoKkkpNyZSirKCDA18t_0MNqg7uCI75OLPe75__DFU3vUYJuTL-Je0lc8x1ec0_ezD-_yPIMZRwqwxpZOXAZlnSbYhZGvMKO1GfoU6KxdaKHocsvO3oh9eeske2ShbGlKJGN4CIB4TObpJc9E-GzI2ZIaxkvS3Rk9ICg0Cx1nUaC_AyTiw01c725S2JuOyzjRiqgpaD0583FTB_2ewUK205fQTkl7tA_HXWa0IAdfees8MkM_cvc8VUP3gdi-nu6i78Kk0nzYSvXd8JtoyHZkfFgQzEu5mOfBtO6yLq3MvRrzewX9n4LBlq1gXqmqN59mbzaj2ezp1op1EjiczxlnfHlKainTUyae-TWhfln8TF_zTb-Mtr6NQOM01dP_JFNfBIocV7lP6fVg7qh9I4JZ4hi_ORZLZtZsydyZypL",
    "expirationDate": "2024-09-27T13:26:54.000Z"
}
```

O “**token**” deverá ser armazenado para utilização das chamadas a API. O campo “**expirationDate**” faz referência ao momento (UTC) em que token emitido deixa de ser válido, o tempo de duração é de 15 minutos após sua emissão.

## Endpoints e dados contratuais

Nossa API se concentra em te entregar dados sobre de seus colaboradores e prestadores através do vínculo com seus contratos. A partir dos contratos, conseguimos te entregar mais informação sobre os funcionários e prestadores e a situação de sua relação com a empresa.

Para identificar um contrato utilizamos um identificador que chamamos de "**contractId**”, ele será o valor base não só para consultar dados sobre seus colabores e prestadores como também o identificador utilizado nas chamadas de webhook para avisar sobre um desligamento ou admissão por exemplo.

### Dados do contrato

Para consultar a informação de um contrato em específico o seguinte endpoint deverá ser consultado:

```jsx
GET https://customer-api.8arm.io/api/v1/organization/contracts/<contractId>

Headers:
Authorization: Bearer <token>
```

O endpoint retornará os seguintes dados:

```jsx
{
    "name": "Nome do colaborador",
    "contractId": "e614dca1-402c-4aeb-abad-6bdaa86ceeca",
    "contractType": "br:clt",
    "status": "active",
    "personId": "64734db5-261c-40bb-868f-1cf2f75dab36",
    "legalEntityId": "d1623472-2ff2-56f7-8bdc-947c4f7548c3",
    "workerId": "216",
    "managerId": "fd90286c-041e-44c6-87a9-8ad07e179df0",
    "department": "Engenharia",
    "roleName": "Software Developer I",
    "emailCorp": "colaborador@emailcorporativo.com",
    "costCenterId": "a95cdd1e-3541-54b0-a8ac-f64b4bfb8a27",
    "costCenterName": "Engenharia e Tecnologia",
    "costCenterCode": "0010",
    "startDate": "2023-01-01",
    "endDate": "2099-01-01",
    "cpf": "89212439098"
}
```

Nem todos os dados são retornados todas para todos os contratos, já que dependem do contexto e preenchimento pelo cliente. Segue a descrição de cada campo retornado:

- **name**: Nome do colaborador, sempre será retornado.
- **contractId**: Identificador do contrato, sempre será retornado.
- **contractType**: Tipo do contrato, podendo ser un dos valores "**br:clt**" e "**br:pj**". Sempre será retornado.
- **status**: Estado atual do contrato, sempre será retornado podendo ter os seguintes valores:
    - "**onAdmission**”: Colaborador ou prestador em fase de admissão.
    - “**active**”: O contrato está ativo.
    - "**onLeave**”: Colaborador ou prestados em afastamento/licença.
    - "**terminated**": Contrato terminado.
- **personId**: Identificador da pessoa física, o valor pode ser utilizado para algumas validações na existência de mais de um contrato (ativos ou inativos) para a mesma pessoa física, sempre será retornado.
- **legalEntityId**: Identificador da matriz ou filiar do CNPJ para qual o contrato pertence. Pode ser utilizado em cenários onde a empresa possui múltiplas filiais, sempre será retornado.
- “**workerId**": Matrícula do colaborador, só existirá caso cadastrada pelo cliente.
- "**managerId**": Contract id do gestor do colaborador, caso registrado.
- "**department**”: Departamento, só existirá caso cadastrado pelo cliente.
- "**roleName**”: Nome da função, só existirá caso cadastrado pelo cliente.
- “**emailCorp**": Email corporativo, só existirá caso cadastrado pelo cliente. É comum estar vazio por exemplo durante o período de admissão e cadastrado no inicio da vigência do contrato.
- "**costCenterId**”: Identificador da Tako para os centros de custo de uma organização. Só existirá caso um centro de custo tenha sido vinculado ao contrato pelo cliente.
- “**costCenterName**”: Nome do centro de custo, só existirá caso cadastrado pelo cliente.
- “**costCenterCode**”: Código do centro de custo, só existirá caso cadastrado pelo cliente.
- “**startDate**”: Data de inicio da vigência do contrato.
- "**endDate**”: Data de fim do contrato, só existirá caso o contrato já tenha sido encerrado ou tenha um encerramento agendado.
- "**cpf**”: CPF do colaborador, sempre será retornado. 

### Sumário dos contratos da organização

Também é possível consultar um sumário te todos os contratos de uma organização, filtrando pelo status de desejado. 

O sistema de paginação mapeia no máximo 100 registros por página, e pode ser informado no parâmetro "**pagination.size**”.

Exemplo:

```jsx
POST https://customer-api.8arm.io/api/v1/organization/contracts

Headers:
Authorization: Bearer <token>

Body: {
  "pagination": {
      "page": 0,
      "size": 100
  },
  "status": "terminated"
}
```

A resposta da requisição virá no seguinte formato:

```jsx
{
    "total": 3,
    "data": [
        {
            "contractId": "799451de-04fa-4559-8d5b-a91fac5d25f7",
            "contractType": "br:pj",
            "name": "Alice Fernando Pejota",
            "status": "terminated"
        },
        {
            "contractId": "726410f9-0976-41e3-b2a6-9f52e8e223f1",
            "contractType": "br:clt",
            "managerId": "fd90286c-041e-44c6-87a9-8ad07e179df0",
            "name": "Beatriz Arruma Celete",
            "status": "terminated"
        },
        {
            "contractId": "98e59650-068b-4840-bd12-afea0038ec59",
            "contractType": "br:clt",
            "managerId": "fd90286c-041e-44c6-87a9-8ad07e179df0",
            "name": "Cleide Celete Castro",
            "status": "terminated"
        },
    ],
    "size": 3,
    "page": 0,
    "unfilteredTotal": 35
}
```

Na resposta acima temos os seguintes campos:

- **total**: Número total de contratos identificados pela busca. Por exemplo, se sua organização tivesse 500 contratos "**terminated**” o total teria valor 500.
- **size**: Número de contratos retornado no página/chamada. Existe uma limitação da busca feito pelo processo de paginação, onde uma página/chamada retorna no máximo 100 registros (Ou o valor informado no mesmo parâmetro na chamada).
- **page**: Página dos resultados consultada.
- **unfilteredTotal**: Total de registros não mapeados pelo filtro, por exemplo no caso da requisição acima existem 35 contratos com estado diferente de "**terminated**”.

O sumário de contratos retorna somente o **contractId**, **contractType**, **name**, **managerId** e **status**. Os valores são retornados para todos os contratos.

# Webhooks

## Recebimento

A partir do setup do recebimento de webhooks, deverá ser informado pelo cliente o endpoint para qual as chamadas serão enviadas. Uma requisição POST será feita para cada **mudança do status de um contrato**.

Exemplo de chamada:

```jsx
POST https://<informed url>

Headers:
"Content-Type": "application/json"

Body: {
  "payload": {
    "contractType": "br:clt",
    "contractId": "987e96e8-ff8b-4d1e-8f6e-6660e6335bf7",
    "event": "termination",
    "effectiveDate": "2024-10-30",
    "recordTimestamp": "2024-10-11T19:11:28.850Z"
  },
  "base64Payload": "eyJjb250cmFjdElkIjoiOTg3ZTk2ZTgtZmY4Yi00ZDFlLThmNmUtNjY2MGU2MzM1YmY3IiwiY29udHJhY3RUeXBlIjoiYnI6Y2x0IiwiZXZlbnQiOiJ0ZXJtaW5hdGlvbiIsImVmZmVjdGl2ZURhdGUiOiIyMDI0LTEwLTMwIiwicmVjb3JkVGltZXN0YW1wIjoiMjAyNC0xMC0xMVQxOToxMToyOC44NTBaIn0",
  "signatureAlgorithm": "RSASSA_PKCS1_V1_5_SHA_256",
  "signature": "aKxZaMpdxGpRO7RenQDk93qoW80_iBe7sgtkZfFNZBF_ZYDBIZOyq_QTOCGD9KpZtiilRbGuM0St8FH3nmLVvlKqn-rRsQpuRKtjqUdIhDHumjnaGPHifAQ6p1T9t_JohVo0isAJ9ycKanWoiX5JQlit-xyS54r-UhOiyf0tMNcLkOXKEiP8lMnTyFJzBHEX2bSDzibf2EcKOqhAr31lLbqWiG_veqlOVWNYqF6fF1UWXSqHiStRbgOIwpveetg2odF7i_N42gXXXeETmBUVabhBs4IzMoDF4yOogstALNTCrGj4QYdU7SGgwh9qUIDAf7gIcwiJWY8SUbUSNHfZaL6S5zvPpS8AzIGgujn6G-Ks3APBwoepjOkBrICU2g8rKcPUuS7wgGEmHWnhAByMlXV6Y_DkQCgPVGrpU6KEAAd5RkqDxU_snCr_TWnij09ZuIcY4lDybqiBm7RwkBgBKDZmcV5N-_Yi3Y7FAzavkSlKLRWAoevQqQ9jd8RDKI1OhF7ULgLWCyCNLHubpWQw0VZbglq1IZlE1TikUrvxKNLKEf0xu6TLbbpM1LZQAQ1LwjTKJ6ErEJC2E2epaLr-rgxw4JKp59eS07W4-Ivnh0EXGEPbJg5DWcVIXslLyiYpwmUswbko_m9ujmVY88QNavI7rrmU8BfWYzTtkZVP_f4"
}
```

## Payload

Os dados contidos dentro do campo “**payload”** servem para que seja possível identificar as mudanças em um contrato dentro da Tako. 

Os campos podem ser interpretados da seguinte forma:

- **contractId**: Identificador do contrato, deve ser utilizado para chamar a API e consultar demais dados relacionados ao contrato informado.
- **contractType**: Tipo do contrato, atualmente valores suportados são "**br:clt**" ou "**br:pj**".
- **effectiveDate**: Data em que a mudança será efetivada, pode ser tanto no passado (retificação), como no futuro (agendamento). Formato é YYYY-MM-DD.
- “**event**”: Mapeia qual o tipo de mudança ocorrida no contrato, hoje temos as seguintes:
    - "**admission**": Um novo contrato está sendo admitido. Esse evento ocorre quando um novo contrato é criado e registrado no sistema.
    - "**termination**": O contrato foi encerrado. Esse evento ocorre quando um contrato chega ao fim ou é encerrado por algum motivo, como demissão, rescisão ou término do prazo acordado.
    - "**wentOnVacation**": O contrato entrou em estado de "férias". Esse evento ocorre quando o colaborador associado ao contrato entra em férias e o contrato é temporariamente suspenso.
    - "**returnedFromVacation**": O contrato retornou ao estado ativo após o colaborador voltar de férias. Esse evento marca o fim do período de férias e a retomada das atividades normais do contrato.
    - "**wentOnLeave**": O contrato foi temporariamente suspenso por licença. Esse evento ocorre quando o colaborador associado ao contrato entra em licença (como licença médica, maternidade ou qualquer outro tipo de afastamento).
    - "**returnedFromLeave**": O contrato foi reativado após o término da licença. Esse evento marca o retorno do colaborador às suas atividades normais após o período de afastamento.
    - "**test**": Evento foi enviado por uma chamada ao endpoint de teste de webhook.
- "**recordTimestamp**”: Data e hora de quando esse evento foi registrado na plataforma.

## Assinatura

Para fazer a validação da autenticidade e integridade dos dados enviados é possível verificar a assinatura enviada na chamada. 

Os dados enviados na chamada que serão utilizados para a verificação são:

- **base64payload**: Para que seja possível a normalização dos dados enviados e consequentemente a consistência no processo de assinatura enviamos uma versão do **payload** com encoding base64.
- **signatureAlgorithm**: Algoritmo utilizado para realizar a assinatura do base64payload. Hoje utilizamos por padrão **"RSASSA_PKCS1_V1_5_SHA_256**”. O algoritmo utiliza criptografia assimétrica, ou seja, um certificado contendo a chave pública deverá ser utilizado para realizar a validação.
- **signature**: Assinatura a ser validada.

> O **certificado contendo a chave pública** para a verificação da assinatura nas chamadas de webhook será entregue junto a solicitação do setup do fluxo para a organização.
> 

Exemplo em python3 de como a validação pode ser feita:

```python
import base64
import hashlib
import json
import rsa

def base64_url_decode(input_str):
    """
    Decodifica uma string base64 URL-safe removendo os caracteres de preenchimento ('=').
    """
    padding = 4 - len(input_str) % 4
    input_str += "=" * padding
    return base64.urlsafe_b64decode(input_str)

def verify_signature(payload_b64, signature_b64, public_key_pem):
    """
    Verifica se a assinatura é válida usando a chave pública.
    """

    message = f"{payload_b64}".encode()

    # Decodifica a assinatura do formato base64
    signature = base64_url_decode(signature_b64)

    # Decodifica a chave pública do formato PEM
    public_key = rsa.PublicKey.load_pkcs1_openssl_pem(public_key_pem)

    # Verifica a assinatura usando RSASSA-PKCS1-v1_5 e SHA-256
    try:
        rsa.verify(message, signature, public_key)
        return True  # Assinatura válida
    except rsa.VerificationError:
        return False  # Assinatura inválida

def validate_signature(payload_b64, signature_b64, public_key_pem):

    # Decodifica o payload de base64 para JSON
    payload_json = base64_url_decode(payload_b64)
    payload_dict = json.loads(payload_json)

    # Verifica a assinatura
    if verify_signature(payload_b64, signature_b64, public_key_pem):
        print("Assinatura válida.")
        return payload_dict
    else:
        print("Assinatura inválida.")
        return None

### Exemplo de uso

payload_b64 = "eyJjb250cmFjdElkIjoiZTYzMjMxZTMtODUzNi00ODU4LThhMjctNTUxODQ2ZmNmYmJlIiwiZXZlbnRUeXBlIjoiU1RBVFVTX0NIQU5HRSIsInN0YXR1cyI6InRlcm1pbmF0ZWQiLCJwcmV2aW91c1N0YXR1cyI6ImFjdGl2ZSJ9"
signature_b64 = "SDHd111NSZFOAmP9TN4P7kgt1FjgfN6ukpt3h8JJZhZ_equwfXSwTqLGC4Y5hOxPOo711Tpyf5JkHn951FXR2pRr9z9Isaf083pBr7gysxZBQDztEyLH0yzJ8h0MQRf1I05kNby_TU7ddmF77RutlsFIsizsXyDX8jyt0xYrg-nofQDCdV8qyIAAtYe1VvLYVjKikqrG2IZ7Si3qaQKH_5EsKvV3eXNQoElcNk4yx4X7qx7ISAnr9V4HafFiR6FX_4wlLpxo3_oPv9wBzwnybVfgBci771zE333LoWIJGLV2EIAmcrn2ZfU9qPhibfRx2HVINvB4KV1-j0jEZGRFArHZpxWKBnufzasPgs15ylAPE2ce5c1pCpO_6hY3vNU9YBNpiVXzNeKsuEj6h0SooNe9JSwTD8N_qQ14GID4PXfpIcmJzKap3wJA0tbs_m3lSLS40cp8nXWzVxhk7WxHfZxabP7vZcYDO8exqhWAqb9vikT0cgzTjKjmbLXMCDcvEuGU5kHkzB3wjqEuu7bitqkHgjHgvRqO5InkLucoJrWTbbPtqXJlu9U3P7EnuoAYT6qoLofSDSWYITfuZYR3C69skjtBHb7zM9aCYkDH0d_t2oHT404T21xV0eks8Tpi03GKj4COxIvYsKrOzgb_JSOjGVLzVbpzrW0C0uEbW14"

public_key_pem = b"""-----BEGIN PUBLIC KEY-----
MIICIjANBgkqhkiG9w0BAQEFAAOCAg8AMIICCgKCAgEAs9LO2KjTnoLUwg85UhDN
cIr9EyY2Nxwc+/OgFTh+hT9MOhJYOfJHNMy5M+IXqiy+LQLSo6koCxYn6DDkNJP3
DMK0+a7OHBIYxgxvZ/L/5SYIXpyX6XqavVAKJRUVz8IUTbgDodcDOBczrAAbKCUu
1CTWqbAbMvwAAase0oXrWZ4Ow07P+xm2NlCobARfkdI0gDl3J/gbWy5j52WZm8GX
RXKsgxrSK7h2zPo5KnQWPgkuGSrPB7N2io6fKB/16GkwK//tnbwVepYfvZI1/uxQ
0a+uzOLRyETM10mpKHS357C7AT8D7E1CQst/HI7n2b0nPwV6pRm8044AQAT3WU+7
tvTfINHUAA7LQbtJOtmQUdJfvFXt45d7/c5TR533m5i98/Z5ljGvhthHIeOxcxNo
AwSPbfGnDVL/C/xhMRP+ifDKZtkOp7B/OT1lQlOSeJsBar8vN0gV7+b4aDjq/cQN
7jGCUVqG3N8fby9Qzu7/6iF5VLk6mckTmsex5VkWDclLhujX7rBIDE258+rD8Uni
vtkE86ojJovjqIGs78SXWaqKh0D/cZl2xhyJ6Q7brE77wh0NMICt85xWpTFaX+it
ORIJTnXWu0VaqRxIEx7PeHl2isgEQ4kv/AWSTSF48u1Gmlr3cSzumyH5W4DWbYFI
TnCRtyU99M2dIZGo9CFBsPUCAwEAAQ==
-----END PUBLIC KEY-----"""

payload = validate_signature(payload_b64, signature_b64, public_key_pem)

if payload:
    print("Payload:", payload)
else:
    print("Falha na verificação do assinatura.")
```

## Endpoint de teste de webhook

A funcionalidade de teste de webhook permite que os usuários de nossa API verifiquem se as configurações de webhook da organização estão corretas e operacionais. Ao chamar este endpoint, é realizada uma simulação de envio de dados para a URL configurada do webhook da organização, ajudando a garantir que a integração esteja funcionando conforme esperado.

```jsx
POST https://customer-api.8arm.io/api/v1/webhook/test

Headers:
Authorization: Bearer <token>

Body: {
  "contractId": <uuid>,
  "effectiveDate": <YYYY-MM-DD>
}
```

* Os valores no corpo da request são opcionais. Caso não sejam enviados, um `contractId` aleatório será gerado e a data de hoje será usada como `effectiveDate`.

### Exemplo de resposta

```jsx
POST https://<informed url>

Headers:
"Content-Type": "application/json"

Body: {
  "payload": {
    "contractType": "br:clt",
    "contractId": "52c9573c-96c6-49ec-9f4a-72206dbc3ee1",
    "event": "test",
    "effectiveDate": "2023-10-03",
    "recordTimestamp": "2024-10-14T18:22:38.571"
  },
  "base64Payload": "eyJjb250cmFjdElkIjoiNTJjOTU3M2MtOTZjNi00OWVjLTlmNGEtNzIyMDZkYmMzZWUxIiwiY29udHJhY3RUeXBlIjoiYnI6Y2x0IiwiZXZlbnQiOiJ0ZXN0IiwiZWZmZWN0aXZlRGF0ZSI6IjIwMjMtMTAtMDMiLCJyZWNvcmRUaW1lc3RhbXAiOiIyMDI0LTEwLTE0VDE4OjIyOjM4LjU3MSJ9",
  "signatureAlgorithm": "RSASSA_PKCS1_V1_5_SHA_256",
  "signature": "rR5fJWG3HBooTtwK17UaHJwd9EgZTauglT2Wc4433tsvrwvToHwjJu4Z8ohP_W1qibnFSpAIOGokZgwZ3oz_JDFiAi-YcVVPS3VTRpLe3Fo1qQXN_PJiXQs1FCVWqIpvAp_4_lX3PLIVYMbMNcR0pMo60gPBArnKXrKTv5QwOn-mY4giQabooP-HsBOYCk3KfcPr_n1q9-622ls3hlwTif4_B_wATycYjhR0f8DPlW7ANPocUEqpie6ng6a-ioaA8W8s65kHG7fYNsGOHZBlSion9Y60Qo4cQ05gmBv84VEGqzVcWJANIl9pa1km-Dtc9eoYnKKpidZWUPywvk01LODPsBuigZVYlV1AyO4qqr5e1R7zxXyVjNA0BgGlaupEF64WOrD8qQjum4UG6VYAJXMsfpIAyw58aziCDb6TD64-6UTMor-JEaFRH7hQTK0FBzpAPzpAN5VD10XfNjyPcVu_wu1jRlncqdtixC6G6L9eFVrYqDotRHyc78mGIC3w3jqQIIzN_37Vwe1TngydNMKQO4-CWtYs3Dsh7WKwmtiSZH1BZkIN4Z3TIaap_G83JD4xypGiqeucW3WBqL44ddyqXCJIim8g_ywSbC_4a2S2qnb-GlOYPmbxBzSO4oxF-LFBcztdu7kJ0KFpktrwbQB0CgW1JeRlcBIQ7ZbYXOo"
}
```
