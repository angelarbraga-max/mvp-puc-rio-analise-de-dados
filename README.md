# 📊 MVP - Pipeline de Dados de Turismo Internacional: Mercado Emissor Espanha

Este repositório contém o projeto prático desenvolvido para a pós-graduação da **PUC-Rio**, focado na construção de um pipeline de dados "do zero" na nuvem utilizando a plataforma **Databricks**, **PySpark**, **Delta Lake** e **Unity Catalog**.
1. Visão Geral e Objetivo do Negócio
A Espanha é o 6º maior mercado emissor de turistas para o Brasil. O objetivo deste projeto é ingerir, tratar e agregar os dados brutos de chegadas de visitantes internacionais, permitindo responder a perguntas estratégicas de negócio:
Volume de Chegadas: Qual a quantidade acumulada de visitantes espanhóis nos estados brasileiros de janeiro a agosto de 2026?
Distribuição Regional: Quais são os principais estados de destino (UFs) e polos de atração?
Vias de Acesso: Qual a representatividade do modal aéreo em comparação com o terrestre e aquaviário?
Governança do Pipeline: Qual foi o carimbo de data/hora da última atualização e ingestão dos dados?
2. Marco Temporal e Abrangência dos Dados
Período de Análise: Janeiro a Agosto de 2026 (Acumulado até o mês de Agosto).
Granularidade: Dados consolidados mensalmente por Unidade da Federação (UF) e via de acesso (Aérea, Terrestre, Marítima e Fluvial).
Filtro Aplicado na Camada Silver: Mercado emissor da Espanha (origem de 116.840 visitantes no período acumulado de 2026).
Data da Última Ingestão no Pipeline: Registo dinâmico capturado na coluna ultima_atualizacao na Camada Gold via current_timestamp().
3. Arquitetura da Solução
(Arquitetura Medalhão)O pipeline segue a padronização Medalhão para garantir qualidade, rastreabilidade e integridade dos dados:
Camada Bronze (workspace.default.bronze_chegadas_turistas): Ingestão dos dados brutos em formato Delta, com padronização do delimitador ;, tratamento de arrays com a função get() para prevenção de exceções de índice e criação da coluna de metadados de ingestão (data_ingestao).
Camada Silver (workspace.default.silver_chegadas_espanha): Limpeza de caracteres Unicode corrompidos (\uFFFD), correção de acentuação dos nomes dos estados e vias de acesso com regexp_replace, conversão de tipos numéricos e filtragem exclusiva do mercado da Espanha.
Camada Gold (workspace.default.gold_turistas_espanha_por_uf): Consolidação dos dados para consumo analítico, contendo as somatórias de passageiros por estado e via de transporte.
4. Evidência de Execução e Resultados (Camada Gold)Abaixo está a visualização da tabela Gold no Databricks após o processamento completo do pipeline:
5. Dicionário de Dados (Tabela Gold)ColunaTipoDescriçãouf_destinoSTRINGEstado brasileiro (UF) de entrada/desembarque do visitante.via_acessoSTRINGModal de transporte utilizado (Aérea, Terrestre, Marítima, Fluvial).total_turistas_espanhoisBIGINTSomatório acumulado de turistas espanhóis no período.ultima_atualizacaoTIMESTAMPRegistros do carimbo de data/hora do processamento no Delta Lake.
6. Tecnologias UtilizadasDatabricks Community / ServerlessApache Spark / PySpark & Spark SQLDelta Lake & Unity CatalogGit / GitHub
