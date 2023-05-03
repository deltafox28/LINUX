## Compreendendo os serviços de inicialização do Linux
```sh
: http://docs.fedoraproject.org/en-US/quick-docs/understanding-and-administering-systemd
: ps -e | head (ver o sistema de inicializacao)
: init 0 (desliga o sistema de forma abrupta)
: Temos uma tabela la no Viva O Linux muito bem escrito: https://www.vivaolinux.com.br/artigo/Runlevel
```

## Trabalhando com Systemd
```sh
: cd /usr/lib/systemd/system (arquivos de configuracao do sistema,todos os servicos do linux)
: cd /etc/systemd/system (modificações feitas pelo administrador do sistema (usuário))
: cd /run/systemd/system ()
: less /usr/lib/systemd/system/tmp.mount (ver os padroes do arquivo)
: systemctl show (ver unidades disponiveis para utilizar)
: systemctl show sshd 
: sudo apt-get -y install vsftpd (file transfer protocol)
: systemctl start vsftpd 
: systemctl enable vsftpd (habilitar startup)
: systemctl status vsftpd 
: ls /etc/systemd/system/multi-user.target.wants (servicos rodando automaticamente)
: systemctl list-dependencies vsftpd (dependencias do sevico)
: systemctl --type=service (Mostra apenas unidades de serviço)
: systemctl list-units --type=service --all (Mostra unidades de serviço inativas, bem como unidades de serviço ativas)
: systemctl --failed --type=service (Mostra todos os serviços que falharam)
```

##  Compreendendo o PID 1 (primeiro processo a ser inicializado)
```sh
: ps -ef (ver processos em execucao)
: pstree -p (visualizar processos pais/filhos)
: pstree -sp 819 (filtrar)
: ps -eo pid,user,stat,comm (ver info completa dos servicos)
: man 1 pstree (info do comando)
: man 1 ps (info do comando)
```

## Estados dos serviços com systemctl
```sh
: systemctl (nos auxilia no controle do próprio systemd e, também, atua como gerenciador de serviços)
: systemctl > /tmp/systemctl-units.txt (cria um arquivo de saida para resultados)
: systemctl --all (todos os servicos)
: systemctl list-unit-files (filtrar servicos de unidade)
: systemctl list-unit-files --type=service (servico(podemos alterar) + unidade)
: systemctl list-unit-files --type=service --state=enabled (filtra servicos ativados)
: systemctl list-unit-files --type=service --state=disabled (filtra servicos desativados)
: systemctl list-unit-files --type=service --state=static (filtra servicos estaticos)
: systemctl list-unit-files --type=service --state=masked (servico mascarado)
: systemctl list-unit-files --type=service --state=indirect (servicos que dependem de outros servicos)
: systemctl list-unit-files --type=service --state=generated (convertidos pelo sistema)
```

## Vendo a compatibilidade com os sistema SysVinit
```sh
: init 0 ()
: https://www.vivaolinux.com.br/artigo/Runlevel 
: init 3 ()
: runlevel (Runlevel é o nível de inicialização do sistema. Este nível decide qual serviço vai inicializar ou não com o sistema.)
: telinit 6 ()
: runlevel0.target (Logo após iniciar o sistema o mesmo já finaliza todos os serviços e desliga)
: runlevel1.target (Temos ao iniciar o modo monousuário)
: runlevel2.target (Temos os modos multiusuários que podem ser tanto gráfico como no modo texto)
: runlevel3.target 
: runlevel4.target 
: runlevel5.target 
: runlevel6.target (Modo que reinicia a máquina)
: ls -l /lib/systemd/system/runlevel*.target (ver os runlevel que existem)
: ls -l /etc/systemd/system/default.target (RHEL/CentOS) (qual config esta sendo usada)
: ls -l /etc/systemd/system/default.target.wants (Debian/Ubuntu) (qual config esta sendo usada)
: systemctl show --property "WantedBy" getty.target (visualizar o comando)
```

## Gerenciando os Units
```sh
: iptables (O iptables é um programa escrito em C, utilizado como ferramenta que configura regras para o protocolo de internet IPv4 na tabela de filtragem de pacotes, utilizando os módulos e framework do kernel Linux)
: firewalld (firewalld é uma ferramenta de gerenciamento de firewall para sistemas operacionais Linux. Ele fornece recursos de firewall atuando como um front-end para o framework netfilter do kernel Linux. O backend padrão atual do firewalld é nftables. Antes da v0.6.0, o iptables era o backend padrão) 
: yum install -y iptables-services / sudo apt install iptables-persistent ()
: less /lib/systemd/system/iptables.service; alias / less /lib/systemd/system/firewalld.service; alias(verificar se ha conflitos)
: sudo apt-get install firewalld
: systemctl status iptables 
: systemctl status firewalld 
: systemctl start firewalld 
: systemctl start iptables 
: systemctl mask iptables (forcar a ativacao)
: systemctl --type=target (inclusao de units)
: systemctl --type=target --all 
```

##  Configurando um nível de execução
```sh
: ls -l /etc/systemd/system/default.target (RHEL/CentOS) (ver o modo target)
: ls -l /usr/lib/systemd/system/default.target  (Ubuntu/Debian) (ver o modo target)
: ls -l /lib/systemd/system/runlevel5.target (RHEL/CentOS) (modificar)
: ls -l /usr/lib/systemd/system/runlevel5.target (Ubuntu/Debian) (modificar)
: systemctl get-default (verificar unidade de destino)
: systemctl set-default runlevel3.target (mudar unidade de destino)
```

## Adicionando novos serviços ao systemd
```sh
: vim test.py3 (criar arquivo em python)
   #!/usr/bin/python3
   import time 
   def your_function():
   print("Hello, World")
   while True:
   your_function()
   time.sleep(10) 
: python3 test.py3 (chamar o programa)
: cd /etc/systemd/system (arquivos de servicos)
: vim /etc/systemd/system/test.service
  [Unit]
  Description=Python test 
  After=network-online.target
  Wants=network-online.target systemd-networkd-wait-online.service
  [Service]
  Type=simple
  Restart=on-failure
  RestartSec=5s
  User=root
  ExecStart=/usr/bin/python3 /root/test.py3
  [Install]
  WantedBy=multi-user.target
: systemctl start test
: systemctl status test
: systemctl enable test
 (outras configuracoes)
  After=mysqld.service
  Restart=always
  RestartSec=1
  StartLimitBurst=5
  StartLimitIntervalSec=10: StartLimitAction=reboot
```

## Compreendendo o serviço Cron (O utilitário de linha de comando cron é um agendador de tarefas em sistemas operacionais do tipo Unix)
```sh
: vim /etc/crontab (arquivo de configuracao)
: crontab -e (editor)
: tarefa 1
  30 22 2,10 * * root echo "Teste de Cron!" 
: tarefa 2
: 30 22 2,10 * * root systemctl restart sshd
: Para Ubuntu/Debian, não precisa colocar o nome do usuário:
: tarefa 1
: * * * * * echo "Teste de Cron!"
: tarefa 2
: 1 * * * * echo "Teste de Cron!"
: systemctl restart crond.service (RHEL/CentOS)
: Ou:
: systemctl restart cron.service  (Debian/Ubuntu)
: E para deixar habilitado:
: systemctl enable cron.service (Debian/Ubuntu)
  Ou
: systemctl enable crond.service (RHEL/CentOS)
: tail -100 /var/log/syslog (mensagens de log)
  Ou
: tail /var/log/cron
: @daily /usr/local/bin/db_backup.sh (backup de arquivo)
```

## Trabalhando com GRUB
```sh
: cat /etc/default/grub
: GRUB-CMDLINE_LINUX
: vim /etc/default/grub
: "rhgb" e "quiet"
: Altere o GRUB_TIMEOUT para 10 segundos
: grub2-mkconfig > /boot/grub2/grub.cfg
```

## agendando tarefas com anacron:
```sh
: apt install anacron
: vim /etc/anacrontab
: ls -l /var/spool/anacron/ (arquivos de exemplo)
: @daily  10  example.daily  /bin/bash /root/backup.sh (executar comando)
```

## agendando tarefas com at:
```sh
: apt install at
: sudo systemctl enable --now atd (ativar)
: at 09:00 (abre a shell) -- tar -xf /home/arthur/file.tar.gz (criar backup)
: echo "comando a ser usado" | at 09:00 (mais facil)
: at 09:00 -M (enviar email)
: at sundady +10 minutes (programar data)
: at 1pm + 2 days (uma da tarde daqui ha dois dias)
: at 12:30 102120 (mes,dia,ano)
: batch (executados em lote)
: atq (trabalhos pendentes) -- atrm 9 (remover trabalho)
```

## Configuração da data em nosso Linux
```sh
: date (mostra data e hora)
: date -s "19 Dec 2020 12:00:00" (alterar)
: timedatectl (mostra as horas,time zone)
: timedatectl set-ntp false (ver se esta inativo)
: timedatectl status
: date +%T -s "20:00:00" (alterar apenas hora)
: date +%H -s "4" (alterar apenas hora)
: timedatectl set-time 10:00:00 (alterar apenas hora)
: timedatectl set-time 2019-03-01 (alterar data)
: timedatectl set-time '2019-03-01 10:00:00' (alterar data e hora)
: date
: timedatectl list-timezones (mostra as timezone)
: timedatectl set-timezone America/Sao_Paulo (aciona timezone)
: ls -l /etc/localtime (visualizar a configuracao)
: cat /etc/timezone (Debian/Ubuntu)
```

## Usando os comandos tzselect tzconfig hwclock
```sh
: tzselect (alterar fuso horario do sistema) -- 2 2 9 8
: dpkg-reconfigure tzdata -- tzconfig (interface grafica para fuso horario)
: ln -s /usr/share/zoneinfo/America/Sao_Paulo localtime (linkar)
: cat /etc/timezone
: hwclock (ver hora)
: hwclock --systohc (ajuste da data e hora do hardware baseado nas informações do sistema)
: hwclock --hctosys (Configura a hora do sistema (kernel) a partir do relógio de hardware)
: cat /etc/adjtime
: hwclock -r --utc (Lê o relógio de hardware e mostra o valor na saída padrão)
: hwclock --show --utc
```

## Criando um backup simples
```sh
: df -h (ver diretorios das memorias)
: cp -auv arquivo.txt /run/media/root/A5C9-19F1/backup (backup no pendrive)
: cp -uv -R /home/arthur/ /run/media/root/A5C9-19F1/backup (diretorio home)
: ls -l /run/media/root/A5C9-19F1/backup
: crontab -e (agendar tarefa)
: 30 01 * * * /bin/cp -uv -R /home/arthur /run/media/root/A5C9-19F1/backup
: lsblk (ver particoes removiveis)
```

## Trabalhando com rsync
```sh
: rsync -avl root@192.168.0.25:/usr/share/man/man1/ /tmp/ (fazer backup)
: rsync -avl root@192.168.0.25:/usr/share/man/man1 /tmp (verifica que ja foi copiado, evita duplicidade)
: ls -l /usr/share/man/man1/batch* /tmp/man1/batch* (verificar tamanho do arquivo)
: rsync --help
: cp /etc/services /usr/share/man/man1 (fazer a copia de servicos no diretorio)
: rsync -avl root@192.168.0.25:/usr/share/man/man1 /tmp (fazer o backup dos novos arquivos)
: sudo rm /usr/share/man/man1/services (remover)
: rsync -avl root@192.168.0.25:/usr/share/man/man1 /tmp (foi encrementado)
: rsync -avl /usr/share/man/man1 localhost:/tmp (backup local)
: rsync -avl --delete root@192.168.0.25:/usr/share/man/man1 /tmp (remover)
```

## Incluir ou excluir dados por listas
```sh
: vim include-list.txt 
 # include file list
  anaconda-ks.cfg
  /Documentos/
  /Imagens/
  teste.txt
  /Downloads/
: rsync -av ~ --files-from ~/include-list.txt /backup/pendrive/ (fazer o backup da lista)
: vim exclude-include-list.txt (criar novo dir)
 #exclude-include-list.txt
 #incluir o diretório inicial /root/
  + /root/
 #Incluir o arquivo 'teste.txt'
  + /root/teste.txt
 #Incluir arquivos ocultos
  + /root/.bash_history
  + /root/.bashrc
 #Excluir todos os outros arquivos ocultos
  - /root/.*
 #Incluir todo o diretório "/Documentos/DOC1" e o arquivo 'teste2.txt'
  + /root/Documentos/
  + /root/Documentos/DOC1/
  + /root/Documentos/teste2.txt
 #Exclui todo o restante do diretório '/root/Documentos/'
  - /root/Documentos/* 
 #Exlui todo o resto
  - /root/*
: rsync -av ~ --exclude-from=/root/include-list.txt /backup/pendrive/
: vim exclude-include-list2.txt (criar novo dir)
# exclude-include-list2.txt 
# include home directory
  + /root/
# include all ogg and flac files
  + *.txt
  + *.zip
# excluir arquivos do tipo .wav de cache e temp
 - *.wav
 - cache*
 - temp*
 - /root/*
: rsync --bwlimit=512 -av ~ --exclude-from=/root/exclude-include-list2.txt /backup/pendrive/ (apagar backup)
```

## Servidor de backup rsync
```sh
RHEL/CentOS 
 dnf install rsync
 dnf install rsync-daemon
 Ubuntu/Debian
 apt install rsync (instalar)
 RHEL/Centos:
 firewall-cmd --permanent --add-service rsyncd && sudo firewall-cmd --reload (liberar a porta)
 Ubuntu Server:
 ufw allow 873/tcp (liberar a porta)
 vim /etc/rsyncd.conf (editar/criar)
  #/etc/rsyncd: configuration file for
  rsync daemon mode
  See rsyncd.conf man page for more options.
  modules
  [backup_server]
  path = /backups
  comment = "backup público"
  list = yes
  read only = no
  use chroot = no 
  uid = 0
  gid = 0
 sudo mkdir /backups/ (criar o caminho)
 sudo chmod 0700 /backups/ (dar permissoes)
 sudo systemctl start rsync.service
 rsync localhost:: (cria o servidor na maquina)
 hostname -I (saber ipv4 e ipv6)
 rsync 172.31.10.52::
 rsync -av teste2.txt 172.31.10.52::backup_server
 vim /etc/rsyncd.conf
 exclude = *.txt (proibir essas extensoes)
 systemctl restart rsyncd.service 
```

## Aumentando a segurança do rsync
```sh
vim /etc/rsyncd-users 
  rsync-users 
  ubuntu:12345
  user1:56789
  user2:12345
 chmod 0600 /etc/rsyncd-users
 ls -la /etc/rsyncd-users (verificar as permissoes)
 vim /etc/rsyncd.conf
  [servidor_backup]
  path = /backups/privado
  comment = Backup Privado
  list = yes
  read only = no
  auth users = vitor
  secrets file =/etc/rsyncd-users
  use chroot = no
  strict modes = yes
  uid = root
  gid = root
  hosts allow = *.local.srv
  hosts allow = IP
 sudo mkdir -p /backups/privado/
 sudo chmod -R 0700 /backups/privado/
 systemctl restart rsync.service
 rsync -av ~/backup vitor@192.168.15.69::servidor_backup
```
