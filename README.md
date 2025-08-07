# projeto-portfolio-engenharia-de-dados

Este projeto é um laboratório prático para aprimorar minhas habilidades em **Engenharia de Dados** e **Business Intelligence**. O projeto visa construir, do zero, um ecossistema de dados completo, simulando um ambiente de produção real com dados fictícios.

A documentação detalhada de cada etapa, desde a arquitetura até a visualização final, está neste repositório.

##  Objetivos do Projeto

* **Infraestrutura:** Utilizar recursos de hardware "on-premise" (máquinas pessoais) para simular um ambiente de servidor, desenvolvendo uma arquitetura de dados escalável e robusta.
* **Engenharia de Dados:** Criar pipelines de ETL/ELT para ingestão, limpeza e transformação de dados, utilizando Python e orquestração.
* **Modelagem e Armazenamento:** Implementar um Data Lake e um Data Warehouse, aplicando conceitos de modelagem dimensional (Star Schema).
* **Business Intelligence:** Desenvolver dashboards e relatórios para extrair insights valiosos e tomar decisões estratégicas.
* **Habilidades Práticas:** Demonstrar proficiência em ferramentas e tecnologias como PostgreSQL, Docker, Python e Power BI.

## Tecnologias Utilizadas

* **Orquestração:** Apache Airflow (via Docker)
* **Linguagem de Programação:** Python
* **Manipulação de Dados:** Pandas
* **Bancos de Dados:** PostgreSQL (Data Warehouse), MySQL (Fonte de dados simulada)
* **Ferramentas de BI:** Power BI (Desktop), Metabase/Apache Superset (para visualização web)
* **Containers:** Docker, Docker Compose
* **Controle de Versão:** Git e GitHub

##  Arquitetura do Ecossistema de Dados

Para este projeto, estou utilizando três máquinas pessoais para simular um ambiente de produção. Abaixo, um diagrama simplificado da arquitetura planejada:

* **Máquina 1 (Servidor Central):** Ubuntu Server com 32GB de RAM, rodando o PostgreSQL (Data Warehouse) e o Apache Airflow. Este é o coração do ecossistema.
* **Máquina 2 (Fonte de Dados):** Ubuntu com 12GB de RAM, rodando um banco de dados MySQL para simular um sistema transacional de origem (ERP/CRM).
* **Máquina 3 (Ambiente de Desenvolvimento):** Windows/Ubuntu com 12GB de RAM, usada para a codificação dos pipelines em Python, desenvolvimento dos dashboards em Power BI e gerenciamento da infraestrutura.

## Documentação da Jornada (Até o Momento)

### **Passo 1: Preparação do Ambiente de Desenvolvimento**

A primeira etapa foi a preparação de um dos notebooks (Ubuntu) para ser o ambiente principal de codificação.

* **Problema Inicial:** Enfrentei um erro de "externally-managed-environment" ao tentar instalar bibliotecas Python, um recurso de segurança do Ubuntu 24.04.
* **Solução:** Segui a recomendação de utilizar um ambiente virtual (`venv`). Isso isola as dependências do projeto do sistema operacional, garantindo a estabilidade e a portabilidade do código.
* **Ferramentas e Bibliotecas Instaladas (dentro do `venv`):**
    * `pandas` - Para manipulação e análise de dados.
    * `sqlalchemy` - Para interação com bancos de dados.
    * `psycopg2-binary` - Adaptador Python para PostgreSQL.
    * `faker` - Para a geração de dados fictícios para o projeto.
* **Status:** O ambiente de desenvolvimento está configurado e pronto para o próximo passo.

---
