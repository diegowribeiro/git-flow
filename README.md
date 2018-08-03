# Trabalhando com git e git-flow no dia a dia - (básico)
Vou assumir que voce já sabe de forma conceitual o que é **git** e **git-flow** e como funciona o processo de versionamento, o intuito aqui é trazer um guia de utilização para o dia a dia, para se familiarizar com os comandos e utilização, versionando  seus códigos e trabalhando com branchs de uma forma mais segura.

# Iniciando...

 
Uma vez com o projeto já criado no repositório remoto, temos duas opções para iniciar a utilização do git:

Clonando o repositorio direto do repositório remoto

`git clone <url.git>`

Iniciando o git direto da sua máquina local e apontando para o projeto do repositório remoto:

`git init`

`git remote add origin <url.git>`

Até aqui já temos o git presente em nosso projeto e podemos iniciar o git-flow e trabalhar com as branchs

## Iniciando o git-flow

Primeiro passo é verificar se o git-flow já veio instalado com o seu git:

`git flow version`

Caso não retorne a versão, será preciso instalar:

- [Windows](https://github.com/petervanderdoes/gitflow-avh/wiki/Installing-on-Windows)

- [Linux](https://github.com/petervanderdoes/gitflow-avh/wiki/Installing-on-Linux,-Unix,-etc.)

- [Mac OS X](https://github.com/petervanderdoes/gitflow-avh/wiki/Installing-on-Mac-OS-X)

  

Inicie o git-flow no projeto com o seguinte comando:

`git flow init -d `

  

Com este comando, cria-se toda estrutura de branch padrão que o git-flow necessita, sem precisar da confirmação.

  

**OBS: Esses passos, todos envolvidos no projeto precisam executar, inclusive o git-flow**

  

Até aqui temos um projeto com o git, apontando para o repositório remoto repositório remoto e já com o framework git-flow instalado e startado no projeto

  

## Fluxo de trabalho

  

Precisa criar uma nova api? melhorar o que está em produção? ou até mesmo evoluir a aplicação de alguma forma? então vamos criar uma feature branch, no meu caso estou criando com o nome `endpoint-api`:

`git flow feature start endpoint-api `

  

Com esse comando, foi criado uma branch a partir da develop e automaticamente ele te "joga" para essa branch.

  

Agora vamos publicar essa branch para que tenha uma cópia do repositório remoto, assim mais pessoas podem trabalhar nela:

`git flow feature publish endpoint-api`

  

Para realizar um pull e atualizar uma feature publicada no repositório remoto:

`git flow feature pull endpoint-api`

  

A medida que for desenvolvendo, vá adicionando ao git:

`git add endpoint.txt`

`git commit -m "inserindo novo endpoint"`

  

Ao final de cada período (almoço, fim do dia e etc), envie para o repositório:

`git flow feature publish endpoint-api`

  

Finalizou a codificação da sua api? testou localmente e está ok? chegou a hora de finalizar a feature branch:

`git flow feature finish endpoint-api`

  

Com esse comando, o git flow faz o merge de tudo que desenvolveu com a branch develop, deleta a branch que foi criada (`endpoint-api`) tanto local quanto remoto

  

**OBS: Esse é o processo base que mais será utilizado no dia a dia pelos desenvolvedores, com a feature finalizada, pode iniciar as demais feature's que irá compor seu projeto até que tenha um pacote estável para "deployar" em produção**

  

## Gerando uma release

  

Nos passo anteriores, já foram criadas várias features, o pacote gerado com todas essas features está estável e funcional... agora precisamos iniciar uma branch do tipo release para que possamos testar no ambiente de pré-prod e após validar com sucesso, realizar o deploy em produção conforme janela e encerrar essa branch mergiando com a Master e Develop.

  

Iniciando a brande de release, neste exemplo a versão do meu release será `v1.0.0`

  

`git flow release start v1.0.0`

  

Vamos publicar o release para o repositório remoto, assim todos sabem que temos um release candidate e consequentemente realizar um deploy em pré-prod:

`git flow release publish v1.0.0`

  

Testado em pré-prod? realizar o deploy em produção na janela previamente agendada e caso não ocorra nenhum problema com o deploy, chegou a hora realizar o merge com a Master, Develop e encerrar a branch de release remoto e local:

`git flow release finish v1.0.0`

  

No momento que finalizar, o git abre em sequência três editores de texto:

- O primeiro é relacionado ao merge da branch release `v1.0.0` com a Master, que você pode detalhar o que contempla esta release.

- O segundo é relacionado tag, para facilitar a mudança e identificação, incluir a tag da versão que estão indo para produção `v1.0.0`

- Por ultimo o merge da release com com a Develop.

  

Pronto! :-) neste momento temos nosso código estável que está em produção na branch master, sem realizar nenhum commit nela.

  
  

## BUG crítico em produção :-(

  

Após alguns dias depois do Deploy em produção, foi encontrado um BUG que está impactando 50% dos usuários e ao realizar uma análise trata-se de algo que é possível corrigir em algumas horas.

  

Vamos iniciar nossa branch de Hotfix, para corrigir este BUG e estabilizar o ambiente:

`git flow hotfix start v1.0.1`

  

Após corrigir o BUG, realizar teste locais, podemos realizar o deploy em produção e validar. Está ok? BUG corrigido com sucesso? vamos realizar o merge deste código para Master, Develop e encerrar a branch hotfix:

`git flow hotfix finish v1.0.1`

  

Assim como na finalização da branch de release, abrem três arquivos texto com o mesmo propósito

  

Pronto! :-) BUG corrigido e já temos esse código com a correção na branch Master e Develop para que no próximo release esse BUG não volte para produção.

  

**OBS: Aqui podemos ter um cenário que o git-flow não contempla, vamos imaginar que na descoberta deste BUG já tinha uma próxima branch de release (V2.0.0) sendo preparada, neste caso antes de finalizar a branch hotfix é preciso realizar o deploy manual da correção do BUG para a branch de release (v2.0.0) em andamento, desta forma conseguimos garantir que o mesmo erro aconteça em produção.**
