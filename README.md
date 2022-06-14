# mngm-mikrotik
Coleção de **playbooks** para gerenciamento de Roteadores MIKROTIK utilizando *AnsibleAWX* e *Community RouterOS Collection*


### adicionando hostnames sem serviço de DNS

Quando incluir o host pelo nome o bloco de texto para variáveis deve ter a declaração do IP 

`ansible_host: <IP.IP.IP.IP>`

### na criação do inventário estão as variáveis de acesso dos ativos (mikrotik routers)

`ansible_user: <MIKROTIK_SSH_USER>`\
`ansible_ssh_pass: <MIKROTIK_SSH_PASSWORD>`\
`ansible_connection: ansible.netcommon.network_cli`\
`ansible_network_os: community.routeros.routeros`

##### OBS: na criação do projeto que se define o repositório (ou acesso local) dos *playbooks* que serão executadas contra os ativos

### na criação dos templates de backup estão as variáveis de acesso ao servidor FTP que armazaena os backups dos ativos

 `ftp_server: <IP.IP.IP.IP>`\
 `ftp_user: <FTP_USER>`\
 `ftp_pass: <FTP_PASSWORD>`
