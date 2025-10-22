# RAG-Agent: Agente de IA com Langchain

Este projeto implementa um Agente de IA utilizando a arquitetura Retrieval-Augmented Generation (RAG) com a biblioteca Langchain. O agente é capaz de responder a perguntas com base num conhecimento pré-definido, carregado a partir de documentos PDF e armazenado numa base de dados vetorial Chroma.

## Funcionalidades

*   **Geração de Respostas**: Responde a perguntas do utilizador utilizando um modelo de linguagem grande (LLM) da OpenAI.
*   **Recuperação de Informação**: Pesquisa e recupera informações relevantes de uma base de conhecimento vetorial (Chroma DB) para fundamentar as respostas.
*   **Processamento de Documentos**: Carrega documentos PDF, divide-os em fragmentos (chunks) e vetoriza-os para criação da base de conhecimento.

## Estrutura do Projeto

O projeto é composto pelos seguintes ficheiros principais:

*   `main.py`: O script principal que executa o agente RAG, recebe perguntas do utilizador e gera respostas.
*   `criar_db.py`: O script responsável por carregar documentos PDF, processá-los e criar/atualizar a base de dados vetorial Chroma.
*   `.env`: Ficheiro de configuração para armazenar variáveis de ambiente sensíveis, como a chave da API da OpenAI.
*   `.gitignore`: Define os ficheiros e diretórios a serem ignorados pelo sistema de controlo de versões Git.
*   `README.md`: Este ficheiro, que fornece uma visão geral do projeto, instruções de configuração e uso.
*   `anatomia.pdf`: Um exemplo de documento PDF que pode ser usado como base de conhecimento.

## Configuração do Ambiente

Para executar este projeto, siga os passos abaixo:

### 1. Pré-requisitos

Certifique-se de ter o Python 3.x instalado no seu sistema.

### 2. Instalação de Dependências

Instale as bibliotecas Python necessárias utilizando `pip`:

```bash
pip install python-dotenv langchain langchain-openai langchain-community langchain-chroma chromadb openai pypdf
```

### 3. Configuração das Variáveis de Ambiente

Crie um ficheiro `.env` na raiz do projeto e adicione a sua chave da API da OpenAI. Substitua `sua_chave_api_openai_aqui` pela sua chave real.

```dotenv
OPENAI_API_KEY = sua_chave_api_openai_aqui
```

### 4. Preparação da Base de Conhecimento

Crie uma pasta chamada `base` na raiz do projeto. Coloque todos os seus documentos PDF (por exemplo, `anatomia.pdf`) dentro desta pasta. Estes documentos serão usados para construir a base de conhecimento do agente.

## Uso

### 1. Criar a Base de Dados Vetorial

Antes de fazer perguntas ao agente, é necessário criar a base de dados vetorial a partir dos seus documentos PDF. Execute o script `criar_db.py`:

```bash
python criar_db.py
```

Este script irá processar os PDFs na pasta `base`, dividi-los em fragmentos e armazená-los numa base de dados Chroma localizada no diretório `database`.

### 2. Interagir com o Agente RAG

Após a criação da base de dados, pode executar o agente principal e começar a fazer perguntas:

```bash
python main.py
```

O agente irá solicitar que insira a sua pergunta e, em seguida, fornecerá uma resposta baseada nas informações recuperadas da base de conhecimento e geradas pelo LLM.

## Como Funciona (Visão Geral Técnica)

1.  **Carregamento de Documentos**: O script `criar_db.py` utiliza `PyPDFDirectoryLoader` para carregar todos os ficheiros PDF da pasta `base`.
2.  **Divisão em Chunks**: Os documentos são divididos em fragmentos menores (`chunks`) usando `RecursiveCharacterTextSplitter`. Isso ajuda a gerir o tamanho do contexto para o LLM e a melhorar a relevância da pesquisa.
3.  **Vetorização (Embedding)**: Cada chunk é convertido num vetor numérico (embedding) usando `OpenAIEmbeddings`. Estes embeddings capturam o significado semântico do texto.
4.  **Base de Dados Vetorial (Chroma DB)**: Os embeddings são armazenados na base de dados Chroma. Esta base de dados permite uma pesquisa eficiente por similaridade.
5.  **Consulta do Agente (`main.py`)**:
    *   Quando o utilizador faz uma pergunta, a pergunta é também convertida num embedding.
    *   A base de dados Chroma é consultada para encontrar os chunks mais semanticamente semelhantes à pergunta (`similarity_search_with_relevance_scores`).
    *   Os chunks recuperados são combinados com a pergunta do utilizador para formar um `prompt` enriquecido.
    *   Este `prompt` é enviado para um `ChatOpenAI` (LLM) que gera uma resposta contextualizada.

###Feito por: 

Pedro Rodrigues Almeida 

