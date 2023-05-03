##  criar um novo usuário no Linux:
```sh
sudo adduser username
sudo passwd username
sudo adduser -m username (criar o diretório inicial do usuário como /home/username)
ls -la /home/username/
deluser username
deluser username –remove-home
passwd –l username (bloquear a conta)
passwd –u username (desbloquear a conta)
```

## criar um novo grupo:
```sh
groupadd newgroup
adduser username newgroup (adicionar os usuários desejados ao grupo)
groups username (mostrar os grupos a qual o usuário pertence)
```

## chmod, whoami e chown(altera o proprietário do arquivo ou diretório especificado pelo parâmetro Arquivo ou Diretório para o usuário especificado pelo parâmetro Proprietário ):
```sh
Read    4
Write   2
Execute 1
whoami 
chmod 750 pasta 
ls -l
chown novousuario /TestUnix (alterar o usuário dono do diretório)
chown novousuario:novogrupo /TestUnix
chown :novogrupo /TestUnix
```






