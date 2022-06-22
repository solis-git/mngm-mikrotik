# mngm-mikrotik
Coleção de **playbooks** para gerenciamento de Roteadores MIKROTIK utilizando *AnsibleAWX* e *Community RouterOS Collection*

## Requisitos para utilização dos scripts via AWX

####  Instalação e configuração do servidor AWX

https://github.com/solis-git/installAWX

## Configuração de ambiente

Após completar a instalação e conectar na interface Web é necessário agregar os hosts, inventários, projetos e templates de execução ao AWX para executar os *playbooks* do ansible contra os ativos de rede.


- #### Adicionando Hosts (hosts que não tem entradas no serviço de DNS)

  Quando incluir o host pelo nome o bloco de texto para variáveis deve ter a declaração do IP 

   `ansible_host: <IP.IP.IP.IP>`


- #### Adicionando Inventátios

  Os inventários no AWX nada mais são que um agrupamento de hosts, ou seja, para criar inventário necessito de hosts cadastrados

  ##### Na criação do inventário estão as variáveis de acesso dos ativos (mikrotik routers)
  ```
  ansible_user: <MIKROTIK_SSH_USER>
  ansible_ssh_pass: <MIKROTIK_SSH_PASSWORD>
  ansible_connection: ansible.netcommon.network_cli
  ansible_network_os: community.routeros.routeros
  ```

- #### Adicionando Projetos
  
  Na criação do projeto que se define o repositório (ou acesso local) dos *playbooks* que serão executados contra os ativos. No caso do projeto em questão, todos os *playbooks* para gerenciamento dos ativos Mikrotik estão no mesmo projeto. Fazendo assim que todos *templates* de gerenciamento dos Mikrotiks estejam disponíveis através do mesmo projeto.


- #### Adicionando Templates *(playbooks)*
  
  Os *templates* para gerenciamento dos Mikrotiks vão referenciar o mesmo projeto porque todos os *playbooks* estão no mesmo repositório. 
  
  ##### As variáveis de acesso ao servidor FTP (que armazena os backups dos Mikrotiks) devem estar no *template de backup*
  ```
  ftp_server: <IP.IP.IP.IP>
  ftp_user: <FTP_USER>
  ftp_pass: <FTP_PASSWORD>
  ```
  ##### *PlayBook para adição de * (que armazena os backups dos Mikrotiks) devem estar no *template de backup*
  ```
  ftp_server: <IP.IP.IP.IP>
  ftp_user: <FTP_USER>
  ftp_pass: <FTP_PASSWORD>
  ```
