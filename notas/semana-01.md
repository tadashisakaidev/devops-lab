# Semana 1: Setup do ambiente

## Objetivo
Montar o ambiente de estudos de DevOps com dois computadores:
um PC com Windows 10 (WSL2 + Ubuntu) e um notebook com Ubuntu 24.04
funcionando como servidor de testes, tudo sincronizado pelo GitHub.

## O que fiz

### PC (Windows 10)
- Instalei o WSL2 com Ubuntu (`wsl --install -d Ubuntu`)
- Atualizei o sistema e instalei git, curl e build-essential
- Configurei o Git (nome, e-mail noreply do GitHub, branch `main`)
- Gerei uma chave SSH ed25519 e cadastrei no GitHub
- Instalei o VS Code com a extensão WSL
- Criei o repositório `devops-lab` e clonei por SSH em `~/projetos`

### Notebook (Ubuntu 24.04)
- Instalei e ativei o servidor SSH (`openssh-server`)
- Copiei a chave do PC para o notebook (`ssh-copy-id`) e passei a
  entrar sem senha
- Criei o apelido `notebook` em `~/.ssh/config` (WSL) e também no
  Windows, para o VS Code
- Conectei o VS Code do PC ao notebook com a extensão Remote-SSH
- Configurei Git e uma chave SSH própria no notebook e cadastrei no GitHub
- Clonei o `devops-lab` no notebook
- Testei o fluxo: commit e push no notebook, `git pull` no PC

## Comandos principais
```bash
wsl --install -d Ubuntu
ssh-keygen -t ed25519 -C "comentario"
ssh -T git@github.com
git clone git@github.com:usuario/devops-lab.git

sudo apt install -y openssh-server
sudo systemctl enable --now ssh
ssh-copy-id usuario@192.168.x.x
ssh notebook
```

## Problemas e soluções
- **`config is not a git command`:** erro de digitação no
  `git config`. Passei a reler o comando antes de executar.
- **Resposta errada no prompt do `ssh-keygen`:** digitei um comando
  quando ele pedia o caminho da chave. Cancelei com `Ctrl + C` e
  refiz, aceitando o padrão com Enter.
- **Push rejeitado (GH007):** o GitHub bloqueou o push porque o
  commit tinha meu e-mail pessoal. Solução: usar o e-mail `noreply`
  do GitHub no `git config` e refazer o autor com
  `git commit --amend --reset-author`.
- **`Unit ssh.service could not be found`:** o comando de instalação
  tinha erros de digitação (um `update` a mais e o pacote escrito
  errado), então o `openssh-server` nunca foi instalado. Refiz
  copiando e colando o comando.
- **SSH instalado mas `inactive`:** o serviço não inicia sozinho.
  Resolvi com `sudo systemctl enable --now ssh`.
- **Host key verification failed:** o terminal perguntava
  `yes/no` e eu não respondi `yes` por extenso.
- **VS Code (Windows) não enxergava as chaves do WSL:** o Remote-SSH
  usa o SSH do Windows, não o do Ubuntu. Criei uma chave separada no
  PowerShell, copiei para o notebook e criei o `config` do lado do
  Windows.
- **Quase sobrescrevi a chave do GitHub:** rodei o `ssh-keygen` no
  terminal errado e respondi `n` ao aviso de sobrescrita. Lição:
  conferir o prompt (`DESKTOP-...` é o PC, `SAKAI-DESK` é o notebook)
  antes de rodar comandos.

## O que aprendi
- WSL2 e Windows têm ambientes de SSH separados, cada um com as
  próprias chaves
- Só a chave pública (`.pub`) pode ser compartilhada
- O GitHub protege o e-mail pessoal em commits de repositórios públicos
- Olhar o prompt do terminal diz em qual máquina o comando vai rodar
- Rotina entre as duas máquinas: `git pull` antes de começar e
  `git add`, `git commit`, `git push` ao terminar
- O IP do notebook vem do roteador e pode mudar. Se o SSH falhar,
  conferir com `hostname -I`

## Próximos passos
- Semana 2: Linux (arquivos, permissões e usuários), curso Linux
  Fundamentals da 4Linux, com prática no notebook
- Reservar um IP fixo para o notebook no roteador