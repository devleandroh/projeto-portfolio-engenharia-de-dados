# projeto-portfolio-engenharia-de-dados

Este projeto narra a construção de um pipeline de dados end-to-end (de ponta a ponta) que simula o sistema de ingestão de dados de uma frota de veículos e suas manutenções. O grande desafio foi implementar a extração de dados de uma fonte, o carregamento em um banco de dados relacional e, por fim, a automação completa de todo o fluxo.

Arquitetura do Projeto

A arquitetura do projeto foi pensada para operar inicialmente em duas máquinas, usando a nuvem como ponto de integração. No entanto, o design foi feito de forma a permitir uma fácil escalabilidade para uma arquitetura mais complexa de três máquinas, que seria dedicada ao processamento analítico.

<img width="2048" height="2048" alt="Gemini_Generated_Image_pv0kplpv0kplpv0k" src="https://github.com/user-attachments/assets/9fe3e453-dce6-48a0-9f82-e9f9e71c48ed" />

Máquina 1 (Windows): Este ambiente foi usado como a Fonte de Dados. Foi aqui que criei os scripts que simulam a geração de novos dados operacionais e que foram usados para publicá-los na nuvem.

Google Drive: O Google Drive foi escolhido para atuar como a Área de Staging, um local central e temporário que serviu como ponte entre as máquinas.

Máquina 2 (Ubuntu): Este foi o Servidor de Dados, com a responsabilidade de consumir os dados da nuvem e carregá-los em um banco de dados relacional MySQL.


Tecnologias e Ferramentas

Para dar vida ao projeto, optei por um conjunto de ferramentas robustas e de código aberto:

    Python: A linguagem de programação que usei para a orquestração e o desenvolvimento de todos os scripts do pipeline.

    Pandas: Esta biblioteca foi fundamental para a manipulação dos dados, permitindo a criação e o gerenciamento dos DataFrames que representam os arquivos CSV.

    SQLAlchemy: Utilizei este toolkit de SQL para prover uma camada de abstração com o banco de dados MySQL, facilitando a comunicação de forma segura e eficiente.

    MySQL: O banco de dados relacional foi escolhido para a persistência dos dados brutos, que serviriam de base para futuras análises.

    rclone: Uma ferramenta de linha de comando que usei para sincronizar de forma confiável os arquivos CSV da máquina local para a nuvem.

    Git: O versionamento do código foi feito com o Git, uma prática essencial que me ajudou a manter o controle sobre o desenvolvimento.

    Cron e Agendador de Tarefas do Windows: A automação do pipeline foi alcançada com o uso dessas ferramentas nativas de agendamento, que garantiram que os scripts fossem executados em intervalos pré-definidos.

Configuração e Pré-requisitos

Para replicar este projeto, o primeiro passo foi seguir uma série de configurações:

1. Configuração do Ambiente Python

Em ambas as máquinas (Windows e Ubuntu), o ambiente virtual foi criado e as dependências foram instaladas.

```# O repositório foi clonado
git clone <URL_DO_SEU_REPOSITORIO>
cd <NOME_DO_PROJETO>

# Um ambiente virtual foi criado e ativado
python -m venv venv
# Linux
source venv/bin/activate
# Windows
.\venv\Scripts\activate

# As bibliotecas necessárias foram instaladas
pip install pandas sqlalchemy pymysql rclone```

2. Configuração do MySQL

No servidor de banco de dados (Máquina 2), um banco de dados e um usuário foram criados para o pipeline.


```CREATE DATABASE logistica_fonte;
CREATE USER '<NOME_DE_USUARIO_ETL>'@'localhost' IDENTIFIED BY '<SENHA_FORTE>';
GRANT ALL PRIVILEGES ON logistica_fonte.* TO '<NOME_DE_USUARIO_ETL>'@'localhost';
FLUSH PRIVILEGES;```

Observação: Para permitir o acesso remoto de outra máquina, foi necessário alterar o bind-address do MySQL e liberar uma das portas no firewall apenas para IPs autorizados.


3. Estrutura de Dados (DDL)

As tabelas foram criadas de forma programática através do script em Python, mas a sua estrutura foi definida para acomodar os dados de veículos e manutenções da seguinte forma:

Tabela veiculos

```CREATE TABLE `veiculos` (
  `placa` VARCHAR(10) NOT NULL,
  `tipo_veiculo` VARCHAR(50) DEFAULT NULL,
  `status` VARCHAR(50) DEFAULT NULL,
  `quilometragem_total` INT DEFAULT NULL,
  `data_aquisicao` DATE DEFAULT NULL,
  PRIMARY KEY (`placa`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;```


Tabela manutencoes

```CREATE TABLE `manutencoes` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `placa_veiculo` VARCHAR(10) DEFAULT NULL,
  `tipo_manutencao` VARCHAR(100) DEFAULT NULL,
  `data_manutencao` DATE DEFAULT NULL,
  `custo` DECIMAL(10,2) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `manutencoes_ibfk_1` (`placa_veiculo`),
  CONSTRAINT `manutencoes_ibfk_1` FOREIGN KEY (`placa_veiculo`) REFERENCES `veiculos` (`placa`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;```

O Uso do Pipeline

No início do projeto, o pipeline era executado de forma manual:

    Na Máquina 1 (Windows): O script generate_and_upload_data.py era rodado para gerar e enviar os arquivos CSV para o Google Drive.

    Na Máquina 2 (Linux): Em seguida, o script load_data_to_mysql.py era executado para fazer o download dos arquivos e carregá-los no MySQL.

A Automação do Pipeline

A automação foi a etapa que transformou o projeto em um sistema autossuficiente. A orquestração das tarefas foi feita através de agendadores nativos de cada sistema operacional:

    Na Máquina 1 (Windows): O Agendador de Tarefas do Windows foi configurado para executar o script de geração e envio de dados em uma frequência recorrente.

    Na Máquina 2 (Ubuntu): O Cron foi utilizado para agendar a execução do script de carregamento de dados alguns minutos depois da tarefa do Windows, garantindo que os arquivos já estivessem na nuvem.

Otimizações e o Roadmap Futuro

Com o pipeline já funcionando, tracei um roadmap para futuras melhorias, com o objetivo de evoluir o projeto para um ambiente mais profissional:

    Pipeline ELT Avançado: Uma terceira máquina, de alta capacidade de memória (32 GB RAM), pode ser integrada para extrair os dados brutos, realizar transformações mais complexas e carregar o resultado em um Data Warehouse (Data Warehouse).

    Orquestração de Fluxo de Trabalho: Em vez de usar agendadores nativos, ferramentas como Apache Airflow poderiam ser implementadas para gerenciar, visualizar e monitorar as dependências entre as tarefas do pipeline de forma mais robusta.

    Monitoramento e Logs: A inclusão de um sistema de monitoramento para enviar alertas em caso de falhas e um sistema de logs mais detalhado seriam essenciais para a confiabilidade do pipeline.

    Controle de Versão de Dados: O uso de ferramentas como o DVC (Data Version Control) poderia ser explorado para gerenciar as diferentes versões dos datasets ao longo do tempo.
