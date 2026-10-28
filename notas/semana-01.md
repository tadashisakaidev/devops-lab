# Semana 1: Setup do ambiente

## Objetivo
Montar o ambiente de estudos de DevOps: WSL2 no PC com Windows 10,
Git, chave SSH, VS Code e o repositório `devops-lab`.

## O que fiz
- Instalei o WSL2 com Ubuntu (`wsl --install -d Ubuntu`)
- Atualizei o sistema e instalei git, curl e build-essential
- Configurei o Git (nome, e-mail, branch padrão `main`)
- Gerei uma chave SSH ed25519 e cadastrei no GitHub
- Instalei o VS Code com a extensão WSL
- Criei o repositório `devops-lab` e clonei por SSH em `~/projetos`

## Comandos principais
```bash
wsl --install -d Ubuntu
ssh-keygen -t ed25519 -C "meu-email"
ssh -T git@github.com
git clone git@github.com:usuario/devops-lab.git
```

## Problemas e soluções
- **`config is not a git command`:** erro de digitação no comando
  `git config`. Reli o comando antes de executar.
- **Chave SSH no lugar errado:** digitei um comando no prompt do
  `ssh-keygen` (que pedia o caminho do arquivo). Cancelei com
  `Ctrl + C` e rodei de novo, aceitando o caminho padrão com Enter.
- **Push rejeitado (GH007):** o GitHub bloqueou o push porque o commit
  tinha meu e-mail pessoal. Solução: usar o e-mail `noreply` do GitHub
  no `git config` e refazer o autor com
  `git commit --amend --reset-author`.

## O que aprendi
- Trabalhar dentro da pasta home do Linux (`~`) é mais rápido no WSL
  do que usar `/mnt/c`
- Só a chave pública (`.pub`) pode ser compartilhada
- O GitHub protege o e-mail pessoal em commits de repositórios públicos

## Próximos passos
- Preparar o notebook Ubuntu com servidor SSH
- Acessar o notebook pelo PC com VS Code Remote-SSH