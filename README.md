# Azure Cognitive Search: Utilizando AI Search para indexação e consulta de Dados

## Crie um recurso do Azure AI Search

Entre no portal do Azure.

Clique no botão + Criar um recurso, pesquise o Azure AI Search e crie um recurso do Azure AI Search.

Selecione Revisar + criar e, depois de ver a resposta Validação bem-sucedida, selecione Criar.

## Crie um recurso de serviços de IA do Azure

Retorne à página inicial do portal do Azure. 

Clique no botão ＋Criar um recurso e pesquise os serviços de IA do Azure. 

Selecione criar um plano de serviços de IA do Azure. 

Você será levado a uma página para criar um recurso de serviços de IA do Azure. 

Configure-o.

Selecione Revisar + criar. Depois de ver a resposta Validação aprovada, selecione Criar.

Aguarde a conclusão da implantação e visualize os detalhes da implantação.

## Crie uma conta de armazenamento

Retorne à página inicial do portal do Azure e selecione o botão + Criar um recurso.

Procure a conta de armazenamento e crie um recurso de conta de armazenamento.

Clique em Revisar e em Criar. Aguarde a conclusão da implantação e vá para o recurso implantado.

Na conta de Armazenamento do Azure que você criou, no painel de menu esquerdo, selecione Configuração (em Configurações).

Altere a configuração de Permitir acesso anônimo de Blob para Habilitado e selecione Salvar.

## Carregar documentos para o armazenamento do Azure

No painel de menu esquerdo, selecione Contêineres.

Selecione + Contêiner. Um painel do seu lado direito é aberto.

Insira as configurações e clique em Criar.

No portal do Azure, selecione o contêiner de avaliações. No contêiner, selecione Carregar.

No painel Carregar blob, selecione Selecionar um arquivo.

Na janela do Explorer, selecione todos os arquivos de avaliações, selecione Abrir e, em seguida, selecione Carregar.

Depois que o upload for concluído, você poderá fechar o painel Upload blob. Seus documentos estão agora em seu contêiner de armazenamento de avaliações.

## Indexar os documentos

No portal do Azure, navegue até o recurso do Azure AI Search

Na página Visão geral, selecione Importar dados.

Na página Conectar-se aos seus dados, na lista Fonte de Dados, selecione Armazenamento de Blobs do Azure. Preencha os detalhes do armazenamento de dados.

Na seção Anexar Serviços Cognitivos, selecione o seu recurso de serviços Azure AI.

Na seção Adicionar enriquecimentos:

- Altere o nome da Skillset.
- Marque a caixa de seleção Habilitar OCR e mesclar todo o texto no campo merged_content.
- Certifique-se de que o campo Dados de origem esteja definido como merged_content.
- Altere o nível de granularidade de enriquecimento para Páginas (blocos de 5.000 caracteres).
- Não selecione Habilitar enriquecimento incremental

Selecione os campos enriquecidos.

Selecione Escolha uma conexão existente. Escolha a conta de armazenamento que você criou anteriormente.

Clique em + Container para criar um novo contêiner chamado armazenamento de conhecimento com o nível de privacidade definido como Privado e selecione Criar.

Selecione o contêiner de armazenamento de conhecimento e clique em Selecionar na parte inferior da tela.

Selecione projeções de blob do Azure: Documento. Uma configuração para o nome do contêiner com as exibições preenchidas automaticamente do contêiner de armazenamento de conhecimento. Não altere o nome do contêiner.

Selecione Próximo: Personalizar índice de destino. Altere o nome do índice para índice de café.

Certifique-se de que a chave esteja definida como metadata_storage_path. Deixe o nome do sugeridor em branco e o modo de pesquisa preenchido automaticamente.

Revise as configurações padrão dos campos de índice. Selecione filtrável para todos os campos que já estão selecionados por padrão.

Selecione Próximo: Criar um indexador.

Altere o nome do indexador para indexador de café.

Deixe a programação definida como Uma vez.

Expanda as opções avançadas. Certifique-se de que a opção Base-64 Encode Keys esteja selecionada, pois as chaves de codificação podem tornar o índice mais eficiente.

Selecione Enviar para criar a fonte de dados, o conjunto de habilidades, o índice e o indexador. O indexador é executado automaticamente e executa o pipeline de indexação, que:
- Extrai os campos de metadados do documento e o conteúdo da fonte de dados.
- Executa o conjunto de habilidades cognitivas para gerar campos mais enriquecidos.
- Mapeia os campos extraídos para o índice.

Volte à página de recursos do Azure AI Search. No painel esquerdo, em Gerenciamento de pesquisa, selecione Indexadores. Selecione o indexador de café recém-criado. Atualize até que o Status indique sucesso.

Selecione o nome do indexador para ver mais detalhes.

## Consultar o índice

Na página Visão geral do serviço de pesquisa, selecione Explorador de pesquisa na parte superior da tela.

Observe como o índice selecionado é o índice de café que você criou. Abaixo do índice selecionado, altere a visualização para JSON.

No campo do editor de consultas JSON, copie e cole:

>{
>    "search": "*",
>    "count": true
>}

Selecione Pesquisar. A consulta de pesquisa retorna todos os documentos no índice de pesquisa, incluindo uma contagem de todos os documentos no campo @odata.count. O índice de pesquisa deve retornar um documento JSON contendo os resultados da pesquisa.

Agora vamos filtrar por localização. No campo do editor de consultas JSON, copie e cole:

>{
> "search": "locations:'Chicago'",
> "count": true
>}

Selecione Pesquisar. A consulta pesquisa todos os documentos no índice e filtra revisões com localização em Chicago. Você deverá ver 3 no campo @odata.count.

Agora vamos filtrar por sentimento. No campo do editor de consultas JSON, copie e cole:

>{
> "search": "sentiment:'negative'",
> "count": true
>}

Selecione Pesquisar.

A consulta pesquisa todos os documentos no índice e filtra revisões com sentimento negativo. Você deverá ver 1 no campo @odata.count.

## Revise o armazenamento de conhecimento

No portal do Azure, navegue de volta para a sua conta de armazenamento do Azure.

No painel de menu esquerdo, selecione Contêineres. Selecione o contêiner de armazenamento de conhecimento.

Selecione qualquer um dos itens e clique no arquivo objectprojection.json.

Selecione Editar para ver o JSON produzido para um dos documentos do seu armazenamento de dados do Azure.

Selecione a localização atual do blob de armazenamento no canto superior esquerdo da tela para retornar aos contêineres da conta de armazenamento.

Em Containers, selecione o container coffee-skillset-image-projection. Selecione qualquer um dos itens.

Selecione qualquer um dos arquivos .jpg. Selecione Editar para ver a imagem armazenada no documento. Observe como todas as imagens dos documentos são armazenadas desta forma.

Selecione a localização atual do blob de armazenamento no canto superior esquerdo da tela para retornar aos contêineres da conta de armazenamento.

Selecione Navegador de armazenamento no painel esquerdo e selecione Tabelas. Há uma tabela para cada entidade no índice. Selecione a tabela coffeeSkillsetKeyPhrases.

Observe as frases-chave que o armazenamento de conhecimento conseguiu capturar do conteúdo das avaliações. Muitos dos campos são chaves, portanto você pode vincular as tabelas como um banco de dados relacional. O último campo mostra as frases-chave que foram extraídas pelo conjunto de habilidades.
