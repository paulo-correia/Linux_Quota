# Quota

A cota de disco é um limite definido pelo administrador do sistema que restringe a certos aspectos do uso do sistema de arquivos em sistemas operacionais modernos. A função do uso de cotas de disco é alocar espaço em disco limitado de forma razoável.

## Instalação

Toda a instalação é feita como **root**

### Debian

Instalar o pacote quota com o comando:

 `apt-get install quota`

### CentOS

Instalar o pacote quota com o comando:

 `yum -y install quota`

## Configuração

Editar o arquivo etc/fstab

E onde está o defaults complete com ,usrquota,grpquota

Exemplo:

`/dev/sdx /montagem tipo_de_file_system  defaults,usrquota,grpquota 1 1`

Remonte a montagem com o comando:

 `mount -o remount /montagem`

Verifique se montou corretamente com o comando:

 ```
mount | grep /montagem <br></br>
O retorno deve ter quota,usrquota,grpquota dentro dos parenteses
```

Crie o banco de dados com o comando:

 `quotacheck -cugv /montagem`

Habilitar as quotas

 `quotaon /montagem`

Adicionar quota por usuário

 `edquota -u usuário`

Exemplo:

Disk quotas for user usuário (uid xxxx):

|Filesystem | blocks | soft | hard | inodes | soft | hard |
|-----------|--------|------|------|--------|------|------|
| /dev/sdbx |  0     |  0   |  0   |  0     |  0   |  0   |


Onde os limites são colocados em Kbytes nos primeiros campos soft e hard

512000 são 512 MB

Adicionar quota por grupo

 `edquota -g grupo`

Exemplo:

Disk quotas for group grupo (uid xxxx):

|Filesystem | blocks | soft | hard | inodes | soft | hard |
|-----------|--------|------|------|--------|------|------|
| /dev/sdbx |  0     |  0   |  0   |  0     |  0   |  0   |

Onde os limites são colocados em Kbytes nos primeiros campos soft e hard

512000 são 512 MB

Verificação de quotas

 ```
quota -uv usuário
quota -gv grupo
```

Relatório

 `repquota /montagem`

Desabilitar quotas

 `quotaoff /montagem`

# Envio de e-mail

**Obs**: requer SMTP configurado

Editar o arquivo /etc/quotatab adicionar no final do arquivo:

 `/dev/sdx:nome`

Editar o arquivo /etc/warquota.conf

 ```
FROM      = "root@localhost" (Colocar o From)
SUBJECT   = Assunto
CC_TO     = "root@localhost" (Colocar o e-mail de cópia)
SUPPORT   = ""
MESSAGE   = O usuario %i excedeu a cota em disco
SIGNATURE = ""
```

Colocar no cron o comando:

 `warnquota -s`