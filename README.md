# Projeto DevOps IFPB - Infraestrutura com Vagrant e Ansible

Este projeto tem como objetivo provisionar e configurar uma infraestrutura virtual utilizando **Vagrant** e **Ansible**, promovendo práticas de DevOps e Infraestrutura como Código (IaC).

## Integrantes

- **Diego Costa Sales** — Matrícula: 20221380034
- **Igor de Oliveira Teixeira** — Matrícula: 20221380025

Disciplina: Admnistração de Sistemas Abertos
Professor: Leonidas Lima

## Estrutura do Projeto

```
.
├── Vagrantfile
├── hosts.ini
├── ssh_config
├── playbooks/
│   ├── all.yml
│   ├── vars.yml
│   ├── conf-dhcp.yml
│   ├── conf/
│   │   ├── conf-app.yml
│   │   ├── conf-arq-storage.yml
│   │   ├── conf-cli.yml
│   │   └── conf-db.yml
│   ├── files/
│   │   ├── id_rsa.diego
│   │   ├── id_rsa.igor
│   │   ├── id_rsa.pub.diego
│   │   └── id_rsa.pub.igor
│   └── templates/
│       ├── dhcpd.conf.j2
│       ├── index.html.j2
│       └── motd.j2
└── .gitignore
```

## Componentes

- **Vagrantfile**: Define 4 VMs (arq, db, app, cli) com configurações de rede e armazenamento.
- **Ansible Playbooks**:
  - `all.yml`: Configuração comum a todas as VMs (usuários, SSH, NTP, sudo, etc).
  - `conf-dhcp.yml`: Configuração do servidor DHCP na VM `arq`.
  - `conf/conf-app.yml`: Configuração do servidor web Apache na VM `app`.
  - `conf/conf-db.yml`: Configuração do MariaDB e autofs na VM `db`.
  - `conf/conf-cli.yml`: Configuração do cliente (firefox, xauth, autofs) na VM `cli`.
  - `conf/conf-arq-storage.yml`: Configuração de LVM e NFS na VM `arq`.
  - `vars.yml`: Variáveis globais (usuários, IPs, etc).
- **Templates**: Arquivos Jinja2 para configuração dinâmica (motd, index.html, dhcpd.conf).
- **Files**: Chaves públicas/privadas dos usuários para acesso SSH.

## Como usar

1. **Suba as VMs com Vagrant**  
   No diretório do projeto:
   ```sh
   vagrant up
   ```

2. **Acesse as VMs via SSH**  
   Use o arquivo `ssh_config` para facilitar o acesso:
   ```sh
   ssh -F ssh_config arq
   ssh -F ssh_config db
   ssh -F ssh_config app
   ssh -F ssh_config cli
   ```

3. **Execute os playbooks Ansible**  
   No diretório `playbooks/`, execute:
   ```sh
   ansible-playbook -i ../hosts.ini all.yml
   ansible-playbook -i ../hosts.ini conf-dhcp.yml
   ansible-playbook -i ../hosts.ini conf/conf-arq-storage.yml
   ansible-playbook -i ../hosts.ini conf/conf-db.yml
   ansible-playbook -i ../hosts.ini conf/conf-app.yml
   ansible-playbook -i ../hosts.ini conf/conf-cli.yml
   ```

## Integrantes

- **Diego Costa Sales** — Matrícula: 20221380034
- **Igor de Oliveira Teixeira** — Matrícula: 20221380025

## Observações

- As chaves privadas dos usuários estão no `.gitignore` por segurança.
- Confirme os nomes dos discos no Vagrantfile antes de executar o playbook de storage.
- O projeto foi desenvolvido para a disciplina ASA, período 2025.1, professor Leonidas Lima.

---
```
