# Wiz - Gateway

![](https://github.com/wizsolucoes/gateway-wiz-template/workflows/.NET%20Core/badge.svg)

- [Desenvolvimento, por onde começar](#desenvolvimento-por-onde-começar)
- [Execução do projeto](#execução-do-projeto)
- [Estrutura](#estrutura)
- [Dependências](#dependências)
- [Build e testes](#build-e-testes)
- [CI/CD](#ci/cd)
- [README](#readme)

## Desenvolvimento, por onde começar

Passos para execução do projeto:

1. Abrir *Prompt de Comando* de sua preferência (**CMD** ou **PowerShell**);

2. Criar pasta para o projeto no local desejado;

3. Executar os seguintes comandos;
  > *dotnet new -i Wiz.Dotnet.Template.Gateway*    
    *dotnet new wizgateway -n [NomeProjeto]*

4. Executar comando para configurar aplicação em modo (HTTPS);
  > *dotnet dev-certs https --trust*

5. Incluir configurações de sistema *SSO (Single Sign-On)* no caminho abaixo:

### **Local**

```
├── src (pasta física)
  ├── Wiz.[NomeProjeto].Gateway (projeto)
    ├── appsettings.{ENVIRONMENT}.json
```

Dentro do arquivo *appsettings.{ENVIRONMENT}.json*, há o conteúdo para modificação das variáveis:

```json
  "IdentityServer": {
    "ProviderKey": "PROVIDER_KEY",
    "Authority": "AUTHORITY",
    "ApiName": "API_NAME",
    "ApiSecret": "API_SECRET"
  }
```

6. *(Opcional)* Inserir chave do **Application Insights** conforme configurado no Azure no arquivo *appsettings.{ENVIRONMENT}.json*.

```json
  "ApplicationInsights": {
    "InstrumentationKey": "KEY_APPLICATION_INSIGHTS"
  }
```

Caso não há chave de configuração no Azure, não é necessário inserir para executar o projeto local.

### **Docker**

```
├── Dockerfile
```

Dentro do arquivo *Dockerfile*, há o conteúdo para modificação das variáveis:

```docker
ENV ApplicationInsights:InstrumentationKey=KEY_APPLICATION_INSIGHTS
ENV IdentityServer:ProviderKey=PROVIDER_KEY
ENV IdentityServer:Authority=AUTHORITY
ENV IdentityServer:ApiName=API_NAME
ENV IdentityServer:ApiSecret=API_SECRET
```

Para executar o projeto via **docker** é necessário inserir a URL do **storage** do Azure.

## Execução do projeto

### **Visual Studio**

1. Executar projeto via **Kestrel**;

Executar o projeto via **Kestrel** facilita a troca de ambientes *(environments)* e a verificação de logs em execução da aplicação em projetos .NET Core. Os ambientes podem ser configurados dentro das propriedades do projeto, conforme caminho abaixo:

```
├── src (pasta física)
  ├── Wiz.[NomeProjeto].Gateway (projeto)
    ├── Properties (pasta física)
      ├── launchSettings.json
```

Dentro do arquivo *launchSettings.json*, há o conteúdo que indica a configuração de ambiente via **Kestrel**:

```json
    "Wiz.[NomeProjeto].Gateway": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "applicationUrl": "https://localhost:5001;http://localhost:5000"
    }
```

### **Visual Studio Code**

1. *(Recomendado)* Instalar extensões para desenvolvimento:
  + [ASP.NET core VS Code Extension Pack](https://marketplace.visualstudio.com/items?itemName=temilaj.asp-net-core-vs-code-extension-pack)
  + [Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
  + [GitLens — Git supercharged](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
  + [NuGet Package Manager](https://marketplace.visualstudio.com/items?itemName=jmrog.vscode-nuget-package-manager)
  + [vscode-icons](https://marketplace.visualstudio.com/items?itemName=vscode-icons-team.vscode-icons)

2. *(Recomendado)* Instalar extensões para testes:
  + [.NET Core Test Explorer](https://marketplace.visualstudio.com/items?itemName=formulahendry.dotnet-test-explorer)
  + [Coverage Gutters](https://marketplace.visualstudio.com/items?itemName=ryanluker.vscode-coverage-gutters)

3. Executar projeto via **Kestrel** *(Tecla F5)*;

Por padrão, todo projeto executado no **Visual Studio Code** é executado via **Kestrel** *(Tecla F5)*. Os ambientes podem ser configurados dentro das propriedades do projeto, conforme caminho abaixo:

```
  ├── .vscode (pasta física)
    ├── launch.json
```

4. Utilizar a função **task** para executar ações dentro do projeto. A função está presente no caminho do *menu* abaixo:

```
Terminal -> Run Task
```

5. Selecionar a função **task** a ser executada no projeto:
   + *clean* - Limpar solução 
   + *restore* - Restaurar pacotes da solução
   + *build* - Compilar pacotes da solução

### **Docker**

1. Executar comando na **raiz** do projeto:

> *docker-compose up -d*

2. logs de execução:

> *docker-compose logs*

3. Parar e remover container:

> *docker-compose down*

## Estrutura

Padrão das camadas do projeto:

1. **Wiz.[NomeProjeto].Gateway**: responsável pela camada de *disponibilização* dos endpoints e autenticação dos microsserviços.

Formatação do projeto dentro do repositório:

```
├── src 
  ├── Wiz.[NomeProjeto].Gateway (projeto)
├── Wiz.[NomeProjeto] (solução)
```

## Dependências

* [ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core)
* [Identity Server 4](https://identityserver4.readthedocs.io/en/latest/)
* [Ocelot](https://ocelot.readthedocs.io/en/latest/introduction/gettingstarted.html)

## Build e testes

Não há obrigatoriedade de realização de testes unitários ou de integração. Todos os testes são executados pelos **microsserviços** disponibilizados.

### **Sonar**

1. Dentro do arquivo dos projetos **(.csproj)** no campo **PropertyGroup**, é necessário adicionar um GUID no formato abaixo:

```
<PropertyGroup>
  <ProjectGuid>{b5c970c2-a7cc-4052-b07b-b599b83fc621}</ProjectGuid>
</PropertyGroup>
```

2. O GUID pode ser coletado no arquivo da solution ou criado pelo site: https://www.guidgenerator.com/.

## CI/CD

* Arquivo de configuração padrão: [azure-pipelines.yml](azure-pipelines.yml).
* Caso há necessidade de incluir mais *tasks* ao pipeline, verfique a documentação para inclusão: [Azure DevOps - Yaml Schema](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema).

## README

* Incluir documentação padrão no arquivo [README.md](README.md).