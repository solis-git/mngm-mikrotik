# mngm-mikrotik
Coleção de **playbooks** para gerenciamento de Roteadores MIKROTIK utilizando *AnsibleAWX* e *Community RouterOS Collection*

#### Documentação completa do Módulo para MikroTik *RouterOS*
https://docs.ansible.com/ansible/devel/collections/community/routeros/

#### *FACTS* Retornados por este módulo (lista de cahves *facts* disponíveis)
https://docs.ansible.com/ansible/devel/collections/community/routeros/facts_module.html#ansible-collections-community-routeros-facts-module

Coleta um conjunto básico de *facts* de um dispositivo remoto que está executando o RouterOS. Este módulo precede todas as chaves de *facts* da rede básica com ansible_net_<fact>. O módulo de *facts* sempre coletará um conjunto básico de *facts* do dispositivo e pode habilitar ou desabilitar a coleta de *facts* adicionais.

**gather_subset**, quando fornecido esse argumento restringirá os fatos coletados a um determinado subconjunto. Os valores possíveis para este argumento incluem `all`, `hardware`, `config`, `interfaces` e `routing`. Pode especificar uma lista de valores para incluir um subconjunto maior. Os valores também podem ser usados com uma inicial ! para especificar que um subconjunto específico não deve ser coletado. Padrão: `!config`

##### Exemplos
```
- name: Collect all facts from the device
  community.routeros.facts:
    gather_subset: all

- name: Collect only the config and default facts
  community.routeros.facts:
    gather_subset:
      - config

- name: Do not collect hardware facts
  community.routeros.facts:
    gather_subset:
      - "!hardware"
```


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
  
  ##### PlayBook para adição de regras no Firewall Mikrotik
  - As variáveis abaixo devem estar no template que irá executar o playbook **mkt-add-firewall-rule.yml**
  - **ATENÇÃO:** Os valores abaixo são somente exemplos, esse variáveis definem as propriedades da regra que será adicionada ao firewall

  ```
  NETofINTERFACE: 192.168.0.0/16
  PORTofSERVICE: 80
  PROTO: tcp
  ACT: drop
  CHAIN: input
  COMMENTofRULE: '"bloqueio entrada porta 80"'
  ```
