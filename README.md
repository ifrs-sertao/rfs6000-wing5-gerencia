# rfs6000-wing5-gerencia
A ideia é centralizar aqui comandos e estratégias de gerência das controladores wifi Mototola RFS6000 que estão com sistema Wing5. Se possível usar Ansible.

## Criando uma VLAN usando comandos
Para criar uma nova VLAN é necessário acessar o profile desejado, nesse exemplo o nome do profile é **default-rfs6000**

```shell
profile rfs6000 default-rfs6000

show context

```
## Confirar porta TRUNK controladora RFS6000
```shell
profile rfs6000 default-rfs6000
interface up1
switchport trunk allowed vlan add 1-2000
commit
commit write
```
## Criar rede WIRELESS
O comando wlan <NAME> cria uma rede Wireless se não existir.
```shell
rfs01(config)#wlan IOT
rfs01(config-wlan-IOT)#broadcast-ssid
rfs01(config-wlan-IOT)#vlan 610
rfs01(config)#commit write
```

## Mostrar as redes wireless criadas 
```shell
rfs01#show wireless wlan 
-----------------------------------------------------------------------
  WLAN               SSID         CLIENTS            RADIOS             
-----------------------------------------------------------------------
LOCAL              primei..acesso 14                 52                 
radio              SertaoAplicado 3                  52                 
IFRS-Alunos        IFRS-Alunos    2                  39                 
IFRS-SERTAO        IFRS-SERTAO    0                  52                 
IFRS-Visitantes    IFRS-V..antes2 0                  13                 
IFRS-Professores   IFRS-P..ssores 0                  26                 
IFRS-Adm..trativos IFRS-A..ativos 0                  26                 
-----------------------------------------------------------------------
Total number of wlans displayed: 7
```
## Atribuindo Wireless ao profile do AP
É necessário atribuir as WLAN criadas à interface radio do AP (se for dual band atribuir em cada frequência 2.4 e 5GHz)
```shell
rfs01(config)#profile ap621 default-ap621
rfs01(config-profile-default-ap621)#interface radio 1
rfs01(config-profile-default-ap621-if-radio1)#wlan IOT
rfs01(config-profile-default-ap621-if-radio1)#commit write
```
  
## show context 
Mostra as configurações do contexto atual, por exempl no default-rfs6000 constarão todas as configurações de perfil tais como configurações VLAN, STP, Portas, etc.
```shell
interface vlan100
  description VISITANTES
  ip address dhcp
```
## commit e commit write
O comando **commit** isolado funciona como um teste de sintaxe, covém usá-lo primeiro. Se o commit passar sem erros, lembre-se sempre de executar **commit write** para que as configurações fiquem persistentes.

```shell
commit
```
Se commit não der erros, execute:
```shell
commit write
```
## Ajustes na porta Trunk de um AP ( ge 1) 
Comandos para configurar porta trunk do AP e add VLANs

Comando para acessar uma AP específico via MAC (precisa estar no contexto config)
```shell
ap621 5C-0E-8B-E9-AC-95
```
Acessando a interface trunk (uplink) do AP
```shell
interface ge 1
```
Configura porta Trunk, define VLAN Nativa e adiciona VLANs
```shell
switchport mode trunk
switchport trunk native vlan 911
switchport trunk allowed vlan add
912,540,525,40
commit 
commit write
```
