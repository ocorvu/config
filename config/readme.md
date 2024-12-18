# O que é?

Passo a passo de configuração de ambiente WSL2 Ubuntu para desenvolvimento em PHP.

Mencionarei aqui apenas sobre instalação e configuração de:
- WSL2
- Editor de código (VSCode)
- PHP e suas extensões
- Composer
- Bancos de dados: MySQL e SQLite3

Demais tópicos como Laravel, Nginx e Apache não serão mencionados aqui diretamente. Porém, a [documentação do Laravel](https://laravel.com/docs) é muito rica e fácil de ser seguida. Nos artigos indicados nas [fontes](#fontes) você poderá encontrar sobre Nginx e Apache.

## WSL2

Para desenvolver no Windows utilizando o WSL2 é necessário realizar a instalação de sistema linux.

O jeito mais simples de realizar essa instação é seguir a documentação da própria Microsoft, que pode ser encontrada **[aqui](https://learn.microsoft.com/pt-br/windows/wsl/install)** ou buscando no Google por "**Como instalar o Linux no Windows com o WSL**".

Siga todos os passos da documentação corretamente e você não deverá ter problemas.

> *Alguns sistemas operacionais tem limitações quanto a virtualização, importante verificar sobre esses pontos caso enfrente algum problema.*

### Executando o WSL

Para executar o WSL basta pressionar a tecla do windows e digitar "**WSL**". Um executável com ícone de pinguim aparecerá. Ao executá-lo o seu terminal padrão abrirá com o seu usuário Linux conectado.

### Atualização de dependências

Execute os comandos abaixo para garantir que as dependências do sistema estão devidamente atualizadas.

**Utilize esses comandos regularmente.**

```shell
sudo apt update
sudo apt upgrade
```

Você pode adicionar **-y** ao comando de **upgrade** para que os downloads já sejam feitos automaticamente, sem a necessidade de uma ação sua após o comando:

```shell
sudo apt -y upgrade
```

## Editor de código

O editor de código fica a seu critério. Estou utilizando o VSCode diretamente no windows com a extensão **WSL** da própria microsoft.

Para a instalação tanto do VSCode como da extensão, recomendo também a própria documentação da Microsoft disponível **[aqui](https://learn.microsoft.com/pt-br/windows/wsl/tutorials/wsl-vscode)** ou buscando no Google por **"Para você começar a utilizar o VS Code com o WSL"**.

## PHP

A partir de agora, vamos utilizar linhas de comando diretamente no WSL2. Se você ainda não tem o subsistema instalado, retorne a seção **[dedicada](#wsl2)** a isso. 

### Atualização de dependências

Primeiro, verifique se todas as dependências estão devidamente atualizadas. Isso também pode ser encontrado na seção de instalação do WSL.

### Adicionando o repositório

Agora, vamos adicionar o repositório específico para instalação de diversas versões do PHP no Ubuntu.

Caso não tenha um WSL com Ubuntu, após executar o comando abaixo, ele incluirá diversas orientações sobre como adicionar o repositório em outras versões do Linux.

```shell
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
```

### Instalação da versão do PHP desejada

No reposositório que adicionamos temos diversas versões do PHP: 5, 7 e 8. Vou utilizar aqui a versão major 8, porém nas linha identificarei como `8.x`, pois fica ao seu critério qual versão específica (8.1, 8.2, 8.3 e afins) deseja instalar.

Vamos instalar o PHP CLI e também o PHP com diversas extensões comuns disponíveis no commando `php8.x-common`.

```shell
sudo apt install php8.x-common php8.x-cli -y
```

Assim que os downloads e instalações forem finalizados, basta executar `php -v` para verificar a versão instalada.

### Instalando extensões para o PHP

Como mencionado anteriormente, a instalação utilizando `php8.x-common` traz diversas extensões e você pode conferir quais delas estão disponíveis utilizando o comando `php -m`.

Porém, caso precise instalar alguma outra extensão que não esteja nesse "pacote", utilize a convensão `php8.x-name` onde `name` é o nome da extensão.

Por exemplo, para instalar a extensão **mbstring** no **PHP 8.0** basta utilizar:

```shell
sudo apt install php8.0-mbstring
```

Para instalar diversas extensões em uma única linha de comando existem duas formas: repertir o comando `php8.x-name` para todas as extensões que deseja instalar ou utilizar a convensão de `php8.x-{ext1,ext2,...}`.

Exemplo com PHP 8.0:

```shell
sudo apt install php8.0-ext1 php8.0-ext2

sudo apt install php8.0-{ext1,ext2,ext3}
```

**Instale extensões conforme a necessidade de utilização.**

### Update da versão do PHP

Caso você esteja utilizando a versão **8.0** e deseja atualizar para a versão **8.3** por exemplo, ou até mesmo ir de uma versão **7.4** para **8.0**, basta seguir os passos de [instalação](#instalação-da-versão-do-php-desejada).

### Remover uma versão do PHP que não deseja utilizar

Para remover uma versão que não deseja mais utilizar, basta inserir o comando `sudo apt purge '^php8.x.*'`.

O asterisco (`*`) significa que independente do **patch**, aquela versão será expurgada. Utilizando a versão 8.0 como exemplo, temos:

```shell
sudo apt purge '^php8.0.*'
```

Caso sejam encontrados arquivos para remover, o sistema exibirá quantos foram localizados e pedirá uma confirmação para prosseguir.

## Composer

Após a instalação do PHP, é preciso instalar o gerenciador de pacotes da linguagem: o Composer.

Para isso, também é recomendado utilizar a **[documentação oficial](https://getcomposer.org/download/)**, visto que alguns links e versões são constantemente atualizados.

Para que o composer seja instalado globalmente e você possa executá-lo sem dificuldades siga até o passo de instalação que orienta a mover o arquivo `composer.phar` ou, após a instalação, siga por **[aqui](https://getcomposer.org/doc/00-intro.md#globally).**

Seguindo a documentação, você não deverá ter problemas.

## MYSQL

Para a maioria dos projetos a utilização do SQLite3 deve atender as necessidades, sem que seja necessário iniciar um driver de banco de dados mais complexo como MySQL ou PostegreSQL.

De toda forma, vou deixar aqui como configurar o MySQL no WSL para utilização nos mais diversos projetos.

Recomendo seguir o **[artigo da Microsoft](https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-database#install-mysql)** sobre instalação de bancos de dados no WSL. Nele você encontrará auxílio para realizar a instalação de diversos bancos de dados SQL além de links para as documentações desses bancos de dados.

Após a instalação, será necessário [adicionar a extensão](#instalando-extensões-para-o-php) `php8.x-mysql` do MySQL.

Se você utilizou o caminho de instalação do PHP informado aqui, você não precisará instalar a extensão do PDO pois ja está inclusa no `php8.x-common`. Mas se não seguiu por aqui, basta adiciona-la utilizando `php8.x-pdo`.

## SQLite3

Conforme mencionado na seção do MySQL, acredito que para a maioria dos projetos o SQLite atende as necessidades de desenvolvimento.

Para utilizar, basta ter a [extensão](#instalando-extensões-para-o-php) `php8.x-sqlite3` do SQlite e PDO instaladas, e um arquivo SQLite3 no seu projeto.

Se você utilizou o caminho de instalação do PHP informado aqui, você não precisará instalar a extensão do PDO pois ja está incluída no `php8.x-common`. Mas se não seguiu por aqui, basta adiciona-la utilizando `php8.x-pdo`.

Uma extensão muito útil do VSCode para desenvolvimento com o SQLite3 é a [SQLite Viewer](https://marketplace.visualstudio.com/items?itemName=qwtel.sqlite-viewer), que transforma os textos do arquivo SQLite em tabelas para que você consiga ver quais registros estão persistidos no banco.

## Fontes

Artigos e documentações utilizadas que não foram mencionadas no texto:

- [Windows PHP8 Development Setup With WSL2](https://joshpress.net/blog/wsl-debian-php8)
- [How to install/update PHP 8.0 (Debian/Ubuntu)](https://php.watch/articles/php-8.0-installation-update-guide-debian-ubuntu)
- [How to Upgrade PHP on Ubuntu 20.04 | 22.04](https://www.dedicatedcore.com/blog/upgrade-php-ubuntu/)

- [Instalando o PHP no Linux e no Windows](https://devfuria.com.br/php/instalando-php-no-linux-e-no-windows/)