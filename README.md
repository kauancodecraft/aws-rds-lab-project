# Laboratório AWS: Criar um Servidor de Banco de Dados e Interagir com um Aplicativo

Este projeto documenta o processo de utilização de uma instância de banco de dados gerenciada pela AWS (**Amazon RDS**) para atender às necessidades de um banco de dados relacional em uma infraestrutura de nuvem.

## Visão Geral

O **Amazon Relational Database Service (Amazon RDS)** facilita a configuração, operação e escalonamento de bancos de dados relacionais. Este laboratório foca na implementação de uma instância MySQL com **Alta Disponibilidade (Multi-AZ)**.

### Objetivos

*   Executar uma instância de banco de dados Amazon RDS com alta disponibilidade.
*   Configurar grupos de segurança para permitir conexões do servidor web.
*   Interagir com um aplicativo web conectado ao banco de dados.

---

## Arquitetura do Projeto

Abaixo está o diagrama representativo da infraestrutura criada:

![Arquitetura AWS RDS](./images/architecture-diagram.png)

---

## Tarefas Realizadas

### 1. Criar um Grupo de Segurança para o RDS
Configuração de um grupo de segurança para permitir que o servidor web acesse a instância de banco de dados na porta **3306 (MySQL)**.
*   **Nome:** `DB Security Group`
*   **Regra de Entrada:** Permitir tráfego do `Web Security Group`.

### 2. Criar um Grupo de Sub-redes de Banco de Dados
Definição das sub-redes em diferentes Zonas de Disponibilidade (AZs) que o RDS pode utilizar.
*   **Sub-redes:** 10.0.1.0/24 e 10.0.3.0/24.

### 3. Criar uma Instância de Banco de Dados Amazon RDS
Lançamento de uma instância MySQL Multi-AZ.
*   **Identificador:** `lab-db`
*   **Classe:** `db.t3.medium`
*   **Configurações:** Multi-AZ habilitado para alta disponibilidade.

### 4. Interagir com o Banco de Dados
Conexão de um aplicativo web ao endpoint do RDS para realizar operações de CRUD (Create, Read, Update, Delete) em um catálogo de endereços.

---

## Conclusão

Ao final deste laboratório, a infraestrutura está pronta para suportar aplicações de produção com redundância e escalabilidade, utilizando as melhores práticas da AWS para bancos de dados gerenciados.

---
*Este projeto foi criado como parte de um exercício prático de laboratório AWS.*
