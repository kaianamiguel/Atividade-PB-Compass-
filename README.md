# Atividade-PB-Compass
Versionamento das atividades realizadas no Programa de Bolsas DevSecOps - Compass.uol.

## Objetivos
<p>Subir uma segunda VM (seguindo as mesmas 
regras da atividade anterior);</p>
<p>Criar uma apresentação de slides com 1 slide 
para cada um dos topicos.
</p>

## Instruções
<p> 1 - Subir uma segunda VM (seguindo as mesmas 
regras da atividade anterior); <br />
2 - Mudar a VLAN das 2 para BRIGDE e fazer os 
ajustes necessarios; <br />
3 - Criar uma apresentação de slides com 1 slide 
para cada um dos topicos: <br />
  3.1 - Resuma as informações do arquivo /etc
/passwd <br />
  3.2 - Qual o comando usado para saber se o 
firewall esta ativo? <br />
  3.3 - Explique o permissionamento 754 em um 
arquivo <br />
  3.4 - Explique o que é um NFS
4 - Configurar a relação de confiança entre as 
duas VMs; <br />
5 - Fazer o versionamento da atividade; <br />
6 - Fazer a documentação explicando o processo 
de instalação do Linux
</p>

## Instalando o Linux

<p>[ISO Oracle Linux](https://yum.oracle.com/ISOS/OracleLinux/OL8/u6/x86_64/OracleLinux-R8-U6-x86_64-dvd.iso)
<p/>
  
Na tela INSTALLATION SUMARY, altere em:

#### LOCALIZATION

<p>
KEYBOARD = Portugues; <br/>
LANGUAGE SUPPORT = Inglês; <br/>
TIME & DATE = São Paulo; <br/>
<p/>

#### SOFTWARE

<p>
INSTALLATION SOURCE = Local Media - Não alterar; <br/>
SOFTWARE SELECTION = Minimal Install; <br/>
<p/>
  
#### SYSTEM 

<p>
KDUMP = Enable - Não alterar; <br/>
NETWORK & HOSTNAME = ON Ethernet(enp0s3); <br/>
INSTALLATION DESTINATION = Em "LOCAL STANDARD DISK" selecionar o SDA. <br/>
Para particionar o disco, em "STORAGE CONFIGURATION" selecionar a opção "Custom" e clicar em "Done". <br/>
Surgirá a tela "MANUAL PARTITIONING", clique em "+" para adicionar as partições do disco uma a uma. <br/>
<p/>

Clique em "Done" e em "Accept Changes".

#### USER SETTINGS

<p>  
ROOTPASSWORD = Crie a senha do Root; <br/>
USER CREATION = Crie um usuário; <br/>
<p/>

Clique em "Begging Installation".

## Passo a passo

### Alterando Hostname

Altere o arquivo "hostname":
```
$ sudo vi /etc/hostname
```

Substitua "localhost" pelo nome desejado.

### Fixando IP

Verifique o seu número de IP e Broadcast:

```
$ ifconfig -a
```

Verifique o seu número de Gateway:
```
$ route -n
```

Altere o arquivo /etc/sysconfig/network-scripts/ifcfg-enp0s3:
```
$ sudo vi /etc/sysconfig/network-scripts/ifcfg-enp0s3
```

Adicione as seguintes linhas ao final do arquivo:
```
IPADDR= 192.168.0.103
NETMASK=255.255.255.0
GATEWAY=192.168.0.1 
BROADCATS=92.168.0.255
DNS=8.8.8.8
```

Altere o BOOTPROTO, substituindo "dhcp" por "static".

### Alterando DNS

Altere o arquivo "hosts":
```
$ sudo vi /etc/hosts
```

Inclua uma nova linha com o IP fixo e DNS da VM atual.
Inclua outra linha com o IP fixo e o DNS da VM que será acessada.

### Configurando SSH

Configure o SSH para iniciá-lo automaticamente junto ao sistema:
```
$ sudo systemctl enable sshd
```

Verifique se o SSH está ativo :
```
$ sudo systemctl status sshd
```

### Bloqueando acesso SSH ao root

Edite o arquivo sshd_config:
```
$ sudo vi /etc/ssh/sshd_config
```

Altere a linha "PermitRootLogin", substituindo o "yes" por "no".

### Configurando par de chaves

No terminal do cliente, gere o par de chaves:
```
$ ssh-keygen
```

Copie a chave para a máquina que será acessada:
```
$ ssh-copy-id -i /home/usuário/.ssh/id_rsa.pub usuário@dns_servidor
```

Acesse a outra maquina:
```
$ ssh usuário@dns_servidor -i ~/.ssh/id_rsa
```


