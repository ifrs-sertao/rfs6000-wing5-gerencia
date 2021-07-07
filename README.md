# rfs6000-wing5-gerencia
A ideia é centralizar aqui comandos e estratégias de gerência das controladores wifi Mototola RFS6000 que estão com sistema Wing5. Se possível usar Ansible.

## Criando uma VLAN usando comandos
Para criar uma nova VLAN é necessário acessar o profile desejado, nesse exemplo o nome do profile é **default-rfs6000**

```shell
profile rfs6000 default-rfs6000

show context


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
