# Analisador de Semelhança de Documentos com Embeddings


## **INTRODUÇÃO**
Essa aplicação demonstra o poder dos embeddings na análise de similaridade entre textos. Ela recebe como entrada um conjunto de documentos e uma consulta, e então utiliza embeddings gerados pelo modelo Gemini para encontrar o documento mais semelhante à consulta. Um gráfico interativo permite visualizar a relação entre os embeddings no espaço vetorial.


## **AJUDA**
Caso você sinta dificuldade em entender algum dos conteudos a seguir recomendo olhar o documento que criei sintetizando os tópicos de Engenharia de Prompt e Manipulação de IAs:

https://docs.google.com/document/d/15er_ycXyRA8rs-qy3Fhtxrxtb8OMkvI8cAtOaiE28bo/edit?usp=sharing


## **⚙️ Seção 1: Instalação de Bibliotecas e Configurações Essenciais**

Essa seção inicial prepara o ambiente para a execução da aplicação, instalando as bibliotecas necessárias e definindo configurações importantes.

**Bibliotecas:**
1.   `google-generativeai:` Fornece acesso à API do Google Generative AI, permitindo utilizar o modelo Gemini para gerar embeddings.
2.   `sklearn`: Oferece ferramentas para Machine Learning, incluindo o algoritmo PCA para redução de dimensionalidade.
3.   `matplotlib`: Biblioteca para criação de gráficos e visualizações, utilizada para plotar os embeddings.
4.   `numpy`: Ferramenta fundamental para computação numérica em Python, utilizada para manipular vetores e matrizes.
5.   `pandas`: Biblioteca para análise e manipulação de dados, utilizada para criar e manipular o DataFrame que armazena os documentos.
6.   `textwrap`: Será usada para exibir o resultado final de forma mais legível.

**Configurações:**
1.   `GOOGLE_API_KEY`: Armazena a sua chave de API do Google Generative AI, essencial para utilizar os serviços da plataforma.
2.   `config_geral`: Define parâmetros para a geração de texto com o modelo Gemini, como temperatura e número de candidatos.
3.   `model`: Especifica o modelo de embedding a ser utilizado, no caso, "models/embedding-001".

Ao instalar as bibliotecas e definir as configurações corretas, essa seção garante que a aplicação tenha todas as ferramentas e recursos necessários para funcionar corretamente.

### **OBSERVAÇÃO:** 
Pode ser que apareça uma mensagem de erro, todavia basta ignorar porque a aplicação funcionará normalmente.


## **💻 Seção 2: Funções Auxiliares para Análise de Embeddings:**
Essa seção define três funções auxiliares que serão utilizadas na aplicação:

1.   `embed_fn`: Responsável por gerar os embeddings dos documentos utilizando o modelo Gemini. Recebe como entrada o título e o conteúdo do documento e retorna o vetor de embedding.
2.   `gerar_e_buscar_consulta`: Realiza a busca pelo documento mais semelhante à consulta. Recebe a consulta, a base de dados (DataFrame) e o modelo de embedding como entrada. Calcula o produto escalar entre o embedding da consulta e os embeddings de cada documento, retornando o conteúdo do documento mais similar, o embedding da consulta e o índice do documento correspondente.
3.   `minimize_embedding`: Reduz o tamanho do embedding para exibição no DataFrame. Trunca o vetor de embedding, mostrando apenas os primeiros 50 caracteres seguidos de "...".

Essas funções encapsulam tarefas importantes da aplicação, tornando o código mais organizado, legível e modular.


## **📁 Seção 3: Coleta de Dados: Entrada dos Documentos**

Esta seção é responsável por obter as informações essenciais do usuário: os documentos que serão analisados pela aplicação.

**Passo a passo:**
1.   **Número de documentos:** O usuário informa quantos documentos deseja inserir, o que define o tamanho da base de dados.
2.   **Criação da lista:** Uma lista vazia chamada `documents` é criada para armazenar os dados dos documentos.
3.   **Loop de entrada:** Um loop percorre a quantidade de documentos especificada pelo usuário. Em cada iteração:
*   Solicita ao usuário o título do documento.
*   Solicita ao usuário o conteúdo do documento.
*   Armazena o título e o conteúdo em um dicionário.
*   Adiciona o dicionário à lista `documents`.

Ao final dessa seção, a lista `documents` conterá todas as informações fornecidas pelo usuário, prontas para serem processadas e analisadas pela aplicação.


## **📋 Seção 4: Criação e Formatação do DataFrame**
Esta seção processa os dados dos documentos coletados e os organiza em um DataFrame, além de gerar os embeddings e estilizar a tabela para melhor visualização.

**Passo a passo:**
1.   **Criação do DataFrame:** A lista `documents` é utilizada para criar um DataFrame Pandas (`df`), estrutura ideal para manipulação e organização dos dados.
2.   **Definição de colunas:** As colunas do DataFrame são renomeadas para "Titulo" e "Conteudo", representando as informações de cada documento.
3.   **Geração de Embeddings:** A função `embed_fn` é aplicada a cada linha do DataFrame para gerar os embeddings dos documentos, que são armazenados na coluna "Embeddings".
4.   **Estilizando o DataFrame:** A aparência do DataFrame é aprimorada com:
*   Alinhamento do texto à esquerda para o conteúdo e centralizado para o cabeçalho.
*   Ocultação dos índices numéricos das linhas.
*   Adição de bordas para delimitar as células.
*   Formatação dos embeddings utilizando a função `minimize_embedding` para exibição resumida.

Ao final dessa seção, um DataFrame estilizado contendo os títulos, conteúdos e embeddings dos documentos é apresentado ao usuário, facilitando a visualização e compreensão dos dados.


## **🔍 Seção 5: Busca por Semelhança: Processando a Consulta**
Esta seção lida com a entrada da consulta do usuário e realiza a busca pelo documento mais semelhante na base de dados.

**Passo a passo:**
1.   **Entrada da consulta:** O usuário insere a consulta em formato de texto, representando o tema ou a informação que deseja encontrar nos documentos.
2.   **Busca por similaridade:** A função gerar_e_buscar_consulta é utilizada para:
*   Gerar o embedding da consulta.
*   Comparar o embedding da consulta com os embeddings de cada documento no DataFrame, utilizando o produto escalar como medida de similaridade.
*   Identificar o documento com maior similaridade.
3.   **Obtenção de informações:** A função retorna:
*   O conteúdo do documento mais similar (resultado).
*   O embedding da consulta (embedding_consulta).
*   O índice do documento mais similar no DataFrame (indice_resultado).

As informações retornadas serão utilizadas na próxima etapa para visualizar a relação entre a consulta e os documentos no espaço vetorial.


## **📊 Seção 6: Visualização Interativa dos Embeddings com PCA**
Esta seção utiliza a técnica de redução de dimensionalidade PCA para visualizar os embeddings da consulta e dos documentos em um gráfico bidimensional, permitindo uma análise visual da relação de similaridade entre eles.

**Passo a passo:**
1.   **Redução de dimensionalidade:** O algoritmo PCA é aplicado para transformar os embeddings, originalmente em um espaço de alta dimensão, em um espaço bidimensional, preservando ao máximo a variância dos dados.
2.   **Plotagem dos embeddings:** Um gráfico de dispersão é criado, onde cada ponto representa um embedding:
*   **Documentos:** Representados por círculos cinza.
*   **Consulta:** Representada por uma estrela azul.
*   **Documento mais similar:** Representado por um círculo vermelho com borda.
3.   **Identificação dos documentos:** Os títulos dos documentos são exibidos ao lado de seus respectivos pontos no gráfico, facilitando a identificação visual.
4.   **Elementos visuais:** A legenda, os rótulos dos eixos, a grade e o título do gráfico tornam a visualização mais clara, informativa e agradável.

Essa visualização interativa permite ao usuário compreender a relação entre os embeddings, verificando a proximidade espacial entre a consulta e os documentos. Observa-se que o documento mais similar se encontra mais próximo da consulta no espaço vetorial.


## **✅ Seção 7: Apresentação do Resultado: Documento Mais Semelhante**
Esta seção final apresenta ao usuário o resultado da busca: o conteúdo do documento que foi identificado como mais semelhante à consulta fornecida.

**Passo a passo:**
1.   **Exibição do título:** Uma mensagem simples e clara indica que o conteúdo a seguir é o resultado da busca.
2.   **Impressão do conteúdo:** O conteúdo completo do documento mais similar é exibido ao usuário.
3.   **Quebra de linha:** É usado o `textwrap.fill` para facilitar a leitura do resultado final.

Com isso, a aplicação conclui seu objetivo de encontrar e apresentar a informação mais relevante para o usuário, com base na análise de similaridade utilizando embeddings.
