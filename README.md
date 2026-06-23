# Pipeline ETL de Pluviosidade - INMET (Timóteo/MG) 🌧️

## Visão Geral
Este projeto consiste em uma pipeline automatizada de Extração, Transformação e Carga (ETL) desenvolvida no **Apache Hop**. O objetivo é buscar, limpar e processar dados meteorológicos históricos diretamente do portal do Instituto Nacional de Meteorologia (INMET), focando na análise da precipitação anual da cidade de **Timóteo - MG** para os anos de **2023** e **2024**.

O projeto demonstra técnicas avançadas de Engenharia de Dados, como o processamento de arquivos compactados remotos em memória (VFS), desvio de cabeçalhos de metadados irregulares e conversão dinâmica de tipos de dados (*type casting*).

## Arquitetura da Pipeline
Abaixo está a representação visual da esteira de dados configurada no Apache Hop:

<img width="460" height="480" alt="image" src="https://github.com/user-attachments/assets/63953326-820b-4714-b864-9a8276607a66" />


## Principais Características
Em vez de depender de armazenamento local massivo, este projeto implementa uma abordagem altamente eficiente de extração:
1. **Extração Remota (VFS):** Utiliza o Sistema de Arquivos Virtual do Apache Hop (`zip:https://...`) para ler os arquivos `.CSV` diretamente de dentro dos arquivos `.zip` hospedados nos servidores do governo, economizando largura de banda e espaço em disco.
2. **Filtro de Localidade (Regex):** Configurado especificamente com a expressão regular `.*TIMOTEO.*\.CSV` para isolar cirurgicamente os dados da estação meteorológica de Timóteo - MG.[cite: 1]
3. **Tratamento de Cabeçalhos:** A pipeline pula automaticamente as primeiras 8 linhas de metadados do INMET antes de iniciar a leitura do dataset.[cite: 1]
4. **Conversão de Tipos Dinâmica (Type Casting):** Para evitar quebras causadas por dados ausentes ou textuais em colunas numéricas, todos os dados são ingeridos inicialmente como `String` e convertidos explicitamente para `Number` imediatamente antes da agregação.[cite: 1]

## 🛠️ Tecnologias Utilizadas
* **Apache Hop:** Orquestração de dados e desenvolvimento da pipeline ETL.[cite: 1]
* **VFS (Virtual File System):** Extração remota de arquivos ZIP.[cite: 1]
* **Regex (Expressões Regulares):** Direcionamento dinâmico do arquivo da cidade-alvo.[cite: 1]

## Fluxo de Execução (`.hpl`)
`Get file names (VFS)` ➔ `Text file input (Pular cabeçalho)` ➔ `Select values (Limpar metadados do sistema)` ➔ `Strings cut (Extrair Ano)` ➔ `Select values (Converter String para Number)` ➔ `Sort rows` ➔ `Group by (Soma da Precipitação Anual)`[cite: 1]

## Resultados Finais
Após o processamento completo dos dados de 2023 e 2024, a soma acumulada da precipitação horária gerou o seguinte resultado consolidado:[cite: 1]

<img width="761" height="565" alt="image" src="https://github.com/user-attachments/assets/452d4a7a-78fe-4736-9a98-c8d176425e70" />


## Como Executar o Projeto
1. Clone este repositório:
```bash
   git clone [https://github.com/Fillipython/inmet-rainfall-pipeline.git](https://github.com/Fillipython/inmet-rainfall-pipeline.git)
