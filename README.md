# Plano de Teste - Lojinha API



[TOC]

## Como Montar seu Script de Teste de API



### Introdução

Este tutorial detalha o processo para criar e executar scripts de teste de automação para a Lojinha API usando o JMeter. A seguir, você encontrará o passo a passo para configurar o plano de teste, criar grupos de usuários, gravar ações, configurar cookies e variáveis, e executar os testes.

---

### Passo 1: Criar Plano de Teste

**Criar o Plano de Teste:**

   -   Crie um novo plano de teste chamado **Lojinha API Testes** > Salve.

---

### Passo 2: Criar Grupo de Usuários

 Clique com o botão direito em **Lojinha API Testes** > **Adicionar** > **Threads (users)** > **Grupo de Usuários**.
   * * **Nome**: Login e Cadastro de Produto	

     -   **Prioridades de Usuário Virtual** : Número de usuários virtuais (threads): 1

---

### Passo 3: Criar Requisição Http

1. Abra o Swagger da Lojinha

   

   ![image-20240909202358126](C:\Users\maria\AppData\Roaming\Typora\typora-user-images\image-20240909202358126.png)



2. Clique com o botão direito em **Lojinha API Testes** > **Adicionar** > **Testador** > Requisição HTTP

   * **Nome**: Capturar Token

     * Aba: Basic > 

       * Servidor Web

         * Protocolo [http]: http

         * Nome do Servidor ou Ip: 165.227.93.41

         * Requisição Http

           * Post
           * Caminho: /lojinha/v2/login

         * Aba: Body Data

           > ```json
           > {
           >   "usuarioLogin": "admin",
           >   "usuarioSenha": "admin"
           > }
           > ```



![image-20240909203056904](C:\Users\maria\AppData\Roaming\Typora\typora-user-images\image-20240909203056904.png)

------

### Passo 4: Capturar token - Adicionar informações do Cabeçalho Http

* Clique com o botão direito em **Capturar Token** > Adicionar > Elemento de configuração > Gerenciador de Cabeçalho HTTP
  * Nome: Content-Type
  * Valor: application/json

![image-20240909221036851](C:\Users\maria\AppData\Roaming\Typora\typora-user-images\image-20240909221036851.png)

------

### Passo 5: Relatório da execução dos testes

1. Clique com o botão direito em **Login com Cadastro de Produto** > Adicionar > Ouvinte > Ver Arvore de Resultados

   ![image-20240909221447033](C:\Users\maria\AppData\Roaming\Typora\typora-user-images\image-20240909221447033.png)

2. Execute o Teste

   ![image-20240909222130897](C:\Users\maria\AppData\Roaming\Typora\typora-user-images\image-20240909222130897.png)

   3. Capturar o Token

      1. Selecione a visualização: **JSON Path Tester**
      2. Em **Json Patch Expression** (você coloca um expressão para identificar elementos): **$.data.token**

      ![image-20240909223244174](C:\Users\maria\AppData\Roaming\Typora\typora-user-images\image-20240909223244174.png)

------

### Passo 6: Adicionar Requisição HTTP

1. Clique com o botão direito em **Login com Cadastro de Produto** > Adicionar > Testador > Requisição HTTP

   1. Nome: Cadastrar Produto

   2. Protocolo [http]:

   3. Porta: não precisa preencher pois usa o padrão 80 já liberado no Windows

   4. Requisição Http: Post

   5. Caminho: /lojinha/v2/produto

   6. Body Data 

      > ```
      > {
      >   "produtoNome": "Samsung Expert",
      >   "produtoValor": 100.00,
      >   "produtoCores": [
      >     "preto"
      >   ],
      >   "produtoUrlMock": "string",
      >   "componentes": [
      >     {
      >       "componenteNome": "Fonte do Carregador",
      >       "componenteQuantidade": 5
      >     }
      >   ]
      > }
      > ```

![image-20240909223827230](C:\Users\maria\AppData\Roaming\Typora\typora-user-images\image-20240909223827230.png)

------

### Passo 7: Cadastro Produto - Adicionar informações do Cabeçalho Http

1. Clique com o botão direito em **Cadastrar Produto** > Adicionar > Elemento de Configuração > Gerenciador de Cabeçalhos HTTP

   * Adicionar

     * Nome: Content-Type | Valor: application/json

     * Nome: token | Valor: ${token}

       

   ![image-20240909225303829](C:\Users\maria\AppData\Roaming\Typora\typora-user-images\image-20240909225303829.png)

------

### Passo 8: Capturar Token da Resposta

1. Clique com o botão direito em **Capturar Token** > Adicionar > Pós-Processadores >  JSON Extractor

   * Names of created variables: token

   * JSON Path expressions: $.data.token

   * Match No. (0 for Random): 1

     

![image-20240909224837586](C:\Users\maria\AppData\Roaming\Typora\typora-user-images\image-20240909224837586.png)

------

### Passo 9: Executar Teste

![image-20240909225529254](C:\Users\maria\AppData\Roaming\Typora\typora-user-images\image-20240909225529254.png)

------

### Passo 10: Variáveis Definidas pelo usuário

1. Clique com o botão direito em **Login com Cadastro de Produto** > Adicionar > Elemento de Configuração > Variáveis Definidas pelo usuário

   * Adicionar

     * Nome: usuarioPadrao | Valor: admin

     * Nome: senhaPadrao | Valor: admin

       

![image-20240909225938508](C:\Users\maria\AppData\Roaming\Typora\typora-user-images\image-20240909225938508.png)

2. Capturar Token > Aba: Body Data

   1. Substituir usuário e senha pelo nome das variáveis

      

   ![image-20240909230329120](C:\Users\maria\AppData\Roaming\Typora\typora-user-images\image-20240909230329120.png)

------

### Passo 11: Adicionar Padrão de Requisições HTTP

1. Clique com o botão direito em **Login com Cadastro de Produtos** > Adicionar > Elemento de configuração > Padrão de Requisição HTTP

   * Aba: Basic > 

     * Servidor Web

       * Protocolo [http]: http

       * Nome do Servidor ou Ip: 165.227.93.41

         

2. Remova a configuração de requisição do **Captura Token** e **Cadastro de Produto**

![image-20240909230643558](C:\Users\maria\AppData\Roaming\Typora\typora-user-images\image-20240909230643558.png)

------

### Passo 12: Executar Teste

3. Execute os testes para verificar se está tudo funcionando

   ![image-20240909231054844](C:\Users\maria\AppData\Roaming\Typora\typora-user-images\image-20240909231054844.png)

   ![image-20240909231116339](C:\Users\maria\AppData\Roaming\Typora\typora-user-images\image-20240909231116339.png)