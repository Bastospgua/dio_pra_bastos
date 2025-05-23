# dio_pra_bastos

## Processo de Redundância de Arquivos na Azure com Databricks

### Introdução

Este guia abrangente detalha o processo de desenvolvimento de um projeto prático e completo de redundância de arquivos utilizando recursos do Microsoft Azure. O foco principal é a integração entre ambientes on- 
premises e a nuvem, utilizando Azure Data Factory e Azure Databricks para orquestração, movimentação e transformação de dados.

O objetivo é fornecer um passo a passo detalhado, desde a criação e configuração dos recursos necessários até a validação, execução e aplicação de boas práticas de segurança, permitindo a implementação de uma solução 
robusta e eficiente para redundância de dados.
Este documento compila todas as etapas e informações geradas ao longo do projeto, organizadas de forma lógica e sequencial para facilitar a consulta e implementação.
Criação do Azure Data Factory

O Azure Data Factory (ADF) é um serviço de integração de dados baseado em nuvem que permite criar fluxos de trabalho orientados a dados para orquestrar e automatizar a movimentação e transformação de dados. Neste 
guia, vamos detalhar o processo de criação do Azure Data Factory chamado "adf-dio-bastos" para nosso projeto de redundância de arquivos.
Pré-requisitos

Antes de iniciar a criação do Azure Data Factory, certifique-se de que você possui:
Uma assinatura ativa do Microsoft Azure
Permissões adequadas para criar recursos na sua assinatura (Contributor ou Owner)
Conhecimento básico do portal Azure
Processo de Criação do Azure Data Factory

1. Acessando o Portal Azure
O primeiro passo é acessar o Portal Azure através do endereço https://portal.azure.com e fazer login com suas credenciais. Após o login, você será direcionado para a página inicial do portal.

2. Criando um Novo Recurso
Para criar um novo Azure Data Factory:
Clique no botão "+ Criar um recurso" no canto superior esquerdo da tela
Na barra de pesquisa, digite "Data Factory" e selecione "Data Factory" nos resultados
Clique no botão "Criar" para iniciar o processo de criação

3. Configurando o Data Factory

Na página "Criar Data Factory", você precisará preencher as seguintes informações:

Guia Básico:

Assinatura: Selecione a assinatura Azure que deseja utilizar

Grupo de Recursos: Selecione um grupo existente ou crie um novo (recomendamos criar um grupo específico para este projeto, como "rg-redundancia-arquivos")

Região: Escolha a região mais próxima da sua localização geográfica ou dos seus usuários para minimizar a latência

Nome: Digite "adf-dio-bastos" (este é o nome específico solicitado para o projeto)

Versão: Selecione "V2"

Guia Rede: Mantenha as configurações padrão para conectividade pública
Guia Segurança:
Habilite o Microsoft Defender for Data Factory (opcional, mas recomendado para ambientes de produção)
Configure a identidade gerenciada (recomendamos usar a identidade gerenciada atribuída pelo sistema)
Guia Tags:
Adicione tags relevantes para organização e governança (opcional, mas recomendado)
Exemplo: Projeto=RedundanciaArquivos, Ambiente=Desenvolvimento
Guia Revisão + criar:
Revise todas as configurações e clique em "Criar"

5. Aguardando a Implantação
A implantação do Azure Data Factory pode levar alguns minutos. Você pode acompanhar o progresso na página de notificações do portal Azure. Quando a implantação for concluída, você receberá uma notificação.
6. Acessando o Data Factory
Após a implantação bem-sucedida:
Clique em "Ir para o recurso" na notificação ou navegue até o recurso através da barra de pesquisa
Na página inicial do seu Data Factory, você verá um resumo do recurso e várias opções de configuração
Para começar a criar pipelines e outros componentes, clique no botão "Abrir Azure Data Factory Studio" ou "Autor e Monitor"
7. Explorando o Azure Data Factory Studio
O Azure Data Factory Studio é uma interface web onde você pode:
Criar pipelines, datasets, linked services e outros componentes
Monitorar a execução dos pipelines
Configurar triggers para automação
Gerenciar recursos do Data Factory
Familiarize-se com a interface, pois será utilizada extensivamente nas próximas etapas do projeto.
Considerações Importantes
Custo: O Azure Data Factory é cobrado com base no uso. Certifique-se de entender o modelo de preços antes de implementar em produção.
Segurança: Configure corretamente as permissões de acesso ao Data Factory para garantir que apenas usuários autorizados possam modificar os recursos.
Monitoramento: Habilite o Azure Monitor para acompanhar o desempenho e a integridade do seu Data Factory.
Backup: Considere usar o controle de versão do Git para manter um histórico das alterações no seu Data Factory.
Próximos Passos
Após a criação bem-sucedida do Azure Data Factory, o próximo passo será configurar o Integration Runtime para estabelecer a conexão entre o ambiente de nuvem e o ambiente local (on-premises). Isso permitirá a movimentação de dados entre o banco de dados local (ItibereBD) e o Azure Blob Storage.
Instalação e Configuração do Integration Runtime
O Integration Runtime (IR) é um componente crucial do Azure Data Factory que serve como ponte entre o ambiente de nuvem e o ambiente local (on-premises). Neste guia, vamos detalhar o processo de instalação e configuração do Integration Runtime chamado "integrationRuntimeB" para nosso projeto de redundância de arquivos.
O que é o Integration Runtime?
O Integration Runtime é a infraestrutura de computação utilizada pelo Azure Data Factory para fornecer as seguintes capacidades:
Movimentação de dados entre ambientes de nuvem e locais
Despacho de atividades para diferentes ambientes de computação
Execução de pacotes SSIS (SQL Server Integration Services) na nuvem
Para nosso projeto, utilizaremos o tipo "Self-hosted Integration Runtime", que permite a conexão segura com fontes de dados locais a partir do Azure Data Factory.
Pré-requisitos
Antes de iniciar a instalação do Integration Runtime, certifique-se de que você possui:
Azure Data Factory já criado (conforme detalhado no documento anterior)
Um computador local com as seguintes especificações:
Sistema operacional: Windows 8 ou posterior, ou Windows Server 2012 ou posterior
Processador: 2.0 GHz ou superior
Memória: 4 GB ou superior
Disco: 1 GB de espaço livre
Conexão com a internet
Permissões de administrador no computador local
Portas de saída 443 (HTTPS) liberadas no firewall
Processo de Instalação e Configuração do Integration Runtime
1. Criando o Integration Runtime no Azure Data Factory
Acesse o Azure Data Factory Studio clicando em "Autor e Monitor" na página do seu Data Factory no portal Azure
No Azure Data Factory Studio, clique em "Gerenciar" no menu à esquerda
Selecione "Integration Runtimes" no painel de navegação
Clique em "+ Novo" para criar um novo Integration Runtime
Na janela "Configuração do Integration Runtime", selecione "Realizar transferência de dados entre redes e nuvem" e clique em "Continuar"
Selecione "Auto-hospedado" como tipo de rede e clique em "Continuar"
Dê o nome "integrationRuntimeB" ao seu Integration Runtime e clique em "Criar"
2. Instalando o Integration Runtime no Computador Local
Após criar o Integration Runtime no Azure Data Factory, você verá uma janela com instruções para instalação no computador local:
Clique em "Clique aqui para iniciar a configuração expressa" para baixar o instalador
Alternativamente, você pode clicar em "Baixar e instalar manualmente" para baixar o instalador e executá-lo posteriormente
Execute o instalador baixado com privilégios de administrador
Na janela de instalação do Integration Runtime, aceite os termos de licença e clique em "Avançar"
Escolha a pasta de instalação ou mantenha o padrão e clique em "Avançar"
Clique em "Instalar" para iniciar a instalação
Após a conclusão da instalação, o assistente de configuração do Integration Runtime será iniciado automaticamente
3. Registrando o Integration Runtime
Após a instalação, você precisará registrar o Integration Runtime com o Azure Data Factory:
No assistente de configuração do Integration Runtime, você verá duas opções para registrar o IR:
Opção 1: Usando a chave de autenticação
Copie uma das chaves de autenticação exibidas na janela do Azure Data Factory Studio
Cole a chave no campo correspondente no assistente de configuração
Opção 2: Usando o registro interativo
Clique em "Registrar com o Azure Data Factory"
Faça login com sua conta Azure quando solicitado
Selecione o Data Factory e o Integration Runtime corretos
Clique em "Registrar" para concluir o processo de registro
Aguarde a confirmação de que o Integration Runtime foi registrado com sucesso
4. Configurando o Node do Integration Runtime
Após o registro bem-sucedido, você precisará configurar o node do Integration Runtime:
Na janela de configuração, dê um nome ao node: "BS_BASTOS" (este é o nome específico solicitado para o projeto)
Opcionalmente, você pode configurar:
Proxy HTTP: Se sua rede utiliza um servidor proxy para acesso à internet
Criptografia: Para aumentar a segurança das credenciais transferidas
Clique em "Concluir" para finalizar a configuração
5. Verificando o Status do Integration Runtime
Para verificar se o Integration Runtime está funcionando corretamente:
No Azure Data Factory Studio, vá para "Gerenciar" > "Integration Runtimes"
Verifique se o status do "integrationRuntimeB" está como "Em execução"
Você também pode verificar o status no computador local através do aplicativo "Microsoft Integration Runtime Configuration Manager", que é instalado junto com o Integration Runtime
6. Configurando o Integration Runtime como Serviço
Para garantir que o Integration Runtime continue em execução mesmo após o reinício do computador:
O Integration Runtime é instalado como um serviço do Windows por padrão
Verifique se o serviço está configurado para iniciar automaticamente:
Abra o "Gerenciador de Serviços" do Windows (services.msc)
Localize o serviço "Integration Runtime" na lista
Verifique se o "Tipo de Inicialização" está definido como "Automático"
Se não estiver, clique com o botão direito no serviço, selecione "Propriedades" e altere o tipo de inicialização
Considerações Importantes
Alta Disponibilidade: Para ambientes de produção, considere instalar o Integration Runtime em múltiplos computadores para garantir alta disponibilidade.
Segurança: O Integration Runtime estabelece conexões de saída com a nuvem Azure. Não é necessário abrir portas de entrada no firewall.
Desempenho: O desempenho da transferência de dados depende da capacidade do computador onde o Integration Runtime está instalado e da largura de banda da rede.
Atualizações: O Integration Runtime é atualizado automaticamente quando novas versões estão disponíveis, a menos que você desabilite essa opção.
Solução de Problemas Comuns
Falha na Conexão: Verifique se o computador tem acesso à internet e se as portas necessárias estão abertas no firewall.
Falha no Registro: Verifique se a chave de autenticação está correta e não expirou.
Desempenho Lento: Verifique os recursos do computador (CPU, memória, disco) e a largura de banda da rede.
Próximos Passos
Após a instalação e configuração bem-sucedida do Integration Runtime, o próximo passo será configurar o acesso ao banco de dados local (ItibereBD) e ao Azure Blob Storage, criando os Linked Services necessários no Azure Data Factory.
Configuração do Ambiente Local (BS_BASTOS)
A configuração adequada do ambiente local é fundamental para garantir a comunicação eficiente entre o Azure Data Factory e os recursos on-premises. Neste guia, vamos detalhar o processo de configuração do ambiente local identificado como "BS_BASTOS" para nosso projeto de redundância de arquivos.
Entendendo o Ambiente Local
O ambiente local, também conhecido como ambiente on-premises, refere-se à infraestrutura de TI que está fisicamente presente nas instalações da organização, em contraste com os recursos baseados em nuvem. No contexto do nosso projeto, o ambiente local inclui:
O servidor onde o Integration Runtime está instalado
O servidor de banco de dados que hospeda o ItibereBD
A rede local que conecta esses componentes
Pré-requisitos
Antes de iniciar a configuração do ambiente local, certifique-se de que você possui:
Integration Runtime instalado e configurado (conforme detalhado no documento anterior)
Acesso administrativo ao servidor local
Permissões adequadas no banco de dados ItibereBD
Conhecimento básico de redes e configuração de servidores Windows
Processo de Configuração do Ambiente Local
1. Preparação do Servidor BS_BASTOS
O primeiro passo é garantir que o servidor onde o Integration Runtime está instalado (que chamamos de BS_BASTOS) esteja devidamente preparado:
Verificação de Recursos:
Certifique-se de que o servidor possui recursos adequados (CPU, memória, disco) para executar o Integration Runtime e outras aplicações necessárias
Recomendamos no mínimo 4 núcleos de CPU, 8 GB de RAM e 100 GB de espaço em disco
Configuração de Rede:
Verifique se o servidor possui uma conexão estável com a internet
Certifique-se de que as portas necessárias estão abertas no firewall:
Porta 443 (HTTPS) para comunicação com o Azure
Portas necessárias para comunicação com o banco de dados (geralmente 1433 para SQL Server)
Configuração de Segurança:
Mantenha o sistema operacional atualizado com as últimas atualizações de segurança
Instale e configure um software antivírus
Implemente políticas de segurança adequadas, como senhas fortes e autenticação de dois fatores
Configuração de Energia:
Configure o servidor para nunca entrar em modo de suspensão ou hibernação
Considere a utilização de um UPS (Uninterruptible Power Supply) para proteger contra quedas de energia
2. Configuração do Integration Runtime no BS_BASTOS
Conforme detalhado no documento anterior, o Integration Runtime deve ser configurado no servidor BS_BASTOS:
Verificação do Serviço:
Abra o "Gerenciador de Serviços" do Windows (services.msc)
Localize o serviço "Integration Runtime" na lista
Verifique se o status está como "Em execução" e o tipo de inicialização como "Automático"
Configuração de Alta Disponibilidade (opcional, mas recomendado para ambientes de produção):
Para garantir alta disponibilidade, você pode instalar o Integration Runtime em múltiplos servidores
Todos os nodes do Integration Runtime devem ser registrados com o mesmo Integration Runtime no Azure Data Factory
Para adicionar um novo node:
Instale o Integration Runtime em outro servidor
Durante o registro, use a mesma chave de autenticação do primeiro node
Dê um nome diferente ao novo node (por exemplo, "BS_BASTOS_2")
Monitoramento do Integration Runtime:
Abra o "Microsoft Integration Runtime Configuration Manager" no servidor
Verifique o status da conexão, uso de CPU e memória
Configure alertas para ser notificado em caso de problemas
3. Configuração de Acesso ao Banco de Dados
Para que o Integration Runtime possa acessar o banco de dados ItibereBD, é necessário configurar o acesso adequado:
Verificação de Conectividade:
Certifique-se de que o servidor BS_BASTOS pode se comunicar com o servidor de banco de dados
Teste a conectividade usando ferramentas como o SQL Server Management Studio ou comandos como ping e telnet
Configuração de Firewall:
Se o banco de dados estiver em outro servidor, certifique-se de que as portas necessárias estão abertas no firewall
Para SQL Server, a porta padrão é 1433, mas pode ser diferente dependendo da configuração
Configuração de Autenticação:
Crie um usuário específico no banco de dados para ser usado pelo Integration Runtime
Este usuário deve ter permissões mínimas necessárias para as operações que serão realizadas (geralmente SELECT para leitura de dados)
Recomendamos usar autenticação SQL Server em vez de autenticação Windows para simplificar a configuração
Teste de Conexão:
Teste a conexão com o banco de dados a partir do servidor BS_BASTOS usando as credenciais que serão utilizadas pelo Integration Runtime
Verifique se todas as tabelas necessárias estão acessíveis
4. Configuração de Armazenamento Local
Para o processamento eficiente de dados, pode ser necessário configurar um armazenamento local temporário:
Alocação de Espaço em Disco:
Aloque um espaço em disco adequado para armazenamento temporário de dados durante o processamento
Recomendamos pelo menos 50 GB de espaço livre em um volume separado
Configuração de Permissões:
Certifique-se de que o usuário sob o qual o serviço Integration Runtime está sendo executado tem permissões de leitura e escrita no diretório de armazenamento temporário
Monitoramento de Espaço:
Configure o monitoramento do espaço em disco para evitar problemas de falta de espaço durante a execução dos pipelines
5. Configuração de Logs e Monitoramento
Para facilitar a solução de problemas e o monitoramento do ambiente:
Configuração de Logs:
Configure o Integration Runtime para gerar logs detalhados
Defina uma política de retenção de logs para evitar o consumo excessivo de espaço em disco
Monitoramento de Desempenho:
Configure o monitoramento de desempenho do servidor (CPU, memória, disco, rede)
Utilize ferramentas como o Monitor de Desempenho do Windows ou soluções de monitoramento de terceiros
Alertas:
Configure alertas para ser notificado em caso de problemas com o servidor ou com o Integration Runtime
Defina thresholds adequados para métricas como uso de CPU, memória e espaço em disco
Considerações Importantes
Manutenção Regular: Realize manutenção regular no servidor BS_BASTOS, incluindo atualizações de segurança e verificação de logs
Backup: Implemente uma estratégia de backup para os componentes críticos do ambiente local
Documentação: Mantenha documentação atualizada sobre a configuração do ambiente local, incluindo diagramas de rede, credenciais (armazenadas de forma segura) e procedimentos de recuperação
Teste de Recuperação: Realize testes periódicos de recuperação para garantir que você pode restaurar o ambiente em caso de falha
Próximos Passos
Após a configuração adequada do ambiente local BS_BASTOS, o próximo passo será detalhar a configuração do banco de dados local ItibereBD, incluindo a estrutura de tabelas e os dados que serão transferidos para o Azure Blob Storage.
Configuração do Banco de Dados Local (ItibereBD)
A configuração adequada do banco de dados local é essencial para garantir que os dados possam ser acessados e transferidos corretamente pelo Azure Data Factory. Neste guia, vamos detalhar o processo de configuração do banco de dados local chamado "ItibereBD" para nosso projeto de redundância de arquivos.
Entendendo o Banco de Dados ItibereBD
O ItibereBD é o banco de dados local (on-premises) que contém os dados que serão transferidos para o Azure Blob Storage como parte do nosso processo de redundância. Este banco de dados pode estar hospedado no mesmo servidor onde o Integration Runtime está instalado (BS_BASTOS) ou em um servidor separado na mesma rede.
Pré-requisitos
Antes de iniciar a configuração do banco de dados, certifique-se de que você possui:
Um servidor de banco de dados SQL Server instalado e configurado
Permissões administrativas no SQL Server
Conhecimento básico de administração de SQL Server e T-SQL
Integration Runtime configurado e funcionando corretamente
Processo de Configuração do Banco de Dados ItibereBD
1. Criação do Banco de Dados
Se o banco de dados ItibereBD ainda não existir, você precisará criá-lo:
Acesso ao SQL Server Management Studio (SSMS):
Abra o SQL Server Management Studio
Conecte-se ao servidor SQL Server usando credenciais administrativas
Criação do Banco de Dados:
Clique com o botão direito em "Bancos de Dados" no Object Explorer
Selecione "Novo Banco de Dados..."
Digite "ItibereBD" como nome do banco de dados
Configure as opções de arquivo e crescimento de acordo com suas necessidades
Clique em "OK" para criar o banco de dados
Configuração de Recuperação:
Defina o modelo de recuperação apropriado (Simples, Completo ou Bulk-Logged)
Para um ambiente de produção, recomendamos o modelo Completo para permitir a recuperação pontual
2. Criação de Tabelas e Estruturas de Dados
Após criar o banco de dados, você precisará criar as tabelas e outras estruturas de dados necessárias:
Identificação dos Dados a Serem Transferidos:
Identifique quais dados precisam ser transferidos para o Azure Blob Storage
Determine a estrutura das tabelas que conterão esses dados
Criação de Tabelas:
Crie as tabelas necessárias usando comandos T-SQL ou o designer de tabelas do SSMS
Exemplo de comando T-SQL para criar uma tabela simples:
sql
USE ItibereBD;

CREATE TABLE Clientes (
    ClienteID INT PRIMARY KEY IDENTITY(1,1),
    Nome NVARCHAR(100) NOT NULL,
    Email NVARCHAR(100),
    Telefone NVARCHAR(20),
    DataCadastro DATETIME DEFAULT GETDATE()
);

CREATE TABLE Produtos (
    ProdutoID INT PRIMARY KEY IDENTITY(1,1),
    Nome NVARCHAR(100) NOT NULL,
    Descricao NVARCHAR(500),
    Preco DECIMAL(10, 2) NOT NULL,
    Estoque INT NOT NULL DEFAULT 0
);

CREATE TABLE Pedidos (
    PedidoID INT PRIMARY KEY IDENTITY(1,1),
    ClienteID INT FOREIGN KEY REFERENCES Clientes(ClienteID),
    DataPedido DATETIME DEFAULT GETDATE(),
    ValorTotal DECIMAL(10, 2) NOT NULL
);

CREATE TABLE ItensPedido (
    ItemID INT PRIMARY KEY IDENTITY(1,1),
    PedidoID INT FOREIGN KEY REFERENCES Pedidos(PedidoID),
    ProdutoID INT FOREIGN KEY REFERENCES Produtos(ProdutoID),
    Quantidade INT NOT NULL,
    PrecoUnitario DECIMAL(10, 2) NOT NULL
);
Criação de Índices:
Crie índices apropriados para melhorar o desempenho das consultas
Exemplo de comando T-SQL para criar índices:
sql
USE ItibereBD;

CREATE INDEX IX_Clientes_Nome ON Clientes(Nome);
CREATE INDEX IX_Produtos_Nome ON Produtos(Nome);
CREATE INDEX IX_Pedidos_ClienteID ON Pedidos(ClienteID);
CREATE INDEX IX_Pedidos_DataPedido ON Pedidos(DataPedido);
CREATE INDEX IX_ItensPedido_PedidoID ON ItensPedido(PedidoID);
CREATE INDEX IX_ItensPedido_ProdutoID ON ItensPedido(ProdutoID);
3. Configuração de Segurança e Permissões
Para que o Integration Runtime possa acessar o banco de dados, você precisará configurar a segurança e as permissões adequadas:
Criação de Login e Usuário:
Crie um login no SQL Server para o Integration Runtime
Crie um usuário no banco de dados ItibereBD associado a esse login
Exemplo de comandos T-SQL:
sql
USE master;

-- Criar login
CREATE LOGIN IR_User WITH PASSWORD = 'SenhaSegura123!';

USE ItibereBD;

-- Criar usuário
CREATE USER IR_User FOR LOGIN IR_User;
Atribuição de Permissões:
Atribua as permissões mínimas necessárias para o usuário
Para operações de leitura, a permissão SELECT é suficiente
Exemplo de comandos T-SQL:
sql
USE ItibereBD;

-- Conceder permissão de leitura em todas as tabelas
GRANT SELECT ON SCHEMA::dbo TO IR_User;

-- Ou conceder permissões específicas por tabela
GRANT SELECT ON dbo.Clientes TO IR_User;
GRANT SELECT ON dbo.Produtos TO IR_User;
GRANT SELECT ON dbo.Pedidos TO IR_User;
GRANT SELECT ON dbo.ItensPedido TO IR_User;
Configuração de Autenticação:
Certifique-se de que o SQL Server está configurado para permitir autenticação SQL Server (além da autenticação Windows)
Isso pode ser configurado nas propriedades do servidor no SQL Server Management Studio
4. Configuração de Rede e Firewall
Para garantir que o Integration Runtime possa se comunicar com o banco de dados:
Habilitação do Protocolo TCP/IP:
Abra o SQL Server Configuration Manager
Navegue até "Configuração de Rede do SQL Server" > "Protocolos para [INSTÂNCIA]"
Certifique-se de que o protocolo TCP/IP está habilitado
Configuração de Porta:
Por padrão, o SQL Server usa a porta 1433 para comunicação TCP/IP
Você pode verificar ou alterar a porta nas propriedades do protocolo TCP/IP
Configuração de Firewall:
Certifique-se de que a porta do SQL Server está aberta no firewall do servidor
Se o banco de dados estiver em um servidor diferente do Integration Runtime, configure o firewall para permitir a comunicação entre os servidores
5. Teste de Conectividade
Após configurar o banco de dados, é importante testar a conectividade:
Teste com SQL Server Management Studio:
Tente se conectar ao banco de dados ItibereBD usando as credenciais do usuário criado para o Integration Runtime
Verifique se você consegue visualizar e consultar as tabelas
Teste com Ferramentas de Linha de Comando:
Use o utilitário sqlcmd para testar a conectividade a partir do servidor onde o Integration Runtime está instalado
Exemplo de comando:
sqlcmd -S [SERVIDOR] -d ItibereBD -U IR_User -P SenhaSegura123!
Teste com uma Consulta Simples:
Execute uma consulta simples para verificar se você consegue acessar os dados
Exemplo de consulta:
sql
SELECT TOP 10 * FROM Clientes;
Considerações Importantes
Backup: Implemente uma estratégia de backup regular para o banco de dados ItibereBD
Monitoramento: Configure o monitoramento do banco de dados para identificar problemas de desempenho ou segurança
Manutenção: Realize tarefas de manutenção regulares, como reconstrução de índices e atualização de estatísticas
Segurança: Mantenha as credenciais de acesso ao banco de dados em local seguro e considere a rotação periódica de senhas
Auditoria: Configure a auditoria de acesso ao banco de dados para rastrear quem está acessando os dados e quando
Próximos Passos
Após a configuração adequada do banco de dados ItibereBD, o próximo passo será configurar o Azure Blob Storage, que será o destino dos dados transferidos do ambiente local.
Configuração do Azure Blob Storage
O Azure Blob Storage é um serviço de armazenamento de objetos na nuvem da Microsoft, otimizado para armazenar grandes quantidades de dados não estruturados. Neste guia, vamos detalhar o processo de configuração do Azure Blob Storage para nosso projeto de redundância de arquivos, que servirá como destino para os dados transferidos do banco de dados local ItibereBD.
Entendendo o Azure Blob Storage
O Azure Blob Storage é projetado para:
Servir imagens ou documentos diretamente a um navegador
Armazenar arquivos para acesso distribuído
Transmitir vídeo e áudio
Armazenar dados para backup e restauração, recuperação de desastres e arquivamento
Armazenar dados para análise por serviços locais ou hospedados no Azure
No contexto do nosso projeto, utilizaremos o Blob Storage para armazenar cópias dos dados do banco de dados local, organizados em diferentes camadas (raw, bronze, etc.), garantindo redundância e disponibilidade.
Pré-requisitos
Antes de iniciar a configuração do Azure Blob Storage, certifique-se de que você possui:
Uma assinatura ativa do Microsoft Azure
Permissões adequadas para criar recursos na sua assinatura (Contributor ou Owner)
Conhecimento básico do portal Azure e conceitos de armazenamento em nuvem
Processo de Configuração do Azure Blob Storage
1. Criação da Conta de Armazenamento
O primeiro passo é criar uma conta de armazenamento no Azure:
Acesso ao Portal Azure:
Acesse o Portal Azure através do endereço https://portal.azure.com
Faça login com suas credenciais
Criação da Conta de Armazenamento:
Clique no botão "+ Criar um recurso" no canto superior esquerdo da tela
Na barra de pesquisa, digite "Conta de armazenamento" e selecione "Conta de armazenamento" nos resultados
Clique no botão "Criar" para iniciar o processo de criação
Configuração Básica:
Assinatura: Selecione a assinatura Azure que deseja utilizar
Grupo de Recursos: Selecione o mesmo grupo de recursos utilizado para o Azure Data Factory (por exemplo, "rg-redundancia-arquivos")
Nome da conta de armazenamento: Digite um nome único globalmente, como "stredundanciabastos" (use apenas letras minúsculas e números)
Região: Selecione a mesma região utilizada para o Azure Data Factory
Desempenho: Selecione "Standard" para a maioria dos casos de uso
Redundância: Selecione "Armazenamento com redundância geográfica e acesso de leitura (RA-GRS)" para máxima disponibilidade e durabilidade
Configuração Avançada:
Acesso: Selecione "Habilitar acesso público a todos os blobs" se precisar de acesso público, ou "Desabilitar acesso público" para maior segurança
Nível de acesso padrão: Selecione "Hot" para dados acessados frequentemente
Exigir transferência segura: Mantenha habilitado para garantir que apenas conexões HTTPS sejam aceitas
Configuração de Rede:
Conectividade de rede: Selecione "Ponto de extremidade público (todas as redes)" para permitir acesso de qualquer rede, ou "Ponto de extremidade público (redes selecionadas)" para restringir o acesso
Firewall: Se escolher "redes selecionadas", adicione os endereços IP ou redes que devem ter acesso
Proteção de Dados:
Versionamento de blob: Habilite para manter versões anteriores dos blobs
Exclusão reversível para blobs: Habilite para permitir a recuperação de blobs excluídos
Período de retenção: Defina o período de retenção para blobs excluídos (por exemplo, 7 dias)
Criptografia:
Mantenha as configurações padrão para usar chaves gerenciadas pela Microsoft
Tags:
Adicione tags relevantes para organização e governança (opcional, mas recomendado)
Exemplo: Projeto=RedundanciaArquivos, Ambiente=Desenvolvimento
Revisão e Criação:
Revise todas as configurações e clique em "Criar"
Aguarde a conclusão da implantação
2. Criação de Contêineres
Após a criação da conta de armazenamento, você precisará criar contêineres para organizar os dados:
Acesso à Conta de Armazenamento:
No Portal Azure, navegue até a conta de armazenamento recém-criada
No menu lateral, selecione "Contêineres" na seção "Armazenamento de dados"
Criação de Contêineres para Diferentes Camadas:
Clique no botão "+ Contêiner" para criar um novo contêiner
Para cada camada de dados, crie um contêiner separado:
raw: Para armazenar os dados brutos extraídos do banco de dados
bronze: Para armazenar dados após a primeira transformação
silver: Para armazenar dados após transformações adicionais (opcional)
gold: Para armazenar dados prontos para consumo (opcional)
Configuração de Cada Contêiner:
Nome: Digite o nome do contêiner (por exemplo, "raw", "bronze", etc.)
Nível de acesso público: Selecione "Privado (sem acesso anônimo)" para maior segurança
Clique em "Criar" para criar o contêiner
3. Configuração de Políticas de Acesso
Para controlar o acesso aos dados armazenados no Blob Storage:
Configuração de Assinaturas de Acesso Compartilhado (SAS):
No menu lateral da conta de armazenamento, selecione "Assinaturas de acesso compartilhado"
Configure os parâmetros da SAS:
Serviços permitidos: Selecione "Blob"
Tipos de recursos permitidos: Selecione "Contêiner" e "Objeto"
Permissões permitidas: Selecione as permissões necessárias (por exemplo, "Leitura", "Gravação", "Listagem")
Datas de início e expiração: Defina um período de validade adequado
Clique em "Gerar SAS e cadeia de conexão"
Salve a cadeia de conexão SAS e o token SAS para uso posterior no Azure Data Factory
Configuração de Identidades Gerenciadas (recomendado para maior segurança):
No menu lateral da conta de armazenamento, selecione "Controle de acesso (IAM)"
Clique em "+ Adicionar" e selecione "Adicionar atribuição de função"
Selecione a função "Colaborador de Dados do Blob de Armazenamento"
Em "Atribuir acesso a", selecione "Identidade gerenciada"
Selecione a identidade gerenciada do seu Azure Data Factory
Clique em "Salvar"
4. Configuração de Ciclo de Vida
Para gerenciar automaticamente o ciclo de vida dos dados:
Acesso à Configuração de Ciclo de Vida:
No menu lateral da conta de armazenamento, selecione "Gerenciamento do ciclo de vida"
Clique em "+ Adicionar uma regra"
Configuração de Regras de Ciclo de Vida:
Nome da regra: Digite um nome descritivo (por exemplo, "MoverParaArquivo")
Escopo da regra: Selecione "Aplicar regra a todos os blobs na conta de armazenamento" ou limite a contêineres específicos
Tipo de blob base: Selecione "Blobs de blocos"
Configuração de Ações:
Você pode configurar ações como:
Mover para armazenamento frio (Cool) após um determinado número de dias
Mover para armazenamento de arquivo (Archive) após um determinado número de dias
Excluir blobs após um determinado número de dias
Configure as ações de acordo com suas necessidades de retenção de dados
Revisão e Adição:
Revise as configurações e clique em "Adicionar" para criar a regra
5. Configuração de Monitoramento
Para monitorar o uso e o desempenho do Blob Storage:
Habilitação de Métricas:
No menu lateral da conta de armazenamento, selecione "Métricas"
Configure os gráficos para monitorar métricas relevantes, como:
Disponibilidade
Latência
Transações
Capacidade
Configuração de Alertas:
No menu lateral da conta de armazenamento, selecione "Alertas"
Clique em "+ Nova regra de alerta"
Configure alertas para condições importantes, como:
Capacidade próxima do limite
Erros de transação acima de um determinado limiar
Latência elevada
6. Teste de Acesso
Após configurar o Blob Storage, é importante testar o acesso:
Teste de Upload Manual:
No menu lateral da conta de armazenamento, selecione "Contêineres"
Selecione um dos contêineres criados
Clique em "Carregar" e selecione um arquivo de teste
Verifique se o upload foi bem-sucedido
Teste de Download Manual:
Selecione o arquivo carregado
Clique em "Baixar" e verifique se o download foi bem-sucedido
Teste de Acesso Programático:
Você pode testar o acesso programático usando ferramentas como o Azure Storage Explorer ou bibliotecas de cliente para várias linguagens de programação
Considerações Importantes
Custo: O Azure Blob Storage é cobrado com base no armazenamento utilizado, transações e transferência de dados. Monitore o uso para evitar custos inesperados.
Segurança: Utilize o princípio do menor privilégio ao conceder permissões de acesso. Evite usar chaves de acesso compartilhado quando possível, preferindo identidades gerenciadas.
Desempenho: O desempenho do Blob Storage pode ser afetado por fatores como tamanho dos blobs, padrões de acesso e localização geográfica. Otimize esses fatores conforme necessário.
Conformidade: Certifique-se de que a configuração do Blob Storage atende aos requisitos de conformidade da sua organização, como GDPR, HIPAA, etc.
Próximos Passos
Após a configuração adequada do Azure Blob Storage, o próximo passo será estabelecer a infraestrutura de integração, configurando os Linked Services no Azure Data Factory para conectar o banco de dados local ItibereBD e o Azure Blob Storage.
Estabelecendo Conexões Seguras com Ambientes On-Premises
A segurança é um aspecto crítico ao estabelecer conexões entre ambientes de nuvem e ambientes locais (on-premises). Neste guia, vamos detalhar como estabelecer conexões seguras entre o Azure Data Factory e o ambiente on-premises para nosso projeto de redundância de arquivos.
Importância das Conexões Seguras
Ao transferir dados entre ambientes on-premises e a nuvem, é essencial garantir que:
Os dados sejam transmitidos de forma segura, protegidos contra interceptação
Apenas serviços e usuários autorizados possam acessar os dados
A comunicação seja confiável e resiliente
As políticas de segurança corporativas e requisitos regulatórios sejam atendidos
Arquitetura de Segurança com o Integration Runtime
O Integration Runtime (IR) auto-hospedado que configuramos anteriormente é o componente central para estabelecer conexões seguras entre o Azure Data Factory e o ambiente on-premises. Vamos entender como ele garante a segurança:
1. Modelo de Comunicação Segura
O Integration Runtime estabelece um modelo de comunicação seguro através dos seguintes mecanismos:
Comunicação de Saída Apenas: O IR inicia apenas conexões de saída para a nuvem Azure. Não é necessário abrir portas de entrada no firewall corporativo, o que reduz significativamente a superfície de ataque.
Canal Criptografado: Toda a comunicação entre o IR e o Azure Data Factory ocorre através de canais criptografados usando o protocolo HTTPS/TLS 1.2 ou superior.
Autenticação Baseada em Chave: O IR é autenticado no Azure Data Factory usando uma chave de autenticação gerada no momento do registro, garantindo que apenas IRs autorizados possam se comunicar com o seu Data Factory.
2. Configuração de Segurança Avançada
Para aumentar ainda mais a segurança das conexões, você pode implementar as seguintes configurações:
2.1. Configuração de Proxy
Se sua organização utiliza um servidor proxy para controlar o tráfego de saída para a internet:
Abra o Microsoft Integration Runtime Configuration Manager no servidor BS_BASTOS
Clique na guia "Configurações"
Configure as informações do servidor proxy:
Endereço do servidor proxy
Porta
Credenciais de autenticação (se necessário)
2.2. Criptografia de Credenciais
O Integration Runtime pode criptografar as credenciais utilizadas para acessar fontes de dados:
Abra o Microsoft Integration Runtime Configuration Manager
Clique na guia "Configurações"
Na seção "Criptografia de Credenciais", clique em "Alterar"
Defina uma senha forte para criptografar as credenciais
Opcionalmente, você pode configurar a rotação automática da chave de criptografia
2.3. Isolamento de Rede
Para ambientes com requisitos de segurança mais rigorosos:
Segmentação de Rede: Coloque o servidor BS_BASTOS em um segmento de rede isolado, com acesso controlado ao banco de dados e à internet
Firewall de Aplicação Web (WAF): Implemente um WAF para filtrar o tráfego de saída para o Azure
Monitoramento de Tráfego: Configure ferramentas de monitoramento de tráfego de rede para detectar padrões anômalos
3. Configuração de Alta Disponibilidade e Recuperação de Desastres
Para garantir a continuidade das operações:
Múltiplos Nodes: Instale o Integration Runtime em múltiplos servidores, todos registrados com o mesmo IR no Azure Data Factory
Balanceamento de Carga: O Azure Data Factory automaticamente distribui a carga entre os nodes disponíveis
Failover Automático: Se um node ficar indisponível, o Azure Data Factory automaticamente redireciona as operações para os nodes disponíveis
Melhores Práticas de Segurança
1. Atualizações Regulares
Mantenha o Integration Runtime sempre atualizado:
Por padrão, o IR é atualizado automaticamente quando novas versões estão disponíveis
Verifique periodicamente se as atualizações automáticas estão funcionando corretamente
Se as atualizações automáticas estiverem desabilitadas por algum motivo, atualize manualmente o IR regularmente
2. Monitoramento e Auditoria
Implemente monitoramento e auditoria abrangentes:
Logs do Integration Runtime: Configure o IR para gerar logs detalhados
Azure Monitor: Utilize o Azure Monitor para acompanhar as atividades do Data Factory
Alertas: Configure alertas para ser notificado sobre eventos importantes, como falhas de conexão ou atividades suspeitas
3. Princípio do Menor Privilégio
Aplique o princípio do menor privilégio:
Crie contas de serviço dedicadas para o Integration Runtime com apenas as permissões necessárias
Revise e audite regularmente as permissões concedidas
Implemente a rotação regular de credenciais
4. Segurança Física
Não se esqueça da segurança física:
Garanta que o servidor BS_BASTOS esteja em um local seguro, com acesso físico controlado
Implemente medidas de segurança física, como câmeras de vigilância e controle de acesso
Verificação da Segurança da Conexão
Após implementar as medidas de segurança, é importante verificar se a conexão está funcionando corretamente e de forma segura:
Teste de Conectividade:
No Azure Data Factory Studio, vá para "Gerenciar" > "Integration Runtimes"
Selecione o "integrationRuntimeB"
Clique em "Testar conexão" para verificar se o IR pode se comunicar com o Azure Data Factory
Verificação de Logs:
Verifique os logs do Integration Runtime para garantir que não há erros ou avisos relacionados à segurança
Os logs estão localizados em C:\ProgramData\Microsoft\Integration Runtime\Logs por padrão
Monitoramento de Tráfego:
Utilize ferramentas de monitoramento de rede para verificar se o tráfego entre o IR e o Azure está sendo criptografado corretamente
Verifique se apenas as portas necessárias estão sendo utilizadas
Solução de Problemas Comuns
1. Problemas de Conectividade
Se o Integration Runtime não conseguir se conectar ao Azure Data Factory:
Verifique se o servidor tem acesso à internet
Confirme se as configurações de proxy estão corretas
Verifique se as portas necessárias estão abertas no firewall
Reinicie o serviço do Integration Runtime
2. Problemas de Autenticação
Se houver problemas de autenticação:
Verifique se a chave de autenticação não expirou
Se necessário, gere uma nova chave de autenticação no Azure Data Factory e registre novamente o IR
3. Problemas de Desempenho
Se a transferência de dados estiver lenta:
Verifique a largura de banda da conexão com a internet
Monitore os recursos do servidor (CPU, memória, disco) para identificar possíveis gargalos
Considere adicionar mais nodes ao Integration Runtime para distribuir a carga
Próximos Passos
Após estabelecer conexões seguras entre o Azure Data Factory e o ambiente on-premises, o próximo passo será configurar os Linked Services para o banco de dados SQL local (ItibereBD) e o Azure Blob Storage, que permitirão a transferência efetiva de dados entre os ambientes.
Configuração de Linked Services para o Banco de Dados SQL Local
Os Linked Services no Azure Data Factory são componentes essenciais que definem as informações de conexão necessárias para que o serviço se conecte a fontes de dados externas. Neste guia, vamos detalhar o processo de configuração do Linked Service para o banco de dados SQL local (ItibereBD) em nosso projeto de redundância de arquivos.
O que são Linked Services?
Os Linked Services no Azure Data Factory funcionam de maneira semelhante a cadeias de conexão, definindo as informações de conexão necessárias para que o Data Factory se conecte a recursos externos. Eles são a base para a criação de Datasets e atividades em pipelines.
No contexto do nosso projeto, precisamos criar um Linked Service para o banco de dados SQL local (ItibereBD) que permitirá ao Azure Data Factory acessar e extrair dados desse banco de dados através do Integration Runtime auto-hospedado.
Pré-requisitos
Antes de iniciar a configuração do Linked Service para o banco de dados SQL local, certifique-se de que você possui:
Azure Data Factory criado e configurado
Integration Runtime auto-hospedado instalado, configurado e em execução no servidor BS_BASTOS
Banco de dados ItibereBD configurado e acessível a partir do servidor BS_BASTOS
Credenciais de acesso ao banco de dados com permissões adequadas
Processo de Configuração do Linked Service para o Banco de Dados SQL Local
1. Acesso ao Azure Data Factory Studio
Acesse o Portal Azure através do endereço https://portal.azure.com
Navegue até o seu Azure Data Factory (adf-dio-bastos)
Clique em "Autor e Monitor" para abrir o Azure Data Factory Studio
2. Criação do Linked Service
No Azure Data Factory Studio, clique em "Gerenciar" no menu à esquerda
Selecione "Conexões" no painel de navegação
Clique em "+ Novo" na seção "Linked Services"
3. Seleção do Tipo de Conexão
Na janela "Novo linked service", pesquise por "SQL Server" na barra de pesquisa
Selecione "SQL Server" na lista de resultados
Clique em "Continuar"
4. Configuração das Propriedades de Conexão
Na janela de configuração do Linked Service, preencha as seguintes informações:
Seção Geral:
Nome: Digite um nome descritivo para o Linked Service, como "LS_SQLServer_ItibereBD"
Descrição (opcional): Digite uma descrição que ajude a identificar o propósito deste Linked Service
Seção Conectar via integration runtime:
Selecione "integrationRuntimeB" na lista suspensa (este é o Integration Runtime auto-hospedado que configuramos anteriormente)
Seção Informações de conexão:
Nome do servidor: Digite o nome do servidor onde o banco de dados ItibereBD está hospedado. Se estiver no mesmo servidor que o Integration Runtime, você pode usar "localhost" ou "127.0.0.1". Caso contrário, use o nome ou endereço IP do servidor.
Tipo de autenticação: Selecione o tipo de autenticação que você está usando para acessar o banco de dados:
Autenticação do Windows: Se estiver usando autenticação integrada do Windows
Autenticação SQL: Se estiver usando autenticação nativa do SQL Server
Nome de usuário: Digite o nome de usuário para acessar o banco de dados
Senha: Digite a senha associada ao nome de usuário
Banco de dados: Digite "ItibereBD" (o nome do banco de dados que queremos acessar)
Opções avançadas (opcional):
Usar conexão SSL: Marque esta opção se desejar usar SSL para criptografar a conexão com o banco de dados
Tempo limite de conexão: Defina o tempo limite para estabelecer uma conexão com o banco de dados
Tempo limite de comando: Defina o tempo limite para a execução de comandos no banco de dados
5. Teste da Conexão
Clique no botão "Testar conexão" para verificar se as informações de conexão estão corretas
Aguarde a conclusão do teste
Se o teste for bem-sucedido, você verá uma mensagem de confirmação
Se o teste falhar, verifique as informações de conexão e certifique-se de que o Integration Runtime está em execução
6. Finalização da Criação do Linked Service
Após o teste bem-sucedido, clique em "Criar" para criar o Linked Service
Aguarde a conclusão da criação
O novo Linked Service aparecerá na lista de Linked Services na seção "Conexões"
Configurações Avançadas e Melhores Práticas
1. Segurança de Credenciais
Para aumentar a segurança das credenciais de banco de dados:
Uso do Azure Key Vault:
Crie um Azure Key Vault para armazenar as credenciais de banco de dados
Configure um Linked Service para o Azure Key Vault
Referencie os segredos do Key Vault no Linked Service do SQL Server
Rotação de Credenciais:
Implemente uma política de rotação regular de senhas
Atualize o Linked Service ou os segredos no Key Vault quando as credenciais forem alteradas
2. Monitoramento e Diagnóstico
Para facilitar o monitoramento e a solução de problemas:
Habilitação de Logs:
Configure o Azure Monitor para coletar logs detalhados do Azure Data Factory
Analise os logs regularmente para identificar problemas de conexão
Alertas:
Configure alertas para ser notificado sobre falhas de conexão ou outros problemas
3. Alta Disponibilidade
Para garantir a alta disponibilidade da conexão com o banco de dados:
Múltiplos Nodes de Integration Runtime:
Instale o Integration Runtime em múltiplos servidores
Configure todos os nodes para usar o mesmo Integration Runtime no Azure Data Factory
Failover de Banco de Dados:
Se possível, configure o SQL Server com alta disponibilidade (como Always On Availability Groups)
Configure o Linked Service para se conectar ao listener do grupo de disponibilidade
Solução de Problemas Comuns
1. Falha na Conexão
Se o teste de conexão falhar:
Verifique as Credenciais:
Certifique-se de que o nome de usuário e a senha estão corretos
Tente se conectar ao banco de dados usando o SQL Server Management Studio com as mesmas credenciais
Verifique o Integration Runtime:
Certifique-se de que o Integration Runtime está em execução
Verifique os logs do Integration Runtime para identificar possíveis problemas
Verifique a Rede:
Certifique-se de que o servidor onde o Integration Runtime está instalado pode acessar o servidor de banco de dados
Verifique se as portas necessárias estão abertas no firewall
2. Problemas de Desempenho
Se a conexão estiver lenta:
Otimize as Consultas:
Revise as consultas SQL utilizadas nos pipelines
Adicione índices apropriados no banco de dados
Verifique os Recursos:
Monitore o uso de CPU, memória e disco no servidor de banco de dados
Aumente os recursos se necessário
Próximos Passos
Após a configuração bem-sucedida do Linked Service para o banco de dados SQL local (ItibereBD), o próximo passo será configurar o Linked Service para o Azure Blob Storage, que será o destino dos dados transferidos do banco de dados local.
Configuração de Linked Services para o Azure Blob Storage
Os Linked Services no Azure Data Factory são componentes fundamentais que definem as informações de conexão necessárias para acessar fontes de dados externas. Neste guia, vamos detalhar o processo de configuração do Linked Service para o Azure Blob Storage em nosso projeto de redundância de arquivos.
Importância do Linked Service para o Azure Blob Storage
O Linked Service para o Azure Blob Storage permite que o Azure Data Factory se conecte à sua conta de armazenamento na nuvem, possibilitando a transferência de dados do banco de dados local (ItibereBD) para o armazenamento em nuvem. Este é um componente essencial para nosso projeto de redundância de arquivos, pois define o destino onde os dados serão armazenados de forma segura e redundante.
Pré-requisitos
Antes de iniciar a configuração do Linked Service para o Azure Blob Storage, certifique-se de que você possui:
Azure Data Factory criado e configurado
Conta de Azure Blob Storage criada e configurada (conforme detalhado no documento anterior)
Permissões adequadas para acessar a conta de armazenamento
Chave de acesso da conta de armazenamento ou configuração de identidade gerenciada
Processo de Configuração do Linked Service para o Azure Blob Storage
1. Acesso ao Azure Data Factory Studio
Acesse o Portal Azure através do endereço https://portal.azure.com
Navegue até o seu Azure Data Factory (adf-dio-bastos)
Clique em "Autor e Monitor" para abrir o Azure Data Factory Studio
2. Criação do Linked Service
No Azure Data Factory Studio, clique em "Gerenciar" no menu à esquerda
Selecione "Conexões" no painel de navegação
Clique em "+ Novo" na seção "Linked Services"
3. Seleção do Tipo de Conexão
Na janela "Novo linked service", pesquise por "Azure Blob Storage" na barra de pesquisa
Selecione "Azure Blob Storage" na lista de resultados
Clique em "Continuar"
4. Configuração das Propriedades de Conexão
Na janela de configuração do Linked Service, preencha as seguintes informações:
Seção Geral:
Nome: Digite um nome descritivo para o Linked Service, como "LS_AzureBlobStorage"
Descrição (opcional): Digite uma descrição que ajude a identificar o propósito deste Linked Service
Seção Conectar via integration runtime:
Selecione "AutoResolveIntegrationRuntime" na lista suspensa (este é o Integration Runtime gerenciado pelo Azure, adequado para conexões com serviços Azure)
Seção Método de autenticação:
Você tem várias opções de autenticação:
Chave de conta: Utiliza a chave de acesso da conta de armazenamento
Cadeia de conexão: Utiliza uma cadeia de conexão completa
Identidade gerenciada: Utiliza a identidade gerenciada do Azure Data Factory (recomendado para maior segurança)
Entidade de serviço: Utiliza uma entidade de serviço do Azure AD
Para este guia, vamos utilizar a opção "Chave de conta", que é a mais simples:
Selecione "Chave de conta" como método de autenticação
Seleção de conta de armazenamento: Escolha "Inserir manualmente"
Nome da conta de armazenamento: Digite o nome da sua conta de armazenamento (por exemplo, "stredundanciabastos")
Chave da conta de armazenamento: Digite a chave de acesso da sua conta de armazenamento (você pode obtê-la no Portal Azure, na seção "Chaves de acesso" da sua conta de armazenamento)
Alternativamente, se preferir usar a identidade gerenciada (recomendado para ambientes de produção):
Selecione "Identidade gerenciada" como método de autenticação
URL do serviço Blob: Digite a URL do serviço Blob da sua conta de armazenamento (por exemplo, "https://stredundanciabastos.blob.core.windows.net" )
Opções avançadas (opcional):
Tempo limite de conexão: Defina o tempo limite para estabelecer uma conexão com o armazenamento
Tempo limite de comando: Defina o tempo limite para a execução de comandos no armazenamento
5. Teste da Conexão
Clique no botão "Testar conexão" para verificar se as informações de conexão estão corretas
Aguarde a conclusão do teste
Se o teste for bem-sucedido, você verá uma mensagem de confirmação
Se o teste falhar, verifique as informações de conexão e certifique-se de que a conta de armazenamento está acessível
6. Finalização da Criação do Linked Service
Após o teste bem-sucedido, clique em "Criar" para criar o Linked Service
Aguarde a conclusão da criação
O novo Linked Service aparecerá na lista de Linked Services na seção "Conexões"
Configurações Avançadas e Melhores Práticas
1. Segurança de Credenciais
Para aumentar a segurança das credenciais de armazenamento:
Uso do Azure Key Vault:
Crie um Azure Key Vault para armazenar as chaves de acesso da conta de armazenamento
Configure um Linked Service para o Azure Key Vault
Referencie os segredos do Key Vault no Linked Service do Azure Blob Storage
Uso de Identidade Gerenciada:
Configure o Azure Data Factory para usar identidade gerenciada
Atribua as permissões necessárias à identidade gerenciada na conta de armazenamento
Configure o Linked Service para usar identidade gerenciada em vez de chaves de acesso
2. Monitoramento e Diagnóstico
Para facilitar o monitoramento e a solução de problemas:
Habilitação de Logs:
Configure o Azure Monitor para coletar logs detalhados do Azure Data Factory
Habilite os logs de diagnóstico na conta de armazenamento
Analise os logs regularmente para identificar problemas de acesso
Alertas:
Configure alertas para ser notificado sobre falhas de acesso ou outros problemas
3. Otimização de Desempenho
Para otimizar o desempenho da transferência de dados:
Localização dos Recursos:
Mantenha o Azure Data Factory e a conta de armazenamento na mesma região para reduzir a latência
Se não for possível, considere usar o Azure ExpressRoute para melhorar a conectividade
Configuração de Paralelismo:
Ajuste as configurações de paralelismo nos pipelines para otimizar a transferência de dados
Monitore o desempenho e ajuste as configurações conforme necessário
Solução de Problemas Comuns
1. Falha na Conexão
Se o teste de conexão falhar:
Verifique as Credenciais:
Certifique-se de que a chave de acesso da conta de armazenamento está correta
Tente regenerar a chave de acesso e atualizar o Linked Service
Verifique as Permissões:
Se estiver usando identidade gerenciada, certifique-se de que a identidade tem as permissões necessárias na conta de armazenamento
Verifique as políticas de acesso e as configurações de firewall da conta de armazenamento
Verifique a Rede:
Certifique-se de que a conta de armazenamento está acessível a partir do Azure Data Factory
Verifique se há restrições de firewall ou de rede virtual que possam estar bloqueando o acesso
2. Problemas de Desempenho
Se a transferência de dados estiver lenta:
Otimize o Tamanho dos Blocos:
Ajuste o tamanho dos blocos para otimizar a transferência de dados
Experimente diferentes configurações para encontrar o valor ideal
Verifique a Largura de Banda:
Monitore a utilização da largura de banda durante a transferência de dados
Considere aumentar a largura de banda se necessário
Próximos Passos
Após a configuração bem-sucedida do Linked Service para o Azure Blob Storage, o próximo passo será configurar o Linked Service para o Azure SQL Database (se aplicável) e, em seguida, criar os Datasets e Pipelines no Azure Data Factory para implementar o processo de redundância de arquivos.
Configuração de Linked Services para Azure SQL Database (Opcional)
O Azure SQL Database é um serviço de banco de dados relacional totalmente gerenciado na nuvem da Microsoft. Embora o foco principal do nosso projeto de redundância de arquivos seja a transferência de dados do banco de dados local (ItibereBD) para o Azure Blob Storage, em alguns cenários pode ser útil incluir o Azure SQL Database como parte da solução. Neste guia, vamos detalhar o processo de configuração do Linked Service para o Azure SQL Database, caso seja aplicável ao seu ambiente.
Quando Utilizar o Azure SQL Database no Projeto
A inclusão do Azure SQL Database em nosso projeto de redundância pode ser benéfica nos seguintes cenários:
Acesso Rápido a Dados Estruturados: Quando você precisa manter uma cópia dos dados em formato relacional para consultas rápidas
Transformações Complexas: Quando você precisa realizar transformações complexas que são mais eficientes em um ambiente de banco de dados relacional
Integração com Aplicações: Quando você tem aplicações na nuvem que precisam acessar os dados
Análise de Dados: Quando você precisa realizar análises avançadas nos dados
Se nenhum desses cenários se aplica ao seu caso específico, você pode pular esta etapa e continuar com a criação de Datasets e Pipelines utilizando apenas o banco de dados local e o Azure Blob Storage.
Pré-requisitos
Se você decidir incluir o Azure SQL Database, certifique-se de que possui:
Uma instância do Azure SQL Database criada e configurada
Permissões adequadas para acessar o banco de dados
Credenciais de acesso ou configuração de identidade gerenciada
Processo de Criação do Azure SQL Database (Se Necessário)
Se você ainda não possui uma instância do Azure SQL Database, siga estas etapas para criá-la:
Acesso ao Portal Azure:
Acesse o Portal Azure através do endereço https://portal.azure.com
Faça login com suas credenciais
Criação do Banco de Dados:
Clique no botão "+ Criar um recurso" no canto superior esquerdo da tela
Na barra de pesquisa, digite "SQL Database" e selecione "SQL Database" nos resultados
Clique no botão "Criar" para iniciar o processo de criação
Configuração Básica:
Assinatura: Selecione a assinatura Azure que deseja utilizar
Grupo de Recursos: Selecione o mesmo grupo de recursos utilizado para os outros componentes
Nome do banco de dados: Digite um nome para o banco de dados, como "sqldb-redundancia"
Servidor: Selecione um servidor existente ou crie um novo
Se criar um novo, defina um nome de servidor, localização, e credenciais de administrador
Deseja usar o pool elástico SQL?: Selecione "Não" para um banco de dados único
Computação + armazenamento: Clique em "Configurar banco de dados" para selecionar o nível de desempenho adequado
Para ambientes de teste, o nível Básico ou Standard pode ser suficiente
Para ambientes de produção, considere o nível Premium ou Business Critical
Configuração de Rede:
Conectividade de rede: Selecione "Ponto de extremidade público" para permitir acesso de qualquer rede, ou "Ponto de extremidade privado" para acesso mais restrito
Regras de firewall: Configure para permitir acesso dos serviços Azure
Configuração Adicional:
Ordenação: Mantenha o padrão ou selecione a ordenação adequada para seu caso
Azure Defender para SQL: Habilite para maior segurança (recomendado para ambientes de produção)
Revisão e Criação:
Revise todas as configurações e clique em "Criar"
Aguarde a conclusão da implantação
Processo de Configuração do Linked Service para o Azure SQL Database
1. Acesso ao Azure Data Factory Studio
Acesse o Portal Azure através do endereço https://portal.azure.com
Navegue até o seu Azure Data Factory (adf-dio-bastos)
Clique em "Autor e Monitor" para abrir o Azure Data Factory Studio
2. Criação do Linked Service
No Azure Data Factory Studio, clique em "Gerenciar" no menu à esquerda
Selecione "Conexões" no painel de navegação
Clique em "+ Novo" na seção "Linked Services"
3. Seleção do Tipo de Conexão
Na janela "Novo linked service", pesquise por "Azure SQL Database" na barra de pesquisa
Selecione "Azure SQL Database" na lista de resultados
Clique em "Continuar"
4. Configuração das Propriedades de Conexão
Na janela de configuração do Linked Service, preencha as seguintes informações:
Seção Geral:
Nome: Digite um nome descritivo para o Linked Service, como "LS_AzureSQLDatabase"
Descrição (opcional): Digite uma descrição que ajude a identificar o propósito deste Linked Service
Seção Conectar via integration runtime:
Selecione "AutoResolveIntegrationRuntime" na lista suspensa (este é o Integration Runtime gerenciado pelo Azure, adequado para conexões com serviços Azure)
Seção Método de autenticação:
Você tem várias opções de autenticação:
Autenticação SQL: Utiliza nome de usuário e senha
Identidade gerenciada: Utiliza a identidade gerenciada do Azure Data Factory (recomendado para maior segurança)
Entidade de serviço: Utiliza uma entidade de serviço do Azure AD
Autenticação AAD com identidade MSI: Utiliza a identidade gerenciada atribuída pelo sistema
Para este guia, vamos utilizar a opção "Autenticação SQL", que é a mais simples:
Selecione "Autenticação SQL" como método de autenticação
Nome do servidor totalmente qualificado: Digite o nome do servidor do Azure SQL Database (por exemplo, "servidor-redundancia.database.windows.net")
Nome do banco de dados: Digite o nome do banco de dados (por exemplo, "sqldb-redundancia")
Nome de usuário: Digite o nome de usuário para acessar o banco de dados
Senha: Digite a senha associada ao nome de usuário
Alternativamente, se preferir usar a identidade gerenciada (recomendado para ambientes de produção):
Selecione "Identidade gerenciada" como método de autenticação
Nome do servidor totalmente qualificado: Digite o nome do servidor do Azure SQL Database
Nome do banco de dados: Digite o nome do banco de dados
Opções avançadas (opcional):
Usar conexão SSL: Mantenha habilitado para garantir que a conexão seja criptografada
Tempo limite de conexão: Defina o tempo limite para estabelecer uma conexão com o banco de dados
Tempo limite de comando: Defina o tempo limite para a execução de comandos no banco de dados
5. Teste da Conexão
Clique no botão "Testar conexão" para verificar se as informações de conexão estão corretas
Aguarde a conclusão do teste
Se o teste for bem-sucedido, você verá uma mensagem de confirmação
Se o teste falhar, verifique as informações de conexão e certifique-se de que o banco de dados está acessível
6. Finalização da Criação do Linked Service
Após o teste bem-sucedido, clique em "Criar" para criar o Linked Service
Aguarde a conclusão da criação
O novo Linked Service aparecerá na lista de Linked Services na seção "Conexões"
Configurações Avançadas e Melhores Práticas
1. Segurança de Credenciais
Para aumentar a segurança das credenciais de banco de dados:
Uso do Azure Key Vault:
Crie um Azure Key Vault para armazenar as credenciais de banco de dados
Configure um Linked Service para o Azure Key Vault
Referencie os segredos do Key Vault no Linked Service do Azure SQL Database
Uso de Identidade Gerenciada:
Configure o Azure Data Factory para usar identidade gerenciada
Atribua as permissões necessárias à identidade gerenciada no Azure SQL Database
Configure o Linked Service para usar identidade gerenciada em vez de credenciais
2. Monitoramento e Diagnóstico
Para facilitar o monitoramento e a solução de problemas:
Habilitação de Logs:
Configure o Azure Monitor para coletar logs detalhados do Azure Data Factory
Habilite os logs de auditoria no Azure SQL Database
Analise os logs regularmente para identificar problemas de acesso
Alertas:
Configure alertas para ser notificado sobre falhas de acesso ou outros problemas
3. Otimização de Desempenho
Para otimizar o desempenho da transferência de dados:
Localização dos Recursos:
Mantenha o Azure Data Factory e o Azure SQL Database na mesma região para reduzir a latência
Configuração de Índices:
Crie índices apropriados no Azure SQL Database para otimizar as consultas
Considere o uso de tabelas particionadas para grandes volumes de dados
Solução de Problemas Comuns
1. Falha na Conexão
Se o teste de conexão falhar:
Verifique as Credenciais:
Certifique-se de que o nome de usuário e a senha estão corretos
Tente se conectar ao banco de dados usando o SQL Server Management Studio com as mesmas credenciais
Verifique as Permissões:
Se estiver usando identidade gerenciada, certifique-se de que a identidade tem as permissões necessárias no banco de dados
Verifique as políticas de acesso e as configurações de firewall do servidor
Verifique a Rede:
Certifique-se de que o Azure SQL Database está acessível a partir do Azure Data Factory
Verifique se há restrições de firewall que possam estar bloqueando o acesso
2. Problemas de Desempenho
Se a transferência de dados estiver lenta:
Otimize as Consultas:
Revise as consultas SQL utilizadas nos pipelines
Adicione índices apropriados no banco de dados
Verifique o Nível de Desempenho:
Monitore o DTU ou vCore utilização no Azure SQL Database
Considere aumentar o nível de desempenho se necessário
Conclusão
A inclusão do Azure SQL Database em nosso projeto de redundância de arquivos é opcional e depende dos requisitos específicos do seu ambiente. Se você decidir incluí-lo, o Linked Service configurado permitirá que o Azure Data Factory se conecte ao banco de dados na nuvem, possibilitando a transferência e transformação de dados entre os diferentes componentes da solução.
Próximos Passos
Após a configuração dos Linked Services necessários (banco de dados SQL local, Azure Blob Storage e, opcionalmente, Azure SQL Database), o próximo passo será criar os Datasets e Pipelines no Azure Data Factory para implementar o processo de redundância de arquivos.
Criação de Datasets para Leitura de Dados do Banco Local
Os Datasets no Azure Data Factory são objetos que representam estruturas de dados e definem como os dados serão acessados e manipulados. Neste guia, vamos detalhar o processo de criação de Datasets para leitura de dados do banco de dados local (ItibereBD) em nosso projeto de redundância de arquivos.
Importância dos Datasets de Leitura
Os Datasets de leitura são componentes fundamentais em nosso projeto, pois definem:
Quais dados serão extraídos do banco de dados local
Como esses dados serão estruturados
Como o Azure Data Factory interpretará esses dados
A configuração correta desses Datasets é essencial para garantir que os dados sejam extraídos de forma eficiente e precisa.
Pré-requisitos
Antes de iniciar a criação dos Datasets para leitura, certifique-se de que você possui:
Azure Data Factory criado e configurado
Integration Runtime auto-hospedado instalado e em execução
Linked Service para o banco de dados SQL local configurado
Conhecimento das tabelas e estruturas de dados no banco de dados ItibereBD
Processo de Criação de Datasets para Leitura
1. Acesso ao Azure Data Factory Studio
Acesse o Portal Azure através do endereço https://portal.azure.com
Navegue até o seu Azure Data Factory (adf-dio-bastos)
Clique em "Autor e Monitor" para abrir o Azure Data Factory Studio
2. Criação de um Novo Dataset
No Azure Data Factory Studio, clique em "Autor" no menu à esquerda (ícone de lápis)
Clique em "+" (mais) e selecione "Dataset" no menu suspenso
3. Seleção do Tipo de Dataset
Na janela "Novo dataset", pesquise por "SQL Server" na barra de pesquisa
Selecione "SQL Server" na lista de resultados
Clique em "Continuar"
4. Configuração das Propriedades do Dataset
Na janela de configuração do Dataset, preencha as seguintes informações:
Seção Geral:
Nome: Digite um nome descritivo para o Dataset, como "DS_SQLServer_Clientes" (se estiver criando um Dataset para a tabela Clientes)
Linked service: Selecione o Linked Service para o banco de dados SQL local que você criou anteriormente (por exemplo, "LS_SQLServer_ItibereBD")
Modo de importação: Selecione "Tabela" se quiser importar uma tabela inteira, ou "Consulta" se quiser usar uma consulta SQL personalizada
Seção Tabela:
Se você selecionou "Tabela" como modo de importação:
Nome da tabela: Digite o nome da tabela que deseja importar (por exemplo, "Clientes")
Se você selecionou "Consulta" como modo de importação:
Consulta: Digite a consulta SQL que deseja executar (por exemplo, "SELECT * FROM Clientes WHERE DataCadastro > '2023-01-01'")
Seção Esquema:
Clique em "Importar esquema" para detectar automaticamente a estrutura da tabela
Alternativamente, você pode definir o esquema manualmente clicando em "Editar" e adicionando as colunas necessárias
5. Visualização dos Dados (Opcional)
Clique na guia "Visualização de dados" para verificar se o Dataset está configurado corretamente
Clique em "Visualizar dados" para ver uma amostra dos dados da tabela
Verifique se os dados estão sendo exibidos corretamente e se todas as colunas estão presentes
6. Finalização da Criação do Dataset
Clique em "OK" para criar o Dataset
O novo Dataset aparecerá na lista de Datasets no painel "Autor"
Criação de Múltiplos Datasets para Diferentes Tabelas
Em um projeto real, você provavelmente precisará criar múltiplos Datasets para diferentes tabelas do banco de dados. Repita o processo acima para cada tabela que deseja incluir no processo de redundância.
Exemplo de Datasets para o Projeto
Considerando o exemplo de banco de dados ItibereBD com as tabelas Clientes, Produtos, Pedidos e ItensPedido, você pode criar os seguintes Datasets:
DS_SQLServer_Clientes:
Tabela: Clientes
Colunas: ClienteID, Nome, Email, Telefone, DataCadastro
DS_SQLServer_Produtos:
Tabela: Produtos
Colunas: ProdutoID, Nome, Descricao, Preco, Estoque
DS_SQLServer_Pedidos:
Tabela: Pedidos
Colunas: PedidoID, ClienteID, DataPedido, ValorTotal
DS_SQLServer_ItensPedido:
Tabela: ItensPedido
Colunas: ItemID, PedidoID, ProdutoID, Quantidade, PrecoUnitario
Uso de Parâmetros nos Datasets
Para tornar seus Datasets mais flexíveis e reutilizáveis, você pode usar parâmetros:
Criação de Parâmetros:
Na janela de configuração do Dataset, clique na guia "Parâmetros"
Clique em "+ Novo" para adicionar um novo parâmetro
Digite um nome para o parâmetro (por exemplo, "TableName")
Defina um valor padrão (opcional)
Uso de Parâmetros:
Na seção "Tabela", clique no ícone de parâmetro ao lado do campo "Nome da tabela"
Selecione o parâmetro que você criou (por exemplo, "@dataset().TableName")
Benefícios dos Parâmetros:
Permite reutilizar o mesmo Dataset para diferentes tabelas
Facilita a manutenção e atualização dos pipelines
Permite a criação de pipelines dinâmicos
Configurações Avançadas e Melhores Práticas
1. Otimização de Consultas
Para melhorar o desempenho da extração de dados:
Filtragem de Dados:
Use consultas SQL com cláusulas WHERE para filtrar os dados na origem
Isso reduz a quantidade de dados transferidos pela rede
Seleção de Colunas:
Selecione apenas as colunas necessárias em vez de usar "SELECT *"
Isso reduz a largura de banda necessária e melhora o desempenho
Particionamento:
Para tabelas muito grandes, considere particionar os dados usando consultas com filtros de data ou ID
Isso permite processar os dados em lotes menores e mais gerenciáveis
2. Tratamento de Tipos de Dados
Para garantir a compatibilidade de tipos de dados:
Mapeamento de Tipos:
Verifique se os tipos de dados no Dataset correspondem aos tipos de dados na tabela de origem
Ajuste os tipos de dados conforme necessário na guia "Esquema"
Tratamento de Valores Nulos:
Defina como os valores nulos serão tratados
Considere usar transformações para substituir valores nulos por valores padrão
3. Segurança e Governança
Para garantir a segurança e a governança dos dados:
Mascaramento de Dados Sensíveis:
Use consultas SQL para mascarar ou excluir dados sensíveis na origem
Considere implementar políticas de mascaramento de dados no SQL Server
Auditoria de Acesso:
Configure a auditoria de acesso no SQL Server para rastrear quem está acessando os dados
Monitore os logs de auditoria regularmente
Solução de Problemas Comuns
1. Falha na Conexão
Se o Dataset não conseguir se conectar ao banco de dados:
Verifique o Linked Service:
Certifique-se de que o Linked Service está configurado corretamente
Teste a conexão do Linked Service
Verifique o Integration Runtime:
Certifique-se de que o Integration Runtime está em execução
Verifique os logs do Integration Runtime para identificar possíveis problemas
Verifique as Permissões:
Certifique-se de que o usuário configurado no Linked Service tem permissões para acessar a tabela
2. Problemas com o Esquema
Se o esquema não for detectado corretamente:
Importação Manual do Esquema:
Defina o esquema manualmente na guia "Esquema"
Certifique-se de que os tipos de dados estão corretos
Verificação da Tabela:
Verifique se a tabela existe no banco de dados
Verifique se o nome da tabela está correto, incluindo maiúsculas e minúsculas (se relevante)
3. Problemas de Desempenho
Se a extração de dados estiver lenta:
Otimize as Consultas:
Revise as consultas SQL utilizadas
Adicione índices apropriados no banco de dados
Verifique os Recursos:
Monitore o uso de CPU, memória e disco no servidor de banco de dados
Aumente os recursos se necessário
Próximos Passos
Após a criação bem-sucedida dos Datasets para leitura de dados do banco de dados local, o próximo passo será criar os Datasets para escrita no Azure Blob Storage, que serão utilizados para armazenar os dados extraídos em formato de arquivo.
Criação de Datasets para Escrita no Azure Blob Storage
Os Datasets de escrita no Azure Data Factory são objetos que definem a estrutura e o formato dos dados que serão gravados no destino. Neste guia, vamos detalhar o processo de criação de Datasets para escrita no Azure Blob Storage em nosso projeto de redundância de arquivos.
Importância dos Datasets de Escrita
Os Datasets de escrita são componentes essenciais em nosso projeto, pois definem:
Onde os dados serão armazenados no Azure Blob Storage
Em qual formato os dados serão gravados
Como os dados serão organizados e estruturados
A configuração correta desses Datasets é fundamental para garantir que os dados sejam armazenados de forma eficiente e acessível.
Pré-requisitos
Antes de iniciar a criação dos Datasets para escrita, certifique-se de que você possui:
Azure Data Factory criado e configurado
Linked Service para o Azure Blob Storage configurado
Contêineres criados no Azure Blob Storage para as diferentes camadas de dados (raw, bronze, etc.)
Datasets de leitura configurados para o banco de dados local
Processo de Criação de Datasets para Escrita
1. Acesso ao Azure Data Factory Studio
Acesse o Portal Azure através do endereço https://portal.azure.com
Navegue até o seu Azure Data Factory (adf-dio-bastos)
Clique em "Autor e Monitor" para abrir o Azure Data Factory Studio
2. Criação de um Novo Dataset
No Azure Data Factory Studio, clique em "Autor" no menu à esquerda (ícone de lápis)
Clique em "+" (mais) e selecione "Dataset" no menu suspenso
3. Seleção do Tipo de Dataset
Na janela "Novo dataset", pesquise por "Azure Blob Storage" na barra de pesquisa
Selecione o formato de arquivo desejado:
DelimitedText: Para arquivos CSV ou TXT com valores separados por delimitadores
JSON: Para arquivos no formato JSON
Parquet: Para arquivos no formato Parquet (binário colunar)
Binary: Para arquivos binários genéricos
Para nosso projeto, vamos selecionar "DelimitedText" para criar arquivos TXT.
Clique em "Continuar"
4. Configuração das Propriedades do Dataset
Na janela de configuração do Dataset, preencha as seguintes informações:
Seção Geral:
Nome: Digite um nome descritivo para o Dataset, como "DS_Blob_Clientes_Raw" (se estiver criando um Dataset para a tabela Clientes na camada raw)
Linked service: Selecione o Linked Service para o Azure Blob Storage que você criou anteriormente (por exemplo, "LS_AzureBlobStorage")
Caminho do arquivo: Clique no botão "Procurar" para selecionar o contêiner e o caminho onde os arquivos serão gravados
Selecione o contêiner (por exemplo, "raw")
Defina o caminho da pasta (por exemplo, "clientes")
Defina o nome do arquivo (por exemplo, "clientes.txt") ou use parâmetros para nomes dinâmicos
Seção Formato de arquivo:
Formato de coluna: Selecione o formato de delimitação (por exemplo, "Delimitado por vírgula (,)")
Delimitador de coluna: Selecione o delimitador que será usado para separar as colunas (por exemplo, vírgula, tabulação, ponto e vírgula)
Delimitador de linha: Selecione o delimitador que será usado para separar as linhas (geralmente, quebra de linha)
Usar primeira linha como cabeçalho: Marque esta opção se quiser incluir os nomes das colunas na primeira linha do arquivo
Codificação: Selecione a codificação do arquivo (por exemplo, UTF-8)
Seção Esquema:
Clique em "Importar esquema" para detectar automaticamente a estrutura do arquivo (se houver um arquivo de exemplo)
Alternativamente, você pode definir o esquema manualmente clicando em "Editar" e adicionando as colunas necessárias
Para um novo arquivo, você pode definir o esquema com base no Dataset de leitura correspondente
5. Finalização da Criação do Dataset
Clique em "OK" para criar o Dataset
O novo Dataset aparecerá na lista de Datasets no painel "Autor"
Criação de Múltiplos Datasets para Diferentes Camadas
Em nosso projeto de redundância de arquivos, precisamos criar Datasets para diferentes camadas de armazenamento. Repita o processo acima para cada camada e tabela que deseja incluir.
Exemplo de Datasets para o Projeto
Considerando o exemplo de banco de dados ItibereBD com as tabelas Clientes, Produtos, Pedidos e ItensPedido, e as camadas raw e bronze, você pode criar os seguintes Datasets:
Camada Raw (dados brutos extraídos do banco de dados):
DS_Blob_Clientes_Raw:
Contêiner: raw
Caminho: clientes/clientes.txt
Formato: DelimitedText
DS_Blob_Produtos_Raw:
Contêiner: raw
Caminho: produtos/produtos.txt
Formato: DelimitedText
DS_Blob_Pedidos_Raw:
Contêiner: raw
Caminho: pedidos/pedidos.txt
Formato: DelimitedText
DS_Blob_ItensPedido_Raw:
Contêiner: raw
Caminho: itens_pedido/itens_pedido.txt
Formato: DelimitedText
Camada Bronze (dados após a primeira transformação):
DS_Blob_Clientes_Bronze:
Contêiner: bronze
Caminho: clientes/clientes.txt
Formato: DelimitedText
DS_Blob_Produtos_Bronze:
Contêiner: bronze
Caminho: produtos/produtos.txt
Formato: DelimitedText
DS_Blob_Pedidos_Bronze:
Contêiner: bronze
Caminho: pedidos/pedidos.txt
Formato: DelimitedText
DS_Blob_ItensPedido_Bronze:
Contêiner: bronze
Caminho: itens_pedido/itens_pedido.txt
Formato: DelimitedText
Uso de Parâmetros nos Datasets
Para tornar seus Datasets mais flexíveis e reutilizáveis, você pode usar parâmetros:
Criação de Parâmetros:
Na janela de configuração do Dataset, clique na guia "Parâmetros"
Clique em "+ Novo" para adicionar um novo parâmetro
Digite um nome para o parâmetro (por exemplo, "FolderPath" ou "FileName")
Defina um valor padrão (opcional)
Uso de Parâmetros no Caminho do Arquivo:
Na seção "Caminho do arquivo", você pode usar parâmetros para definir o caminho e o nome do arquivo
Por exemplo, para o caminho: @concat(dataset().FolderPath, '/', dataset().FileName)
Isso permite reutilizar o mesmo Dataset para diferentes tabelas e camadas
Benefícios dos Parâmetros:
Permite criar Datasets genéricos que podem ser reutilizados
Facilita a manutenção e atualização dos pipelines
Permite a criação de pipelines dinâmicos
Configurações Avançadas e Melhores Práticas
1. Nomenclatura de Arquivos
Para facilitar a organização e o gerenciamento dos arquivos:
Padrão de Nomenclatura:
Use um padrão consistente para nomear os arquivos
Inclua informações como nome da tabela, data de extração, etc.
Exemplo: clientes_20230101.txt
Particionamento por Data:
Considere particionar os arquivos por data para facilitar o gerenciamento e a consulta
Exemplo de estrutura de pastas: raw/clientes/ano=2023/mes=01/dia=01/clientes.txt
Versionamento:
Considere incluir informações de versão nos nomes dos arquivos
Isso é útil para rastrear mudanças e atualizações
2. Compressão de Arquivos
Para reduzir o espaço de armazenamento e melhorar o desempenho:
Habilitação da Compressão:
Na seção "Formato de arquivo", expanda "Configurações adicionais"
Marque a opção "Compressão"
Selecione o tipo de compressão (por exemplo, GZip, Deflate)
Considerações sobre Compressão:
A compressão reduz o espaço de armazenamento, mas pode aumentar o tempo de processamento
Alguns formatos, como Parquet, já incluem compressão integrada
3. Tratamento de Erros
Para lidar com erros durante a escrita:
Configuração de Tolerância a Falhas:
Nas atividades de cópia, configure a tolerância a falhas
Defina o número máximo de linhas com erro permitido
Configure o comportamento em caso de erro (continuar ou falhar)
Registro de Erros:
Configure o registro de erros para capturar informações sobre falhas
Armazene os registros de erro em um local separado para análise posterior
Solução de Problemas Comuns
1. Falha na Escrita
Se o Dataset não conseguir escrever no Azure Blob Storage:
Verifique o Linked Service:
Certifique-se de que o Linked Service está configurado corretamente
Teste a conexão do Linked Service
Verifique as Permissões:
Certifique-se de que o Linked Service tem permissões para escrever no contêiner
Verifique as políticas de acesso da conta de armazenamento
Verifique o Caminho:
Certifique-se de que o contêiner existe
Verifique se o caminho está correto, incluindo maiúsculas e minúsculas (o Azure Blob Storage é sensível a maiúsculas e minúsculas)
2. Problemas com o Formato do Arquivo
Se o arquivo não for criado no formato correto:
Verifique as Configurações de Formato:
Certifique-se de que o delimitador de coluna está correto
Verifique se a opção de cabeçalho está configurada corretamente
Verifique a Codificação:
Certifique-se de que a codificação está correta
Considere usar UTF-8 para compatibilidade máxima
3. Problemas de Desempenho
Se a escrita de dados estiver lenta:
Otimize o Tamanho dos Blocos:
Ajuste o tamanho dos blocos para otimizar a transferência de dados
Experimente diferentes configurações para encontrar o valor ideal
Verifique a Largura de Banda:
Monitore a utilização da largura de banda durante a transferência de dados
Considere aumentar a largura de banda se necessário
Próximos Passos
Após a criação bem-sucedida dos Datasets para escrita no Azure Blob Storage, o próximo passo será criar os Pipelines no Azure Data Factory para copiar os dados do banco de dados local para o Azure Blob Storage, converter os dados em arquivos TXT e organizá-los em diferentes camadas.
Criação de Pipeline para Cópia de Dados On-Premises para o Azure Data Lake
Os Pipelines no Azure Data Factory são a unidade lógica de orquestração que agrupa atividades para realizar uma tarefa específica. Neste guia, vamos detalhar o processo de criação de um Pipeline para copiar dados do banco de dados local (ItibereBD) para o Azure Blob Storage, que funcionará como nosso Data Lake.
Importância dos Pipelines de Cópia
Os Pipelines de cópia são componentes fundamentais em nosso projeto de redundância de arquivos, pois:
Automatizam o processo de extração de dados do ambiente on-premises
Garantem a transferência segura e eficiente dos dados para a nuvem
Permitem a programação e o monitoramento das operações de cópia
Fornecem recursos de tratamento de erros e recuperação
Pré-requisitos
Antes de iniciar a criação do Pipeline de cópia, certifique-se de que você possui:
Azure Data Factory criado e configurado
Integration Runtime auto-hospedado instalado e em execução
Linked Services configurados para o banco de dados SQL local e o Azure Blob Storage
Datasets de leitura e escrita criados e configurados
Processo de Criação do Pipeline de Cópia
1. Acesso ao Azure Data Factory Studio
Acesse o Portal Azure através do endereço https://portal.azure.com
Navegue até o seu Azure Data Factory (adf-dio-bastos)
Clique em "Autor e Monitor" para abrir o Azure Data Factory Studio
2. Criação de um Novo Pipeline
No Azure Data Factory Studio, clique em "Autor" no menu à esquerda (ícone de lápis)
Clique em "+" (mais) e selecione "Pipeline" no menu suspenso
Na janela de propriedades do Pipeline, digite um nome descritivo, como "PL_CopiarDados_OnPremises_para_DataLake"
3. Adição de Atividades de Cópia
Para cada tabela que deseja copiar, você precisará adicionar uma atividade de cópia:
Adição da Atividade:
No painel "Atividades", expanda a categoria "Mover e transformar"
Arraste a atividade "Copiar dados" para a tela de design do pipeline
Configuração Geral da Atividade:
Na guia "Geral", digite um nome descritivo para a atividade, como "Copy_Clientes"
Opcionalmente, adicione uma descrição
Configuração da Origem:
Na guia "Origem", selecione o Dataset de leitura correspondente à tabela que deseja copiar (por exemplo, "DS_SQLServer_Clientes")
Se necessário, configure opções adicionais:
Usar consulta: Marque esta opção se quiser usar uma consulta personalizada em vez de selecionar toda a tabela
Consulta: Digite a consulta SQL (por exemplo, "SELECT * FROM Clientes WHERE DataCadastro > '2023-01-01'")
Tamanho do lote: Defina o número de linhas a serem lidas em cada lote (útil para tabelas grandes)
Tempo limite de consulta: Defina o tempo máximo para a execução da consulta
Configuração do Destino:
Na guia "Destino", selecione o Dataset de escrita correspondente (por exemplo, "DS_Blob_Clientes_Raw")
Configure as opções de gravação:
Método de cópia de arquivo: Selecione "Adicionar" para adicionar novos arquivos ou "Substituir" para substituir arquivos existentes
Tamanho do bloco (MB): Defina o tamanho dos blocos para upload (padrão: 4 MB)
Máximo de conexões simultâneas: Defina o número máximo de conexões simultâneas (padrão: 10)
Mapeamento de Colunas:
Na guia "Mapeamentos", verifique se as colunas de origem estão corretamente mapeadas para as colunas de destino
Se necessário, ajuste os mapeamentos:
Clique em "Importar esquemas" para detectar automaticamente os esquemas
Adicione, remova ou modifique mapeamentos conforme necessário
Configure transformações de tipo de dados, se aplicável
Configurações de Desempenho:
Na guia "Configurações", ajuste as opções de desempenho:
Grau de cópia paralela: Defina o número de cópias paralelas (útil para grandes volumes de dados)
Linhas por lote: Defina o número de linhas a serem processadas em cada lote
Tempo limite de cópia: Defina o tempo máximo para a operação de cópia
Tratamento de Falhas:
Na guia "Configurações", configure as opções de tratamento de falhas:
Tolerância a falhas: Defina o número máximo de linhas com erro permitido
Armazenar configurações de log de falhas: Configure o armazenamento de logs de erro
4. Repetição para Múltiplas Tabelas
Repita o processo acima para cada tabela que deseja copiar. Por exemplo:
Copy_Clientes: Para copiar a tabela Clientes
Copy_Produtos: Para copiar a tabela Produtos
Copy_Pedidos: Para copiar a tabela Pedidos
Copy_ItensPedido: Para copiar a tabela ItensPedido
5. Configuração de Dependências entre Atividades (Opcional)
Se houver dependências entre as tabelas (por exemplo, se você precisar copiar a tabela Clientes antes da tabela Pedidos), configure as dependências:
Selecione a atividade que deve ser executada depois (por exemplo, "Copy_Pedidos")
Na guia "Dependências", clique em "Adicionar"
Selecione a atividade que deve ser executada antes (por exemplo, "Copy_Clientes")
Defina a condição de dependência (geralmente "Sucesso")
6. Configuração de Parâmetros do Pipeline (Opcional)
Para tornar o pipeline mais flexível e reutilizável, você pode configurar parâmetros:
Clique no pipeline (fora de qualquer atividade)
Na guia "Parâmetros", clique em "+ Novo"
Adicione parâmetros como:
DataInicio: Para filtrar dados por data de início
DataFim: Para filtrar dados por data de fim
CamadaDestino: Para definir a camada de destino (raw, bronze, etc.)
Use os parâmetros nas atividades:
Na consulta SQL: SELECT * FROM Clientes WHERE DataCadastro BETWEEN '@{pipeline().parameters.DataInicio}' AND '@{pipeline().parameters.DataFim}'
No caminho do arquivo de destino: @concat(pipeline().parameters.CamadaDestino, '/clientes/')
7. Validação e Publicação do Pipeline
Validação:
Clique no botão "Validar" na barra de ferramentas para verificar se o pipeline está configurado corretamente
Corrija quaisquer erros ou avisos
Publicação:
Clique no botão "Publicar tudo" para publicar o pipeline e todos os recursos relacionados
Confirme a publicação na janela de diálogo
8. Execução Manual do Pipeline (Teste)
Clique no botão "Adicionar gatilho" na barra de ferramentas
Selecione "Acionar agora"
Se o pipeline tiver parâmetros, preencha os valores dos parâmetros
Clique em "OK" para iniciar a execução
9. Monitoramento da Execução
Clique em "Monitor" no menu à esquerda
Selecione "Execuções de pipeline" para ver o status da execução
Clique na execução para ver detalhes
Verifique o status de cada atividade e os detalhes de execução
Configurações Avançadas e Melhores Práticas
1. Estratégias de Cópia Incremental
Para otimizar o processo de cópia e evitar a transferência de dados redundantes:
Cópia Baseada em Carimbo de Data/Hora:
Use uma coluna de data/hora para identificar registros novos ou alterados
Armazene o último valor processado em uma tabela de controle ou variável
Use esse valor para filtrar os dados na próxima execução
Cópia Baseada em Chave de Alteração:
Use uma coluna que indica se um registro foi alterado (por exemplo, uma coluna "Modificado" ou "Versão")
Filtre os dados com base nessa coluna
Implementação:
Use variáveis ou parâmetros para armazenar o último valor processado
Use a atividade "Lookup" para recuperar o último valor
Use a atividade "Stored Procedure" para atualizar o valor após a cópia
2. Paralelização e Otimização de Desempenho
Para melhorar o desempenho da cópia de dados:
Particionamento de Dados:
Divida grandes conjuntos de dados em partições menores
Use múltiplas atividades de cópia para processar as partições em paralelo
Ajuste de Configurações de Desempenho:
Experimente diferentes valores para "Grau de cópia paralela"
Ajuste o "Tamanho do lote" e "Linhas por lote"
Monitore o desempenho e ajuste as configurações conforme necessário
Uso de Compressão:
Habilite a compressão para reduzir o volume de dados transferidos
Considere o impacto da compressão no desempenho geral
3. Tratamento de Erros e Recuperação
Para garantir a robustez do pipeline:
Configuração de Tolerância a Falhas:
Defina um número razoável de linhas com erro permitido
Configure o armazenamento de logs de erro para análise posterior
Implementação de Mecanismos de Retry:
Configure políticas de retry para atividades que podem falhar temporariamente
Use a opção "Retry" nas configurações da atividade
Notificações de Falha:
Configure alertas para ser notificado em caso de falha
Use a atividade "Web" para enviar notificações por e-mail ou outros canais
Solução de Problemas Comuns
1. Falhas de Conexão
Se o pipeline falhar devido a problemas de conexão:
Verifique o Integration Runtime:
Certifique-se de que o Integration Runtime está em execução
Verifique os logs do Integration Runtime para identificar possíveis problemas
Verifique os Linked Services:
Teste a conexão dos Linked Services
Verifique se as credenciais estão corretas
Verifique a Rede:
Certifique-se de que o servidor onde o Integration Runtime está instalado pode acessar o banco de dados e a internet
Verifique se as portas necessárias estão abertas no firewall
2. Problemas de Desempenho
Se a cópia de dados estiver lenta:
Otimize as Consultas:
Revise as consultas SQL utilizadas
Adicione índices apropriados no banco de dados
Ajuste as Configurações de Desempenho:
Experimente diferentes valores para "Grau de cópia paralela"
Ajuste o "Tamanho do lote" e "Linhas por lote"
Verifique os Recursos:
Monitore o uso de CPU, memória e disco no servidor de banco de dados e no servidor do Integration Runtime
Aumente os recursos se necessário
3. Problemas com o Formato dos Dados
Se houver problemas com o formato dos dados:
Verifique os Mapeamentos:
Certifique-se de que as colunas estão corretamente mapeadas
Verifique se os tipos de dados são compatíveis
Verifique as Configurações de Formato:
Certifique-se de que o delimitador de coluna está correto
Verifique se a opção de cabeçalho está configurada corretamente
Próximos Passos
Após a criação bem-sucedida do Pipeline para cópia de dados do ambiente on-premises para o Azure Blob Storage, o próximo passo será configurar a conversão dos dados em arquivos TXT e organizar os arquivos em diferentes camadas (raw, bronze, etc.).
Configuração para Conversão dos Dados em Arquivos .TXT
A conversão dos dados em arquivos .TXT é uma etapa importante no processo de redundância de arquivos, pois permite armazenar os dados em um formato universal, facilmente acessível e independente de plataforma. Neste guia, vamos detalhar o processo de configuração para converter os dados extraídos do banco de dados local em arquivos .TXT no Azure Data Factory.
Importância da Conversão para Arquivos .TXT
Os arquivos .TXT oferecem várias vantagens em um projeto de redundância de dados:
Universalidade: Podem ser lidos por praticamente qualquer sistema ou aplicação
Portabilidade: Não dependem de software específico para serem acessados
Simplicidade: São fáceis de manipular e processar
Compressibilidade: Geralmente apresentam boas taxas de compressão
Longevidade: O formato é estável e não está sujeito a obsolescência como formatos proprietários
Abordagens para Conversão em Arquivos .TXT
No Azure Data Factory, existem duas abordagens principais para converter dados em arquivos .TXT:
1. Conversão Direta Durante a Cópia
Esta abordagem utiliza a própria atividade de cópia para converter os dados durante a transferência do banco de dados para o Blob Storage. É a abordagem mais simples e eficiente para a maioria dos casos.
2. Conversão em Duas Etapas
Esta abordagem envolve primeiro copiar os dados para um formato intermediário (como JSON ou Parquet) e depois convertê-los para .TXT usando uma atividade de transformação. É mais complexa, mas oferece maior flexibilidade para transformações avançadas.
Para nosso projeto, vamos focar na primeira abordagem, que é mais direta e eficiente.
Configuração da Conversão Direta Durante a Cópia
1. Configuração dos Datasets
Para configurar a conversão direta, precisamos garantir que os Datasets de destino estejam configurados corretamente:
Acesso ao Azure Data Factory Studio:
Acesse o Portal Azure através do endereço https://portal.azure.com
Navegue até o seu Azure Data Factory (adf-dio-bastos)
Clique em "Autor e Monitor" para abrir o Azure Data Factory Studio
Verificação dos Datasets de Destino:
No painel "Autor", expanda a seção "Datasets"
Verifique se os Datasets de destino (por exemplo, "DS_Blob_Clientes_Raw") estão configurados como "DelimitedText"
Se necessário, edite os Datasets para ajustar as configurações:
Clique no Dataset para abrir suas propriedades
Na seção "Formato de arquivo", verifique as seguintes configurações:
Formato de coluna: Deve estar definido como "Delimitado por vírgula (,)" ou outro delimitador de sua preferência
Delimitador de coluna: Confirme se o delimitador está correto (vírgula, tabulação, ponto e vírgula, etc.)
Delimitador de linha: Geralmente é a quebra de linha (\r\n para Windows ou \n para Unix/Linux)
Usar primeira linha como cabeçalho: Marque esta opção se quiser incluir os nomes das colunas na primeira linha
Codificação: Recomendamos UTF-8 para compatibilidade máxima
2. Ajuste da Atividade de Cópia
Para garantir que a conversão seja realizada corretamente durante a cópia:
Edição do Pipeline:
No painel "Autor", expanda a seção "Pipelines"
Selecione o pipeline de cópia que você criou anteriormente (por exemplo, "PL_CopiarDados_OnPremises_para_DataLake")
Clique na atividade de cópia que deseja ajustar (por exemplo, "Copy_Clientes")
Configuração da Guia "Mapeamentos":
Na guia "Mapeamentos", verifique se as colunas de origem estão corretamente mapeadas para as colunas de destino
Clique em "Importar esquemas" para detectar automaticamente os esquemas, se necessário
Verifique se os tipos de dados estão corretos e faça ajustes se necessário
Para colunas de data/hora, você pode precisar ajustar o formato para garantir que sejam convertidas corretamente para texto
Configuração da Guia "Configurações":
Na guia "Configurações", expanda a seção "Configurações de tipo de arquivo"
Verifique se as configurações de formato de arquivo estão corretas:
Formato de linha: Geralmente é a quebra de linha padrão
Nome do arquivo: Defina o nome do arquivo .TXT de saída (por exemplo, "clientes.txt")
Comportamento de gravação de arquivo: Selecione "Substituir" para substituir arquivos existentes ou "Adicionar" para anexar aos arquivos existentes
3. Tratamento de Tipos de Dados Especiais
Alguns tipos de dados requerem atenção especial ao serem convertidos para texto:
Datas e Horas:
Na guia "Mapeamentos", clique no ícone de edição ao lado da coluna de data/hora
Configure o formato de data/hora desejado (por exemplo, "yyyy-MM-dd HH:mm:ss")
Isso garantirá que as datas sejam convertidas em um formato consistente e legível
Valores Decimais:
Para colunas numéricas com casas decimais, verifique se o separador decimal está correto
Em alguns casos, pode ser necessário ajustar o formato para garantir compatibilidade com outras ferramentas
Campos com Delimitadores:
Se os dados contiverem o caractere usado como delimitador, certifique-se de que estão sendo tratados corretamente
O Azure Data Factory geralmente coloca esses campos entre aspas automaticamente, mas é importante verificar
4. Validação da Configuração
Para garantir que a configuração está correta:
Validação do Pipeline:
Clique no botão "Validar" na barra de ferramentas para verificar se o pipeline está configurado corretamente
Corrija quaisquer erros ou avisos
Execução de Teste:
Clique no botão "Adicionar gatilho" na barra de ferramentas
Selecione "Acionar agora"
Se o pipeline tiver parâmetros, preencha os valores dos parâmetros
Clique em "OK" para iniciar a execução
Verificação dos Resultados:
Após a conclusão da execução, verifique os arquivos .TXT gerados no Azure Blob Storage
Abra um dos arquivos para verificar se os dados foram convertidos corretamente
Certifique-se de que todos os caracteres especiais, datas e números estão sendo exibidos corretamente
Configuração Avançada para Conversão em Duas Etapas (Opcional)
Se você precisar de maior flexibilidade ou transformações mais complexas, pode optar pela abordagem de conversão em duas etapas:
1. Primeira Etapa: Cópia para Formato Intermediário
Criação de Datasets Intermediários:
Crie Datasets para armazenar os dados em um formato intermediário (como JSON ou Parquet)
Configure esses Datasets para apontar para uma pasta temporária no Blob Storage
Configuração do Pipeline de Cópia Inicial:
Configure uma atividade de cópia para transferir os dados do banco de dados para os Datasets intermediários
Ajuste as configurações conforme necessário
2. Segunda Etapa: Conversão para .TXT
Criação de Datasets de Destino Final:
Configure Datasets "DelimitedText" para os arquivos .TXT finais
Adição de Atividade de Transformação:
Adicione uma atividade de transformação (como "Data Flow" ou "Mapping Data Flow") ao pipeline
Configure a transformação para ler os dados do formato intermediário e convertê-los para .TXT
Aplique transformações adicionais conforme necessário (por exemplo, formatação de datas, filtragem de dados, etc.)
Configuração de Dependências:
Configure a atividade de transformação para ser executada após a conclusão da atividade de cópia inicial
Melhores Práticas para Conversão em .TXT
1. Padronização de Formatos
Para garantir a consistência e facilitar o processamento posterior:
Padronização de Datas:
Use um formato de data consistente em todos os arquivos (por exemplo, ISO 8601: "yyyy-MM-dd")
Documente o formato utilizado para referência futura
Padronização de Números:
Use um formato numérico consistente (por exemplo, ponto como separador decimal)
Evite separadores de milhares para simplificar o processamento
Padronização de Delimitadores:
Use o mesmo delimitador em todos os arquivos
Escolha um delimitador que seja improvável de aparecer nos dados (por exemplo, tabulação ou pipe "|")
2. Tratamento de Caracteres Especiais
Para evitar problemas com caracteres especiais:
Codificação UTF-8:
Use UTF-8 para garantir suporte a caracteres especiais e acentuados
Verifique se a codificação está sendo aplicada corretamente
Escape de Delimitadores:
Certifique-se de que campos que contêm o delimitador estão sendo tratados corretamente
Geralmente, isso é feito colocando o campo entre aspas duplas
3. Documentação
Para facilitar o uso e a manutenção:
Cabeçalhos:
Inclua cabeçalhos nos arquivos .TXT para identificar as colunas
Use nomes de colunas claros e descritivos
Arquivos de Metadados:
Considere criar arquivos de metadados separados que descrevem a estrutura dos arquivos .TXT
Inclua informações como nomes de colunas, tipos de dados, formatos, etc.
Solução de Problemas Comuns
1. Problemas de Codificação
Se os caracteres especiais não estiverem sendo exibidos corretamente:
Verifique a Codificação:
Certifique-se de que a codificação está definida como UTF-8
Se necessário, experimente outras codificações (como ISO-8859-1 para caracteres latinos)
Verifique o BOM (Byte Order Mark):
Alguns sistemas requerem o BOM para identificar corretamente a codificação UTF-8
Verifique se o BOM está sendo incluído ou excluído conforme necessário
2. Problemas com Delimitadores
Se os dados não estiverem sendo delimitados corretamente:
Verifique o Delimitador:
Certifique-se de que o delimitador configurado corresponde ao delimitador nos arquivos
Verifique se o delimitador não está presente nos dados
Verifique o Escape:
Certifique-se de que campos que contêm o delimitador estão sendo tratados corretamente
Verifique se as aspas estão sendo usadas corretamente para escapar delimitadores
3. Problemas com Tipos de Dados
Se os tipos de dados não estiverem sendo convertidos corretamente:
Verifique os Mapeamentos:
Certifique-se de que os tipos de dados estão corretamente mapeados
Ajuste os formatos de conversão conforme necessário
Verifique os Valores Nulos:
Certifique-se de que os valores nulos estão sendo tratados corretamente
Configure substituições para valores nulos, se necessário
Próximos Passos
Após configurar a conversão dos dados em arquivos .TXT, o próximo passo será organizar os arquivos em diferentes camadas (raw, bronze, etc.) para facilitar o gerenciamento e o processamento dos dados.
Organização dos Arquivos em Camadas (Raw, Bronze, etc.)
A organização dos arquivos em diferentes camadas é uma prática fundamental em arquiteturas modernas de dados, especialmente em projetos de redundância e data lakes. Neste guia, vamos detalhar o processo de organização dos arquivos em camadas no Azure Blob Storage utilizando o Azure Data Factory.
Importância da Organização em Camadas
A organização dos dados em camadas oferece diversos benefícios:
Separação de Responsabilidades: Cada camada tem um propósito específico e bem definido
Rastreabilidade: Facilita o rastreamento da linhagem dos dados desde a origem até o consumo
Governança: Permite aplicar políticas de governança específicas para cada camada
Otimização de Custos: Possibilita aplicar diferentes níveis de armazenamento (hot, cool, archive) conforme a frequência de acesso
Evolução Independente: Permite que cada camada evolua independentemente sem afetar as outras
Arquitetura de Camadas Comuns
Em um projeto de data lake, é comum utilizar as seguintes camadas:
1. Camada Raw (Bruta)
Propósito: Armazenar os dados exatamente como foram extraídos da fonte, sem nenhuma transformação
Características:
Dados no formato original ou minimamente processados
Preservação completa dos dados de origem
Geralmente não é acessada diretamente pelos usuários finais
Benefícios:
Permite reprocessar os dados a partir do estado original
Serve como "fonte da verdade" para auditoria e rastreabilidade
Facilita a recuperação em caso de problemas nas camadas subsequentes
2. Camada Bronze
Propósito: Armazenar os dados após transformações básicas e padronizações
Características:
Dados convertidos para formatos padronizados (como .TXT, Parquet, etc.)
Estrutura padronizada e documentada
Pode incluir validações básicas e limpeza de dados
Benefícios:
Facilita o processamento subsequente
Reduz a complexidade das transformações nas camadas superiores
Permite acesso mais eficiente aos dados
3. Camada Silver (Opcional)
Propósito: Armazenar os dados após transformações mais complexas e enriquecimento
Características:
Dados enriquecidos com informações adicionais
Relacionamentos entre entidades estabelecidos
Agregações e cálculos intermediários
Benefícios:
Facilita análises mais complexas
Reduz a duplicação de transformações
Melhora o desempenho das consultas
4. Camada Gold (Opcional)
Propósito: Armazenar os dados prontos para consumo pelos usuários finais
Características:
Dados totalmente transformados e otimizados para consulta
Modelos de dados específicos para casos de uso
Agregações e métricas de negócio
Benefícios:
Acesso direto pelos usuários finais
Otimizado para desempenho de consulta
Facilita a criação de relatórios e dashboards
Implementação da Organização em Camadas no Azure Data Factory
1. Estruturação dos Contêineres no Azure Blob Storage
O primeiro passo é estruturar os contêineres no Azure Blob Storage para refletir as camadas:
Acesso ao Portal Azure:
Acesse o Portal Azure através do endereço https://portal.azure.com
Navegue até a sua conta de armazenamento
Criação de Contêineres:
Se ainda não tiver criado, crie contêineres separados para cada camada:
Contêiner "raw" para a camada Raw
Contêiner "bronze" para a camada Bronze
Contêiner "silver" para a camada Silver (opcional)
Contêiner "gold" para a camada Gold (opcional)
Estruturação de Pastas:
Dentro de cada contêiner, crie uma estrutura de pastas organizada:
Por entidade (por exemplo, "clientes", "produtos", "pedidos")
Por data (por exemplo, "ano=2023/mes=01/dia=01")
Por região ou outra dimensão relevante
2. Configuração de Datasets no Azure Data Factory
Para cada camada, você precisará configurar Datasets correspondentes:
Datasets para a Camada Raw:
Configure Datasets apontando para o contêiner "raw"
Exemplo: "DS_Blob_Clientes_Raw", "DS_Blob_Produtos_Raw"
Datasets para a Camada Bronze:
Configure Datasets apontando para o contêiner "bronze"
Exemplo: "DS_Blob_Clientes_Bronze", "DS_Blob_Produtos_Bronze"
Datasets para as Camadas Silver e Gold (opcional):
Configure Datasets apontando para os contêineres "silver" e "gold"
Exemplo: "DS_Blob_Clientes_Silver", "DS_Blob_Clientes_Gold"
3. Criação de Pipelines para Movimentação entre Camadas
Você precisará criar pipelines para mover e transformar os dados entre as camadas:
Pipeline Raw para Bronze:
Crie um pipeline para mover dados da camada Raw para a Bronze
Inclua transformações básicas, como:
Padronização de formatos de data
Limpeza de caracteres especiais
Validação de tipos de dados
Pipeline Bronze para Silver (opcional):
Crie um pipeline para mover dados da camada Bronze para a Silver
Inclua transformações mais complexas, como:
Enriquecimento com dados de referência
Estabelecimento de relacionamentos
Cálculos e agregações intermediárias
Pipeline Silver para Gold (opcional):
Crie um pipeline para mover dados da camada Silver para a Gold
Inclua transformações finais, como:
Criação de modelos dimensionais
Agregações e métricas de negócio
Otimização para consulta
4. Exemplo de Pipeline Raw para Bronze
Vamos detalhar um exemplo de pipeline para mover dados da camada Raw para a Bronze:
Criação do Pipeline:
No Azure Data Factory Studio, clique em "Autor" no menu à esquerda
Clique em "+" (mais) e selecione "Pipeline"
Digite um nome como "PL_Raw_para_Bronze_Clientes"
Adição de Atividade de Cópia:
Arraste a atividade "Copiar dados" para a tela de design
Configure a origem para apontar para o Dataset da camada Raw (por exemplo, "DS_Blob_Clientes_Raw")
Configure o destino para apontar para o Dataset da camada Bronze (por exemplo, "DS_Blob_Clientes_Bronze")
Configuração de Transformações:
Na guia "Mapeamentos", configure transformações básicas:
Clique em uma coluna para configurar transformações específicas
Use expressões para padronizar formatos de data, remover caracteres especiais, etc.
Configuração de Parâmetros:
Adicione parâmetros para tornar o pipeline flexível:
Data de execução
Entidade a ser processada
Outros parâmetros relevantes
Configuração de Gatilhos:
Configure gatilhos para executar o pipeline automaticamente:
Gatilho programado (por exemplo, diariamente às 2h da manhã)
Gatilho baseado em eventos (por exemplo, quando novos dados chegam à camada Raw)
5. Uso do Data Flow para Transformações Complexas
Para transformações mais complexas entre camadas, você pode utilizar o Data Flow do Azure Data Factory:
Criação de um Data Flow:
No Azure Data Factory Studio, clique em "Autor" no menu à esquerda
Clique em "+" (mais) e selecione "Data Flow"
Configure a origem para apontar para o Dataset da camada de origem
Adição de Transformações:
Adicione transformações como:
Derived Column: Para criar novas colunas baseadas em expressões
Filter: Para filtrar linhas com base em condições
Join: Para combinar dados de diferentes fontes
Aggregate: Para realizar agregações
Sink: Para definir o destino dos dados transformados
Integração com o Pipeline:
No pipeline, adicione uma atividade "Data Flow"
Selecione o Data Flow que você criou
Configure os parâmetros conforme necessário
Melhores Práticas para Organização em Camadas
1. Nomenclatura e Documentação
Para facilitar a gestão e o entendimento:
Padrão de Nomenclatura:
Use um padrão consistente para nomear contêineres, pastas e arquivos
Inclua informações como camada, entidade, data, etc.
Documentação:
Documente a estrutura e o propósito de cada camada
Mantenha um dicionário de dados para cada camada
Documente as transformações aplicadas entre camadas
2. Particionamento e Otimização
Para melhorar o desempenho e a gestão:
Particionamento por Data:
Particione os dados por data para facilitar o processamento incremental
Use uma estrutura hierárquica (ano/mês/dia) para organizar os dados
Otimização de Formato:
Considere usar formatos colunar (como Parquet) para as camadas superiores
Isso melhora o desempenho de consulta e reduz o espaço de armazenamento
Compressão:
Aplique compressão adequada para cada camada
Balanceie entre economia de espaço e desempenho de processamento
3. Segurança e Governança
Para garantir a segurança e a conformidade:
Controle de Acesso:
Implemente controle de acesso baseado em função (RBAC) para cada camada
Restrinja o acesso à camada Raw apenas para processos automatizados
Auditoria:
Configure a auditoria de acesso para todas as camadas
Monitore quem está acessando os dados e quando
Políticas de Retenção:
Defina políticas de retenção para cada camada
Considere mover dados mais antigos para níveis de armazenamento mais econômicos
Monitoramento e Manutenção
Para garantir o funcionamento contínuo:
Monitoramento de Pipelines:
Configure alertas para falhas nos pipelines
Monitore o tempo de execução e o volume de dados processados
Validação de Dados:
Implemente verificações de qualidade de dados em cada camada
Configure alertas para problemas de qualidade
Manutenção Regular:
Revise e otimize regularmente a estrutura das camadas
Ajuste as transformações conforme as necessidades evoluem
Solução de Problemas Comuns
1. Problemas de Desempenho
Se o processamento entre camadas estiver lento:
Otimize o Particionamento:
Revise a estratégia de particionamento
Considere processar os dados em lotes menores
Ajuste as Configurações de Paralelismo:
Aumente o grau de paralelismo nas atividades de cópia e transformação
Monitore o uso de recursos e ajuste conforme necessário
2. Problemas de Consistência
Se houver inconsistências entre as camadas:
Verifique as Transformações:
Revise as transformações aplicadas entre camadas
Certifique-se de que não há perda ou corrupção de dados
Implemente Verificações de Integridade:
Adicione atividades de validação nos pipelines
Compare contagens de registros e checksums entre camadas
3. Problemas de Governança
Se houver problemas de governança:
Revise as Políticas de Acesso:
Certifique-se de que apenas usuários autorizados têm acesso a cada camada
Implemente o princípio do menor privilégio
Melhore a Documentação:
Atualize a documentação para refletir a estrutura atual
Documente claramente as transformações e as regras de negócio
Próximos Passos
Após configurar a organização dos arquivos em camadas no Azure Blob Storage, o próximo passo será utilizar o Azure Databricks para realizar transformações mais complexas e enriquecer os dados, aproveitando a estrutura de camadas que estabelecemos.
Configuração do Azure Databricks
O Azure Databricks é um serviço de análise de dados baseado em Apache Spark que fornece uma plataforma colaborativa para engenheiros de dados, cientistas de dados e analistas de negócios. Neste guia, vamos detalhar o processo de configuração do Azure Databricks para nosso projeto de redundância de arquivos, permitindo transformações avançadas nos dados armazenados no Azure Blob Storage.
Importância do Azure Databricks no Projeto
O Azure Databricks complementa nossa arquitetura de redundância de arquivos ao fornecer:
Processamento Distribuído: Capacidade de processar grandes volumes de dados de forma eficiente
Transformações Avançadas: Ferramentas para realizar transformações complexas que vão além das capacidades do Azure Data Factory
Linguagens Familiares: Suporte a Python, SQL, Scala e R para desenvolvimento de transformações
Integração Nativa: Conexão direta com o Azure Blob Storage e outros serviços Azure
Ambiente Colaborativo: Notebooks compartilháveis para documentar e colaborar no desenvolvimento
Pré-requisitos
Antes de iniciar a configuração do Azure Databricks, certifique-se de que você possui:
Uma assinatura ativa do Microsoft Azure
Permissões adequadas para criar recursos na sua assinatura (Contributor ou Owner)
Azure Blob Storage configurado com os dados organizados em camadas (conforme detalhado anteriormente)
Conhecimento básico de Spark e linguagens de programação como Python ou Scala
Processo de Criação e Configuração do Azure Databricks
1. Criação do Workspace do Azure Databricks
Acesso ao Portal Azure:
Acesse o Portal Azure através do endereço https://portal.azure.com
Faça login com suas credenciais
Criação do Recurso:
Clique no botão "+ Criar um recurso" no canto superior esquerdo da tela
Na barra de pesquisa, digite "Databricks" e selecione "Azure Databricks" nos resultados
Clique no botão "Criar" para iniciar o processo de criação
Configuração Básica:
Assinatura: Selecione a assinatura Azure que deseja utilizar
Grupo de Recursos: Selecione o mesmo grupo de recursos utilizado para os outros componentes do projeto
Workspace name: Digite um nome descritivo, como "dbw-redundancia-arquivos"
Região: Selecione a mesma região utilizada para o Azure Blob Storage para minimizar a latência
Tipo de preço: Selecione o tipo adequado às suas necessidades:
Standard: Para a maioria dos casos de uso
Premium: Para recursos adicionais de segurança e escala
Trial: Para avaliação (limitado a 14 dias)
Configuração de Rede (opcional):
Você pode configurar o Databricks para usar uma rede virtual existente
Para nosso projeto básico, podemos manter as configurações padrão
Tags (opcional):
Adicione tags relevantes para organização e governança
Exemplo: Projeto=RedundanciaArquivos, Ambiente=Desenvolvimento
Revisão e Criação:
Revise todas as configurações e clique em "Criar"
Aguarde a conclusão da implantação (pode levar alguns minutos)
2. Acesso ao Workspace do Databricks
Navegação até o Recurso:
Após a conclusão da implantação, clique em "Ir para o recurso"
Alternativamente, você pode encontrar o recurso na lista de recursos do grupo de recursos
Lançamento do Workspace:
Clique no botão "Iniciar Workspace" para abrir o ambiente do Databricks
Você será redirecionado para a interface web do Databricks
Faça login com suas credenciais do Azure, se solicitado
3. Criação de um Cluster
Os clusters são os ambientes de computação onde seus notebooks e jobs serão executados:
Acesso à Seção de Clusters:
No menu lateral do Databricks, clique em "Compute"
Você verá a lista de clusters existentes (se houver)
Criação de um Novo Cluster:
Clique no botão "Create Cluster"
Configure as seguintes opções:
Cluster name: Digite um nome descritivo, como "cluster-redundancia"
Cluster mode: Selecione "Standard" para uso geral
Databricks Runtime Version: Selecione a versão mais recente do runtime (recomendamos uma versão com ML se precisar de recursos de machine learning)
Enable autoscaling: Marque esta opção para permitir que o cluster ajuste automaticamente o número de workers
Workers: Defina o mínimo e máximo de workers (por exemplo, 2-8)
Terminate after: Defina um tempo de inatividade após o qual o cluster será automaticamente encerrado (por exemplo, 30 minutos)
Configurações Avançadas (opcional):
Expanda a seção "Advanced Options" para configurar opções adicionais:
Spark Config: Adicione configurações específicas do Spark, se necessário
Environment Variables: Defina variáveis de ambiente para seus notebooks
Init Scripts: Adicione scripts de inicialização para instalar pacotes ou configurar o ambiente
Criação do Cluster:
Clique no botão "Create Cluster" para criar o cluster
Aguarde a inicialização do cluster (pode levar alguns minutos)
O status mudará para "Running" quando o cluster estiver pronto para uso
4. Configuração de Acesso ao Azure Blob Storage
Para que o Databricks possa acessar os dados no Azure Blob Storage, precisamos configurar o acesso:
Usando Montagem de Blob Storage:
A montagem permite acessar o Blob Storage como se fosse um sistema de arquivos local
Crie um notebook para configurar a montagem (detalhado na próxima seção)
Usando Acesso Direto via ABFS:
O Databricks pode acessar diretamente o Blob Storage usando o protocolo ABFS (Azure Blob File System)
Formato da URL: abfss://<container>@<storage-account>.dfs.core.windows.net/<path>
Configuração de Autenticação:
Chave de Conta: Método mais simples, mas menos seguro
Service Principal: Método mais seguro para ambientes de produção
Managed Identity: Método recomendado para integração com outros serviços Azure
5. Criação de Notebooks
Os notebooks são a principal interface para desenvolvimento no Databricks:
Acesso à Seção de Notebooks:
No menu lateral do Databricks, clique em "Workspace"
Navegue até a pasta onde deseja criar o notebook (ou crie uma nova pasta)
Criação de um Novo Notebook:
Clique no menu suspenso "+" e selecione "Notebook"
Configure as seguintes opções:
Name: Digite um nome descritivo, como "Transformacao_Dados"
Language: Selecione a linguagem que deseja usar (Python, SQL, Scala ou R)
Cluster: Selecione o cluster que você criou anteriormente
Desenvolvimento no Notebook:
O notebook é dividido em células, onde você pode escrever e executar código
Cada célula pode ser executada independentemente clicando no botão de execução ou pressionando Shift+Enter
Você pode adicionar células de texto (Markdown) para documentar seu código
Integração com o Azure Data Factory
Para integrar o Azure Databricks com o Azure Data Factory e incluí-lo em nosso fluxo de redundância de arquivos:
Criação de Linked Service para o Databricks:
No Azure Data Factory Studio, vá para "Gerenciar" > "Conexões" > "Linked Services"
Clique em "+ Novo" e selecione "Compute" > "Azure Databricks"
Configure as seguintes opções:
Name: Digite um nome descritivo, como "LS_AzureDatabricks"
Azure subscription: Selecione sua assinatura
Databricks workspace: Selecione o workspace que você criou
Select cluster: Escolha "New job cluster" para criar um cluster específico para o job ou "Existing interactive cluster" para usar um cluster existente
Se escolher "New job cluster", configure as opções do cluster
Se escolher "Existing interactive cluster", selecione o cluster na lista suspensa
Access token: Gere um token de acesso no Databricks e cole-o aqui (detalhado abaixo)
Geração de Token de Acesso no Databricks:
No workspace do Databricks, clique no seu nome de usuário no canto superior direito
Selecione "User Settings"
Vá para a guia "Access Tokens"
Clique em "Generate New Token"
Digite um comentário para identificar o token (por exemplo, "ADF Integration")
Defina um tempo de expiração (ou deixe em branco para nunca expirar, não recomendado para produção)
Clique em "Generate"
Copie o token gerado (ele será exibido apenas uma vez)
Adição de Atividade do Databricks no Pipeline:
No Azure Data Factory Studio, edite um pipeline existente ou crie um novo
Na seção "Atividades", expanda "Databricks" e arraste "Notebook" para a tela de design
Configure as seguintes opções:
General: Digite um nome para a atividade
Azure Databricks: Selecione o Linked Service que você criou
Settings: Selecione o notebook que deseja executar (caminho relativo no workspace)
Base Parameters: Adicione parâmetros para passar ao notebook, se necessário
Timeout: Defina um tempo limite para a execução do notebook
Configuração de Dependências:
Configure a atividade do Databricks para ser executada após as atividades de cópia de dados
Isso garantirá que os dados estejam disponíveis no Blob Storage antes de iniciar as transformações
Considerações de Segurança e Governança
Para garantir a segurança e a governança adequadas:
Controle de Acesso:
Configure o controle de acesso baseado em função (RBAC) no workspace do Databricks
Atribua permissões adequadas aos usuários e grupos
Segredos e Credenciais:
Armazene credenciais e segredos no Azure Key Vault
Configure o Databricks para acessar o Key Vault usando Managed Identity
Auditoria e Monitoramento:
Habilite logs de diagnóstico para o workspace do Databricks
Configure o Azure Monitor para coletar e analisar os logs
Conformidade e Governança de Dados:
Implemente políticas de governança de dados
Configure a linhagem de dados para rastrear a origem e as transformações dos dados
Otimização de Custos
Para otimizar os custos do Azure Databricks:
Dimensionamento Adequado:
Escolha o tipo de VM adequado para suas necessidades
Configure o auto-scaling para ajustar automaticamente o número de workers
Encerramento Automático:
Configure o encerramento automático dos clusters após um período de inatividade
Use clusters de job para workloads agendadas
Reservas e Compromissos:
Considere o uso de Databricks Units (DBUs) reservadas para workloads previsíveis
Avalie os planos de compromisso para obter descontos
Solução de Problemas Comuns
1. Problemas de Conexão
Se o Databricks não conseguir se conectar ao Blob Storage:
Verifique as Credenciais:
Certifique-se de que as chaves de acesso ou tokens estão corretos
Verifique se as permissões são adequadas
Verifique a Rede:
Certifique-se de que o Databricks pode acessar o Blob Storage
Verifique as configurações de firewall e rede virtual
2. Problemas de Desempenho
Se as transformações estiverem lentas:
Otimize o Código:
Revise o código Spark para identificar gargalos
Use técnicas de otimização como particionamento e caching
Ajuste o Cluster:
Aumente o número de workers ou o tamanho das VMs
Configure as opções do Spark para otimizar o desempenho
3. Problemas de Memória
Se ocorrerem erros de falta de memória:
Ajuste as Configurações de Memória:
Aumente a memória alocada para os executores
Configure o garbage collection
Otimize o Processamento:
Processe os dados em lotes menores
Use técnicas como windowing para reduzir o uso de memória
Próximos Passos
Após configurar o Azure Databricks, o próximo passo será desenvolver notebooks para transformação de dados, que permitirão enriquecer e processar os dados armazenados no Azure Blob Storage.
Utilização do Databricks para Transformação de Dados
O Azure Databricks oferece um ambiente poderoso para realizar transformações complexas nos dados armazenados no Azure Blob Storage. Neste guia, vamos detalhar como utilizar o Databricks para transformar e enriquecer os dados do nosso projeto de redundância de arquivos, com exemplos práticos de notebooks e código.
Importância das Transformações de Dados
As transformações de dados são essenciais em nosso projeto de redundância por várias razões:
Limpeza de Dados: Remoção de inconsistências, valores nulos e duplicatas
Enriquecimento: Adição de informações derivadas ou de fontes externas
Padronização: Conversão para formatos consistentes e normalizados
Agregação: Criação de visões resumidas para análise
Preparação para Consumo: Estruturação dos dados para facilitar o uso por aplicações e usuários finais
Pré-requisitos
Antes de iniciar as transformações de dados, certifique-se de que você possui:
Azure Databricks configurado (conforme detalhado no documento anterior)
Cluster Databricks em execução
Acesso configurado ao Azure Blob Storage
Dados carregados nas camadas raw e/ou bronze do Blob Storage
Conhecimento básico de Spark e Python ou Scala
Exemplos Práticos de Transformação de Dados
Vamos explorar exemplos práticos de transformações comuns que podem ser aplicadas aos dados do nosso projeto. Estes exemplos serão apresentados como células de notebook Databricks em Python com PySpark.
1. Configuração Inicial e Montagem do Blob Storage
O primeiro passo é configurar o acesso ao Azure Blob Storage onde os dados estão armazenados:
python
# Configuração de acesso ao Azure Blob Storage
storage_account_name = "stredundanciabastos"
container_name = "bronze"
mount_point = "/mnt/bronze"

# Configuração usando chave de acesso (para desenvolvimento/teste)
storage_account_key = "sua_chave_de_acesso"
source_url = f"wasbs://{container_name}@{storage_account_name}.blob.core.windows.net"
conf_key = f"fs.azure.account.key.{storage_account_name}.blob.core.windows.net"

# Montar o container se ainda não estiver montado
if not any(mount.mountPoint == mount_point for mount in dbutils.fs.mounts()):
    dbutils.fs.mount(
        source = source_url,
        mount_point = mount_point,
        extra_configs = {conf_key: storage_account_key}
    )

print(f"Container {container_name} montado em {mount_point}")
Para ambientes de produção, é recomendável usar Service Principal ou Managed Identity em vez de chaves de acesso:
python
# Configuração usando Service Principal (recomendado para produção)
client_id = "seu_client_id"
tenant_id = "seu_tenant_id"
client_secret = "seu_client_secret"

configs = {
    f"fs.azure.account.auth.type.{storage_account_name}.dfs.core.windows.net": "OAuth",
    f"fs.azure.account.oauth.provider.type.{storage_account_name}.dfs.core.windows.net": "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
    f"fs.azure.account.oauth2.client.id.{storage_account_name}.dfs.core.windows.net": client_id,
    f"fs.azure.account.oauth2.client.secret.{storage_account_name}.dfs.core.windows.net": client_secret,
    f"fs.azure.account.oauth2.client.endpoint.{storage_account_name}.dfs.core.windows.net": f"https://login.microsoftonline.com/{tenant_id}/oauth2/token"
}

# Montar usando Service Principal
if not any(mount.mountPoint == mount_point for mount in dbutils.fs.mounts( )):
    dbutils.fs.mount(
        source = f"abfss://{container_name}@{storage_account_name}.dfs.core.windows.net/",
        mount_point = mount_point,
        extra_configs = configs
    )
2. Leitura de Dados da Camada Bronze
Após montar o Blob Storage, podemos ler os dados da camada bronze:
python
# Leitura de dados da camada bronze
clientes_df = spark.read.format("csv") \
    .option("header", "true") \
    .option("inferSchema", "true") \
    .option("delimiter", ",") \
    .load(f"{mount_point}/clientes/clientes.txt")

# Exibir o esquema e uma amostra dos dados
clientes_df.printSchema()
display(clientes_df.limit(10))
3. Limpeza e Validação de Dados
A limpeza e validação de dados são etapas essenciais para garantir a qualidade:
python
# Importar funções necessárias
from pyspark.sql.functions import col, when, trim, regexp_replace, isnan, isnull

# Limpeza de dados básica
clientes_limpos_df = clientes_df \
    .withColumn("Nome", trim(col("Nome"))) \
    .withColumn("Email", trim(col("Email"))) \
    .withColumn("Telefone", regexp_replace(col("Telefone"), "[^0-9+]", "")) \
    .filter(col("Nome").isNotNull()) \
    .filter(~(col("Email") == ""))

# Validação de e-mails usando expressão regular
from pyspark.sql.functions import regexp_extract

email_pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
clientes_validados_df = clientes_limpos_df \
    .withColumn("EmailValido", 
                when(regexp_extract(col("Email"), email_pattern, 0) != "", True)
                .otherwise(False))

# Exibir estatísticas de validação
from pyspark.sql.functions import count

validacao_stats = clientes_validados_df \
    .groupBy("EmailValido") \
    .agg(count("*").alias("Quantidade"))

display(validacao_stats)
4. Transformações e Enriquecimento
Podemos aplicar transformações mais complexas e enriquecer os dados:
python
# Importar funções adicionais
from pyspark.sql.functions import to_date, datediff, current_date, year, month, dayofmonth, concat, lit

# Transformação de datas
clientes_transformados_df = clientes_validados_df \
    .withColumn("DataCadastro", to_date(col("DataCadastro"), "yyyy-MM-dd")) \
    .withColumn("DiasDesdeRegistro", datediff(current_date(), col("DataCadastro"))) \
    .withColumn("AnoCadastro", year(col("DataCadastro"))) \
    .withColumn("MesCadastro", month(col("DataCadastro"))) \
    .withColumn("DiaCadastro", dayofmonth(col("DataCadastro")))

# Enriquecimento com informações derivadas
clientes_enriquecidos_df = clientes_transformados_df \
    .withColumn("NomeCompleto", 
                when(col("Nome").contains(" "), col("Nome"))
                .otherwise(concat(col("Nome"), lit(" [Sobrenome não informado]")))) \
    .withColumn("CategoriaCliente", 
                when(col("DiasDesdeRegistro") > 365, "Antigo")
                .when(col("DiasDesdeRegistro") > 180, "Regular")
                .otherwise("Novo"))

# Exibir o resultado
display(clientes_enriquecidos_df.limit(10))
5. Junção de Dados de Múltiplas Fontes
Frequentemente, precisamos combinar dados de diferentes tabelas:
python
# Ler dados de pedidos
pedidos_df = spark.read.format("csv") \
    .option("header", "true") \
    .option("inferSchema", "true") \
    .option("delimiter", ",") \
    .load(f"{mount_point}/pedidos/pedidos.txt")

# Junção de clientes com pedidos
clientes_pedidos_df = clientes_enriquecidos_df \
    .join(pedidos_df, clientes_enriquecidos_df.ClienteID == pedidos_df.ClienteID, "left") \
    .select(
        clientes_enriquecidos_df.ClienteID,
        clientes_enriquecidos_df.Nome,
        clientes_enriquecidos_df.Email,
        clientes_enriquecidos_df.CategoriaCliente,
        pedidos_df.PedidoID,
        pedidos_df.DataPedido,
        pedidos_df.ValorTotal
    )

# Exibir o resultado
display(clientes_pedidos_df.limit(10))
6. Agregações e Análises
As agregações permitem criar visões resumidas dos dados:
python
# Importar funções de agregação
from pyspark.sql.functions import sum, avg, count, max, min, round

# Análise de pedidos por cliente
analise_clientes_df = clientes_pedidos_df \
    .groupBy("ClienteID", "Nome", "CategoriaCliente") \
    .agg(
        count("PedidoID").alias("NumeroPedidos"),
        round(sum("ValorTotal"), 2).alias("ValorTotalGasto"),
        round(avg("ValorTotal"), 2).alias("ValorMedioPedido"),
        max("DataPedido").alias("DataUltimoPedido")
    ) \
    .orderBy(col("ValorTotalGasto").desc())

# Exibir o resultado
display(analise_clientes_df.limit(10))
7. Escrita dos Dados Transformados na Camada Silver
Após as transformações, escrevemos os dados na camada silver:
python
# Montar o container silver
silver_mount_point = "/mnt/silver"
silver_container = "silver"

if not any(mount.mountPoint == silver_mount_point for mount in dbutils.fs.mounts()):
    dbutils.fs.mount(
        source = f"wasbs://{silver_container}@{storage_account_name}.blob.core.windows.net",
        mount_point = silver_mount_point,
        extra_configs = {conf_key: storage_account_key}
    )

# Escrever dados transformados na camada silver
clientes_enriquecidos_df.write \
    .format("parquet") \
    .mode("overwrite") \
    .partitionBy("AnoCadastro", "MesCadastro") \
    .save(f"{silver_mount_point}/clientes/")

# Escrever análises na camada silver
analise_clientes_df.write \
    .format("parquet") \
    .mode("overwrite") \
    .save(f"{silver_mount_point}/analises/clientes_analise/")

print("Dados transformados escritos na camada silver com sucesso!")
8. Criação de Tabelas Delta Lake (Opcional)
O Delta Lake é uma camada de armazenamento de código aberto que traz confiabilidade ao data lake:
python
# Importar bibliotecas Delta Lake
from delta.tables import *

# Escrever dados no formato Delta
clientes_enriquecidos_df.write \
    .format("delta") \
    .mode("overwrite") \
    .partitionBy("AnoCadastro", "MesCadastro") \
    .save(f"{silver_mount_point}/delta/clientes/")

# Criar tabela Delta
spark.sql(f"""
CREATE TABLE IF NOT EXISTS clientes_delta
USING DELTA
LOCATION '{silver_mount_point}/delta/clientes/'
""")

# Consultar a tabela Delta
display(spark.sql("SELECT * FROM clientes_delta LIMIT 10"))
9. Otimização de Desempenho
Para melhorar o desempenho das transformações:
python
# Configuração de particionamento
spark.conf.set("spark.sql.shuffle.partitions", 8)  # Ajuste conforme necessário

# Cache de DataFrames frequentemente utilizados
clientes_df.cache()
pedidos_df.cache()

# Otimização de tabelas Delta
spark.sql("OPTIMIZE clientes_delta")
spark.sql("VACUUM clientes_delta RETAIN 168 HOURS")  # 7 dias
Integração entre Data Factory e Databricks
Para integrar completamente o Databricks em nosso fluxo de redundância de arquivos, vamos detalhar como configurar a execução de notebooks Databricks a partir do Azure Data Factory.
1. Exportação do Notebook para o Workspace Databricks
Primeiro, precisamos salvar nosso notebook no workspace do Databricks:
No Databricks, clique no menu "File" do notebook
Selecione "Export" e escolha o formato "Source File"
Salve o arquivo localmente
Alternativamente, você pode criar o notebook diretamente no workspace do Databricks.
2. Criação de Pipeline no Data Factory com Atividade Databricks Notebook
Agora, vamos criar um pipeline no Data Factory que execute o notebook Databricks:
No Azure Data Factory Studio, crie um novo pipeline ou edite um existente
Arraste a atividade "Databricks Notebook" da seção "Databricks" para a tela de design
Configure a atividade:
Nome: "Transform_Data_Databricks"
Linked Service: Selecione o Linked Service do Databricks criado anteriormente
Notebook path: Digite o caminho do notebook no workspace (por exemplo, "/Transformacoes/TransformacaoClientes")
Base Parameters: Adicione parâmetros para passar ao notebook, como:
json
{
  "data_date": "@pipeline().parameters.DataExecucao",
  "source_container": "bronze",
  "target_container": "silver"
}
3. Configuração de Parâmetros no Notebook Databricks
Para que o notebook possa receber parâmetros do Data Factory:
python
# Receber parâmetros do Data Factory
dbutils.widgets.text("data_date", "", "Data de Execução")
dbutils.widgets.text("source_container", "bronze", "Container de Origem")
dbutils.widgets.text("target_container", "silver", "Container de Destino")

# Ler os valores dos parâmetros
data_date = dbutils.widgets.get("data_date")
source_container = dbutils.widgets.get("source_container")
target_container = dbutils.widgets.get("target_container")

print(f"Executando transformação para a data: {data_date}")
print(f"Container de origem: {source_container}")
print(f"Container de destino: {target_container}")

# Usar os parâmetros nas transformações
source_mount_point = f"/mnt/{source_container}"
target_mount_point = f"/mnt/{target_container}"

# Resto do código de transformação...
4. Configuração de Dependências no Pipeline
Para garantir que o notebook Databricks seja executado após a conclusão das atividades de cópia:
Selecione a atividade "Transform_Data_Databricks"
Na guia "Dependências", clique em "Adicionar"
Selecione as atividades de cópia que devem ser concluídas antes da execução do notebook
Defina a condição de dependência como "Sucesso"
5. Configuração de Gatilhos
Para automatizar a execução do pipeline:
Clique no botão "Adicionar gatilho" na barra de ferramentas
Selecione "Novo/Editar"
Configure o gatilho:
Tipo: "Programação"
Recorrência: Defina a frequência (por exemplo, diariamente às 3h da manhã)
Início: Defina a data de início
Fim: Opcional, defina uma data de término se aplicável
Clique em "OK" para salvar o gatilho
6. Monitoramento da Execução
Para monitorar a execução do pipeline e do notebook Databricks:
No Azure Data Factory Studio, clique em "Monitor" no menu à esquerda
Selecione "Execuções de pipeline" para ver o status das execuções
Clique em uma execução para ver detalhes
Para a atividade Databricks, você pode clicar no link "Output" para ver os logs de execução
Você também pode acessar o Databricks diretamente para ver logs mais detalhados e resultados da execução
Melhores Práticas para Transformação de Dados com Databricks
1. Estruturação de Código
Para manter o código organizado e reutilizável:
Modularização:
Divida o código em funções e módulos
Crie bibliotecas compartilhadas para funcionalidades comuns
Documentação:
Documente o código com comentários claros
Use células Markdown para explicar o propósito e a lógica
Controle de Versão:
Integre o Databricks com o Git para controle de versão
Mantenha um histórico de alterações
2. Otimização de Desempenho
Para melhorar o desempenho das transformações:
Particionamento:
Particione os dados de forma adequada
Use repartition() ou coalesce() para ajustar o número de partições
Caching:
Use cache() ou persist() para DataFrames utilizados múltiplas vezes
Lembre-se de usar unpersist() quando não precisar mais dos dados
Broadcast Joins:
Use broadcast joins para tabelas pequenas
Exemplo: df1.join(broadcast(df2), "key")
3. Tratamento de Erros e Logging
Para garantir a robustez das transformações:
Tratamento de Exceções:
Implemente try/except para capturar e tratar erros
Registre informações detalhadas sobre exceções
Logging:
Implemente logging detalhado
Registre métricas importantes como contagens, tempos de execução, etc.
Validação de Dados:
Implemente verificações de qualidade de dados
Registre métricas de qualidade para monitoramento
4. Segurança e Governança
Para garantir a segurança e a governança dos dados:
Mascaramento de Dados Sensíveis:
Implemente técnicas de mascaramento para dados sensíveis
Use bibliotecas como PySpark Crypto para criptografia
Controle de Acesso:
Implemente controle de acesso granular
Use ACLs para restringir o acesso a notebooks e dados
Linhagem de Dados:
Registre a linhagem dos dados para rastreabilidade
Documente as transformações aplicadas
Solução de Problemas Comuns
1. Problemas de Memória
Se ocorrerem erros de falta de memória:
Ajuste as Configurações de Memória:
Aumente a memória dos executores
Configure o garbage collection
Otimize o Processamento:
Processe os dados em lotes menores
Use técnicas como windowing para reduzir o uso de memória
2. Problemas de Desempenho
Se as transformações estiverem lentas:
Analise o Plano de Execução:
Use explain() para analisar o plano de execução
Identifique e otimize operações caras
Monitore a UI do Spark:
Acesse a UI do Spark para identificar gargalos
Analise a distribuição de tarefas e o uso de recursos
3. Problemas de Integração
Se houver problemas na integração com o Data Factory:
Verifique os Parâmetros:
Certifique-se de que os parâmetros estão sendo passados corretamente
Verifique se o notebook está processando os parâmetros corretamente
Verifique o Linked Service:
Certifique-se de que o token de acesso é válido
Verifique se o cluster está em execução
Conclusão
O Azure Databricks é uma ferramenta poderosa para transformação de dados em nosso projeto de redundância de arquivos. Com os exemplos e práticas detalhados neste guia, você pode implementar transformações complexas e integrá-las ao fluxo de trabalho do Azure Data Factory, garantindo que os dados sejam processados de forma eficiente e confiável.
Próximos Passos
Após implementar as transformações de dados com Databricks, o próximo passo será validar e executar os pipelines completos, aplicando boas práticas de segurança e monitoramento para garantir o funcionamento adequado do sistema de redundância de arquivos.
Validação dos Pipelines
A validação dos pipelines é uma etapa crucial no desenvolvimento de soluções de integração de dados, garantindo que todos os componentes funcionem corretamente antes da implementação em produção. Neste guia, vamos detalhar o processo de validação dos pipelines no Azure Data Factory para nosso projeto de redundância de arquivos.
Importância da Validação de Pipelines
A validação adequada dos pipelines oferece diversos benefícios:
Detecção Precoce de Problemas: Identifica erros e inconsistências antes da implementação em produção
Garantia de Qualidade: Assegura que os dados sejam processados corretamente
Redução de Riscos: Minimiza o risco de falhas em ambiente de produção
Otimização de Recursos: Permite ajustar configurações para melhor desempenho
Documentação: Fornece evidências de que o sistema funciona conforme esperado
Tipos de Validação para Pipelines
1. Validação Sintática
A validação sintática verifica se o pipeline está estruturalmente correto e se todos os parâmetros necessários estão configurados:
Validação Automática do Azure Data Factory:
O Azure Data Factory realiza validação sintática automática durante o desenvolvimento
Erros e avisos são exibidos na interface do usuário
Verificação Manual:
Revisão das configurações de cada atividade
Verificação de parâmetros obrigatórios
Confirmação de que todas as dependências estão corretamente configuradas
2. Validação Funcional
A validação funcional verifica se o pipeline executa as operações esperadas e produz os resultados corretos:
Execução em Ambiente de Desenvolvimento:
Execução do pipeline com dados de teste
Verificação dos resultados em cada etapa
Confirmação de que os dados são transformados corretamente
Testes de Integração:
Verificação da integração entre diferentes componentes
Teste do fluxo completo de dados, desde a origem até o destino
3. Validação de Desempenho
A validação de desempenho avalia a eficiência e a escalabilidade do pipeline:
Medição de Tempos de Execução:
Registro do tempo total de execução
Identificação de gargalos e atividades demoradas
Teste com Volumes Variados:
Execução com pequenos volumes de dados
Execução com volumes maiores para testar escalabilidade
4. Validação de Recuperação
A validação de recuperação verifica como o pipeline se comporta em caso de falhas:
Testes de Falha:
Simulação de falhas em diferentes componentes
Verificação do comportamento de recuperação
Verificação de Logs e Alertas:
Confirmação de que falhas são registradas adequadamente
Verificação de que alertas são gerados conforme esperado
Processo de Validação dos Pipelines no Azure Data Factory
1. Preparação para Validação
Antes de iniciar a validação, é importante preparar o ambiente:
Configuração de Ambiente de Teste:
Crie contêineres separados para teste no Azure Blob Storage
Prepare dados de teste representativos
Definição de Critérios de Aceitação:
Defina claramente o que constitui um teste bem-sucedido
Estabeleça métricas para avaliar o desempenho
Preparação de Ferramentas de Monitoramento:
Configure o Azure Monitor para coletar logs
Prepare consultas para analisar logs e métricas
2. Validação Sintática no Azure Data Factory
O Azure Data Factory oferece ferramentas integradas para validação sintática:
Uso do Botão "Validar":
No Azure Data Factory Studio, clique no botão "Validar" na barra de ferramentas
Revise os erros e avisos exibidos
Corrija os problemas identificados
Botão Validar no ADF
Verificação de Configurações:
Verifique se todos os Linked Services estão configurados corretamente
Confirme se todos os Datasets têm as configurações corretas
Verifique se todas as atividades têm os parâmetros necessários
Validação de Expressões:
Teste expressões dinâmicas usando a ferramenta de expressão do Data Factory
Verifique se as expressões retornam os valores esperados
3. Validação Funcional dos Pipelines
Para validar funcionalmente os pipelines:
Execução Manual:
Clique no botão "Adicionar gatilho" na barra de ferramentas
Selecione "Acionar agora"
Se o pipeline tiver parâmetros, preencha os valores para teste
Clique em "OK" para iniciar a execução
Monitoramento da Execução:
Clique em "Monitor" no menu à esquerda
Selecione "Execuções de pipeline" para ver o status da execução
Clique na execução para ver detalhes
Verifique o status de cada atividade
Verificação dos Resultados:
Após a conclusão da execução, verifique os dados no destino
Confirme se os dados foram copiados e transformados corretamente
Verifique a integridade e a consistência dos dados
Validação de Cada Componente:
a. Validação da Cópia de Dados:
Verifique se todos os registros foram copiados
Compare contagens de registros entre origem e destino
Verifique se os tipos de dados foram preservados corretamente
b. Validação da Conversão para TXT:
Abra os arquivos TXT gerados
Verifique se o formato está correto (delimitadores, cabeçalhos, etc.)
Confirme se caracteres especiais são tratados corretamente
c. Validação da Organização em Camadas:
Verifique se os arquivos estão nos contêineres e pastas corretos
Confirme se a estrutura de camadas (raw, bronze, etc.) está correta
Verifique se as permissões de acesso estão configuradas adequadamente
d. Validação das Transformações Databricks:
Verifique os logs de execução do notebook Databricks
Confirme se as transformações foram aplicadas corretamente
Verifique se os dados transformados estão no formato esperado
4. Validação de Cenários Específicos
É importante validar cenários específicos que podem ocorrer em produção:
Cenário de Dados Incrementais:
Teste a cópia incremental de dados
Verifique se apenas os novos registros são processados
Confirme que não há duplicação de dados
Cenário de Reprocessamento:
Teste o reprocessamento de dados existentes
Verifique se os dados são substituídos ou anexados conforme configurado
Confirme que o histórico é mantido quando necessário
Cenário de Falha e Recuperação:
Simule falhas em diferentes componentes
Verifique se o pipeline falha graciosamente
Teste a recuperação após a correção do problema
5. Documentação dos Resultados da Validação
Documente os resultados da validação para referência futura:
Registro de Testes:
Documente cada teste realizado
Registre os resultados obtidos
Anote quaisquer problemas encontrados e suas soluções
Evidências de Validação:
Capture screenshots das execuções bem-sucedidas
Salve logs de execução
Documente métricas de desempenho
Relatório de Validação:
Crie um relatório resumindo os resultados da validação
Destaque áreas que precisam de atenção
Inclua recomendações para melhorias
Ferramentas e Técnicas para Validação
1. Uso de Consultas SQL para Validação
Para validar dados em bancos de dados:
sql
-- Verificar contagem de registros
SELECT COUNT(*) FROM Clientes;

-- Verificar integridade referencial
SELECT COUNT(*) FROM Pedidos WHERE ClienteID NOT IN (SELECT ClienteID FROM Clientes);

-- Verificar valores nulos em campos críticos
SELECT COUNT(*) FROM Clientes WHERE Nome IS NULL OR Email IS NULL;
2. Uso de Scripts PowerShell para Validação
Para validar arquivos no Blob Storage:
powershell
# Conectar ao Azure
Connect-AzAccount

# Listar arquivos no contêiner
Get-AzStorageBlob -Container "raw" -Context $storageContext | Select-Object Name, Length, LastModified

# Verificar contagem de arquivos
$fileCount = (Get-AzStorageBlob -Container "raw" -Context $storageContext).Count
Write-Output "Número de arquivos: $fileCount"
3. Uso de Notebooks Databricks para Validação
Para validar dados processados pelo Databricks:
python
# Verificar contagem de registros
raw_count = spark.read.format("csv").option("header", "true").load("/mnt/raw/clientes/").count()
silver_count = spark.read.format("parquet").load("/mnt/silver/clientes/").count()

print(f"Contagem na camada raw: {raw_count}")
print(f"Contagem na camada silver: {silver_count}")
print(f"Diferença: {raw_count - silver_count}")

# Verificar esquema
silver_df = spark.read.format("parquet").load("/mnt/silver/clientes/")
silver_df.printSchema()

# Verificar estatísticas básicas
silver_df.describe().show()
Solução de Problemas Comuns
1. Problemas de Conexão
Se ocorrerem falhas de conexão durante a validação:
Verifique os Linked Services:
Teste a conexão de cada Linked Service
Verifique se as credenciais estão corretas e não expiraram
Confirme se os recursos de destino estão acessíveis
Verifique o Integration Runtime:
Certifique-se de que o Integration Runtime está em execução
Verifique os logs do Integration Runtime para identificar problemas
Reinicie o Integration Runtime se necessário
2. Problemas de Transformação
Se os dados não forem transformados corretamente:
Verifique os Mapeamentos:
Revise os mapeamentos de colunas nas atividades de cópia
Confirme se as transformações estão configuradas corretamente
Verifique se os tipos de dados são compatíveis
Verifique os Scripts de Transformação:
Revise o código nos notebooks Databricks
Execute o código passo a passo para identificar problemas
Verifique se as bibliotecas necessárias estão instaladas
3. Problemas de Desempenho
Se o desempenho for insatisfatório:
Analise os Tempos de Execução:
Identifique as atividades que consomem mais tempo
Verifique se há gargalos de rede ou processamento
Otimize as Configurações:
Ajuste o paralelismo nas atividades de cópia
Configure o particionamento adequado
Considere aumentar os recursos dos clusters Databricks
Próximos Passos
Após a validação bem-sucedida dos pipelines, o próximo passo será a publicação e execução dos pipelines em ambiente de produção, seguido pela análise de desempenho e aplicação de boas práticas de segurança.
Publicação e Execução dos Pipelines
Após a validação bem-sucedida dos pipelines, o próximo passo é publicá-los e executá-los em ambiente de produção. Neste guia, vamos detalhar o processo de publicação e execução dos pipelines no Azure Data Factory para nosso projeto de redundância de arquivos.
Importância da Publicação e Execução Controlada
A publicação e execução controlada dos pipelines oferecem diversos benefícios:
Controle de Versão: Mantém um histórico das alterações realizadas
Ambiente Isolado: Separa o desenvolvimento da produção
Execução Programada: Permite automatizar a execução dos pipelines
Monitoramento Centralizado: Facilita o acompanhamento das execuções
Recuperação de Falhas: Permite implementar estratégias de recuperação
Processo de Publicação dos Pipelines
1. Preparação para Publicação
Antes de publicar os pipelines, é importante garantir que tudo esteja pronto:
Revisão Final:
Verifique se todos os componentes foram validados
Confirme que todas as dependências estão configuradas corretamente
Certifique-se de que todos os parâmetros estão definidos adequadamente
Documentação:
Atualize a documentação com as últimas alterações
Documente os parâmetros e configurações dos pipelines
Prepare documentação operacional para a equipe de suporte
Backup:
Se estiver atualizando pipelines existentes, faça um backup da versão atual
Exporte as definições dos recursos para JSON, se necessário
2. Publicação no Azure Data Factory
O Azure Data Factory utiliza um modelo de publicação que separa o ambiente de desenvolvimento do ambiente de produção:
Acesso ao Azure Data Factory Studio:
Acesse o Portal Azure através do endereço https://portal.azure.com
Navegue até o seu Azure Data Factory (adf-dio-bastos)
Clique em "Autor e Monitor" para abrir o Azure Data Factory Studio
Verificação de Alterações Pendentes:
No Azure Data Factory Studio, verifique se há alterações pendentes
Clique no botão "Publicar" na barra de ferramentas
Revise a lista de alterações que serão publicadas
Botão Publicar no ADF
Publicação das Alterações:
Após revisar as alterações, clique em "Publicar" para confirmar
Aguarde a conclusão do processo de publicação
Verifique se não houve erros durante a publicação
Verificação Pós-Publicação:
Navegue até a seção "Monitor" para verificar se os pipelines publicados estão visíveis
Confirme que todos os recursos (Linked Services, Datasets, etc.) foram publicados corretamente
3. Integração com Controle de Versão (Opcional)
Para projetos mais complexos, é recomendável integrar o Azure Data Factory com um sistema de controle de versão:
Configuração do Repositório Git:
No Azure Data Factory Studio, clique em "Configurar Repositório de Código"
Selecione o tipo de repositório (Azure DevOps Git ou GitHub)
Configure as informações do repositório:
Repositório: Selecione ou crie um repositório
Branch de colaboração: Geralmente "main" ou "master"
Pasta raiz: Pasta onde os recursos serão armazenados
Importar recursos existentes: Selecione "Importar"
Fluxo de Trabalho com Git:
Crie uma branch de recurso para desenvolvimento
Faça alterações e teste na branch de recurso
Crie um pull request para mesclar as alterações na branch principal
Após aprovação, mescle o pull request
Publique as alterações da branch principal para o ambiente de produção
Execução dos Pipelines
1. Métodos de Execução
Existem várias maneiras de executar pipelines no Azure Data Factory:
Execução Manual:
Útil para testes e execuções ad-hoc
No Azure Data Factory Studio, navegue até "Autor" > "Pipelines"
Selecione o pipeline desejado
Clique em "Adicionar gatilho" > "Acionar agora"
Preencha os parâmetros, se necessário, e clique em "OK"
Execução Programada:
Ideal para operações regulares e recorrentes
Configure um gatilho de programação:
No pipeline, clique em "Adicionar gatilho" > "Novo/Editar"
Selecione "Programação" como tipo de gatilho
Configure a recorrência (por exemplo, diariamente às 2h da manhã)
Defina a data de início e, opcionalmente, a data de término
Configure os parâmetros do pipeline, se necessário
Execução Baseada em Eventos:
Útil para processar dados assim que estiverem disponíveis
Configure um gatilho de evento:
No pipeline, clique em "Adicionar gatilho" > "Novo/Editar"
Selecione "Evento" como tipo de gatilho
Configure o serviço de eventos (por exemplo, Blob Storage)
Defina as condições que acionarão o pipeline
Execução em Cascata:
Para pipelines que dependem da conclusão de outros
Configure dependências entre pipelines:
Crie um pipeline "mestre" que orquestra a execução
Use a atividade "Execute Pipeline" para chamar outros pipelines
Configure as dependências entre as atividades
2. Configuração de Gatilhos para Execução Automática
Para nosso projeto de redundância de arquivos, vamos configurar gatilhos programados:
Gatilho para Cópia de Dados:
No pipeline de cópia de dados, clique em "Adicionar gatilho" > "Novo/Editar"
Configure as seguintes opções:
Tipo: "Programação"
Recorrência: Diária
Hora: 01:00 (ou outro horário de baixo uso)
Fuso horário: Selecione o fuso horário apropriado
Data de início: Defina a data de início da execução
Data de término: Opcional, deixe em branco para execução contínua
Gatilho para Transformação de Dados:
No pipeline de transformação, configure um gatilho similar
Programe para iniciar após a conclusão esperada do pipeline de cópia
Por exemplo, se o pipeline de cópia leva 1 hora, programe para 02:00
Ativação dos Gatilhos:
Após configurar os gatilhos, eles precisam ser ativados
Clique no botão "Publicar" para publicar as alterações
Os gatilhos serão ativados automaticamente após a publicação
3. Monitoramento da Execução
O monitoramento é essencial para garantir que os pipelines estejam funcionando corretamente:
Monitoramento em Tempo Real:
No Azure Data Factory Studio, clique em "Monitor" no menu à esquerda
Selecione "Execuções de pipeline" para ver as execuções recentes
Filtre por status, período de tempo ou nome do pipeline
Clique em uma execução para ver detalhes
Detalhes da Execução:
Na visualização detalhada, você pode ver:
Status geral do pipeline
Duração da execução
Status de cada atividade
Logs de execução
Parâmetros utilizados
Visualização Gráfica:
Clique na guia "Visualização" para ver uma representação gráfica da execução
As atividades são coloridas de acordo com o status:
Verde: Sucesso
Vermelho: Falha
Azul: Em andamento
Cinza: Pendente
Logs de Execução:
Para cada atividade, você pode acessar os logs detalhados
Clique no ícone de "Output" ao lado da atividade
Analise os logs para identificar problemas ou confirmar o sucesso
4. Tratamento de Falhas
É importante implementar estratégias para lidar com falhas de execução:
Configuração de Retry:
Configure políticas de retry para atividades que podem falhar temporariamente
Nas propriedades da atividade, expanda "Configurações de falha"
Configure o número de tentativas e o intervalo entre elas
Notificações de Falha:
Configure alertas para ser notificado sobre falhas
No Portal Azure, navegue até o recurso do Data Factory
Selecione "Alertas" e configure regras de alerta para falhas de pipeline
Recuperação Manual:
Em caso de falha que não pode ser resolvida automaticamente:
Identifique a causa da falha nos logs
Corrija o problema
Reinicie a execução do pipeline a partir do ponto de falha, se possível
Pipelines de Recuperação:
Para cenários críticos, crie pipelines específicos de recuperação
Esses pipelines podem ser acionados manualmente após a resolução de problemas
Eles devem ser projetados para retomar o processamento a partir do ponto de falha
Implementação de Execução para o Projeto de Redundância
Para nosso projeto específico de redundância de arquivos, vamos implementar a seguinte estratégia de execução:
1. Pipeline Principal de Redundância
Configuração do Pipeline Mestre:
Crie um pipeline "PL_Redundancia_Master" que orquestra todo o processo
Use atividades "Execute Pipeline" para chamar os pipelines específicos:
Pipeline de cópia de dados on-premises para raw
Pipeline de transformação de raw para bronze
Pipeline de processamento Databricks (opcional)
Configuração de Dependências:
Configure as atividades para executar em sequência
Adicione condições para verificar o sucesso de cada etapa
Parametrização:
Adicione parâmetros para controlar a execução:
Data de execução
Modo de execução (completo ou incremental)
Tabelas a serem processadas
2. Programação de Execução
Execução Diária Completa:
Configure um gatilho para executar o pipeline completo diariamente
Programe para um horário de baixo uso (por exemplo, 01:00)
Execução Incremental Frequente (opcional):
Configure um gatilho adicional para execução incremental mais frequente
Por exemplo, a cada 4 horas para dados críticos
Execução de Fim de Semana (opcional):
Configure uma execução especial para o fim de semana
Esta execução pode incluir tarefas adicionais, como limpeza ou manutenção
3. Monitoramento Específico
Dashboard de Monitoramento:
Crie um dashboard no Azure para monitorar as execuções
Inclua métricas como:
Taxa de sucesso dos pipelines
Tempo de execução
Volume de dados processados
Alertas Personalizados:
Configure alertas específicos para o projeto:
Alerta se o tempo de execução exceder um limite
Alerta se o volume de dados estiver abaixo do esperado
Alerta para falhas em componentes críticos
Melhores Práticas para Publicação e Execução
1. Versionamento e Documentação
Controle de Versão:
Mantenha um histórico claro de versões
Documente as alterações em cada versão
Use tags ou branches para marcar versões estáveis
Documentação Operacional:
Crie documentação detalhada para operações
Inclua procedimentos para situações comuns:
Como reiniciar pipelines falhos
Como verificar o status da execução
Como identificar e resolver problemas comuns
2. Estratégia de Implantação
Implantação Gradual:
Implemente alterações gradualmente
Comece com um subconjunto de dados antes de aplicar a todos
Janelas de Manutenção:
Defina janelas de manutenção para atualizações
Comunique as janelas de manutenção aos usuários
Rollback Plan:
Tenha sempre um plano de rollback
Documente os passos para reverter para a versão anterior
3. Otimização de Execução
Balanceamento de Carga:
Distribua as execuções ao longo do tempo
Evite executar todos os pipelines simultaneamente
Priorização:
Priorize pipelines críticos
Configure recursos adequados para cada pipeline
Limpeza Regular:
Implemente rotinas de limpeza para dados temporários
Monitore o uso de armazenamento e outros recursos
Solução de Problemas Comuns
1. Falhas na Publicação
Se ocorrerem problemas durante a publicação:
Verificação de Dependências:
Certifique-se de que todos os recursos referenciados existem
Verifique se não há referências circulares
Limpeza de Cache:
Tente limpar o cache do navegador
Feche e reabra o Azure Data Factory Studio
Publicação Parcial:
Se possível, publique os recursos em lotes menores
Identifique quais recursos estão causando problemas
2. Falhas na Execução
Se os pipelines falharem durante a execução:
Análise de Logs:
Examine os logs detalhados da atividade que falhou
Identifique a causa raiz do problema
Verificação de Recursos:
Certifique-se de que todos os recursos externos estão disponíveis
Verifique se há limites de recursos atingidos
Execução Isolada:
Execute a atividade problemática isoladamente
Teste com um subconjunto menor de dados
Próximos Passos
Após a publicação e configuração da execução dos pipelines, o próximo passo será analisar o desempenho das execuções e implementar boas práticas de segurança para garantir a proteção dos dados e a eficiência do sistema de redundância de arquivos.
Análise de Performance das Execuções
A análise de performance das execuções é fundamental para garantir a eficiência e a escalabilidade do sistema de redundância de arquivos. Neste guia, vamos detalhar como analisar o desempenho das execuções dos pipelines no Azure Data Factory e identificar oportunidades de otimização.
Importância da Análise de Performance
A análise de performance oferece diversos benefícios:
Identificação de Gargalos: Localiza pontos de estrangulamento que afetam o desempenho
Otimização de Recursos: Permite alocar recursos de forma mais eficiente
Previsibilidade: Ajuda a estimar tempos de execução para planejamento
Redução de Custos: Identifica oportunidades para reduzir custos operacionais
Escalabilidade: Garante que o sistema possa lidar com volumes crescentes de dados
Métricas Importantes para Análise de Performance
1. Métricas de Tempo
As métricas de tempo são essenciais para entender a duração das execuções:
Tempo Total de Execução:
Duração total do pipeline do início ao fim
Tendência ao longo do tempo (aumentando ou diminuindo)
Tempo por Atividade:
Duração de cada atividade individual
Identificação das atividades mais demoradas
Tempo de Espera:
Tempo gasto esperando recursos ou dependências
Intervalos entre atividades
2. Métricas de Volume
As métricas de volume ajudam a entender a quantidade de dados processados:
Quantidade de Dados Processados:
Volume total de dados lidos e escritos
Tamanho dos arquivos gerados
Taxa de Transferência:
Velocidade de processamento (MB/s ou GB/h)
Comparação com benchmarks ou execuções anteriores
Contagem de Registros:
Número de registros processados
Registros por segundo
3. Métricas de Recursos
As métricas de recursos mostram a utilização de infraestrutura:
Utilização de CPU:
Percentual de CPU utilizado durante a execução
Picos de utilização
Utilização de Memória:
Consumo de memória durante a execução
Ocorrências de pressão de memória
Utilização de Rede:
Largura de banda consumida
Latência de rede
4. Métricas de Confiabilidade
As métricas de confiabilidade mostram a estabilidade das execuções:
Taxa de Sucesso:
Percentual de execuções bem-sucedidas
Tendência de falhas ao longo do tempo
Tipos de Falhas:
Categorização das falhas (rede, timeout, permissões, etc.)
Frequência de cada tipo de falha
Tempo Médio Entre Falhas (MTBF):
Intervalo médio entre ocorrências de falhas
Estabilidade do sistema ao longo do tempo
Ferramentas para Análise de Performance no Azure
1. Azure Data Factory Monitor
O Azure Data Factory oferece ferramentas integradas para monitoramento:
Visualização de Execuções de Pipeline:
No Azure Data Factory Studio, clique em "Monitor" no menu à esquerda
Selecione "Execuções de pipeline" para ver o histórico de execuções
Analise a duração e o status de cada execução
Visualização Detalhada de Atividades:
Clique em uma execução específica para ver detalhes
Analise a duração de cada atividade
Identifique atividades que consomem mais tempo
Exportação de Dados de Monitoramento:
Clique no botão "Baixar" para exportar os dados de monitoramento
Analise os dados em ferramentas externas como Excel ou Power BI
2. Azure Monitor e Log Analytics
Para análise mais avançada, utilize o Azure Monitor e Log Analytics:
Configuração de Diagnóstico:
No Portal Azure, navegue até o recurso do Data Factory
Selecione "Configurações de diagnóstico" no menu lateral
Clique em "Adicionar configuração de diagnóstico"
Selecione os logs e métricas desejados
Escolha o destino (Log Analytics, Storage Account, Event Hub)
Consultas no Log Analytics:
Acesse o workspace do Log Analytics
Use consultas KQL (Kusto Query Language) para analisar os dados
Exemplo de consulta para analisar duração de pipelines:
kusto
ADFPipelineRun
| where TimeGenerated > ago(7d)
| project PipelineName, RunId, Status, Duration = End - Start
| summarize AvgDuration = avg(Duration), MaxDuration = max(Duration), MinDuration = min(Duration) by PipelineName
| order by AvgDuration desc
Criação de Dashboards:
Crie dashboards personalizados no Azure
Adicione gráficos e visualizações baseados em consultas
Configure atualizações automáticas
3. Azure Application Insights (para Databricks)
Para monitorar o desempenho do Databricks:
Integração com Application Insights:
Configure o Databricks para enviar telemetria para o Application Insights
Use a biblioteca OpenCensus para instrumentar o código
Análise de Desempenho:
Monitore o tempo de execução de células e notebooks
Analise o uso de recursos do cluster
Identifique operações demoradas
Processo de Análise de Performance
1. Estabelecimento de Linha de Base
Antes de otimizar, estabeleça uma linha de base de desempenho:
Coleta de Dados Iniciais:
Execute os pipelines várias vezes em condições normais
Registre métricas de desempenho para cada execução
Calcule médias e desvios padrão
Definição de KPIs:
Identifique os indicadores-chave de desempenho (KPIs)
Estabeleça valores de referência para cada KPI
Defina metas de melhoria
Documentação da Linha de Base:
Documente as métricas iniciais
Crie gráficos para visualização
Estabeleça um ponto de comparação para futuras otimizações
2. Identificação de Gargalos
Com a linha de base estabelecida, identifique os gargalos:
Análise de Tempos de Execução:
Identifique as atividades que consomem mais tempo
Verifique se há padrões ou tendências
Procure variações significativas entre execuções
Análise de Recursos:
Verifique se há saturação de recursos (CPU, memória, rede)
Identifique períodos de baixa utilização
Procure correlações entre uso de recursos e desempenho
Análise de Dependências:
Verifique se há esperas desnecessárias entre atividades
Identifique dependências que podem ser executadas em paralelo
Procure oportunidades para otimizar o fluxo de execução
3. Implementação de Otimizações
Com base na análise, implemente otimizações:
Otimizações de Configuração:
Ajuste parâmetros de configuração (tamanho de lote, grau de paralelismo, etc.)
Otimize configurações de recursos (tamanho de cluster, número de workers, etc.)
Ajuste timeouts e políticas de retry
Otimizações de Código:
Refatore código ineficiente em notebooks Databricks
Otimize consultas SQL
Implemente técnicas de otimização específicas (particionamento, caching, etc.)
Otimizações de Arquitetura:
Reorganize o fluxo de atividades para maximizar o paralelismo
Divida pipelines grandes em pipelines menores e mais gerenciáveis
Considere arquiteturas alternativas para componentes problemáticos
4. Medição e Iteração
Após implementar otimizações, meça os resultados e itere:
Coleta de Dados Pós-Otimização:
Execute os pipelines otimizados várias vezes
Registre as mesmas métricas coletadas na linha de base
Compare com os valores de referência
Análise de Impacto:
Calcule a melhoria percentual em cada KPI
Verifique se as metas de otimização foram atingidas
Identifique efeitos colaterais inesperados
Iteração:
Se necessário, implemente otimizações adicionais
Repita o ciclo de medição e análise
Documente as lições aprendidas
Otimizações Específicas para o Projeto de Redundância
Para nosso projeto específico de redundância de arquivos, considere as seguintes otimizações:
1. Otimizações de Cópia de Dados
Para melhorar o desempenho da cópia de dados:
Ajuste do Grau de Paralelismo:
Na atividade de cópia, ajuste o "Grau de cópia paralela"
Comece com 4 e aumente gradualmente, monitorando o impacto
O valor ideal depende dos recursos disponíveis e da natureza dos dados
Otimização do Tamanho do Lote:
Ajuste o "Tamanho do lote" na atividade de cópia
Valores maiores geralmente melhoram o desempenho, mas aumentam o uso de memória
Teste diferentes valores (1000, 5000, 10000) e monitore o impacto
Compressão de Dados:
Habilite a compressão para reduzir o volume de dados transferidos
Na atividade de cópia, expanda "Configurações" e habilite a compressão
Selecione um formato de compressão adequado (GZip, Deflate, etc.)
2. Otimizações do Databricks
Para melhorar o desempenho das transformações no Databricks:
Dimensionamento do Cluster:
Ajuste o tamanho e o número de workers no cluster
Monitore a utilização de recursos durante a execução
Considere o uso de auto-scaling para ajustar dinamicamente o tamanho do cluster
Otimização de Código Spark:
Use broadcast joins para tabelas pequenas
Particione adequadamente os dados
Utilize caching para DataFrames frequentemente acessados
Monitore e ajuste o número de partições
Configurações do Spark:
Ajuste configurações como spark.sql.shuffle.partitions
Otimize a alocação de memória para executores
Configure o garbage collection adequadamente
3. Otimizações de Armazenamento
Para otimizar o armazenamento e acesso aos dados:
Formato de Arquivo:
Considere usar formatos colunar como Parquet para as camadas superiores
Esses formatos oferecem melhor desempenho de consulta e compressão
Estratégia de Particionamento:
Particione os dados de forma adequada (por data, região, etc.)
Isso melhora o desempenho de consultas e facilita o processamento incremental
Políticas de Ciclo de Vida:
Implemente políticas de ciclo de vida no Blob Storage
Mova dados mais antigos para níveis de armazenamento mais econômicos
Archive ou exclua dados que não são mais necessários
Ferramentas e Técnicas para Análise Avançada
1. Análise de Tendências
Para identificar tendências de desempenho ao longo do tempo:
Gráficos de Séries Temporais:
Crie gráficos mostrando métricas-chave ao longo do tempo
Identifique padrões sazonais ou tendências de degradação
Análise de Correlação:
Correlacione métricas de desempenho com fatores externos
Por exemplo, volume de dados, hora do dia, dia da semana, etc.
Previsão:
Use técnicas de previsão para antecipar problemas de desempenho
Implemente alertas proativos baseados em tendências
2. Análise Comparativa (Benchmarking)
Para comparar o desempenho com referências:
Comparação Interna:
Compare o desempenho entre diferentes pipelines
Compare o desempenho antes e depois de otimizações
Comparação Externa:
Compare com benchmarks da indústria, quando disponíveis
Compare com implementações similares em outros projetos
Análise de Gap:
Identifique a diferença entre o desempenho atual e o desejado
Priorize otimizações com base no tamanho do gap
3. Simulação e Teste de Carga
Para entender o comportamento sob diferentes condições:
Testes com Volumes Variados:
Execute testes com diferentes volumes de dados
Analise como o desempenho escala com o aumento do volume
Testes de Concorrência:
Execute múltiplos pipelines simultaneamente
Verifique o impacto da concorrência no desempenho
Testes de Recuperação:
Simule falhas e analise o comportamento de recuperação
Meça o tempo de recuperação após diferentes tipos de falha
Documentação e Comunicação dos Resultados
1. Relatórios de Performance
Documente os resultados da análise de performance:
Relatórios Regulares:
Crie relatórios periódicos (semanais, mensais)
Inclua métricas-chave, tendências e insights
Dashboards:
Desenvolva dashboards para visualização em tempo real
Torne os dashboards acessíveis às partes interessadas
Alertas:
Configure alertas para notificar sobre problemas de desempenho
Defina limiares baseados na análise histórica
2. Recomendações de Otimização
Documente e comunique recomendações:
Plano de Otimização:
Crie um plano detalhando as otimizações recomendadas
Priorize as otimizações com base no impacto esperado
Análise de Custo-Benefício:
Estime o custo e o benefício de cada otimização
Considere tanto custos financeiros quanto operacionais
Roadmap de Implementação:
Desenvolva um cronograma para implementação das otimizações
Defina marcos e métricas de sucesso
Solução de Problemas Comuns de Performance
1. Lentidão na Cópia de Dados
Se a cópia de dados estiver lenta:
Verificação de Rede:
Meça a largura de banda disponível
Verifique se há limitações de rede
Considere o uso de ExpressRoute para conexões mais rápidas
Verificação de Origem e Destino:
Verifique o desempenho do banco de dados de origem
Verifique a capacidade de E/S do armazenamento de destino
Considere escalar recursos se necessário
Otimização de Consultas:
Revise as consultas SQL utilizadas
Adicione índices apropriados no banco de dados
Filtre dados na origem para reduzir o volume transferido
2. Problemas de Desempenho no Databricks
Se as transformações no Databricks estiverem lentas:
Análise do Plano de Execução:
Use explain() para analisar o plano de execução
Identifique operações caras ou ineficientes
Otimize joins, agregações e outras operações
Monitoramento de Recursos:
Verifique a utilização de recursos no Databricks
Identifique se há gargalos de CPU, memória ou E/S
Ajuste o tamanho do cluster ou as configurações do Spark
Otimização de Código:
Refatore código ineficiente
Implemente técnicas como broadcast joins e caching
Considere o uso de UDFs em C++ para operações intensivas
3. Problemas de Escalabilidade
Se o sistema não escalar bem com o aumento do volume:
Análise de Arquitetura:
Revise a arquitetura para identificar limitações
Considere mudanças arquiteturais para melhorar a escalabilidade
Paralelização:
Aumente o grau de paralelismo
Divida grandes conjuntos de dados em partições menores
Processe partições em paralelo
Recursos Elásticos:
Implemente auto-scaling para recursos
Configure recursos para escalar automaticamente com a demanda
Monitore e ajuste os limites de escala
Conclusão
A análise de performance das execuções é um processo contínuo que permite identificar oportunidades de otimização e garantir que o sistema de redundância de arquivos opere de forma eficiente e escalável. Ao estabelecer uma linha de base, identificar gargalos, implementar otimizações e medir os resultados, você pode melhorar continuamente o desempenho do sistema e reduzir custos operacionais.
Próximos Passos
Após analisar o desempenho das execuções, o próximo passo será implementar boas práticas de configuração e segurança para garantir a proteção dos dados e a confiabilidade do sistema de redundância de arquivos.
Boas Práticas de Configuração e Segurança
A implementação de boas práticas de configuração e segurança é essencial para garantir a confiabilidade, a proteção e a eficiência do sistema de redundância de arquivos. Neste guia, vamos detalhar as melhores práticas para configuração e segurança no Azure Data Factory, Databricks e demais componentes do projeto.
Importância das Boas Práticas de Configuração e Segurança
A adoção de boas práticas oferece diversos benefícios:
Proteção de Dados: Garante que dados sensíveis estejam protegidos contra acesso não autorizado
Conformidade: Ajuda a atender requisitos regulatórios e políticas corporativas
Confiabilidade: Aumenta a estabilidade e a resiliência do sistema
Manutenibilidade: Facilita a manutenção e a evolução do sistema ao longo do tempo
Eficiência Operacional: Reduz o risco de incidentes e o tempo de resolução de problemas
Boas Práticas de Configuração
1. Organização e Nomenclatura
Uma estrutura organizacional clara e uma nomenclatura consistente são fundamentais:
Padrões de Nomenclatura:
Adote um padrão de nomenclatura consistente para todos os recursos
Inclua informações como ambiente, tipo de recurso e propósito
Exemplos:
Pipelines: PL_[Função]_[Origem]_para_[Destino]
Datasets: DS_[Tipo]_[Entidade]_[Camada]
Linked Services: LS_[Tipo]_[Recurso]
Organização de Recursos:
Agrupe recursos relacionados em pastas lógicas
No Azure Data Factory, use pastas para organizar pipelines, datasets e outros recursos
No Databricks, organize notebooks em pastas por função ou domínio
Documentação Integrada:
Adicione descrições detalhadas a todos os recursos
Use células markdown nos notebooks Databricks para documentar o código
Mantenha um repositório central de documentação atualizado
2. Configuração de Ambientes
A separação adequada de ambientes é crucial para desenvolvimento seguro:
Separação de Ambientes:
Mantenha ambientes separados para desenvolvimento, teste e produção
Use grupos de recursos distintos para cada ambiente
Configure controle de acesso específico para cada ambiente
Configuração de Desenvolvimento:
Habilite a integração com Git para controle de versão
Configure branches separadas para desenvolvimento e produção
Implemente um processo de revisão de código antes da mesclagem
Configuração de Produção:
Restrinja o acesso direto ao ambiente de produção
Implemente um processo formal de implantação
Configure monitoramento e alertas abrangentes
3. Parametrização e Configuração
A parametrização adequada aumenta a flexibilidade e a reutilização:
Uso de Parâmetros:
Parametrize valores que podem mudar entre ambientes ou execuções
Exemplos de parâmetros:
Caminhos de armazenamento
Nomes de servidores
Configurações de execução (modo, janela de tempo, etc.)
Centralização de Configurações:
Armazene configurações em um local centralizado
Use o Azure Key Vault para armazenar configurações sensíveis
Considere o uso de arquivos de configuração para valores não sensíveis
Variáveis Globais:
Defina variáveis globais para valores usados em múltiplos lugares
Atualize variáveis globais em um único local
Documente o propósito e o uso de cada variável
4. Logging e Monitoramento
Configurações adequadas de logging e monitoramento são essenciais:
Configuração de Logs:
Habilite logs detalhados para todos os componentes
Configure a retenção de logs adequada
Centralize logs em um único local para análise
Configuração de Alertas:
Configure alertas para condições críticas
Defina limiares baseados em análise histórica
Configure notificações para as pessoas certas
Dashboards de Monitoramento:
Crie dashboards personalizados para visualização
Inclua métricas-chave de desempenho e saúde
Torne os dashboards acessíveis às partes interessadas
Boas Práticas de Segurança
1. Gerenciamento de Identidade e Acesso
O controle adequado de acesso é a base da segurança:
Princípio do Menor Privilégio:
Conceda apenas as permissões mínimas necessárias
Revise e ajuste permissões regularmente
Use funções (roles) predefinidas quando possível
Autenticação e Autorização:
Use o Azure Active Directory para autenticação
Implemente autenticação multifator para contas privilegiadas
Configure controle de acesso baseado em função (RBAC)
Gerenciamento de Contas de Serviço:
Use identidades gerenciadas em vez de contas de serviço quando possível
Implemente rotação regular de credenciais
Monitore e audite o uso de contas de serviço
2. Proteção de Dados
A proteção adequada dos dados é crucial:
Criptografia:
Habilite criptografia em repouso para todos os armazenamentos
Use HTTPS/TLS para comunicação
Considere criptografia adicional para dados sensíveis
Mascaramento de Dados Sensíveis:
Implemente mascaramento de dados para informações sensíveis
Limite o acesso a dados não mascarados
Documente quais dados são sensíveis e como são protegidos
Controle de Acesso a Dados:
Implemente controle de acesso granular aos dados
Use políticas de acesso condicional
Audite o acesso a dados sensíveis
3. Segurança de Rede
A segurança de rede protege contra ameaças externas:
Isolamento de Rede:
Use redes virtuais (VNets) para isolar recursos
Configure grupos de segurança de rede (NSGs)
Implemente endpoints privados para serviços Azure
Firewall e Proteção contra Ameaças:
Configure o Azure Firewall ou firewalls de terceiros
Implemente proteção contra DDoS
Configure detecção de intrusão e prevenção
Conexões Seguras:
Use ExpressRoute ou VPN para conexões híbridas
Restrinja o acesso público a endpoints
Monitore e audite o tráfego de rede
4. Segurança de Aplicações e Código
A segurança do código e das aplicações previne vulnerabilidades:
Revisão de Código:
Implemente revisão de código obrigatória
Use ferramentas de análise estática de código
Verifique vulnerabilidades conhecidas
Gerenciamento de Dependências:
Mantenha bibliotecas e frameworks atualizados
Verifique vulnerabilidades em dependências
Use fontes confiáveis para pacotes
Segurança de Notebooks e Scripts:
Evite credenciais hardcoded em notebooks
Valide e sanitize entradas
Implemente tratamento seguro de erros
Implementação de Segurança no Projeto de Redundância
Vamos aplicar essas boas práticas especificamente ao nosso projeto de redundância de arquivos:
1. Segurança no Azure Data Factory
Para garantir a segurança no Azure Data Factory:
Proteção de Credenciais:
Armazene todas as credenciais no Azure Key Vault
Configure o Data Factory para acessar o Key Vault usando identidade gerenciada
Exemplo de configuração:
json
{
  "name": "LS_SQLServer_ItibereBD_KeyVault",
  "properties": {
    "type": "SqlServer",
    "typeProperties": {
      "connectionString": {
        "type": "AzureKeyVaultSecret",
        "store": {
          "referenceName": "LS_AzureKeyVault",
          "type": "LinkedServiceReference"
        },
        "secretName": "SQLServerConnectionString"
      }
    },
    "connectVia": {
      "referenceName": "integrationRuntimeB",
      "type": "IntegrationRuntimeReference"
    }
  }
}
Configuração de Rede Segura:
Configure o Data Factory para usar endpoints privados
Restrinja o acesso público ao Data Factory
Configure a integração com rede virtual para o Integration Runtime auto-hospedado
Auditoria e Monitoramento:
Habilite logs de diagnóstico para o Data Factory
Configure o Azure Monitor para coletar e analisar logs
Configure alertas para atividades suspeitas
2. Segurança no Azure Blob Storage
Para proteger os dados no Azure Blob Storage:
Controle de Acesso:
Use SAS (Shared Access Signatures) com escopo limitado
Configure políticas de acesso por contêiner
Implemente RBAC para controle granular
Criptografia e Proteção:
Habilite a criptografia em repouso (habilitada por padrão)
Configure o versionamento de blobs para proteção contra alterações acidentais
Habilite a exclusão reversível para recuperação de dados
Isolamento de Rede:
Configure endpoints privados para o Blob Storage
Restrinja o acesso de rede apenas a redes confiáveis
Use firewalls de armazenamento para controle adicional
3. Segurança no Azure Databricks
Para garantir a segurança no Azure Databricks:
Controle de Acesso:
Configure o controle de acesso baseado em função (RBAC)
Restrinja o acesso a notebooks e clusters
Use grupos do Azure AD para gerenciar permissões
Segurança de Cluster:
Configure clusters em redes virtuais
Habilite a criptografia de disco
Restrinja o acesso à internet a partir dos clusters
Segurança de Notebooks:
Evite armazenar credenciais em notebooks
Use o Databricks Secret Scope integrado ao Key Vault
Exemplo de código seguro:
python
# Inseguro:
# connection_string = "jdbc:sqlserver://server;database=db;user=user;password=password"

# Seguro:
connection_string = dbutils.secrets.get(scope="key-vault-secrets", key="jdbc-connection-string")
4. Segurança na Integração On-Premises
Para proteger a integração com o ambiente on-premises:
Segurança do Integration Runtime:
Instale o Integration Runtime em um servidor seguro
Mantenha o sistema operacional e o software atualizados
Configure firewalls para permitir apenas tráfego necessário
Segurança da Comunicação:
Use apenas conexões de saída do ambiente on-premises para a nuvem
Configure TLS/SSL para todas as comunicações
Monitore e audite o tráfego de rede
Proteção de Credenciais Locais:
Use a criptografia de credenciais do Integration Runtime
Implemente rotação regular de senhas
Limite o acesso ao servidor do Integration Runtime
Checklist de Segurança para o Projeto
Use este checklist para verificar se todas as medidas de segurança foram implementadas:
1. Segurança de Identidade e Acesso
 Configuração de RBAC para todos os recursos Azure
 Uso de identidades gerenciadas para autenticação entre serviços
 Implementação de autenticação multifator para contas administrativas
 Revisão regular de permissões e acessos
2. Segurança de Dados
 Criptografia em repouso habilitada para todos os armazenamentos
 Criptografia em trânsito (HTTPS/TLS) para todas as comunicações
 Mascaramento de dados sensíveis implementado
 Políticas de retenção e exclusão de dados configuradas
3. Segurança de Rede
 Endpoints privados configurados para serviços Azure
 Redes virtuais e NSGs implementados
 Firewalls configurados para restringir acesso
 Monitoramento de tráfego de rede implementado
4. Segurança de Configuração
 Todas as credenciais armazenadas no Azure Key Vault
 Logs de diagnóstico habilitados para todos os serviços
 Alertas configurados para atividades suspeitas
 Backups regulares de configurações e dados críticos
5. Segurança Operacional
 Processo de gerenciamento de mudanças implementado
 Procedimentos de resposta a incidentes documentados
 Testes regulares de recuperação de desastres
 Monitoramento contínuo de segurança
Melhores Práticas para Manutenção Contínua da Segurança
A segurança é um processo contínuo, não um estado final. Implemente estas práticas para manutenção contínua:
1. Avaliação Regular de Segurança
Realize avaliações periódicas para identificar e corrigir vulnerabilidades:
Verificações de Segurança:
Execute verificações de segurança automatizadas
Realize avaliações de vulnerabilidade
Contrate auditorias de segurança externas quando necessário
Revisão de Configurações:
Revise regularmente as configurações de segurança
Compare com as melhores práticas atuais
Atualize configurações conforme necessário
Simulações de Ataque:
Realize testes de penetração (com aprovação adequada)
Simule cenários de violação de dados
Documente e corrija vulnerabilidades encontradas
2. Atualizações e Patches
Mantenha todos os componentes atualizados:
Política de Atualizações:
Defina uma política clara para atualizações
Estabeleça janelas de manutenção regulares
Priorize atualizações de segurança críticas
Teste de Atualizações:
Teste atualizações em ambiente não produtivo
Verifique compatibilidade e impacto
Tenha um plano de rollback
Monitoramento de Vulnerabilidades:
Acompanhe boletins de segurança
Monitore CVEs (Common Vulnerabilities and Exposures)
Avalie o impacto para seu ambiente
3. Treinamento e Conscientização
Mantenha a equipe informada e treinada:
Treinamento Regular:
Realize treinamentos de segurança para a equipe
Mantenha a equipe atualizada sobre novas ameaças
Promova a cultura de segurança
Documentação:
Mantenha documentação de segurança atualizada
Documente procedimentos de resposta a incidentes
Crie guias de melhores práticas específicos para o projeto
Simulações de Incidentes:
Realize simulações de resposta a incidentes
Pratique cenários de violação de dados
Refine procedimentos com base nas lições aprendidas
Solução de Problemas Comuns de Segurança
1. Problemas de Acesso
Se ocorrerem problemas de acesso:
Verificação de Permissões:
Verifique as atribuições de função (RBAC)
Confirme se a identidade tem as permissões necessárias
Verifique se não há políticas de negação explícitas
Verificação de Autenticação:
Verifique se as credenciais estão corretas e não expiraram
Confirme se o serviço de autenticação está funcionando
Verifique logs de autenticação para erros específicos
Verificação de Rede:
Confirme se o acesso de rede está permitido
Verifique configurações de firewall e NSGs
Teste a conectividade básica
2. Problemas de Criptografia
Se houver problemas relacionados à criptografia:
Verificação de Chaves:
Confirme se as chaves de criptografia estão acessíveis
Verifique se as chaves não expiraram
Teste o acesso ao Key Vault
Verificação de Configuração:
Confirme se a criptografia está habilitada corretamente
Verifique as versões de protocolo TLS/SSL
Teste com ferramentas de diagnóstico de criptografia
Problemas de Compatibilidade:
Verifique compatibilidade entre diferentes implementações de criptografia
Teste com versões diferentes de bibliotecas criptográficas
Consulte documentação específica do serviço
3. Problemas de Conformidade
Se houver preocupações de conformidade:
Revisão de Requisitos:
Revise os requisitos regulatórios aplicáveis
Identifique lacunas na implementação atual
Desenvolva um plano de remediação
Documentação de Conformidade:
Documente as medidas de segurança implementadas
Mantenha registros de auditorias e avaliações
Prepare documentação para demonstrar conformidade
Consulta Especializada:
Considere consultar especialistas em conformidade
Realize auditorias de conformidade formais
Implemente recomendações de auditoria
Conclusão
A implementação de boas práticas de configuração e segurança é essencial para garantir a proteção, a confiabilidade e a eficiência do sistema de redundância de arquivos. Ao seguir as recomendações detalhadas neste guia, você pode criar um ambiente seguro e bem configurado que atenda aos requisitos de negócio e regulatórios, protegendo dados sensíveis e garantindo operações confiáveis.
A segurança não é um estado final, mas um processo contínuo de avaliação, implementação e melhoria. Mantenha-se atualizado sobre novas ameaças e melhores práticas, e adapte sua estratégia de segurança conforme necessário para proteger seu ambiente em constante evolução.
