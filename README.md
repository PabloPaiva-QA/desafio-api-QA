# Desafio de Testes Automatizados de API

Este repositório contém um teste de QA utilizando a API pública Serverest:
https://serverest.dev

Os testes foram desenvolvidos utilizando Postman, seguindo boas práticas, organização estrutural e cenários críticos definidos pela especificação do desafio.

---
## 📑 Sumário
* [📁 Estrutura do Projeto](#-estrutura-do-projeto)
* [✅ Pré-Requisitos](#-pré-requisitos)
* [▶️ Como rodar os testes](#️-como-rodar-os-testes)
* [📌 Levantamento Geral de Cenários](#-levantamento-geral-de-cenários)
  * [Carrinho](#carrinho)
  * [Login](#login)
  * [Produtos](#produtos)
  * [Usuários](#usuários)
* [⚙️ Dados Dinâmicos](#️-dados-dinâmicos)
* [🧹 Boas Práticas Adotadas](#-boas-práticas-adotadas)
* [Autor](#autor)

---
## 📂 Estrutura do Projeto

A collection está organizada por funcionalidades, seguindo a estrutura abaixo (como na imagem enviada):
```
Carrinho/
  POST Criar carrinho com sucesso
  POST Criar segundo carrinho (Pode falhar se já existir)
  DEL  [Para teste] Excluir produtos do carrinho

Login/
  POST Login com sucesso
  POST Login com senha incorreta

Produtos/
  POST Criar produto com sucesso
  POST Criar produto com nome duplicado

Usuários/
  POST Criar usuário com sucesso
  POST Criar usuário com email existente
```
---
## ✅ Pré-Requisitos

Antes de rodar os testes, certifique-se de ter o seguinte:
1. **Postman instalado ou no Navegador**
   - Download: https://www.postman.com/downloads/
2. **Collection e Environment deste repositório**
   - Eles já estão na pasta `/postman`:
     - `desafio-api-QA.postman_collection.json`
     - `Enviroment API.postman_environment.json`
3. (Opcional) **Conta no Postman**
   - Apenas necessária se quiser sincronizar no cloud, mas não é obrigatório.

---
## ▶️ Como rodar os testes

### **1. Abrir o Postman**
Inicie o Postman no seu computador, seja no aplicativo ou no navegador.

### **2. Importar a Collection e o Environment**
1. No canto superior esquerdo, clique em **Import**  
2. Selecione os arquivos da pasta `/postman`:
   - `desafio-api-QA.postman_collection.json`
   - `Enviroment API.postman_environment.json`
3. O Postman criará automaticamente:
   - A Collection  
   - O Environment

### **3. Selecionar o Environment**
1. No canto superior direito, clique no seletor de ambientes (escrito 'No environment') 
2. Escolha **Enviroment API**

### **4. Executar a Collection**
Existem duas formas:
**Usando o Runner do Postman**
1. Abra a Collection **desafio-api-QA**
2. Clique em **Run** (ou **Collection Runner**)
3. Confirme o Environment selecionado
4. Clique em **Start Run**

**Executando request por request**
1. Expanda a Collection  
2. Clique no endpoint desejado  
3. Pressione **Send**

---
## 📌 Levantamento Geral de Cenários
### Carrinho
**Cenários levantados**
* Criar carrinho com sucesso
* Criar carrinho com produto duplicado
* Criar carrinho quando já existe outro
* Criar carrinho com produto inexistente
* Criar carrinho com quantidade insuficiente
* Criar carrinho com token expirado/inválido/ausente
* Consultar todo o carrinho
* Buscar carrinho por ID
* Concluir compra do carrinho (carrinho é removido)
* Cancelar compra do carrinho (carrinho é removido)
* Excluir carrinho inexistente

**Cenários críticos automatizados**
* Criar carrinho com sucesso ⭐
  * _Motivo da escolha:_ Este é o fluxo principal do módulo de carrinhos. Garantir que um carrinho possa ser criado corretamente é essencial, pois afeta diretamente a jornada de compra. É o cenário que mais valida regras combinadas: autenticação, vínculo com usuário, estrutura dos produtos e disponibilidade em estoque.
* Criar carrinho quando já existe outro (Pode falhar caso não tenha carrinho criado) ⭐
  * _Motivo da escolha:_ Este comportamento é uma regra crítica de negócio, então validar o bloqueio é tão importante quanto validar o sucesso. Previnir a criação repetida evita inconsistências de estoque e duplicação de compras.
* [PARA TESTE] Cancelar compra do carrinho (carrinho é removido)
  * _Motivo da criação:_ Este teste foi incluído como apoio técnico. Ele garante que o ambiente de testes possa ser limpo entre execuções, evitando que um carrinho remanescente impeça a validação dos cenários principais.

### Login
**Cenários levantados**
* Login com credenciais válidas
* Login com senha inválida
* Login com email inexistente
* Login sem informar email
* Login sem informar senha
* Login sem informar e-mail e senha
* Login com formato de email inválido
* Login com usuário inexistente
* Login usando caracteres especiais

**Cenários críticos automatizados**
* Login com credenciais válidas ⭐
  * _Motivo da escolha:_ Este é o ponto de entrada de praticamente todas as outras funcionalidades. Sem autenticação válida, carrinhos, produtos e usuários não podem ser manipulados de forma completa. Por isso, é um dos cenários mais críticos de toda a API.
* Login com senha inválida ⭐
  * _Motivo da escolha:_Este cenário garante que a API valida credenciais incorretas corretamente e que não libera tokens indevidamente, prevenindo riscos de segurança. Também assegura que mensagens de erro e status code estejam adequados.

### Produtos
**Cenários levantados**
* Cadastrar produto com sucesso
* Cadastrar produto com mesmo nome
* Cadastrar produto com token expirado/inválido/ausente
* Cadastrar produto sem preço
* Cadastrar produto sem descrição
* Cadastrar produto sem quantidade
* Cadastrar produto com quantidade negativa
* Cadastrar produto com campos vazios
* Cadastrar produto com formato inválido
* Cadastrar produto sem ser administrador
* Ver todos os produtos
* Buscar produto por ID
* Editar produto com sucesso
* Editar produto não cadastrado
* Editar produto com as mesmas informações
* Editar produto com token expirado/inválido/ausente
* Excluir produto com sucesso
* Excluir produto que está no carrinho
* Excluir produto com token expirado/inválido/ausente
* Excluir produto sem ser administrador

**Cenários críticos automatizados**
* Cadastrar produto com sucesso ⭐
  * _Motivo da escolha:_ Muitos testes dependem de produtos válidos, e falhas nesse ponto impactam carrinhos e todo fluxo de compra. Também é crítico validar regras de campos obrigatórios e estrutura básica.
* Cadastrar produto com nome duplicado ⭐
  * _Motivo da escolha:_ A unicidade do nome do produto é uma regra essencial da API. Esse cenário garante integridade do catálogo e evita erros como sobreposição de itens, confusão no estoque e falhas de consistência.

### Usuários
**Cenários levantados**
* Criar usuário com sucesso
* Criar usuário com email já existente
* Criar usuário sem nome
* Criar usuário sem email
* Criar usuário sem senha
* Criar usuário sem o campo administrador
* Criar usuário com todos os dados vazios
* Criar usuário com formato inválido de email
* Ver todos os usuários cadastrados
* Buscar usuário por ID
* Editar usuário cadastrado
* Editar usuário não cadastrado
* Excluir usuário existente sem produto no carrinho
* Excluir usuário existente com produto no carrinho
* Excluir usuário não existente

**Cenários críticos automatizados**
* Criar usuário com sucesso ⭐
  * _Motivo da escolha:_ Esse cenário valida cadastro básico, estrutura do payload, criação de ID e resposta correta. Impacta diretamente fluxos de login, carrinho e até criação de produtos.
* Criar usuário com email já existente ⭐
  * _Motivo da escolha:_ Como email é o identificador principal do usuário, este é um cenário vital para consistência da base. Garantir que usuários não possam ser duplicados previne falhas graves como problemas de autenticação, segurança, sobrescrita de dados e inconsistência de tokens.


**Todos os testes são:**
✔ Independentes
✔ Utilizam variáveis de ambiente
✔ Usam timestamp para evitar duplicidade
✔ Validados por status, mensagem e comportamento esperado
✔ Organizados por funcionalidade

---
## ⚙️ Dados Dinâmicos

Para garantir que os testes não falhem por duplicidade, foi adicionado um timestamp antes de cada requisição:
```
pm.environment.set("timestamp", Date.now());
```

Isso permite gerar:
* emails únicos
* nomes únicos
* produtos únicos

Exemplo em um Body:
```
"nome": "Produto {{timestamp}}"
```

---
## 🧹 Boas Práticas Adotadas
* Testes isolados
* Nomes claros e padronizados
* Test scripts limpos e legíveis
* Uso de environment para ID, token, timestamps
* Validação de múltiplas mensagens quando necessário
* Organização estrutural igual à da imagem fornecida

---
## Autor
Pablo Paiva
