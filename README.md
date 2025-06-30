Projeto 01 - DevOps com Vagrant e Ansible

Este projeto visa desenvolver competências em DevOps e Infraestrutura como Código (IaC) utilizando 

Vagrant para provisionar infraestrutura virtual e Ansible para automatizar suas configurações. 

Informações Essenciais:

    Disciplina: Administração de Sistemas Abertos 

Professor: Leonidas Lima 

Período: 2025.1 

    Equipe: Diego Costa Sales - 20221380034 e Igor de Oliveira Teixeira 20221380034

Infraestrutura Provisionada (Vagrant): 

    Quatro Máquinas Virtuais (arq, db, app, cli) com Debian Bookworm (64-bit) no VirtualBox. 

Configurações padrão: 512 MB RAM (exceto cli com 1024 MB), clones, SSH desativado. 

arq (Servidor de Arquivos): Hostname arq.diego.igor.devops, IP estático 192.168.56.134, 3 discos de 10 GB. 
db (Servidor de Banco de Dados): Hostname db.diego.igor.devops, IP via DHCP (192.168.56.125 fixo). 
app (Servidor de Aplicação): Hostname app.diego.igor.devops, IP via DHCP (192.168.56.145 fixo). 
cli (Host Cliente): Hostname cli.diego.igor.devops, IP via DHCP. 

Configurações Automatizadas (Ansible): 

    Todas as VMs: Atualização do SO, NTP (chrony), fuso horário America/Recife, criação de grupo ifpb e usuários diego/igor com acesso sudo sem senha. Configuração de SSH (apenas chaves públicas, 

root bloqueado, acesso para vagrant/ifpb, banner de saudação, chaves para diego/igor). Instalação de cliente NFS. 

arq (Servidor de Arquivos): 

    DHCP: Servidor autoritativo para diego.igor.devops (192.168.56.0/24), com faixa dinâmica 50-100, gateway 192.168.56.1, e IPs fixos para db e app. 

    LVM: VG dados com 3 discos, LV ifpb (15GB, ext4), montagem automática em /dados. 

    NFS: Compartilha /dados/nfs (192.168.56.0/24), com usuário nfs-ifpb (sem shell, permissões restritas). 

db (Servidor de Banco de Dados): Instala mariadb-server. Configura 

autofs para montar /dados/nfs em /var/nfs. 

app (Servidor de Aplicação): Instala apache2 com index.html personalizado. Configura 

autofs para montar /dados/nfs em /var/nfs. 

cli (Host Cliente): Instala firefox-esr e xauth. Configura SSH para encaminhamento X11. Configura 

autofs para montar /dados/nfs em /var/nfs. 

Estrutura do Projeto:
O projeto é organizado com Vagrantfile, hosts.ini, e a pasta playbooks contendo playbooks gerais (all.yml, conf-dhcp.yml), variáveis (vars.yml), arquivos estáticos (files/ para chaves SSH) e templates (templates/). Playbooks específicos por máquina (conf-app.yml, conf-arq-storage.yml, etc.) estão em um subdiretório playbooks/conf/.

Execução:

    vagrant up para provisionar VMs (atualizar MACs em dhcpd.conf.j2 se necessário).

    Executar playbooks Ansible em ordem (gerais, DHCP, arq storage, db, app, cli).

Testes:
Verificação de SSH, sudo, NTP, LVM/NFS (arq), MariaDB (db), Apache/web (app), Firefox/X11 (cli), e autofs em clientes.

Segurança e GitHub:
As chaves privadas SSH (id_rsa.diego, id_rsa.igor) são excluídas do repositório Git via .gitignore e NÃO devem ser enviadas para o GitHub.
