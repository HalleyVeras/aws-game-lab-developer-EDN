# 🎮 Jogo de Adivinhação na AWS (Serverless)

Este projeto foi desenvolvido como parte do curso da **Escola da Nuvem**, com foco em construir uma aplicação serverless na AWS utilizando **Lambda**, **API Gateway** e **Amazon S3**.

## 📌 Objetivos do Laboratório

- Criar uma função Lambda em Python com a lógica do jogo.
- Expor essa função através de uma API REST com o Amazon API Gateway.
- Publicar um site estático com frontend em um bucket no Amazon S3.
- Integrar o frontend com a API para interatividade em tempo real.

---

## 🧩 Etapas do Projeto

### 1. Criando a Função Lambda

1. Acesse o [console da AWS](https://console.aws.amazon.com/).
2. Busque por **Lambda** > clique em **Criar função**.
3. Escolha **Criar do zero**.
4. Nomeie como: `LambdaGame-seu-nome`.
5. Selecione **Python 3.9** como tempo de execução.
6. Clique em **Criar função**.
7. Faça upload do arquivo `lambda_function.zip` disponibilizado no curso.
8. Após o upload, clique em **Deploy**.

---

### 2. Criando a API com Amazon API Gateway

1. Acesse o serviço **API Gateway** e clique em **APIs** > **API HTTP** > **Compilar**.
2. Nomeie como `Api-seu-nome`.
3. Em **Integração**, selecione **Lambda** e escolha sua função criada.
4. Configure a rota:
   - Método: `GET`
   - Caminho do recurso: `/jogo`
5. No estágio, renomeie de `$default` para `prod`.
6. Após criar, copie o **Invoke URL** do estágio (Ex: `https://abc123.execute-api.us-east-1.amazonaws.com/jogo`) e salve.
7. Configure o CORS:
   - `Access-Control-Allow-Origin`: `*`
   - `Access-Control-Allow-Headers`: `content-type`
   - `Access-Control-Allow-Methods`: `GET`

---

### 3. Publicando o Frontend no Amazon S3

1. Edite o arquivo `index.html`:
   - Altere a URL para o **Invoke URL** copiado.
   - Ajuste o caminho para `/jogo`.
   - Adicione seu nome após “Escola da Nuvem💙”.
2. No console da AWS, acesse o **S3** > clique em **Criar bucket**:
   - Nome: `s3-website-seu-nome`
   - O nome deve ser único globalmente.
3. Com o bucket criado, faça o upload do `index.html`.
4. Acesse **Propriedades** > role até **Hospedagem de site estático**:
   - Ative a opção e defina o documento de índice como `index.html`.
5. Copie o **Endpoint do site** e teste em seu navegador.

---

### 4. Tornando o Site Público (Permissões)

1. No bucket S3, vá até **Permissões**.
2. Clique em **Editar** na seção "Bloquear acesso público" e desmarque a opção **Bloquear todo o acesso público**.
3. Ainda em **Permissões**, edite a **Política do bucket**:
   - Gere uma política com o seguinte JSON:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::s3-website-seu-nome/*"
    }
  ]
}
