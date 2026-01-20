# Laboratório AWS: Implementação de Banco de Dados Relacional com Amazon RDS

Este repositório contém o guia detalhado para a criação de um servidor de banco de dados gerenciado e sua integração com uma aplicação web, utilizando as melhores práticas de alta disponibilidade na AWS.

---

## 🌐 Visão Geral do Projeto

O objetivo deste projeto é demonstrar como configurar o **Amazon Relational Database Service (RDS)** para operar um banco de dados MySQL com **Alta Disponibilidade (Multi-AZ)**. A infraestrutura garante que os dados sejam replicados em diferentes Zonas de Disponibilidade, permitindo que a aplicação web continue operando mesmo em caso de falha em uma das zonas.

### 🎯 Objetivos Técnicos
*   Provisionar uma instância de banco de dados RDS MySQL.
*   Configurar isolamento de rede via **Security Groups**.
*   Implementar **DB Subnet Groups** para redundância geográfica.
*   Validar a conectividade entre a camada de aplicação (EC2) e a camada de dados (RDS).

---

## 🏗️ Arquitetura da Solução

O diagrama abaixo ilustra a topologia da rede, incluindo as sub-redes públicas e privadas, e a replicação síncrona entre a instância primária e a de standby:

![Arquitetura AWS RDS](./images/architecture-diagram.png)

---

## 🛠️ Guia Passo a Passo Detalhado

### Tarefa 1: Configuração de Segurança de Rede (Security Groups)
O primeiro passo é garantir que apenas o servidor web tenha permissão para "conversar" com o banco de dados.

1.  Acesse o console **VPC** e vá em **Security Groups**.
2.  Crie um novo grupo chamado `DB Security Group`.
3.  **Regra de Entrada (Inbound Rule):**
    *   **Tipo:** MySQL/Aurora (Porta 3306).
    *   **Origem (Source):** Selecione o ID do `Web Security Group`.
    *   *Isso garante que apenas instâncias associadas ao grupo da web acessem o banco.*

### Tarefa 2: Definição de Grupos de Sub-redes (DB Subnet Groups)
Para que o RDS seja Multi-AZ, ele precisa saber em quais sub-redes pode operar.

1.  No console **RDS**, vá em **Subnet Groups**.
2.  Crie um grupo chamado `DB Subnet Group`.
3.  Selecione a **Lab VPC**.
4.  Adicione sub-redes em **duas Zonas de Disponibilidade** diferentes (ex: `us-east-1a` e `us-east-1b`).
5.  Utilize as sub-redes privadas identificadas pelos CIDRs `10.0.1.0/24` e `10.0.3.0/24`.

### Tarefa 3: Provisionamento da Instância Amazon RDS
Agora, lançamos o banco de dados propriamente dito.

1.  No console **RDS**, clique em **Create Database**.
2.  **Configurações Principais:**
    *   **Mecanismo:** MySQL.
    *   **Modelo:** Dev/Test.
    *   **Disponibilidade:** Multi-AZ DB Instance (Cria a instância de standby).
3.  **Especificações da Instância:**
    *   **Identificador:** `lab-db`.
    *   **Credenciais:** Usuário `main` e senha `lab-password`.
    *   **Classe:** `db.t3.medium`.
4.  **Conectividade:**
    *   Selecione a **Lab VPC**.
    *   Associe o `DB Security Group` criado na Tarefa 1.
    *   **Database Name:** Defina o nome inicial como `lab`.

### Tarefa 4: Integração e Teste da Aplicação
Com o banco disponível, conectamos a aplicação web.

1.  Obtenha o **Endpoint** da instância RDS (ex: `lab-db.xyz.us-west-2.rds.amazonaws.com`).
2.  Acesse o endereço IP público do seu **WebServer**.
3.  Navegue até a seção **RDS** da aplicação.
4.  Insira os dados de conexão:
    *   **Endpoint:** O endereço copiado do RDS.
    *   **Database:** `lab`.
    *   **User:** `main`.
    *   **Password:** `lab-password`.
5.  **Validação:** Adicione um contato no "Address Book" para confirmar que os dados estão sendo persistidos no RDS.

---

## 🏁 Conclusão
Este laboratório demonstra a robustez do Amazon RDS em gerenciar tarefas complexas como replicação e failover automaticamente, permitindo que desenvolvedores foquem na lógica da aplicação enquanto a AWS cuida da infraestrutura de dados.

---
*Documentação gerada para fins educacionais e portfólio técnico.*
