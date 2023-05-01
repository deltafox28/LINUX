# COMANDOS LINUX

## virar sudo:
```sh
sudo -i
```

## verificar ip:
```sh
ip a
```

## atualizar pacotes do sistema operacional:
```sh
sudo apt-get update && sudo apt-get upgrade -y
```

## reiniciar o sistema:
```sh
reboot
```

## limpar a tela:
```sh
ctrl l / clear
```

## sair do fim:
```sh
ctrl c
```

## mostrar o diretorio atual:
```sh
pwd
```

## listar os arquivos e diretorios:
```sh
ls
ls -a (ocultos)
ls -R (arquivos nos subdiretorios)
ls  -al(listar com info)
```

## comando cd:
```sh
cd ~/ (diretorio home)
cd / (diretorio root)
cd - (voltar ao ultimo diretorio)
```

## adicionar, remover, mover e copiar diretorios/arquivos:
```sh
mkdir (adiciona)
rmdir (remove)
mv teste2.txt /teste22 (manda um arquivo para outro diretorio e depois apaga)
mv teste.txt teste1.txt (Muda o nome do arquivo teste.txt para teste1.txt)
mv teste.txt teste.new (supondo que teste.new já exista Copia o arquivo teste.txt por cima de teste.new e apaga teste.txt após terminar a cópia)
cp -r testes /root 
```

## adicionar e remover arquivos:
```sh 
touch (cria um novo arquivo)
rm (exclui arquivos)
```

## comando find:
```sh
find . -name "arquivo.txt" (pelo nome)
find / -type d (diretorios)
find / -type b (bloqueados)
find / -type f (arquivo normal)
Find / -user ziri (arquivos deste usuario)
find / -perm 644 (arquivos com permissao para ler e escrever)
```

## comando locate:
```sh
sudo apt-get update
sudo apt-get install plocate
locate .txt
```

## comando tar:
```sh
: tar -cvf delta.tar /home/ziri(compactar) -- tar -xvf sampleArchive.tar(descompactar) -- tar -tvf sampleArchive.tar(listar) -- tar -xvf sampleArchive.tar example.sh(extrair 1) -- tar -xvf sampleArchive.tar --wildcards '*.jpg'(extrai varios do mesmo padrao) -- tar -rvf sampleArchive.tar example.jpg(adicionar arquivos)

: tar -cvzf delta.tar.gz /home/ziri(compactar) -- tar -xvf sampleArchive.tar.gz(descompactar) -- tar -tvf sampleArchive.tar.gz(listar) -- tar -zxvf sampleArchive.tar.gz example.sh(extrair 1) -- tar -zxvf sampleArchive.tar.gz --wildcards '*.jpg'(extrai varios do mesmo padrao) -- X

: tar -cvjf delta.tar.bz2 /home/ziri(compactar) -- tar -xvf sampleArchive.tar.bz2(descompactar) -- tar -tvf sampleArchive.tar.bz2(listar) -- tar -jxvf sampleArchive.tar.bz2 example.sh(extrair 1) -- tar -jxvf sampleArchive.tar.bz2 --wildcards '*.jpg'(extrai varios do mesmo padrao) -- X
```

## comando gzip
```sh
gzip GG.odt(compactar) -- gzip -d GG.odt.gz(descompactar) -- gzip -l GG.odt.gz(listar) -- gzip -t GG.odt.gz(testa integridade)
```

## descobrir qual a sua shell:
```sh
echo $SHELL
```

## imprime todos os comandos shell:
```sh
set
```

## atribuir mensagem de apresentacao do sistema
```sh
vim /etc/issue
```

## atribuir uma mensagem pós login no sistema:
```sh
vim /etc/motd
```

 ## ver os processos em execucao no linux:
```sh
top
```

## lista todos os comandos anteriores:
```sh
history
```

##  pesquisa ultimo comando no historico:
```sh
Ctrl + R
```

## entrar no diretorio em que todos os comandos ja usados foram salvos:
```sh
vim /bash_history
```

## criação de novos arquivos de texto de forma rápida:
```sh
cat > arquivo.txt -- enter -- Ctrl + D
cat arquivo.txt(visualiza o arquivo)
cat fonte.txt > destino.txt(redirecionar os arquivos)
cat fonte1.txt fonte2.txt > destino.txt(concatenar Arquivos)
```

## lista todos os comandos linux:
```sh
ls /bin
```

## modificar o idioma do linux:
```sh
vim /etc/locale.gen -- locale-gen
```

## # ver todas as informacoes do sistema:
```sh
uname -a
exibir o tipo de hardware:
uname -m
exibir o nome da maquina:
uname -n
exibir o processador:
uname -p
exibir a versao do kernel:
uname -r
exibir o nome do SO:
uname -s
exibir a data da compilacao do SO:
uname -v
```

## exemplo basico de funcao bash
```sh
#!/bin/bash
testfunction(){
echo "My first function"
}
testfunction -- bash ./testFunction.sh
```

## comando grep
```sh
grep --help(manual)
grep cerveja arquivostestes(encontrar palavras em um diretorio)
grep -c cerveja arquivostestes (quantas vezes a palavra pesquisada aparece no arquivo de texto) 
```

## editor de textos:
```sh
comando vim
vim -- i(inserir) -- esc(visualizar) -- :wq(sair e salvar) -- q(sair) 
vim /etc/vim/vimrc(acessar configuracoes)

comando nano
sudo apt-get update -- sudo apt-get install nano
Ctrl + x(sair e salvar)

comando emacs
sudo apt-get update -- sudo apt-get install emacs
Ctrl + x + s (salvar arquivo)
Ctrl + x + c (sair)
```
## acessar outro user a partir do root:
```sh
su menfis
```

## acessar a configuracao de root:
```sh
 vim /etc/sudoers
```

## mostrar os maiores processos sendo consumidos:
```sh
ps aux | head
```

## visualizar processos e recursos em sistemas unix.
```sh
htop
```

##  ver a contagem de memoria atual:
```sh
free -m
```

## ver a particao em que esta sendo usada a memoria swap:
```sh
swapon -s
```

## informacoes sobre uso de memoria virtual
```sh
vmstat -- vmstat 2 5(5 loops, 2 segundos)
```

## sysstat(pacote de softwares para monitoramento da performance do seu sistema Linux.):
```sh
sudo apt update; sudo apt upgrade
sudo apt install sysstat
sudo vim /etc/default/sysstat
ENABLED="true"
sudo systemctl enable sysstat
sudo systemctl start sysstat
sudo systemctl status sysstat.service
```

## comandos sysstat:
```sh
iostat (reporta estatísticas de CPU e estatísticas de entrada/saída para dispositivos, partições e sistemas de arquivos de rede)
mpstat (relata estatísticas individuais ou combinadas relacionadas ao processador)
pidstat (relata estatísticas para tarefas Linux (processos): E/S, CPU, memória, etc)
tapestat (relata estatísticas para unidades de fita conectadas ao sistema)
cifsiostat (relata estatísticas CIFS)
sysstat (é apenas uma página de manual para o arquivo de configuração sysstat, fornecendo o significado das variáveis ​​de ambiente usadas pelos comandos sysstat)
```

## comando sleep(temporizador):
```sh
echo "ziri" ; sleep 10 ; echo "menfis"
```

## entender a hierarquia dos processos no Linux:
```sh
pstree
pstree -np (processos e pid)
pstree -s 408 (mostra um unico processo)
pstree ziri (processos iniciados pelo user)
```

## saber o numero dos processos no linux:
```sh
pgrep -u root ssh (id efetivo)
pgrep -l ssh (pid + nome)
pgrep ssh -d' '(com espaco delimitador)
pgrep -f ssh (usa o nome todo do processo)
pgrep -c systemd-journal (contar)
pgrep -lnu root (encontra o processo mais recente)
```

##  verificar ha quanto tempo o sistema esta ligado:
```sh
uptime
uptime -p
```

## cockpit(ferramenta de monitoramento de software):
```sh
sudo apt-get install cockpit
https:// endereço-ip-da-máquina :9090
```

## arquivos de configuracao administrativo:
```sh
cd /etc/ (diretorio padrao de configuracao)
$HOME (saber o diretorio atual)
cd default/ -- ls (ver valores padrao)
cd security/ (arquivos de autenticacao)
journal ctl (visualizar mensagens diarias)
journalctl --list-boots | head (listar a inicializacao do sistema)
journalctl -k (listar ultimas configuracoes) 
```

## inxi (verificacao de hardware):
```sh
sudo apt install inxi
inxi -F (mostra todas as info do hardware)
inxi -A (para ver informações das placas de som/áudio do computador);
inxi -C (para ver informações gerais apenas do CPU);
inxi -f (para ver informações básicas do CPU e das flags que ele suporta);
inxi -D (para ver informações completas de armazenamento dos SSDs/HDDs, etc);
inxi -n (para ver informações sobre a placa de rede, incluindo o mac);
inxi -G (para ver informações sobre a placa de vídeo);
inxi -l (para ver informações sobre a tabela de partições).
```

## configuracoes iniciais do nosso sistema:
```sh
date (saber a data)
export LANG="pt_BR"
export LANG="en_US"
dpkg-reconfigure tzdata (configurar a timezone)
dpkg-reconfigure locales (configurar locais)
apt-get install console-data -- dpkg-reconfigure console-data (escolher mapa do teclado)
dpkg-reconfigure console-setup (escolher a codificacao do idioma)
```

## Manuseando as atualizações de pacotes no Ubuntu:
```sh
vim /etc/apt/sources.list (repositorios do linux)
sudo apt-get update (atualizar pacotes/programas)
sudo apt-get upgrade -y (atualizar os pacotes)
sudo apt-get dist-upgrade -y (atualizacao mais profunda)
apt full-upgrade
ls -l /var/cache/apt/archives/ (todos os programas que foram atualizados)
cd /var/cache/apt/archives/ -- dpkg -c tcpdump* -- dpkg -l tcpdump -- man tcpdump (obter mais info sobre pacotes atualizados)
man man (visualizador manual do sistema)
man apt (vizualizar comandos de instalacao)
df -h (mostra o espaço livre/ocupado de cada partição)
sudo apt-get autoclean (limpar cache)
apt clean
apt remove wget (desinstalar pacotes)
apt purge wget (desinstalar totalmente os pacotes)
```

## Trabalhando com RPM:
```sh
rpm -qR (Lista todas as dependencias necessárias para o seu pacote)
rpm -Va (Verifica todos os pacotes instalados, bem como a sua integrinidade e a versão que está usando)
rpm -qa (Lista todos os pacotes instalados no seu sistema)
which dnsmasq (O comando which no Linux recebe como argumento o nome de um comando e traz como resultado a localização no disco deste comando, nos diretórios referenciados na variável PATH)
rpm -qf $(which dnsmasq) (verificar o nome completo do pacote)
rpm -qi dnsmasq (descricao geral do pacote)
rpm -ql dnsmasq (mostra a localizacao dos programas do pacote)
rpm -qc dnsmasq (localizar o arquivo de configuracao)
```

## Trabalhando com DNF ( é um gerenciador de pacotes que instala, atualiza e remove pacotes de software no Fedora e é o sucessor do YUM):
```sh
https://fedoraproject.org/wiki/DNF
https://github.com/rpm-software-management/dnf
yum install dnf (instalar o gerenciador de pacotes)
dnf search audacity (procurar programa)
dnf install audacity (instalar programa)
dnf remove audacity (remover programa)
dnf help (lista os comandos)
dnf list (lista os pacotes que tem para instalar)
dnf list | grep zabbix
dnf list installed (lista os programas ja instalados)
dnf list available (disponiveis para download)
dnf check-update (listar atualizacoes pendentes)
dnf list updates (lista atualizacoes)
dnf update bash (atualizar um programa especifico)
dnf update (atualizar todos os programas)
dnf upgrade (atualizar todos os programas)
dnf distro-sync (sincronizar pacotes com as ultimas versoes)
dnf download vim (apenas baixar o editor de texto)
dnf downgrade vim (baixar a versao inferior)
dnf groupinstall 'Development Tools' (instalar varios programas ao mesmo tempo)
dnf groupupdate 'Development Tools' (atualizar)
dnf groupremove 'Development Tools' (remover)
```

## DPKG (responsável pelo gerenciamento de pacotes no LINUX em distribuições baseadas em Debian):
dpkg --help (lista os comandos)
dpkg -l vim (ver se o programa esta instalado)
dpkg -l | grep vim (achar todos os pacotes referentes ao nome vim)
dpkg -l | less (mostrar todos os pacotes do linux)
dpkg -s vim (mostra status do pacote)
wget http://archive.ubuntu.com/ubuntu/pool/main/a/automake-1.15/automake_1.15.1-3ubuntu2_all.deb (acessar arquivos do pacote na web)
dpkg -i automake_1.15.1-3ubuntu2_all.deb (instalar o pacote)
dpkg: erro ao processar o pacote automake (--install) (erro comum)
apt-get install -f (instalar todas as dependencias)
dpkg -l | grep automake

## 



















