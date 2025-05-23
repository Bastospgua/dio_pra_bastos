# Guia Completo: Processo de Redundância de Arquivos na Azure com Databricks

Este guia abrangente detalha o processo de desenvolvimento de um projeto prático e completo de redundância de arquivos utilizando recursos do Microsoft Azure. O foco principal é a integração entre ambientes on-premises e a nuvem, utilizando Azure Data Factory e Azure Databricks para orquestração, movimentação e transformação de dados.

O objetivo é fornecer um passo a passo detalhado, desde a criação e configuração dos recursos necessários até a validação, execução e aplicação de boas práticas de segurança, permitindo a implementação de uma solução robusta e eficiente para redundância de dados.

## Sumário

1.  [Introdução](#introdução)
2.  [Criação do Azure Data Factory](#criação-do-azure-data-factory)
3.  [Instalação e Configuração do Integration Runtime](#instalação-e-configuração-do-integration-runtime)
4.  [Configuração do Ambiente Local (BS_BASTOS)](#configuração-do-ambiente-local-bs_bastos)
5.  [Configuração do Banco de Dados Local (ItibereBD)](#configuração-do-banco-de-dados-local-itiberebd)
6.  [Configuração do Azure Blob Storage](#configuração-do-azure-blob-storage)
7.  [Estabelecendo Conexões Seguras com Ambientes On-Premises](#estabelecendo-conexões-seguras-com-ambientes-on-premises)
8.  [Configuração de Linked Services](#configuração-de-linked-services)
9.  [Criação de Datasets para Leitura e Escrita](#criação-de-datasets-para-leitura-e-escrita)
10. [Criação de Pipelines no Data Factory](#criação-de-pipelines-no-data-factory)
11. [Configuração para Conversão de Dados em .TXT](#configuração-para-conversão-de-dados-em-txt)
12. [Organização dos Arquivos em Camadas](#organização-dos-arquivos-em-camadas)
13. [Configuração do Azure Databricks](#configuração-do-azure-databricks)
14. [Utilização do Databricks para Transformação de Dados](#utilização-do-databricks-para-transformação-de-dados)
15. [Validação dos Pipelines](#validação-dos-pipelines)
16. [Publicação e Execução dos Pipelines](#publicação-e-execução-dos-pipelines)
17. [Análise de Performance das Execuções](#análise-de-performance-das-execuções)
18. [Boas Práticas de Configuração e Segurança](#boas-práticas-de-configuração-e-segurança)

---

## Introdução

Este guia abrangente detalha o processo de desenvolvimento de um projeto prático e completo de redundância de arquivos utilizando recursos do Microsoft Azure. O foco principal é a integração entre ambientes on-premises e a nuvem, utilizando Azure Data Factory e Azure Databricks para orquestração, movimentação e transformação de dados.

O objetivo é fornecer um passo a passo detalhado, desde a criação e configuração dos recursos necessários até a validação, execução e aplicação de boas práticas de segurança, permitindo a implementação de uma solução robusta e eficiente para redundância de dados.

Este documento compila todas as etapas e informações geradas ao longo do projeto, organizadas de forma lógica e sequencial para facilitar a consulta e implementação.

---

## Criação do Azure Data Factory

O Azure Data Factory (ADF) é um serviço de integração de dados baseado em nuvem que permite criar fluxos de trabalho orientados a dados para orquestrar e automatizar a movimentação e transformação de dados. Neste guia, vamos detalhar o processo de criação do Azure Data Factory chamado "adf-dio-bastos" para nosso projeto de redundância de arquivos.

### Pré-requisitos

Antes de iniciar a criação do Azure Data Factory, certifique-se de que você possui:

- Uma assinatura ativa do Microsoft Azure
- Permissões adequadas para criar recursos na sua assinatura (Contributor ou Owner)
- Conhecimento básico do portal Azure

### Processo de Criação do Azure Data Factory

#### 1. Acessando o Portal Azure

O primeiro passo é acessar o Portal Azure através do endereço [https://portal.azure.com](https://portal.azure.com) e fazer login com suas credenciais. Após o login, você será direcionado para a página inicial do portal.

#### 2. Criando um Novo Recurso

Para criar um novo Azure Data Factory:

1.  Clique no botão "+ Criar um recurso" no canto superior esquerdo da tela
2.  Na barra de pesquisa, digite "Data Factory" e selecione "Data Factory" nos resultados
3.  Clique no botão "Criar" para iniciar o processo de criação

#### 3. Configurando o Data Factory

Na página "Criar Data Factory", você precisará preencher as seguintes informações:

**Guia Básico:**

-   **Assinatura**: Selecione a assinatura Azure que deseja utilizar
-   **Grupo de Recursos**: Selecione um grupo existente ou crie um novo (recomendamos criar um grupo específico para este projeto, como `rg-redundancia-arquivos`)
-   **Região**: Escolha a região mais próxima da sua localização geográfica ou dos seus usuários para minimizar a latência
-   **Nome**: Digite `adf-dio-bastos` (este é o nome específico solicitado para o projeto)
-   **Versão**: Selecione `V2`

**Guia Rede:**

-   Mantenha as configurações padrão para conectividade pública

**Guia Segurança:**

-   Habilite o Microsoft Defender for Data Factory (opcional, mas recomendado para ambientes de produção)
-   Configure a identidade gerenciada (recomendamos usar a identidade gerenciada atribuída pelo sistema)

**Guia Tags:**

-   Adicione tags relevantes para organização e governança (opcional, mas recomendado)
    -   Exemplo: `Projeto=RedundanciaArquivos`, `Ambiente=Desenvolvimento`

**Guia Revisão + criar:**

-   Revise todas as configurações e clique em "Criar"

#### 4. Aguardando a Implantação

A implantação do Azure Data Factory pode levar alguns minutos. Você pode acompanhar o progresso na página de notificações do portal Azure. Quando a implantação for concluída, você receberá uma notificação.

#### 5. Acessando o Data Factory

Após a implantação bem-sucedida:

1.  Clique em "Ir para o recurso" na notificação ou navegue até o recurso através da barra de pesquisa
2.  Na página inicial do seu Data Factory, você verá um resumo do recurso e várias opções de configuração
3.  Para começar a criar pipelines e outros componentes, clique no botão "Abrir Azure Data Factory Studio" ou "Autor e Monitor"

#### 6. Explorando o Azure Data Factory Studio

O Azure Data Factory Studio é uma interface web onde você pode:

-   Criar pipelines, datasets, linked services e outros componentes
-   Monitorar a execução dos pipelines
-   Configurar triggers para automação
-   Gerenciar recursos do Data Factory

Familiarize-se com a interface, pois será utilizada extensivamente nas próximas etapas do projeto.

### Considerações Importantes

-   **Custo**: O Azure Data Factory é cobrado com base no uso. Certifique-se de entender o modelo de preços antes de implementar em produção.
-   **Segurança**: Configure corretamente as permissões de acesso ao Data Factory para garantir que apenas usuários autorizados possam modificar os recursos.
-   **Monitoramento**: Habilite o Azure Monitor para acompanhar o desempenho e a integridade do seu Data Factory.
-   **Backup**: Considere usar o controle de versão do Git para manter um histórico das alterações no seu Data Factory.

### Próximos Passos

Após a criação bem-sucedida do Azure Data Factory, o próximo passo será configurar o Integration Runtime para estabelecer a conexão entre o ambiente de nuvem e o ambiente local (on-premises). Isso permitirá a movimentação de dados entre o banco de dados local (ItibereBD) e o Azure Blob Storage.

---

## Instalação e Configuração do Integration Runtime

O Integration Runtime (IR) é um componente crucial do Azure Data Factory que serve como ponte entre o ambiente de nuvem e o ambiente local (on-premises). Neste guia, vamos detalhar o processo de instalação e configuração do Integration Runtime chamado `integrationRuntimeB` para nosso projeto de redundância de arquivos.

### O que é o Integration Runtime?

O Integration Runtime é a infraestrutura de computação utilizada pelo Azure Data Factory para fornecer as seguintes capacidades:

-   Movimentação de dados entre ambientes de nuvem e locais
-   Despacho de atividades para diferentes ambientes de computação
-   Execução de pacotes SSIS (SQL Server Integration Services) na nuvem

Para nosso projeto, utilizaremos o tipo "Self-hosted Integration Runtime", que permite a conexão segura com fontes de dados locais a partir do Azure Data Factory.

### Pré-requisitos

Antes de iniciar a instalação do Integration Runtime, certifique-se de que você possui:

-   Azure Data Factory já criado (conforme detalhado no documento anterior)
-   Um computador local com as seguintes especificações:
    -   Sistema operacional: Windows 8 ou posterior, ou Windows Server 2012 ou posterior
    -   Processador: 2.0 GHz ou superior
    -   Memória: 4 GB ou superior
    -   Disco: 1 GB de espaço livre
    -   Conexão com a internet
-   Permissões de administrador no computador local
-   Portas de saída 443 (HTTPS) liberadas no firewall

### Processo de Instalação e Configuração do Integration Runtime

#### 1. Criando o Integration Runtime no Azure Data Factory

1.  Acesse o Azure Data Factory Studio clicando em "Autor e Monitor" na página do seu Data Factory no portal Azure
2.  No Azure Data Factory Studio, clique em "Gerenciar" no menu à esquerda
3.  Selecione "Integration Runtimes" no painel de navegação
4.  Clique em "+ Novo" para criar um novo Integration Runtime
5.  Na janela "Configuração do Integration Runtime", selecione "Realizar transferência de dados entre redes e nuvem" e clique em "Continuar"
6.  Selecione "Auto-hospedado" como tipo de rede e clique em "Continuar"
7.  Dê o nome `integrationRuntimeB` ao seu Integration Runtime e clique em "Criar"

#### 2. Instalando o Integration Runtime no Computador Local

Após criar o Integration Runtime no Azure Data Factory, você verá uma janela com instruções para instalação no computador local:

1.  Clique em "Clique aqui para iniciar a configuração expressa" para baixar o instalador
2.  Alternativamente, você pode clicar em "Baixar e instalar manualmente" para baixar o instalador e executá-lo posteriormente
3.  Execute o instalador baixado com privilégios de administrador
4.  Na janela de instalação do Integration Runtime, aceite os termos de licença e clique em "Avançar"
5.  Escolha a pasta de instalação ou mantenha o padrão e clique em "Avançar"
6.  Clique em "Instalar" para iniciar a instalação
7.  Após a conclusão da instalação, o assistente de configuração do Integration Runtime será iniciado automaticamente

#### 3. Registrando o Integration Runtime

Após a instalação, você precisará registrar o Integration Runtime com o Azure Data Factory:

1.  No assistente de configuração do Integration Runtime, você verá duas opções para registrar o IR:
    -   **Opção 1: Usando a chave de autenticação**
        -   Copie uma das chaves de autenticação exibidas na janela do Azure Data Factory Studio
        -   Cole a chave no campo correspondente no assistente de configuração
    -   **Opção 2: Usando o registro interativo**
        -   Clique em "Registrar com o Azure Data Factory"
        -   Faça login com sua conta Azure quando solicitado
        -   Selecione o Data Factory e o Integration Runtime corretos
2.  Clique em "Registrar" para concluir o processo de registro
3.  Aguarde a confirmação de que o Integration Runtime foi registrado com sucesso

#### 4. Configurando o Node do Integration Runtime

Após o registro bem-sucedido, você precisará configurar o node do Integration Runtime:

1.  Na janela de configuração, dê um nome ao node: `BS_BASTOS` (este é o nome específico solicitado para o projeto)
2.  Opcionalmente, você pode configurar:
    -   **Proxy HTTP**: Se sua rede utiliza um servidor proxy para acesso à internet
    -   **Criptografia**: Para aumentar a segurança das credenciais transferidas
3.  Clique em "Concluir" para finalizar a configuração

#### 5. Verificando o Status do Integration Runtime

Para verificar se o Integration Runtime está funcionando corretamente:

1.  No Azure Data Factory Studio, vá para "Gerenciar" > "Integration Runtimes"
2.  Verifique se o status do `integrationRuntimeB` está como "Em execução"
3.  Você também pode verificar o status no computador local através do aplicativo "Microsoft Integration Runtime Configuration Manager", que é instalado junto com o Integration Runtime

#### 6. Configurando o Integration Runtime como Serviço

Para garantir que o Integration Runtime continue em execução mesmo após o reinício do computador:

1.  O Integration Runtime é instalado como um serviço do Windows por padrão
2.  Verifique se o serviço está configurado para iniciar automaticamente:
    -   Abra o "Gerenciador de Serviços" do Windows (`services.msc`)
    -   Localize o serviço "Integration Runtime" na lista
    -   Verifique se o "Tipo de Inicialização" está definido como "Automático"
    -   Se não estiver, clique com o botão direito no serviço, selecione "Propriedades" e altere o tipo de inicialização

### Considerações Importantes

-   **Alta Disponibilidade**: Para ambientes de produção, considere instalar o Integration Runtime em múltiplos computadores para garantir alta disponibilidade.
-   **Segurança**: O Integration Runtime estabelece conexões de saída com a nuvem Azure. Não é necessário abrir portas de entrada no firewall.
-   **Desempenho**: O desempenho da transferência de dados depende da capacidade do computador onde o Integration Runtime está instalado e da largura de banda da rede.
-   **Atualizações**: O Integration Runtime é atualizado automaticamente quando novas versões estão disponíveis, a menos que você desabilite essa opção.

### Solução de Problemas Comuns

-   **Falha na Conexão**: Verifique se o computador tem acesso à internet e se as portas necessárias estão abertas no firewall.
-   **Falha no Registro**: Verifique se a chave de autenticação está correta e não expirou.
-   **Desempenho Lento**: Verifique os recursos do computador (CPU, memória, disco) e a largura de banda da rede.

### Próximos Passos

Após a instalação e configuração bem-sucedida do Integration Runtime, o próximo passo será configurar o acesso ao banco de dados local (ItibereBD) e ao Azure Blob Storage, criando os Linked Services necessários no Azure Data Factory.

---

## Configuração do Ambiente Local (BS_BASTOS)

A configuração adequada do ambiente local é fundamental para garantir a comunicação eficiente entre o Azure Data Factory e os recursos on-premises. Neste guia, vamos detalhar o processo de configuração do ambiente local identificado como `BS_BASTOS` para nosso projeto de redundância de arquivos.

### Entendendo o Ambiente Local

O ambiente local, também conhecido como ambiente on-premises, refere-se à infraestrutura de TI que está fisicamente presente nas instalações da organização, em contraste com os recursos baseados em nuvem. No contexto do nosso projeto, o ambiente local inclui:

-   O servidor onde o Integration Runtime está instalado
-   O servidor de banco de dados que hospeda o ItibereBD
-   A rede local que conecta esses componentes

### Pré-requisitos

Antes de iniciar a configuração do ambiente local, certifique-se de que você possui:

-   Integration Runtime instalado e configurado (conforme detalhado no documento anterior)
-   Acesso administrativo ao servidor local
-   Permissões adequadas no banco de dados ItibereBD
-   Conhecimento básico de redes e configuração de servidores Windows

### Processo de Configuração do Ambiente Local

#### 1. Preparação do Servidor BS_BASTOS

O primeiro passo é garantir que o servidor onde o Integration Runtime está instalado (que chamamos de `BS_BASTOS`) esteja devidamente preparado:

1.  **Verificação de Recursos**:
    -   Certifique-se de que o servidor possui recursos adequados (CPU, memória, disco) para executar o Integration Runtime e outras aplicações necessárias
    -   Recomendamos no mínimo 4 núcleos de CPU, 8 GB de RAM e 100 GB de espaço em disco

2.  **Configuração de Rede**:
    -   Verifique se o servidor possui uma conexão estável com a internet
    -   Certifique-se de que as portas necessárias estão abertas no firewall:
        -   Porta 443 (HTTPS) para comunicação com o Azure
        -   Portas necessárias para comunicação com o banco de dados (geralmente 1433 para SQL Server)

3.  **Configuração de Segurança**:
    -   Mantenha o sistema operacional atualizado com as últimas atualizações de segurança
    -   Instale e configure um software antivírus
    -   Implemente políticas de segurança adequadas, como senhas fortes e autenticação de dois fatores

4.  **Configuração de Energia**:
    -   Configure o servidor para nunca entrar em modo de suspensão ou hibernação
    -   Considere a utilização de um UPS (Uninterruptible Power Supply) para proteger contra quedas de energia

#### 2. Configuração do Integration Runtime no BS_BASTOS

Conforme detalhado no documento anterior, o Integration Runtime deve ser configurado no servidor `BS_BASTOS`:

1.  **Verificação do Serviço**:
    -   Abra o "Gerenciador de Serviços" do Windows (`services.msc`)
    -   Localize o serviço "Integration Runtime" na lista
    -   Verifique se o status está como "Em execução" e o tipo de inicialização como "Automático"

2.  **Configuração de Alta Disponibilidade** (opcional, mas recomendado para ambientes de produção):
    -   Para garantir alta disponibilidade, você pode instalar o Integration Runtime em múltiplos servidores
    -   Todos os nodes do Integration Runtime devem ser registrados com o mesmo Integration Runtime no Azure Data Factory
    -   Para adicionar um novo node:
        -   Instale o Integration Runtime em outro servidor
        -   Durante o registro, use a mesma chave de autenticação do primeiro node
        -   Dê um nome diferente ao novo node (por exemplo, `BS_BASTOS_2`)

3.  **Monitoramento do Integration Runtime**:
    -   Abra o "Microsoft Integration Runtime Configuration Manager" no servidor
    -   Verifique o status da conexão, uso de CPU e memória
    -   Configure alertas para ser notificado em caso de problemas

#### 3. Configuração de Acesso ao Banco de Dados

Para que o Integration Runtime possa acessar o banco de dados ItibereBD, é necessário configurar o acesso adequado:

1.  **Verificação de Conectividade**:
    -   Certifique-se de que o servidor `BS_BASTOS` pode se comunicar com o servidor de banco de dados
    -   Teste a conectividade usando ferramentas como o SQL Server Management Studio ou comandos como `ping` e `telnet`

2.  **Configuração de Firewall**:
    -   Se o banco de dados estiver em um servidor diferente do `BS_BASTOS`, configure o firewall do servidor de banco de dados para permitir conexões a partir do IP do `BS_BASTOS` na porta do SQL Server (geralmente 1433)

3.  **Credenciais de Acesso**:
    -   Certifique-se de que você possui as credenciais (nome de usuário e senha) de um usuário do SQL Server com permissões de leitura no banco de dados ItibereBD
    -   É recomendado criar um usuário específico para o Data Factory com permissões mínimas necessárias

### Considerações Importantes

-   **Segurança**: Proteja o servidor `BS_BASTOS` e o servidor de banco de dados com medidas de segurança adequadas.
-   **Desempenho**: O desempenho da rede local pode impactar a velocidade de transferência de dados.
-   **Manutenção**: Mantenha o sistema operacional e o software atualizados em todos os servidores locais.

### Próximos Passos

Após a configuração adequada do ambiente local, o próximo passo é configurar o banco de dados ItibereBD em si, garantindo que ele esteja pronto para ser acessado pelo Azure Data Factory.

---

## Configuração do Banco de Dados Local (ItibereBD)

A configuração adequada do banco de dados local `ItibereBD` é essencial para permitir que o Azure Data Factory leia os dados necessários para o processo de redundância.

### Pré-requisitos

-   SQL Server instalado e em execução (SQL Server Express é uma opção gratuita)
-   Acesso administrativo ao SQL Server
-   SQL Server Management Studio (SSMS) instalado (recomendado)

### Processo de Configuração do Banco de Dados

#### 1. Verificação da Instância do SQL Server

1.  Certifique-se de que o serviço do SQL Server está em execução no servidor de banco de dados.
2.  Verifique se a autenticação do SQL Server está habilitada (modo misto: Autenticação do Windows e Autenticação do SQL Server). Isso pode ser configurado nas propriedades da instância no SSMS.
3.  Anote o nome da instância do SQL Server (por exemplo, `SQLEXPRESS` se for uma instância nomeada).

#### 2. Criação ou Verificação do Banco de Dados ItibereBD

1.  Conecte-se à instância do SQL Server usando o SSMS.
2.  Verifique se o banco de dados `ItibereBD` existe. Se não existir, crie-o:
    ```sql
    CREATE DATABASE ItibereBD;
    ```
3.  Certifique-se de que o banco de dados contém as tabelas necessárias para o projeto (por exemplo, `Clientes`, `Produtos`, `Pedidos`, `ItensPedido`). Se necessário, crie as tabelas e popule-as com dados de exemplo.

#### 3. Criação de um Usuário de Banco de Dados para o Data Factory

É uma boa prática criar um login e um usuário específicos para o Azure Data Factory com permissões mínimas:

1.  Crie um novo Login do SQL Server:
    ```sql
    -- Crie um novo login SQL Server
    CREATE LOGIN adf_user WITH PASSWORD = 'SuaSenhaForteAqui';
    ```
    *Substitua `'SuaSenhaForteAqui'` por uma senha segura.* 

2.  Crie um Usuário no banco de dados `ItibereBD` associado a esse login:
    ```sql
    -- Use o banco de dados ItibereBD
    USE ItibereBD;
    GO

    -- Crie um usuário no banco de dados para o login adf_user
    CREATE USER adf_user FOR LOGIN adf_user;
    GO
    ```

3.  Conceda as permissões necessárias (apenas leitura neste caso):
    ```sql
    -- Conceda permissão SELECT nas tabelas necessárias
    GRANT SELECT ON OBJECT::dbo.Clientes TO adf_user;
    GRANT SELECT ON OBJECT::dbo.Produtos TO adf_user;
    GRANT SELECT ON OBJECT::dbo.Pedidos TO adf_user;
    GRANT SELECT ON OBJECT::dbo.ItensPedido TO adf_user;
    -- Adicione permissões para outras tabelas conforme necessário
    GO
    ```
    *Ajuste os nomes das tabelas conforme a estrutura do seu banco de dados.* 

#### 4. Configuração de Rede e Firewall do SQL Server

1.  Abra o **SQL Server Configuration Manager**.
2.  Navegue até **Configuração de Rede do SQL Server** > **Protocolos para [Nome da Instância]**.
3.  Certifique-se de que o protocolo **TCP/IP** está habilitado.
4.  Clique com o botão direito em **TCP/IP** e selecione **Propriedades**.
5.  Na guia **Endereços IP**, role para baixo até a seção **IPAll**.
6.  Anote ou configure a **Porta Dinâmica TCP** (se estiver em branco, portas dinâmicas estão sendo usadas) ou a **Porta TCP** (geralmente 1433 se portas estáticas estiverem configuradas).
7.  Certifique-se de que o Firewall do Windows no servidor de banco de dados permite conexões de entrada na porta TCP configurada (1433 ou a porta dinâmica) a partir do endereço IP do servidor `BS_BASTOS`.

### Considerações Importantes

-   **Segurança**: Use senhas fortes para os logins do SQL Server e armazene-as de forma segura. Não conceda permissões além das necessárias.
-   **Desempenho**: Certifique-se de que as tabelas que serão lidas possuem índices adequados para otimizar as consultas.
-   **Backup**: Mantenha backups regulares do banco de dados `ItibereBD`.

### Próximos Passos

Com o banco de dados local configurado e acessível, o próximo passo é configurar o destino dos dados na nuvem: o Azure Blob Storage.

---

## Configuração do Azure Blob Storage

O Azure Blob Storage é um serviço de armazenamento de objetos da Microsoft otimizado para armazenar grandes quantidades de dados não estruturados. Neste projeto, ele será utilizado como destino para os arquivos de redundância copiados do ambiente on-premises.

### Pré-requisitos

-   Uma assinatura ativa do Microsoft Azure
-   Permissões para criar contas de armazenamento

### Processo de Configuração do Azure Blob Storage

#### 1. Criação da Conta de Armazenamento

1.  Acesse o Portal Azure ([https://portal.azure.com](https://portal.azure.com)).
2.  Clique em "+ Criar um recurso".
3.  Pesquise por "Conta de armazenamento" e selecione-a.
4.  Clique em "Criar".
5.  Preencha as informações na guia **Básico**:
    -   **Assinatura**: Selecione sua assinatura.
    -   **Grupo de Recursos**: Selecione o grupo de recursos criado anteriormente (por exemplo, `rg-redundancia-arquivos`).
    -   **Nome da conta de armazenamento**: Escolha um nome exclusivo globalmente (por exemplo, `stredundanciabastos`). O nome deve ter entre 3 e 24 caracteres e conter apenas letras minúsculas e números.
    -   **Região**: Escolha a mesma região do seu Data Factory ou uma região apropriada.
    -   **Desempenho**: Selecione `Standard` (recomendado para a maioria dos cenários).
    -   **Redundância**: Selecione `LRS (Armazenamento com redundância local)` para a opção mais econômica. Considere `GRS` ou `RA-GRS` para maior resiliência em ambientes de produção.
6.  Navegue pelas outras guias (**Avançado**, **Rede**, **Proteção de dados**, etc.) e ajuste as configurações conforme necessário. Para este projeto, as configurações padrão geralmente são suficientes.
    -   Na guia **Avançado**, certifique-se de que "Permitir acesso anônimo de blob" esteja **desabilitado** por padrão por motivos de segurança.
    -   Considere habilitar o "Namespace hierárquico" se planeja usar o armazenamento como um Data Lake (Azure Data Lake Storage Gen2), o que oferece otimizações para cargas de trabalho analíticas.
7.  Na guia **Revisão + criar**, revise suas configurações e clique em "Criar".

#### 2. Criação de Contêineres

Após a criação da conta de armazenamento, você precisa criar contêineres para organizar seus blobs (arquivos):

1.  Navegue até a conta de armazenamento recém-criada no Portal Azure.
2.  No menu à esquerda, selecione "Contêineres" na seção "Armazenamento de dados".
3.  Clique em "+ Contêiner".
4.  Digite um nome para o contêiner (por exemplo, `raw`). O nome deve estar em letras minúsculas.
5.  Selecione o nível de acesso público (recomendado: `Privado (sem acesso anônimo)`).
6.  Clique em "Criar".
7.  Repita o processo para criar outros contêineres conforme necessário para as diferentes camadas de dados (por exemplo, `bronze`, `silver`, `gold`). Para este projeto, vamos começar com `raw` e `bronze`.

#### 3. Obtenção das Chaves de Acesso

Para permitir que o Azure Data Factory se conecte à sua conta de armazenamento, você precisará das chaves de acesso:

1.  Na página da sua conta de armazenamento no Portal Azure, selecione "Chaves de acesso" no menu à esquerda, na seção "Segurança + rede".
2.  Você verá duas chaves de acesso (key1 e key2). Clique no ícone de cópia ao lado de uma das chaves (por exemplo, key1) para copiá-la para a área de transferência.
3.  **IMPORTANTE**: Mantenha essas chaves seguras. Elas concedem acesso total à sua conta de armazenamento. Armazene-as de forma segura, preferencialmente usando o Azure Key Vault.
4.  Você também pode copiar a **String de conexão** que contém a chave, pois ela pode ser usada diretamente em algumas configurações.

### Considerações Importantes

-   **Custo**: O custo do Blob Storage é baseado na quantidade de dados armazenados, nas operações realizadas (leitura, escrita) e na transferência de dados para fora da região do Azure.
-   **Níveis de Acesso**: O Blob Storage oferece diferentes níveis de acesso (Hot, Cool, Archive) com custos variados. Escolha o nível apropriado com base na frequência com que você acessará os dados.
-   **Segurança**: Configure o acesso à sua conta de armazenamento usando as melhores práticas, como chaves de acesso, SAS (Shared Access Signatures) ou Azure AD.
-   **Organização**: Use contêineres e uma estrutura de pastas lógica para organizar seus dados de forma eficiente.

### Próximos Passos

Com o Azure Blob Storage configurado, o próximo passo é estabelecer a conexão segura entre o Azure Data Factory e os ambientes on-premises e de nuvem, configurando os Linked Services.

---

## Estabelecendo Conexões Seguras com Ambientes On-Premises

Estabelecer uma conexão segura entre o Azure Data Factory na nuvem e o ambiente local (on-premises) é um passo crítico para a movimentação de dados. Isso é realizado principalmente através do Integration Runtime auto-hospedado (`integrationRuntimeB`) que foi instalado e configurado anteriormente.

### Mecanismo de Conexão

1.  **Comunicação de Saída**: O Integration Runtime auto-hospedado instalado no servidor local (`BS_BASTOS`) inicia uma conexão de **saída** segura (via HTTPS na porta 443) com o serviço do Azure Data Factory na nuvem. Ele não requer a abertura de portas de **entrada** no firewall local, tornando a configuração mais segura.
2.  **Polling**: O Integration Runtime verifica periodicamente (faz polling) o serviço do Azure Data Factory em busca de tarefas pendentes (como executar uma atividade de cópia).
3.  **Execução da Tarefa**: Quando uma tarefa é atribuída (por exemplo, copiar dados do `ItibereBD`), o Integration Runtime executa a tarefa localmente:
    -   Conecta-se ao banco de dados `ItibereBD` usando as credenciais fornecidas no Linked Service.
    -   Lê os dados solicitados.
    -   Transfere os dados de forma segura para o destino na nuvem (Azure Blob Storage) através da conexão HTTPS estabelecida.

### Verificações de Segurança e Conectividade

#### 1. Status do Integration Runtime

-   **No Azure Data Factory Studio**: Vá para "Gerenciar" > "Integration Runtimes". Verifique se o status do `integrationRuntimeB` está "Em execução" e se o node `BS_BASTOS` está conectado.
-   **No Servidor Local (`BS_BASTOS`)**: Abra o "Microsoft Integration Runtime Configuration Manager". Verifique se o status é "Conectado ao serviço de nuvem".

#### 2. Firewall Local (`BS_BASTOS`)

-   Certifique-se de que o firewall no servidor `BS_BASTOS` permite conexões de **saída** na porta 443 (HTTPS) para os endpoints do Azure Data Factory. Geralmente, conexões de saída na porta 443 são permitidas por padrão.
-   Verifique se nenhum software de segurança ou proxy está bloqueando a comunicação do serviço do Integration Runtime.

#### 3. Firewall do Servidor de Banco de Dados

-   Conforme mencionado na configuração do banco de dados, o firewall no servidor que hospeda o `ItibereBD` deve permitir conexões de **entrada** na porta do SQL Server (geralmente 1433) a partir do endereço IP do servidor `BS_BASTOS`.

#### 4. Conectividade de Rede

-   Garanta que o servidor `BS_BASTOS` tenha uma conexão de rede estável e confiável com a internet.
-   Verifique se o servidor `BS_BASTOS` pode resolver os nomes DNS dos serviços do Azure.
-   Teste a conectividade do `BS_BASTOS` com o servidor de banco de dados `ItibereBD`.

### Melhores Práticas de Segurança para a Conexão

-   **Atualizações**: Mantenha o Integration Runtime e o sistema operacional do servidor `BS_BASTOS` atualizados com os patches de segurança mais recentes.
-   **Acesso Mínimo**: Execute o serviço do Integration Runtime com uma conta de serviço com privilégios mínimos, se possível (embora a configuração padrão geralmente seja suficiente).
-   **Monitoramento**: Monitore os logs do Integration Runtime no servidor `BS_BASTOS` e os logs de diagnóstico do Azure Data Factory para atividades suspeitas.
-   **Credenciais**: Use credenciais fortes para o banco de dados `ItibereBD` e armazene-as de forma segura no Azure Data Factory (preferencialmente usando o Azure Key Vault).

### Próximos Passos

Com a conexão segura estabelecida e verificada através do Integration Runtime, o próximo passo é definir formalmente essas conexões dentro do Azure Data Factory criando os Linked Services para o banco de dados SQL local e o Azure Blob Storage.

---

## Configuração de Linked Services

Os Linked Services (Serviços Vinculados) no Azure Data Factory são como strings de conexão que definem as informações necessárias para se conectar a armazenamentos de dados externos e serviços de computação. Eles são essenciais para que o Data Factory possa interagir com o banco de dados local `ItibereBD` e o Azure Blob Storage.

### Linked Service para o Banco de Dados SQL Local (ItibereBD)

Este Linked Service permitirá que o Data Factory se conecte ao seu banco de dados SQL Server local através do Integration Runtime auto-hospedado.

#### Passos para Criação:

1.  No Azure Data Factory Studio, vá para a guia "Gerenciar".
2.  Selecione "Linked services" no menu à esquerda e clique em "+ Novo".
3.  Na janela "Novo linked service", pesquise por "SQL Server" e selecione-o.
4.  Clique em "Continuar".
5.  Preencha os detalhes da configuração:
    -   **Nome**: `LS_SQLServer_ItibereBD` (ou um nome descritivo de sua escolha).
    -   **Descrição** (Opcional): Descreva o propósito deste serviço vinculado.
    -   **Conectar via integration runtime**: Selecione `integrationRuntimeB` (o IR auto-hospedado que você configurou).
    -   **Nome do servidor**: Digite o nome do servidor ou instância onde o `ItibereBD` está hospedado (por exemplo, `NOME_DO_SERVIDOR` ou `NOME_DO_SERVIDOR\SQLEXPRESS`).
    -   **Nome do banco de dados**: Digite `ItibereBD`.
    -   **Tipo de autenticação**: Selecione `Autenticação do SQL`.
    -   **Nome de usuário**: Digite o nome do usuário SQL criado para o Data Factory (por exemplo, `adf_user`).
    -   **Senha**: Selecione a forma de fornecer a senha:
        -   **Azure Key Vault**: **(Recomendado)** Se você configurou o Key Vault, selecione-o e forneça o nome do segredo onde a senha está armazenada.
        -   **Senha**: Digite a senha diretamente (menos seguro, adequado para testes).
    -   **Criptografia**: Mantenha `Default` ou ajuste conforme a configuração do seu SQL Server.
6.  Clique em "Testar conexão" para verificar se o Data Factory consegue se conectar ao banco de dados através do Integration Runtime.
    -   Se o teste falhar, revise as configurações do servidor, firewall, credenciais e o status do Integration Runtime.
7.  Após o teste bem-sucedido, clique em "Criar".

### Linked Service para o Azure Blob Storage

Este Linked Service permitirá que o Data Factory se conecte à sua conta do Azure Blob Storage.

#### Passos para Criação:

1.  No Azure Data Factory Studio, vá para a guia "Gerenciar".
2.  Selecione "Linked services" no menu à esquerda e clique em "+ Novo".
3.  Na janela "Novo linked service", pesquise por "Azure Blob Storage" e selecione-o.
4.  Clique em "Continuar".
5.  Preencha os detalhes da configuração:
    -   **Nome**: `LS_AzureBlobStorage` (ou um nome descritivo).
    -   **Descrição** (Opcional).
    -   **Conectar via integration runtime**: Selecione `AutoResolveIntegrationRuntime` (o IR gerenciado pelo Azure, pois o Blob Storage está na nuvem).
    -   **Método de autenticação**: Selecione `Chave da conta`.
    -   **Seleção da conta de armazenamento do Azure**: Selecione `Da assinatura do Azure`.
    -   **Assinatura do Azure**: Selecione sua assinatura.
    -   **Nome da conta de armazenamento**: Selecione a conta de armazenamento criada anteriormente (por exemplo, `stredundanciabastos`).
6.  Clique em "Testar conexão" para verificar se o Data Factory consegue se conectar à conta de armazenamento.
7.  Após o teste bem-sucedido, clique em "Criar".

### Linked Service para Azure SQL Database (Se Aplicável)

Se o seu projeto envolver também a cópia de dados para um Banco de Dados SQL do Azure (além do Blob Storage), você criaria um Linked Service semelhante ao do Blob Storage, mas selecionando "Banco de Dados SQL do Azure" e fornecendo as credenciais apropriadas.

### Considerações Importantes

-   **Segurança de Credenciais**: Sempre prefira usar o Azure Key Vault para armazenar senhas e chaves de acesso em vez de digitá-las diretamente.
-   **Nomenclatura**: Use nomes claros e consistentes para seus Linked Services.
-   **Teste de Conexão**: Sempre teste a conexão após criar ou modificar um Linked Service.

### Próximos Passos

Com os Linked Services configurados, o Azure Data Factory agora sabe como se conectar às suas fontes e destinos de dados. O próximo passo é criar Datasets, que definem a estrutura dos dados a serem lidos e escritos usando esses Linked Services.

---

## Criação de Datasets para Leitura e Escrita

Os Datasets (Conjuntos de Dados) no Azure Data Factory representam os dados que você deseja usar como entradas ou saídas em suas atividades de pipeline. Eles apontam para os dados físicos em seus armazenamentos (como tabelas SQL ou arquivos Blob) através dos Linked Services.

### Dataset para Leitura do Banco de Dados SQL Local (Tabela Clientes)

Este Dataset representará a tabela `Clientes` no banco de dados `ItibereBD`.

#### Passos para Criação:

1.  No Azure Data Factory Studio, vá para a guia "Autor".
2.  Clique no ícone "+" (Adicionar novo recurso) e selecione "Dataset".
3.  Pesquise por "SQL Server" e selecione-o.
4.  Clique em "Continuar".
5.  Preencha as propriedades:
    -   **Nome**: `DS_SQLServer_Clientes`.
    -   **Linked service**: Selecione `LS_SQLServer_ItibereBD`.
    -   **Nome da tabela**: Clique em "Editar" e selecione a tabela `dbo.Clientes` na lista suspensa (ou digite `dbo.Clientes`).
    -   **Importar esquema**: Selecione "Da conexão/armazenamento". Isso tentará importar as colunas e tipos de dados da tabela.
6.  Clique em "OK".
7.  Após a criação, você pode ir para a guia "Esquema" do Dataset para visualizar as colunas importadas.

*Repita este processo para criar Datasets para outras tabelas que você precisa ler do `ItibereBD` (por exemplo, `DS_SQLServer_Produtos`, `DS_SQLServer_Pedidos`).*

### Dataset para Escrita no Azure Blob Storage (Camada Raw - Clientes)

Este Dataset representará os arquivos TXT onde os dados da tabela `Clientes` serão armazenados na camada `raw` do Blob Storage.

#### Passos para Criação:

1.  No Azure Data Factory Studio, vá para a guia "Autor".
2.  Clique no ícone "+" e selecione "Dataset".
3.  Pesquise por "Azure Blob Storage" e selecione-o.
4.  Selecione o formato "DelimitedText" (para arquivos TXT/CSV) e clique em "Continuar".
5.  Preencha as propriedades:
    -   **Nome**: `DS_Blob_Clientes_Raw`.
    -   **Linked service**: Selecione `LS_AzureBlobStorage`.
    -   **Caminho do arquivo**:
        -   **Contêiner**: `raw`.
        -   **Diretório**: `clientes` (esta pasta será criada se não existir).
        -   **Arquivo**: `clientes.txt` (ou use expressões dinâmicas para nomes de arquivo, por exemplo, `clientes_@{formatDateTime(utcnow(),'yyyyMMdd_HHmmss')}.txt`).
    -   **Primeira linha como cabeçalho**: Marque esta caixa se desejar que a primeira linha do arquivo contenha os nomes das colunas.
    -   **Importar esquema**: Selecione "Nenhum". Definiremos o esquema na atividade de cópia ou manualmente.
6.  Na guia **Conexão**, configure as propriedades do formato:
    -   **Delimitador de coluna**: Selecione o delimitador desejado (por exemplo, `Vírgula (,)`, `Tabulação (\t)`).
    -   **Delimitador de linha**: Mantenha `Default (\r,\n, or \r\n)`.
    -   **Codificação**: Selecione `UTF-8` (recomendado).
    -   **Caractere de escape** e **Caractere de aspas**: Configure se necessário.
7.  Clique em "OK".

*Repita este processo para criar Datasets de escrita para outras tabelas e camadas (por exemplo, `DS_Blob_Produtos_Raw`, `DS_Blob_Clientes_Bronze`).*

### Considerações Importantes

-   **Nomenclatura**: Use nomes claros que indiquem a origem/destino, a entidade e a camada (se aplicável).
-   **Esquema**: Importar o esquema ajuda o Data Factory a entender a estrutura dos dados, o que é útil para mapeamento em atividades de cópia. Para Datasets de escrita, o esquema pode ser inferido da origem ou definido manualmente.
-   **Parametrização**: Para maior reutilização, você pode criar Datasets parametrizados onde o nome da tabela, caminho do arquivo, etc., são passados como parâmetros.
-   **Formatos**: Escolha o formato de arquivo apropriado (DelimitedText, Parquet, JSON, etc.) com base em como os dados serão consumidos posteriormente. Parquet é geralmente recomendado para análise devido à sua eficiência.

### Próximos Passos

Com os Linked Services e Datasets definidos, estamos prontos para criar os Pipelines que usarão esses componentes para orquestrar a movimentação e transformação dos dados.

---

## Criação de Pipelines no Data Factory

Os Pipelines no Azure Data Factory são agrupamentos lógicos de atividades que, juntas, realizam uma tarefa. Para nosso projeto, criaremos pipelines para copiar dados do SQL Server local para o Azure Blob Storage.

### Pipeline para Copiar Dados da Tabela Clientes (On-Premises para Raw)

Este pipeline copiará dados da tabela `Clientes` do `ItibereBD` para um arquivo `clientes.txt` na camada `raw` do Blob Storage.

#### Passos para Criação:

1.  No Azure Data Factory Studio, vá para a guia "Autor".
2.  Clique no ícone "+" e selecione "Pipeline".
3.  No painel de propriedades do pipeline à direita, dê um nome ao pipeline, por exemplo, `PL_Copia_Clientes_OnPrem_Para_Raw`.
4.  No painel "Atividades" à esquerda, expanda a seção "Mover e transformar".
5.  Arraste a atividade "Copiar dados" para a tela de design do pipeline.
6.  Selecione a atividade "Copiar dados" recém-adicionada.
7.  Na guia **Geral** na parte inferior, dê um nome à atividade, por exemplo, `CopiaClientesSQLParaBlob`.
8.  Vá para a guia **Origem**:
    -   **Dataset de origem**: Selecione `DS_SQLServer_Clientes`.
    -   **Usar consulta**: Mantenha `Tabela` (ou selecione `Consulta` para escrever uma query SQL específica).
9.  Vá para a guia **Destino**:
    -   **Dataset de destino**: Selecione `DS_Blob_Clientes_Raw`.
    -   **Comportamento de cópia de arquivo**: Selecione `Nenhum` (padrão), `FlattenHierarchy`, `MergeFiles` ou `PreserveHierarchy` conforme necessário. Para este caso, `Nenhum` é adequado.
10. Vá para a guia **Mapeamento**:
    -   Clique em "Importar esquemas". O Data Factory tentará mapear as colunas da origem (SQL Server) para o destino (arquivo de texto).
    -   Revise os mapeamentos. Eles devem corresponder na maioria dos casos. Ajuste se necessário.
11. Vá para a guia **Configurações**:
    -   Ajuste as configurações de tolerância a falhas, grau de paralelismo de cópia, etc., conforme necessário. As configurações padrão são geralmente um bom ponto de partida.

#### Validação e Execução (Debug):

1.  Clique em "Validar" na barra de ferramentas superior do pipeline para verificar se há erros de configuração.
2.  Clique em "Depurar" para executar o pipeline em modo de depuração. Isso executará o pipeline imediatamente sem a necessidade de publicá-lo.
3.  Monitore a execução na guia "Saída" na parte inferior. Verifique se a atividade foi concluída com sucesso.
4.  Verifique no Azure Blob Storage se o arquivo `raw/clientes/clientes.txt` foi criado com os dados esperados.

### Pipeline para Copiar Múltiplas Tabelas

Você pode criar pipelines separados para cada tabela ou um único pipeline que copie várias tabelas.

#### Opção 1: Pipelines Separados (Mais Simples)

-   Crie pipelines individuais como o `PL_Copia_Clientes_OnPrem_Para_Raw` para cada tabela (`Produtos`, `Pedidos`, etc.).

#### Opção 2: Pipeline Único com Múltiplas Atividades

1.  Crie um novo pipeline, por exemplo, `PL_Copia_TodasTabelas_OnPrem_Para_Raw`.
2.  Adicione múltiplas atividades "Copiar dados", uma para cada tabela (`CopiaClientes`, `CopiaProdutos`, `CopiaPedidos`).
3.  Configure a origem e o destino para cada atividade de cópia usando os Datasets apropriados.
4.  **Execução Paralela (Padrão)**: Por padrão, as atividades sem dependências entre si serão executadas em paralelo.
5.  **Execução Sequencial**: Se você precisar que uma cópia termine antes que outra comece (por exemplo, copiar `Pedidos` após `Clientes`), conecte as atividades com setas de dependência (arraste da atividade precedente para a subsequente). Selecione a seta para configurar a condição (Sucesso, Falha, Conclusão, Ignorado).

### Parametrização de Pipelines (Avançado)

Para tornar os pipelines mais reutilizáveis, você pode usar parâmetros:

1.  **Criar Parâmetros de Pipeline**: Na tela de design do pipeline (sem nenhuma atividade selecionada), vá para a guia "Parâmetros" e clique em "+ Novo" para adicionar parâmetros (por exemplo, `NomeTabelaOrigem`, `NomeArquivoDestino`).
2.  **Usar Parâmetros em Datasets**: Modifique seus Datasets para aceitar parâmetros (por exemplo, o nome da tabela ou o caminho do arquivo).
3.  **Usar Parâmetros em Atividades**: Passe os parâmetros do pipeline para os parâmetros do Dataset nas configurações da atividade de cópia.
4.  Isso permite criar um único pipeline genérico que pode copiar diferentes tabelas apenas alterando os parâmetros de entrada.

### Considerações Importantes

-   **Monitoramento**: Após publicar e executar pipelines (manualmente ou por gatilho), monitore suas execuções na guia "Monitorar" do Data Factory Studio.
-   **Tratamento de Erros**: Configure a tolerância a falhas e o log de linhas ignoradas nas configurações da atividade de cópia para lidar com dados problemáticos.
-   **Desempenho**: Ajuste o "Grau de cópia paralela" e as "Unidades de Integração de Dados (DIU)" nas configurações da atividade de cópia para otimizar o desempenho, considerando os custos associados.

### Próximos Passos

Com os pipelines de cópia criados, os dados brutos estão sendo movidos para o Azure Blob Storage. Os próximos passos envolvem a configuração da conversão desses dados para o formato TXT (se ainda não estiverem) e a organização dos arquivos em diferentes camadas (bronze, silver, etc.), potencialmente usando atividades de transformação ou o Azure Databricks.

---

## Configuração para Conversão de Dados em .TXT

No nosso cenário, já configuramos os Datasets de destino (`DS_Blob_Clientes_Raw`, etc.) para usar o formato `DelimitedText`, o que significa que a atividade "Copiar dados" já está escrevendo os dados diretamente como arquivos de texto (por exemplo, CSV ou TSV, dependendo do delimitador escolhido).

Portanto, uma etapa separada de "conversão" para TXT não é estritamente necessária se o objetivo for ter os dados em formato de texto delimitado na camada `raw`.

No entanto, vamos abordar como garantir e refinar essa configuração.

### Verificando a Configuração do Dataset de Destino

1.  Navegue até os Datasets de destino criados anteriormente (por exemplo, `DS_Blob_Clientes_Raw`).
2.  Abra o Dataset e vá para a guia **Conexão**.
3.  Confirme as seguintes configurações:
    -   **Tipo**: Deve ser `DelimitedText`.
    -   **Delimitador de coluna**: Certifique-se de que está definido como o caractere desejado (vírgula `,`, tabulação `\t`, ponto e vírgula `;`, etc.). A escolha depende de como os dados serão consumidos posteriormente e se os próprios dados contêm esses caracteres.
    -   **Delimitador de linha**: Mantenha o padrão (`Default (\r,\n, or \r\n)`).
    -   **Codificação**: `UTF-8` é geralmente a melhor escolha para compatibilidade.
    -   **Primeira linha como cabeçalho**: Marque se você deseja que os nomes das colunas sejam incluídos no arquivo.
    -   **Compressão**: Considere usar compressão (`GZip`, `Deflate`, `BZip2`) para economizar espaço de armazenamento e custos de transferência, especialmente para arquivos grandes. O Data Factory pode ler e escrever arquivos compactados nativamente.
        -   Se usar compressão, ajuste o nome do arquivo no Dataset para refletir isso (por exemplo, `clientes.txt.gz`).

### Ajustando a Atividade de Cópia (Se Necessário)

Normalmente, nenhuma configuração adicional é necessária na atividade "Copiar dados" para a conversão para TXT, pois o formato é definido pelo Dataset de destino.

No entanto, você pode encontrar cenários onde ajustes são úteis:

-   **Mapeamento de Colunas**: Na guia **Mapeamento** da atividade de cópia, você pode:
    -   Excluir colunas que não são necessárias no arquivo TXT de destino.
    -   Alterar a ordem das colunas.
    -   Converter tipos de dados (embora para TXT, a maioria será tratada como string, a formatação pode ser importante, especialmente para datas e números).
-   **Configurações de Formato no Destino**: Algumas configurações de formato podem ser substituídas ou definidas na guia **Destino** da atividade de cópia, em "Configurações de formato de arquivo", mas geralmente é melhor mantê-las no Dataset para consistência.

### Exemplo: Garantindo Saída como CSV separado por Ponto e Vírgula

1.  Abra o Dataset `DS_Blob_Clientes_Raw`.
2.  Vá para a guia **Conexão**.
3.  Defina **Delimitador de coluna** como `Ponto e vírgula (;)`.
4.  Certifique-se de que **Primeira linha como cabeçalho** está marcado.
5.  Salve o Dataset.
6.  Execute novamente o pipeline `PL_Copia_Clientes_OnPrem_Para_Raw`.
7.  Verifique o arquivo `raw/clientes/clientes.txt` no Blob Storage. Ele agora deve ser um arquivo CSV com colunas separadas por ponto e vírgula.

### Considerações Importantes

-   **Consistência**: Mantenha as configurações de formato consistentes para arquivos semelhantes.
-   **Consumo**: Pense em como os arquivos TXT serão consumidos. Diferentes ferramentas podem ter requisitos específicos de delimitador, codificação ou aspas.
-   **Dados Binários**: Se você precisar copiar dados binários (como imagens armazenadas no banco de dados), o formato `DelimitedText` não é adequado. Você precisaria usar um Dataset do tipo `Binary` e potencialmente armazenar esses dados em arquivos separados.

### Próximos Passos

Com os dados sendo copiados e armazenados no formato TXT desejado na camada `raw`, o próximo passo lógico é organizar esses arquivos e potencialmente aplicar transformações iniciais para movê-los para outras camadas, como a camada `bronze`.

---

## Organização dos Arquivos em Camadas

A organização dos dados em camadas (também conhecida como arquitetura medalhão: Bronze, Silver, Gold) é uma prática recomendada em data lakes e plataformas de dados modernas. Ela ajuda a estruturar o processo de ingestão, transformação e consumo de dados.

-   **Camada Raw (Bruta)**: Contém os dados exatamente como foram ingeridos da fonte original, sem transformações. Serve como um registro histórico e ponto de partida para reprocessamento. Os dados copiados do `ItibereBD` para `raw/` no Blob Storage pertencem a esta camada.
-   **Camada Bronze (Validada/Limpada)**: Contém dados da camada Raw após passarem por um processo inicial de limpeza, validação de tipos de dados e talvez alguma padronização básica. O esquema pode ser ligeiramente ajustado.
-   **Camada Silver (Enriquecida/Conformada)**: Contém dados da camada Bronze após serem transformados, enriquecidos, combinados e conformados em modelos de dados mais úteis para análise (por exemplo, tabelas de fatos e dimensões).
-   **Camada Gold (Agregada/Curada)**: Contém dados prontos para consumo por aplicações de negócios, relatórios e dashboards. Geralmente são agregações ou visões específicas de dados da camada Silver.

Neste projeto, focaremos na movimentação para as camadas `Raw` e `Bronze` usando o Data Factory, e opcionalmente usando Databricks para transformações mais complexas (levando a `Silver`/`Gold`).

### Movendo Dados da Camada Raw para Bronze

Podemos criar um novo pipeline no Data Factory para ler os arquivos TXT da camada `raw`, aplicar transformações básicas e escrever na camada `bronze`.

#### Passos para Criação do Pipeline Raw para Bronze:

1.  **Criar Datasets para a Camada Bronze**: Crie novos Datasets `DelimitedText` apontando para o contêiner `bronze` no Blob Storage (por exemplo, `DS_Blob_Clientes_Bronze` apontando para `bronze/clientes/clientes.txt`).
2.  **Criar Novo Pipeline**: Crie um pipeline chamado, por exemplo, `PL_Processa_Raw_Para_Bronze`.
3.  **Adicionar Atividade de Cópia**: Arraste uma atividade "Copiar dados" para o pipeline.
4.  **Configurar Origem**: Selecione o Dataset da camada `raw` (por exemplo, `DS_Blob_Clientes_Raw`).
5.  **Configurar Destino**: Selecione o Dataset correspondente da camada `bronze` (por exemplo, `DS_Blob_Clientes_Bronze`).
6.  **Aplicar Transformações Básicas (Opcional no Mapeamento)**:
    -   Na guia **Mapeamento**, importe os esquemas.
    -   Você pode fazer pequenas transformações aqui:
        -   **Converter Tipos de Dados**: Se a camada `raw` foi lida com todas as colunas como string, você pode tentar converter para tipos mais apropriados (Integer, Datetime) no mapeamento para a camada `bronze`. Use a coluna "Tipo" no mapeamento.
        -   **Renomear Colunas**: Altere os nomes das colunas de destino se necessário.
        -   **Limpeza Simples**: Use "Conteúdo dinâmico" para aplicar funções simples (por exemplo, `trim()` para remover espaços em branco) em colunas específicas.
            ```json
            trim(byName('NomeColuna'))
            ```
7.  **Validação e Execução**: Valide e depure o pipeline.

#### Usando Data Flows (Fluxos de Dados) para Transformações Mais Visuais:

Para transformações mais complexas entre Raw e Bronze sem usar Databricks, você pode usar os **Data Flows** do Azure Data Factory:

1.  **Criar Data Flow**: Na guia "Autor", clique em "+" > "Data Flow".
2.  **Adicionar Origem**: Adicione uma origem apontando para o Dataset da camada `raw`.
3.  **Adicionar Transformações**: Use as transformações visuais disponíveis (Filtro, Coluna Derivada, Seleção, Junção, Agregação, etc.) para limpar, validar e padronizar os dados.
4.  **Adicionar Destino (Sink)**: Adicione um destino apontando para o Dataset da camada `bronze`.
5.  **Criar Pipeline com Atividade Data Flow**: Crie um novo pipeline e adicione uma atividade "Data Flow", selecionando o Data Flow que você criou.

*Nota: Data Flows são mais poderosos para transformações dentro do ADF, mas consomem mais recursos (e custos) durante a execução do que a atividade de Cópia simples.*

### Estrutura de Pastas

É crucial manter uma estrutura de pastas consistente dentro de cada camada:

```
/raw
  /clientes
    clientes_20230101.txt.gz
    clientes_20230102.txt.gz
  /produtos
    produtos_20230101.txt.gz
    produtos_20230102.txt.gz
/bronze
  /clientes
    clientes.parquet  (ou clientes.txt.gz)
  /produtos
    produtos.parquet
/silver
  /dim_clientes
    data.parquet
  /fact_pedidos
    data.parquet
```

-   Considere usar particionamento por data na camada `raw` (por exemplo, `/raw/clientes/ano=2023/mes=01/dia=01/`) para otimizar consultas e gerenciamento.
-   Na camada `bronze` e `silver`, o formato Parquet é frequentemente preferido devido à sua eficiência para análise, mas TXT/CSV ainda pode ser usado.

### Considerações Importantes

-   **Idempotência**: Projete seus pipelines para serem idempotentes, ou seja, executá-los várias vezes com a mesma entrada deve produzir o mesmo resultado. Isso geralmente envolve limpar o destino antes de escrever ou usar estratégias de `upsert`.
-   **Processamento Incremental**: Em vez de processar todos os arquivos `raw` toda vez, implemente lógica para processar apenas arquivos novos ou alterados.
-   **Monitoramento**: Monitore os pipelines que movem dados entre camadas para garantir que estão funcionando corretamente.

### Próximos Passos

Com os dados organizados nas camadas `raw` e `bronze`, estamos prontos para configurar o Azure Databricks, que será usado para realizar transformações mais avançadas e criar as camadas `silver` e `gold`.

---

## Configuração do Azure Databricks

O Azure Databricks é uma plataforma de análise de dados otimizada para a nuvem Microsoft Azure, baseada no Apache Spark. Ele fornece um ambiente colaborativo com notebooks, clusters gerenciados e integração com outros serviços Azure, tornando-o ideal para transformações de dados complexas, engenharia de dados e machine learning.

### Pré-requisitos

-   Uma assinatura ativa do Microsoft Azure (Nota: Databricks pode não estar disponível em todas as ofertas, como algumas contas gratuitas ou de estudante).
-   Permissões para criar workspaces do Azure Databricks.

### Processo de Configuração do Azure Databricks

#### 1. Criação do Workspace do Azure Databricks

1.  Acesse o Portal Azure ([https://portal.azure.com](https://portal.azure.com)).
2.  Clique em "+ Criar um recurso".
3.  Pesquise por "Azure Databricks" e selecione-o.
4.  Clique em "Criar".
5.  Preencha as informações na guia **Básico**:
    -   **Assinatura**: Selecione sua assinatura.
    -   **Grupo de Recursos**: Selecione o grupo de recursos criado anteriormente (por exemplo, `rg-redundancia-arquivos`).
    -   **Nome do workspace**: Escolha um nome exclusivo para seu workspace (por exemplo, `dbw-dio-bastos`).
    -   **Região**: Escolha a mesma região do seu Data Factory e Blob Storage para melhor desempenho e menores custos de transferência de dados.
    -   **Tipo de preço**: Selecione `Premium` para recursos adicionais como controle de acesso baseado em função para notebooks e jobs, ou `Standard` para funcionalidades básicas. Comece com `Standard` se não tiver certeza.
6.  Configure as opções de **Rede** (por exemplo, implantar em uma VNet existente para maior segurança) ou mantenha o padrão.
7.  Configure as opções de **Criptografia** se necessário.
8.  Adicione **Tags** se desejar.
9.  Na guia **Revisão + criar**, revise suas configurações e clique em "Criar".

#### 2. Acessando o Workspace do Databricks

1.  Após a conclusão da implantação (pode levar alguns minutos), navegue até o recurso do Azure Databricks no Portal Azure.
2.  Clique em "Iniciar Workspace". Isso abrirá a interface web do Azure Databricks em uma nova aba.

#### 3. Criação de um Cluster de Computação

Para executar notebooks e jobs, você precisa de um cluster de computação Spark:

1.  Na interface do Databricks, clique em "Compute" (Computação) no menu à esquerda.
2.  Clique em "Create Cluster" (Criar Cluster).
3.  Configure o cluster:
    -   **Cluster Name**: Dê um nome ao seu cluster (por exemplo, `cluster-geral`).
    -   **Cluster Mode**: Selecione `Standard` ou `High Concurrency` (dependendo do seu caso de uso e tipo de preço).
    -   **Databricks Runtime Version**: Escolha uma versão recente do runtime (geralmente uma versão LTS - Long Term Support é uma boa escolha).
    -   **Node Type (Worker e Driver)**: Selecione os tipos de VM apropriados. Comece com tipos menores (como `Standard_DS3_v2`) para economizar custos e aumente se necessário.
    -   **Autoscaling**: Habilite o dimensionamento automático e defina o número mínimo e máximo de workers para que o cluster possa escalar com a carga de trabalho.
    -   **Terminate after inactivity**: Defina um tempo de inatividade (por exemplo, 60 minutos) após o qual o cluster será encerrado automaticamente para economizar custos.
4.  Clique em "Create Cluster". O provisionamento do cluster levará alguns minutos.

#### 4. Configurando o Acesso ao Azure Blob Storage

Para que os notebooks do Databricks possam ler e escrever no seu Azure Blob Storage, você precisa configurar o acesso. Existem várias maneiras, mas a montagem (mounting) é uma abordagem comum:

1.  **Criar um Notebook**: No Databricks, vá para "Workspace", clique com o botão direito em uma pasta e selecione "Create" > "Notebook". Dê um nome (por exemplo, `ConfiguracaoStorage`) e escolha `Python` como linguagem.
2.  **Código para Montar o Blob Storage**: Cole e execute o seguinte código em uma célula do notebook, substituindo os placeholders pelos seus valores:

    ```python
    # Substitua pelos seus valores
    storage_account_name = "stredundanciabastos"
    container_name = "raw"  # Ou o contêiner que deseja montar
    mount_point = f"/mnt/{container_name}" # Ponto de montagem no Databricks File System (DBFS)
    # Obtenha a chave de acesso da sua conta de armazenamento no Portal Azure
    storage_account_key = "SUA_CHAVE_DE_ACESSO_AQUI"

    # Configuração da URL de origem
    source_url = f"wasbs://{container_name}@{storage_account_name}.blob.core.windows.net"
    # Chave de configuração para autenticação
    conf_key = f"fs.azure.account.key.{storage_account_name}.blob.core.windows.net"

    # Verificar se o ponto de montagem já existe antes de montar
    if not any(mount.mountPoint == mount_point for mount in dbutils.fs.mounts()):
      dbutils.fs.mount(
        source = source_url,
        mount_point = mount_point,
        extra_configs = {conf_key: storage_account_key}
      )
      print(f"Container '{container_name}' montado em '{mount_point}'")
    else:
      print(f"Ponto de montagem '{mount_point}' já existe.")

    # Listar arquivos no ponto de montagem para teste
    # display(dbutils.fs.ls(mount_point))
    ```

3.  **Executar a Célula**: Anexe o notebook ao cluster criado e execute a célula (Shift+Enter).
4.  **Repetir para Outros Contêineres**: Repita o processo de montagem para outros contêineres (`bronze`, `silver`, etc.) alterando `container_name` e `mount_point`.

*Nota: Usar chaves de acesso diretamente no código não é a prática mais segura para produção. Considere usar Databricks Secrets com Azure Key Vault ou Service Principals para maior segurança.*

#### 5. Configurando o Linked Service do Databricks no Data Factory

Para permitir que o Data Factory execute notebooks no Databricks, crie um Linked Service:

1.  No Azure Data Factory Studio, vá para "Gerenciar" > "Linked services" > "+ Novo".
2.  Pesquise por "Azure Databricks" e selecione-o.
3.  Configure o Linked Service:
    -   **Nome**: `LS_AzureDatabricks`.
    -   **Conectar via integration runtime**: `AutoResolveIntegrationRuntime`.
    -   **Seleção da conta do Azure**: `Da assinatura do Azure`.
    -   **Assinatura do Azure**: Selecione sua assinatura.
    -   **Workspace do Databricks**: Selecione o workspace criado (`dbw-dio-bastos`).
    -   **Selecionar cluster**: Escolha `Cluster existente`.
    -   **Cluster do Databricks**: Selecione o cluster criado (`cluster-geral`).
    -   **Tipo de autenticação**: `Token de Acesso`.
    -   **Token de Acesso**: Gere um token de acesso pessoal no Databricks (User Settings > Access Tokens > Generate New Token) e cole-o aqui. **(Recomendado usar Azure Key Vault para armazenar o token)**.
4.  Teste a conexão e clique em "Criar".

### Considerações Importantes

-   **Custo**: O Databricks é cobrado com base no tempo de execução do cluster (DBUs) e no tipo de VM utilizada. Lembre-se de configurar o encerramento automático por inatividade.
-   **Segurança**: Configure o acesso ao workspace, clusters e notebooks usando as funcionalidades de controle de acesso do Databricks. Proteja o acesso ao Blob Storage.
-   **Gerenciamento de Cluster**: Escolha os tipos de VM e as configurações de dimensionamento automático adequadas para sua carga de trabalho.

### Próximos Passos

Com o Azure Databricks configurado e conectado ao Blob Storage e ao Data Factory, estamos prontos para criar notebooks Spark para realizar as transformações de dados necessárias para as camadas Silver e Gold.

---

## Utilização do Databricks para Transformação de Dados

O Azure Databricks oferece um ambiente poderoso baseado em Apache Spark para realizar transformações de dados complexas. Usaremos notebooks Databricks para processar os dados das camadas `raw` ou `bronze` e gerar as camadas `silver` e `gold`.

### Pré-requisitos

-   Workspace do Azure Databricks configurado.
-   Cluster Databricks criado e em execução.
-   Acesso ao Azure Blob Storage configurado (por exemplo, via montagem DBFS).
-   Dados disponíveis nas camadas `raw` e/ou `bronze`.
-   Conhecimento básico de Python e PySpark (a API Python para Spark).

### Exemplo de Notebook para Transformação (Bronze para Silver)

Vamos criar um notebook para ler dados da tabela `Clientes` da camada `bronze`, aplicar transformações e salvar na camada `silver` em formato Parquet.

1.  **Criar Notebook**: Crie um novo notebook Python no Databricks (por exemplo, `TransformacaoClientesBronzeParaSilver`).
2.  **Anexar ao Cluster**: Anexe o notebook ao cluster criado anteriormente.

#### Célula 1: Importar Bibliotecas e Definir Caminhos

```python
from pyspark.sql.functions import col, trim, when, to_date, datediff, current_date, year, month, dayofmonth, regexp_replace
from pyspark.sql.types import IntegerType, DateType, StringType

# Definir caminhos usando os pontos de montagem configurados
bronze_path = "/mnt/bronze/clientes/clientes.txt" # Assumindo que bronze está em TXT
silver_path = "/mnt/silver/dim_clientes" # Salvaremos como Parquet na camada Silver
```

#### Célula 2: Ler Dados da Camada Bronze

```python
# Ler o arquivo de texto delimitado da camada bronze
# Inferir esquema pode ser útil, mas para produção é melhor definir explicitamente
df_bronze = spark.read.format("csv") \
    .option("header", "true") \
    .option("delimiter", ",") \
    .option("inferSchema", "true") \
    .load(bronze_path)

print("Schema lido da camada Bronze:")
df_bronze.printSchema()
display(df_bronze.limit(5))
```

#### Célula 3: Aplicar Limpeza e Transformações

```python
# Selecionar e renomear colunas (se necessário)
df_selected = df_bronze.select(
    col("ClienteID").alias("IDCliente"),
    col("Nome"),
    col("Email"),
    col("Telefone"),
    col("DataCadastro")
)

# Limpeza de dados
df_cleaned = df_selected \
    .withColumn("Nome", trim(col("Nome"))) \
    .withColumn("Email", trim(col("Email"))) \
    .withColumn("Telefone", regexp_replace(col("Telefone"), "[^0-9+]", "")) # Remover não numéricos exceto +

# Conversão de tipos e validação
df_typed = df_cleaned \
    .withColumn("IDCliente", col("IDCliente").cast(IntegerType())) \
    .withColumn("DataCadastro", to_date(col("DataCadastro"), "yyyy-MM-dd")) # Ajustar formato se necessário

# Filtrar registros inválidos (exemplo: IDCliente nulo ou DataCadastro nula)
df_validated = df_typed.filter(col("IDCliente").isNotNull() & col("DataCadastro").isNotNull())

print("Schema após limpeza e tipagem:")
df_validated.printSchema()
display(df_validated.limit(5))
```

#### Célula 4: Enriquecimento e Criação de Novas Colunas

```python
# Adicionar colunas derivadas
df_enriched = df_validated \
    .withColumn("DiasDesdeCadastro", datediff(current_date(), col("DataCadastro"))) \
    .withColumn("AnoCadastro", year(col("DataCadastro"))) \
    .withColumn("MesCadastro", month(col("DataCadastro")))

# Adicionar uma coluna de categoria de cliente (exemplo)
df_final = df_enriched \
    .withColumn("CategoriaCliente",
                when(col("DiasDesdeCadastro") > 365, "Ouro")
                .when(col("DiasDesdeCadastro") > 180, "Prata")
                .otherwise("Bronze")
               )

print("Schema final da Dimensão Cliente:")
df_final.printSchema()
display(df_final.limit(10))
```

#### Célula 5: Escrever Dados na Camada Silver (Formato Parquet)

```python
# Escrever o DataFrame final na camada Silver em formato Parquet
# Usar overwrite para substituir os dados existentes (cuidado em produção!)
# Particionar por ano e mês pode otimizar consultas futuras
df_final.write \
    .format("parquet") \
    .mode("overwrite") \
    .partitionBy("AnoCadastro", "MesCadastro") \
    .save(silver_path)

print(f"Dimensão Cliente salva com sucesso em: {silver_path}")
```

### Integração com Azure Data Factory

Para automatizar a execução deste notebook, use o Azure Data Factory:

1.  **Criar Pipeline no ADF**: Crie um pipeline (por exemplo, `PL_Processa_Bronze_Para_Silver`).
2.  **Adicionar Atividade Databricks Notebook**: Arraste a atividade "Notebook" da seção "Databricks" para o pipeline.
3.  **Configurar Atividade**:
    -   **Linked Service do Databricks**: Selecione `LS_AzureDatabricks`.
    -   **Caminho do Notebook**: Navegue e selecione o caminho do seu notebook no workspace do Databricks (por exemplo, `/Users/seu_email@dominio.com/TransformacaoClientesBronzeParaSilver`).
    -   **Parâmetros Base (Opcional)**: Se o seu notebook aceitar parâmetros (usando `dbutils.widgets.get()`), você pode passá-los aqui.
4.  **Configurar Dependências**: Certifique-se de que este pipeline seja executado após a conclusão dos pipelines que populam a camada `bronze`.
5.  **Publicar e Executar/Agendar**: Publique o pipeline e execute-o manualmente ou configure um gatilho para execução agendada.

### Considerações Importantes

-   **Formato Parquet**: É altamente recomendado para as camadas Silver e Gold devido à sua eficiência de armazenamento colunar e otimização para consultas analíticas.
-   **Idempotência**: Garanta que seus notebooks sejam idempotentes. O modo `overwrite` ajuda, mas considere estratégias mais sofisticadas (como `merge`) para atualizações incrementais.
-   **Particionamento**: Particionar os dados na escrita (como feito com `partitionBy("AnoCadastro", "MesCadastro")`) pode melhorar significativamente o desempenho das consultas que filtram por essas colunas.
-   **Gerenciamento de Erros**: Adicione tratamento de erros (try/except) e logging ao seu código PySpark.
-   **Otimização**: Use técnicas de otimização do Spark, como `cache()` para DataFrames reutilizados e ajuste das configurações do cluster.

### Próximos Passos

Após transformar os dados e criar a camada Silver, você pode:

-   Criar notebooks adicionais para gerar tabelas de fatos e outras dimensões.
-   Desenvolver notebooks para criar agregações e visões de negócios na camada Gold.
-   Configurar a validação dos pipelines e dos dados gerados.

---

## Validação dos Pipelines

A validação dos pipelines é uma etapa crucial para garantir que o fluxo de dados funcione corretamente, que os dados sejam transformados conforme esperado e que a solução seja robusta.

### Tipos de Validação

1.  **Validação Sintática (ADF)**: Verifica a configuração do pipeline no Data Factory (já abordada).
2.  **Validação de Conectividade**: Testa se os Linked Services conseguem se conectar às fontes e destinos.
3.  **Validação Funcional**: Executa o pipeline com dados de teste para verificar se as atividades produzem o resultado esperado.
4.  **Validação de Dados**: Compara os dados de origem e destino para garantir a integridade, precisão e completude.
5.  **Validação de Desempenho**: Mede o tempo de execução e o consumo de recursos.
6.  **Validação de Recuperação**: Testa o comportamento do pipeline em caso de falhas.

### Processo de Validação

#### 1. Preparação

-   **Ambiente de Teste**: Idealmente, tenha um ambiente separado (Dev/Test) com cópias dos recursos (ADF, Blob, Databricks) ou use pastas/contêineres específicos para teste no ambiente de desenvolvimento.
-   **Dados de Teste**: Crie um conjunto de dados pequeno, mas representativo, no `ItibereBD` que cubra diferentes cenários (registros válidos, inválidos, nulos, casos extremos).
-   **Critérios de Aceitação**: Defina o que significa um teste bem-sucedido (por exemplo, contagem de registros corresponde, valores específicos estão corretos, formato do arquivo está correto).

#### 2. Validação no Azure Data Factory

-   **Testar Conexão (Linked Services)**: Para cada Linked Service, clique em "Testar conexão".
-   **Validar Pipeline**: Use o botão "Validar" no editor de pipeline.
-   **Execução em Depuração**: Use o botão "Depurar" para executar o pipeline com os dados de teste. Monitore a execução na guia "Saída".

#### 3. Validação de Dados (Exemplos)

Após a execução do pipeline (por exemplo, Raw para Bronze), valide os dados gerados. Isso pode ser feito manualmente, com scripts ou usando notebooks Databricks.

**Exemplo usando Notebook Databricks para validar Bronze:**

```python
# Ler dados da origem (Raw) e destino (Bronze)
raw_path = "/mnt/raw/clientes/clientes.txt"
bronze_path = "/mnt/bronze/clientes/clientes.txt"

df_raw = spark.read.format("csv").option("header", "true").load(raw_path)
df_bronze = spark.read.format("csv").option("header", "true").load(bronze_path)

# 1. Comparar Contagem de Registros
raw_count = df_raw.count()
bronze_count = df_bronze.count()
print(f"Contagem Raw: {raw_count}, Contagem Bronze: {bronze_count}")
assert raw_count == bronze_count, "Contagem de registros não corresponde!"

# 2. Verificar Esquema (Tipos de Dados)
print("Schema Bronze:")
df_bronze.printSchema()
# Verificar manualmente se os tipos estão corretos (ex: IDCliente é integer?)

# 3. Verificar Valores Nulos em Colunas Críticas
null_counts = df_bronze.select([count(when(col(c).isNull(), c)).alias(c) for c in ["IDCliente", "Email"]])
display(null_counts)
# Verificar se há nulos inesperados

# 4. Validar Transformações Específicas (Ex: Limpeza de Telefone)
sample_phones = df_bronze.select("Telefone").distinct().limit(10).collect()
print("Amostra de Telefones (Bronze):")
for row in sample_phones:
    print(row["Telefone"])
    # Verificar manualmente se o formato está correto (apenas números/+) 

# 5. Comparar Amostra de Dados (mais complexo)
# Pode envolver junção dos dataframes raw e bronze por ID e comparação de colunas
```

#### 4. Validação de Desempenho

-   Monitore o tempo de execução de cada atividade e do pipeline como um todo na guia "Monitorar" do ADF.
-   Execute com volumes maiores de dados (em ambiente de teste) para verificar a escalabilidade.
-   Analise os logs do Spark UI no Databricks para identificar gargalos nas transformações.

#### 5. Validação de Recuperação

-   Simule falhas (por exemplo, parando o cluster Databricks durante a execução, introduzindo dados inválidos).
-   Verifique se o pipeline falha corretamente ou se a tolerância a falhas funciona como esperado.
-   Teste a capacidade de reexecutar o pipeline após a correção da falha.

### Considerações Importantes

-   **Automação**: Para projetos maiores, considere automatizar os testes de validação de dados usando frameworks como `pytest` com PySpark ou ferramentas específicas de teste de dados.
-   **Cobertura**: Garanta que seus dados de teste cubram uma variedade de cenários.
-   **Documentação**: Documente os resultados dos testes e quaisquer problemas encontrados.

### Próximos Passos

Após validar completamente os pipelines e os dados gerados, o próximo passo é publicar a solução e configurar a execução regular (agendada ou por evento).

---

## Publicação e Execução dos Pipelines

Após o desenvolvimento, teste e validação dos pipelines, o próximo passo é publicá-los para torná-los operacionais e configurar sua execução (manual ou automatizada).

### Publicação no Azure Data Factory

Publicar no Data Factory significa salvar todas as alterações feitas no Studio (pipelines, datasets, linked services, etc.) no serviço do Data Factory, tornando-as ativas.

#### Passos para Publicação:

1.  No Azure Data Factory Studio, você verá uma indicação de quantas alterações pendentes existem.
2.  Clique no botão "Publicar tudo" na barra de ferramentas superior.
3.  Uma janela lateral mostrará todas as alterações que serão publicadas. Revise-as cuidadosamente.
4.  Clique em "Publicar".
5.  Aguarde a conclusão do processo de publicação. Você pode acompanhar o progresso nas notificações.

#### Integração com Git (Recomendado):

Se você configurou a integração com Git (Azure DevOps ou GitHub):

1.  Faça commit das suas alterações no seu branch de desenvolvimento.
2.  Crie um Pull Request (PR) para mesclar as alterações no branch principal (por exemplo, `main` ou `master`).
3.  Após a aprovação e mesclagem do PR, o processo de publicação geralmente envolve publicar a partir do branch principal. O Data Factory pode ser configurado para publicar automaticamente após a mesclagem (CI/CD) ou você pode publicar manualmente a partir do branch de colaboração.

### Execução dos Pipelines

Existem várias maneiras de executar um pipeline publicado:

#### 1. Execução Manual (Ad hoc)

Útil para testes finais ou execuções pontuais.

1.  Navegue até o pipeline que deseja executar na guia "Autor".
2.  Clique no botão "Adicionar gatilho" na barra de ferramentas do pipeline.
3.  Selecione "Acionar agora".
4.  Se o pipeline tiver parâmetros, você será solicitado a fornecer os valores.
5.  Clique em "OK".
6.  Monitore a execução na guia "Monitorar".

#### 2. Execução Agendada (Gatilho de Agendamento)

Para executar pipelines em intervalos regulares (por exemplo, diariamente, semanalmente).

1.  Navegue até o pipeline desejado na guia "Autor".
2.  Clique em "Adicionar gatilho" > "Novo/Editar".
3.  Clique em "+ Novo" em "Escolher gatilho".
4.  Selecione o **Tipo** como `Agendamento`.
5.  Configure a recorrência:
    -   **Fuso horário**: Selecione seu fuso horário.
    -   **Recorrência**: Escolha Minuto, Hora, Dia, Semana ou Mês.
    -   **Intervalo**: Defina o intervalo (por exemplo, a cada `1` Dia).
    -   **Horas/Minutos específicos**: Defina a hora exata da execução (por exemplo, `03:00`).
    -   **Data de início**: Defina quando o gatilho deve começar.
    -   **Data de término** (Opcional).
6.  Certifique-se de que a opção "Ativado" esteja marcada.
7.  Clique em "OK" e depois em "Publicar tudo" para salvar o gatilho.

#### 3. Execução por Evento (Gatilho de Evento de Armazenamento)

Para executar um pipeline automaticamente quando um evento específico ocorre no armazenamento (por exemplo, um novo arquivo é criado no Blob Storage).

1.  Vá para a guia "Gerenciar" > "Gatilhos" > "+ Novo".
2.  Selecione o **Tipo** como `Evento de armazenamento`.
3.  Configure o gatilho:
    -   **Nome**: Dê um nome ao gatilho.
    -   **Tipo**: `Eventos de Armazenamento`.
    -   **Assinatura do Azure**: Selecione sua assinatura.
    -   **Nome da conta de armazenamento**: Selecione sua conta de armazenamento.
    -   **Nome do contêiner**: Selecione o contêiner a ser monitorado.
    -   **Caminho do blob começa com** (Opcional): Filtre por prefixo de pasta.
    -   **Caminho do blob termina com** (Opcional): Filtre por extensão de arquivo (por exemplo, `.txt`).
    -   **Evento**: Selecione `Blob criado` (ou `Blob excluído`).
    -   **Ignorar blobs vazios**: Marque se desejar.
    -   **Iniciar gatilho na criação**: Marque para ativar.
4.  Clique em "Continuar".
5.  Na próxima tela, você verá os pipelines associados a este gatilho (se houver). Você pode associar o gatilho a um pipeline aqui ou posteriormente.
6.  Clique em "Continuar" e depois em "Criar".
7.  Publique as alterações.
8.  **Associação ao Pipeline**: Abra o pipeline que deve ser acionado, clique em "Adicionar gatilho" > "Novo/Editar" e selecione o gatilho de evento criado.

#### 4. Execução por Janela em Cascata (Gatilho de Janela em Cascata)

Usado para processar dados em fatias de tempo fixas e sequenciais, garantindo que uma janela só seja processada após a anterior ter sido concluída com sucesso.

### Monitoramento das Execuções

1.  Vá para a guia "Monitorar" no Data Factory Studio.
2.  **Execuções de Pipeline**: Veja o histórico de todas as execuções, seus status (Êxito, Falha, Em andamento), duração e gatilho.
3.  **Execuções de Gatilho**: Veja o histórico de execuções iniciadas por gatilhos.
4.  **Detalhes da Execução**: Clique em uma execução de pipeline específica para ver os detalhes de cada atividade, suas entradas, saídas e logs de erro (se houver).
5.  **Alertas**: Configure alertas (na seção Monitorar > Alertas e Métricas) para ser notificado sobre falhas ou execuções longas.

### Considerações Importantes

-   **Dependências**: Certifique-se de que os gatilhos e as dependências entre pipelines estejam configurados corretamente para garantir a ordem de execução desejada (por exemplo, Raw->Bronze antes de Bronze->Silver).
-   **Simultaneidade**: Configure limites de simultaneidade nos gatilhos ou pipelines se precisar controlar quantos pipelines são executados ao mesmo tempo.
-   **Custos**: Lembre-se de que cada execução de pipeline consome recursos e gera custos.

### Próximos Passos

Com os pipelines publicados e em execução, é importante analisar seu desempenho para identificar oportunidades de otimização e garantir a eficiência da solução.

---

## Análise de Performance das Execuções

A análise de performance das execuções dos pipelines é fundamental para garantir que a solução de redundância de arquivos seja eficiente, escalável e econômica. O monitoramento contínuo permite identificar gargalos, otimizar o uso de recursos e prever custos.

### Ferramentas de Monitoramento

O Azure Data Factory e o Azure Databricks oferecem várias ferramentas para monitorar e analisar o desempenho:

1.  **Azure Data Factory Studio - Guia Monitorar**:
    -   **Execuções de Pipeline**: Visão geral das execuções, status, duração, gatilho.
    -   **Detalhes da Execução de Pipeline**: Detalhes por atividade, duração de cada atividade, unidades de integração de dados (DIU) usadas (para cópia), logs de erro.
    -   **Execuções de Gatilho**: Histórico de execuções iniciadas por gatilhos.
    -   **Alertas e Métricas**: Configuração de alertas e visualização de métricas de desempenho (execuções bem-sucedidas/falhadas, latência, etc.).

2.  **Azure Databricks UI**:
    -   **Spark UI**: Acessível a partir da página do cluster, fornece informações detalhadas sobre jobs, stages, tasks, uso de memória, shuffle, etc. Essencial para diagnosticar gargalos em transformações Spark.
    -   **Logs do Driver e Executor**: Logs detalhados disponíveis na interface do cluster.
    -   **Histórico de Jobs**: Registros de execuções de notebooks e jobs.

3.  **Azure Monitor**:
    -   Pode ser configurado para coletar logs de diagnóstico e métricas do Data Factory e Databricks.
    -   Permite criar consultas Kusto (KQL) para análises avançadas.
    -   Possibilita a criação de dashboards e workbooks para visualização.

### Métricas Chave para Análise

-   **Duração Total do Pipeline**: Tempo total desde o início até a conclusão.
-   **Duração por Atividade**: Tempo gasto em cada atividade (cópia, notebook Databricks, etc.). Ajuda a identificar as etapas mais lentas.
-   **Taxa de Transferência (Throughput)**: Para atividades de cópia, a quantidade de dados transferida por unidade de tempo (MB/s ou GB/s).
-   **Uso de DIU/vCore**: Quantidade de recursos de computação consumidos pela atividade de cópia (DIU) ou Data Flow (vCore-hora).
-   **Tempo de Execução do Cluster Databricks**: Tempo total que o cluster esteve ativo para executar um notebook/job.
-   **Uso de Recursos do Cluster (CPU/Memória)**: Monitorar no Spark UI ou Azure Monitor para verificar se o cluster está sub ou superdimensionado.
-   **Taxa de Falhas**: Percentual de execuções de pipeline ou atividade que falharam.
-   **Latência de Gatilho**: Tempo entre o evento (para gatilhos de evento) e o início da execução do pipeline.

### Processo de Análise e Otimização

1.  **Estabelecer Linha de Base**: Execute os pipelines algumas vezes em condições normais para estabelecer métricas de desempenho de linha de base.
2.  **Identificar Gargalos**: Analise as durações das atividades. A atividade que consome a maior parte do tempo é o principal gargalo.
    -   **Cópia Lenta On-Prem->Nuvem**: Pode ser devido a limitações de rede local, recursos insuficientes no servidor do Integration Runtime ou configurações de DIU/paralelismo baixas na atividade de cópia.
    -   **Cópia Lenta Nuvem->Nuvem**: Pode ser devido a configurações de DIU/paralelismo, tipo de VM, ou limitações de throttling no serviço de origem/destino.
    -   **Notebook Databricks Lento**: Requer análise detalhada no Spark UI. Causas comuns incluem código ineficiente, shuffles excessivos, particionamento inadequado, cluster subdimensionado.
3.  **Otimizar Atividades de Cópia**: 
    -   Ajuste o número de **DIUs** e o **Grau de cópia paralela** nas configurações da atividade.
    -   Verifique se o **Integration Runtime** (auto-hospedado) tem recursos suficientes (CPU, memória, rede).
    -   Considere usar **particionamento** na origem se suportado.
4.  **Otimizar Notebooks Databricks**: 
    -   Analise o **Spark UI** para identificar stages lentos ou com muito shuffle.
    -   Otimize o código PySpark (use `filter` antes de `join`, evite UDFs lentas, use `broadcast` para tabelas pequenas em joins).
    -   Ajuste o **particionamento** dos dados (`repartition`, `coalesce`).
    -   Use `cache()` ou `persist()` para DataFrames intermediários usados várias vezes.
    -   Ajuste o **tamanho e tipo do cluster** (mais workers, workers com mais memória/CPU, otimizados para computação/memória).
    -   Use **Delta Lake** e otimizações como `OPTIMIZE` e `ZORDER`.
5.  **Otimizar Fluxo Geral**: 
    -   Execute atividades independentes em **paralelo**.
    -   Implemente **processamento incremental** para evitar reprocessar todos os dados.
    -   Avalie se a **arquitetura em camadas** está otimizada (por exemplo, usar Parquet em vez de CSV nas camadas intermediárias).
6.  **Iterar e Medir**: Aplique uma otimização de cada vez e meça o impacto no desempenho em relação à linha de base.

### Considerações de Custo

-   Otimizar o desempenho muitas vezes leva à redução de custos (menos tempo de execução = menos custo de computação).
-   Monitore o custo associado a cada execução (especialmente Databricks e Data Flows).
-   Escolha os tipos de VM e níveis de serviço apropriados para equilibrar desempenho e custo.
-   Lembre-se de desligar clusters Databricks automaticamente após inatividade.

### Próximos Passos

Após analisar e otimizar o desempenho, o passo final é garantir que a solução esteja configurada e operando de acordo com as melhores práticas de segurança e configuração.

---

## Boas Práticas de Configuração e Segurança

A implementação de boas práticas de configuração e segurança é essencial para garantir a confiabilidade, a proteção e a eficiência do sistema de redundância de arquivos. Neste guia, vamos detalhar as melhores práticas para configuração e segurança no Azure Data Factory, Databricks e demais componentes do projeto.

### Importância das Boas Práticas de Configuração e Segurança

A adoção de boas práticas oferece diversos benefícios:

-   **Proteção de Dados**: Garante que dados sensíveis estejam protegidos contra acesso não autorizado
-   **Conformidade**: Ajuda a atender requisitos regulatórios e políticas corporativas
-   **Confiabilidade**: Aumenta a estabilidade e a resiliência do sistema
-   **Manutenibilidade**: Facilita a manutenção e a evolução do sistema ao longo do tempo
-   **Eficiência Operacional**: Reduz o risco de incidentes e o tempo de resolução de problemas

### Boas Práticas de Configuração

#### 1. Organização e Nomenclatura

Uma estrutura organizacional clara e uma nomenclatura consistente são fundamentais:

1.  **Padrões de Nomenclatura**:
    -   Adote um padrão de nomenclatura consistente para todos os recursos
    -   Inclua informações como ambiente, tipo de recurso e propósito
    -   Exemplos:
        -   Pipelines: `PL_[Função]_[Origem]_para_[Destino]`
        -   Datasets: `DS_[Tipo]_[Entidade]_[Camada]`
        -   Linked Services: `LS_[Tipo]_[Recurso]`

2.  **Organização de Recursos**:
    -   Agrupe recursos relacionados em grupos de recursos distintos por ambiente (Dev, Test, Prod).
    -   No Azure Data Factory, use pastas para organizar pipelines, datasets e outros recursos.
    -   No Databricks, organize notebooks em pastas por função ou domínio.

3.  **Documentação Integrada**:
    -   Adicione descrições detalhadas a todos os recursos (Linked Services, Datasets, Pipelines).
    -   Use células markdown nos notebooks Databricks para documentar o código e a lógica.
    -   Mantenha um repositório central de documentação (como este README ou um Wiki) atualizado.

#### 2. Configuração de Ambientes

A separação adequada de ambientes é crucial para desenvolvimento seguro:

1.  **Separação de Ambientes**: Mantenha ambientes separados para Desenvolvimento, Teste e Produção.
2.  **Controle de Versão (Git)**: Integre o Data Factory e o Databricks com Git (Azure DevOps ou GitHub) para controle de versão, colaboração e CI/CD.
3.  **Implantação (CI/CD)**: Implemente pipelines de CI/CD para automatizar a implantação entre ambientes, garantindo consistência e reduzindo erros manuais.

#### 3. Parametrização e Configuração

A parametrização adequada aumenta a flexibilidade e a reutilização:

1.  **Uso de Parâmetros**: Parametrize valores que mudam entre ambientes ou execuções (nomes de servidores, caminhos de arquivos, nomes de contêineres).
2.  **Azure Key Vault**: Armazene todas as credenciais, strings de conexão e segredos no Azure Key Vault. Configure os serviços (ADF, Databricks) para acessá-lo usando identidades gerenciadas.
3.  **Variáveis Globais (ADF)**: Use variáveis globais para constantes usadas em múltiplos pipelines.

#### 4. Logging e Monitoramento

Configurações adequadas de logging e monitoramento são essenciais:

1.  **Logs de Diagnóstico**: Habilite logs de diagnóstico para o Data Factory, Databricks e Blob Storage. Envie-os para um workspace do Log Analytics (Azure Monitor).
2.  **Azure Monitor**: Use o Log Analytics para consultar logs, criar alertas para condições críticas (falhas, execuções longas) e construir dashboards de monitoramento.
3.  **Logging Detalhado**: Implemente logging explícito dentro dos seus notebooks Databricks para rastrear o progresso e diagnosticar problemas.

### Boas Práticas de Segurança

#### 1. Gerenciamento de Identidade e Acesso

O controle adequado de acesso é a base da segurança:

1.  **Princípio do Menor Privilégio**: Conceda apenas as permissões mínimas necessárias para cada usuário ou serviço.
2.  **Azure Active Directory (Azure AD)**: Use o Azure AD para autenticação e autorização centralizadas.
3.  **Identidades Gerenciadas**: Use identidades gerenciadas atribuídas pelo sistema ou pelo usuário para autenticação segura entre serviços Azure (por exemplo, ADF acessando Key Vault ou Blob Storage) sem armazenar credenciais.
4.  **RBAC (Role-Based Access Control)**: Use funções RBAC do Azure para controlar o acesso aos recursos (Data Factory, Blob Storage, Databricks).
5.  **Controle de Acesso no Databricks**: Use o controle de acesso a tabelas, clusters, jobs e notebooks dentro do Databricks (requer plano Premium).

#### 2. Proteção de Dados

A proteção adequada dos dados é crucial:

1.  **Criptografia**: Habilite a criptografia em repouso (padrão no Blob Storage) e em trânsito (TLS/HTTPS).
2.  **Azure Key Vault**: Armazene todas as chaves, segredos e strings de conexão no Key Vault.
3.  **Mascaramento de Dados**: Considere o mascaramento dinâmico de dados (no SQL ou via transformações) para dados sensíveis.
4.  **Controle de Acesso a Dados**: Use ACLs no Data Lake Storage Gen2 ou RBAC/SAS no Blob Storage para controlar o acesso aos dados.

#### 3. Segurança de Rede

A segurança de rede protege contra ameaças externas:

1.  **Redes Virtuais (VNets)**: Implante recursos como Databricks e o Integration Runtime (se possível) dentro de VNets.
2.  **Endpoints Privados**: Use endpoints privados para permitir que o Data Factory e o Databricks acessem o Blob Storage e outros serviços Azure através da rede privada da Microsoft, sem expô-los à internet pública.
3.  **NSGs (Network Security Groups)**: Use NSGs para filtrar o tráfego de rede de e para os recursos dentro de uma VNet.
4.  **Firewall**: Configure regras de firewall no Blob Storage e no SQL Server local para permitir acesso apenas de fontes confiáveis (como os IPs do Integration Runtime ou endpoints privados).

#### 4. Segurança do Integration Runtime Auto-hospedado

1.  **Servidor Dedicado e Seguro**: Instale o IR em um servidor dedicado e protegido.
2.  **Atualizações**: Mantenha o SO e o software do IR atualizados.
3.  **Acesso Mínimo**: Limite o acesso ao servidor do IR.

### Implementação de Segurança no Projeto

-   **ADF**: Use Key Vault para credenciais, identidade gerenciada para acesso a outros serviços, endpoints privados.
-   **Blob Storage**: Use Key Vault ou identidade gerenciada para acesso, configure regras de firewall e/ou endpoints privados, habilite versionamento e exclusão reversível.
-   **Databricks**: Use Key Vault para segredos (via Databricks Secret Scope), implante em VNet, configure controle de acesso a clusters/notebooks, use identidade gerenciada para acesso ao Blob (pass-through ou service principal).
-   **IR Auto-hospedado**: Instale em servidor seguro, configure firewall local, use credenciais fortes para fontes locais armazenadas no Key Vault.

### Manutenção Contínua

-   Revise permissões e configurações de segurança regularmente.
-   Monitore logs de auditoria e segurança.
-   Mantenha todos os componentes atualizados.
-   Realize treinamentos de segurança para a equipe.

Ao seguir estas boas práticas, você construirá uma solução de redundância de arquivos não apenas funcional, mas também segura, confiável e fácil de manter.
