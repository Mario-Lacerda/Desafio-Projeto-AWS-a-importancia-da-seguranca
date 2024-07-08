# Desafio Dio - Desafio de Projeto AWS - A Importância da Segurança



##### Sobre a **importância da segurança** na implementação de projetos na **AWS (Amazon Web Services)**. 

##### A segurança é um aspecto crítico em qualquer ambiente de computação em nuvem, e a AWS oferece várias ferramentas e práticas recomendadas para garantir a proteção dos dados e recursos.

Além disso, este projeto usa AWS Lambda, Amazon S3 e Amazon DynamoDB para armazenar e gerenciar os códigos QR gerados. Isso permite que você gere códigos QR de forma escalável, econômica e confiável.



### **Pré-requisitos**

- Conta AWS
- Node.js 14 ou superior
- npm 6 ou superior



### **Configuração**

1. Crie uma nova função Lambda no AWS Console:

   - Nome da função: `qr-code-generator`

   - Tempo de execução: Node.js 14.x

   - Manipulador: `index.handler`

     

2. #### Instale as seguintes dependências na sua função Lambda:

   plaintext

   

   #### npm install qrcode aws-sdk

   

3. #### Crie um arquivo `index.js` com o seguinte código:

   javascript

   ```javascript
   const qrcode = require('qrcode')
   const AWS = require('aws-sdk')
   
   const s3 = new AWS.S3()
   const ddb = new AWS.DynamoDB()
   
   exports.handler = async (event) => {
     try {
       const { product } = event.body
   
       const url = await qrcode.toDataURL(JSON.stringify(product))
   
       const params = {
         Bucket: 'seus-bucket-no-s3',
         Key: `${product.nome}.png`,
         Body: Buffer.from(url.replace(/^data:image\/png;base64,/, ''), 'base64'),
         ContentType: 'image/png'
       }
   
       await s3.upload(params).promise()
   
       const item = {
         TableName: 'tabela-qr-codes',
         Item: {
           id: { S: `${product.nome}` },
           url: { S: url }
         }
       }
   
       await ddb.putItem(item).promise()
   
       return {
         statusCode: 200,
         body: JSON.stringify({ url })
       }
     } catch (err) {
       return {
         statusCode: 500,
         body: JSON.stringify({ error: err.message })
       }
     }
   }
   ```

   

4. #### Crie uma tabela do DynamoDB chamada `tabela-qr-codes` com as seguintes chaves primárias:

   - #### id (String)

     

5. #### Implante sua função Lambda.



### **Uso**

Para usar o gerador de QR Code, você pode enviar uma solicitação POST para o endpoint da sua função Lambda com o seguinte JSON no corpo da solicitação:

json



```json
{
  "product": {
    "nome": "Produto 1",
    "descricao": "Descrição do produto 1",
    "preco": "R$ 10,00"
  }
}
```



A função Lambda retornará um JSON com a URL do código QR gerado. Você pode escanear este código QR com um leitor de QR Code para obter mais informações sobre o produto.





##### Sobre a **importância da segurança** na implementação de projetos na **AWS (Amazon Web Services)**. 

##### A segurança é um aspecto crítico em qualquer ambiente de computação em nuvem, e a AWS oferece várias ferramentas e práticas recomendadas para garantir a proteção dos dados e recursos.

#### 

## Medidas de Segurança na AWS:



1. #### **AWS Identity and Access Management (IAM)**:

   - O IAM permite especificar quem ou o que pode acessar serviços e recursos na AWS.

   - Gerencie permissões refinadas de maneira centralizada.

   - [Analise o acesso para refinar as permissões na AWS](https://github.com/JordanOC/AWS-security)[1](https://github.com/JordanOC/AWS-security).

     

2. #### **Amazon Virtual Private Cloud (Amazon VPC)**:

   - Oferece controle total sobre o ambiente de redes virtual.

   - Configure sua VPC para posicionar recursos, definir conectividade e garantir segurança.

   - [Use sub-redes privadas e configure grupos de segurança](https://jovemprojeto.com.br/blog/a-importancia-da-seguranca-na-implementacao-de-projetos-de-cloud-computing/)[2](https://jovemprojeto.com.br/blog/a-importancia-da-seguranca-na-implementacao-de-projetos-de-cloud-computing/).

     

3. #### **AWS Key Management Service (AWS KMS)**:

   - Crie, gerencie e controle chaves criptográficas nas aplicações e serviços da AWS.

   - [Garanta a segurança dos dados por meio de criptografia](https://www.mindtek.com.br/2024/06/as-7-melhores-praticas-de-seguranca-na-aws/)[3](https://www.mindtek.com.br/2024/06/as-7-melhores-praticas-de-seguranca-na-aws/).

     

4. #### **Ferramentas Adicionais**:

   - Utilize o **CloudTrail** para rastrear atividades na sua conta AWS.

   - O **CloudWatch** monitora recursos e eventos.

   - [O **Trusted Advisor** oferece recomendações para otimizar a segurança e eficiência](https://github.com/VivianeSGomes/seguranca_AWS)[4](https://github.com/VivianeSGomes/seguranca_AWS).

     



## Conclusão:



Este é um projeto abrangente que demonstra como criar um gerador de QR Codes para e-commerces usando AWS Lambda, Amazon S3 e Amazon DynamoDB. 

Este projeto é mais abrangente do que os anteriores porque usa AWS Lambda para gerar códigos QR de forma escalável, Amazon S3 para armazenar os códigos QR gerados e Amazon DynamoDB para gerenciar os códigos QR gerados. Você pode expandir este projeto adicionando recursos como geração de códigos QR em lote, envio de códigos QR por e-mail e muito mais.

A implementação dessas ferramentas aumenta a confiabilidade e a credibilidade da aplicação ou serviço oferecido pela empresa. [Recomenda-se continuar utilizando essas ferramentas e explorar novas tecnologias para melhorar ainda mais os processos da organização](https://github.com/JordanOC/AWS-security)
