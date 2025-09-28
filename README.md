# Plataforma de Gestão de Inovação ATRIUM

## 1. Sobre o Projeto

ATRIUM é uma solução de gestão de portfólio de inovação desenvolvida para a Eurofarma. O objetivo da plataforma é centralizar, padronizar e otimizar o processo de submissão, avaliação e priorização de novas ideias, movendo a tomada de decisão de um modelo subjetivo para um quantitativo, baseado em dados e alinhado à estratégia de negócio.

### 1.1. Contexto de Negócio

O desenvolvimento desta plataforma é uma resposta estratégica aos desafios do mercado de inovação corporativa, que incluem a **pressão por resultados de curto prazo** e a necessidade de alinhar os projetos de inovação à **eficiência operacional**. A solução implementa práticas de **gestão de portfólio**, já adotadas por 74% das empresas maduras, garantindo governança e visibilidade ao funil de inovação.

## 2. Arquitetura e Stack Tecnológico

A solução é construída integralmente sobre o ecossistema **Microsoft Power Platform**, garantindo segurança, escalabilidade e integração nativa com a infraestrutura existente da Eurofarma.

* **Backend e Camada de Dados:** `Microsoft Dataverse`
    * Atua como a fonte única da verdade, armazenando todas as propostas, avaliações e dados de usuários.
    * Utiliza Perfis de Segurança (`Security Roles`) para gerenciar permissões de acesso em nível de registro e campo.

* **Frontend (Portal):** `Microsoft Power Pages`
    * Serve como a interface web responsiva para todos os perfis de usuário.
    * A autenticação é gerenciada via `Microsoft Entra ID`, garantindo acesso exclusivo a colaboradores.

* **Análise e Visualização:** `Microsoft Power BI`
    * Os dashboards conectam-se ao Dataverse para **visualizar** os dados e as pontuações já calculadas.
    * A Segurança em Nível de Linha (RLS) é aplicada para filtrar os dados que cada perfil pode visualizar.

## 3. Principais Funcionalidades

* **Fluxo de Submissão:** Usuários podem submeter novas ideias através de um formulário estruturado.
* **Perfis de Acesso:** A plataforma opera com três níveis de permissão:
    * **Usuário Comum:** Submete e visualiza o status de suas próprias propostas.
    * **Gestor:** Realiza a triagem inicial das propostas de seu time (ex: Financeiro).
    * **Comitê:** Especialistas (ex: em Machine Learning) avaliam propostas de sua área de expertise.
* **Motor de Pontuação:** O comitê de especialistas insere as variáveis-chave da proposta (Benefício Esperado (VPL), Recursos Aportados, etc.) através do portal. Após o salvamento, um fluxo no **Power Automate executa o modelo de Pesquisa Operacional**, calcula a pontuação final e a armazena de volta no Dataverse. Este score é então consumido e **visualizado nos dashboards do Power BI**.
* **Dashboards Interativos:** Painéis em Power BI para visualização do funil de inovação, status do portfólio e performance por área.

## 4. Pré-requisitos para Desenvolvimento

* Acesso a um ambiente Power Platform.
* Licenças apropriadas (Power Apps, Power Pages, Power BI, Power Automate).
* [Power Platform CLI](https://docs.microsoft.com/en-us/power-platform/developer/cli/overview) instalado.
* Visual Studio Code com a extensão [Power Platform Tools](https://marketplace.visualstudio.com/items?itemName=microsoft-IsvExpTools.powerplatform-vscode).
* Git para controle de versão.

## 5. Instalação e Ambiente de Desenvolvimento

1.  Clone o repositório: `git clone https://github.com/wesassis/ATRIUM-EUROFARMA`
2.  Autentique-se no seu ambiente de desenvolvimento usando o Power Platform CLI.
3.  Descompacte e importe a solução (`.zip`) do Dataverse encontrada no diretório `/solutions`.
4.  Utilize o Power Platform CLI para fazer o download/upload do conteúdo do portal Power Pages.

## 6. ALM e Deployment

O processo de Application Lifecycle Management (ALM) segue as melhores práticas para a Power Platform:
1.  O desenvolvimento é feito em um ambiente de **Desenvolvimento (DEV)**.
2.  As customizações são exportadas como uma **solução não gerenciada**, descompactadas (`pac solution unpack`) e versionadas no Git.
3.  Um pipeline de CI/CD (ex: Azure DevOps, GitHub Actions) é utilizado para:
    * Empacotar a solução como **gerenciada**.
    * Fazer o deploy da solução gerenciada nos ambientes de **Teste (UAT)** e **Produção (PROD)**.
    * Gerenciar as variáveis de ambiente (`Environment Variables`).
