## Usando o IPtables para configurar um firewall:
iptables -V (ver a versao do firewall)
iptables -L (ver se existem regras)
iptables -F (limpa as regras atuais)
iptables -S (regras das politicas)
lsmod (lista os módulos do kernel na memoria)
lsmod | grep ip_tables (modulos iptables carregados por padrao)
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT(regra para estabelecer conexoes sem interromper) -- iptables -t [tabela] <ordem> <chain> [condicoes] -j <acao>(sintaxe)    
iptables -A INPUT -p tcp --dport 22 -j ACCEPT(aceitar conexoes de fora do ssh)     
iptables -I INPUT 1 -i lo -j ACCEPT(saber se o server continua funcionando corretamente, comunicacao entre servicos sem serem bloqueados)
iptables -L -v (ver pacotes de entrada e saida)
iptables -A INPUT -j DROP (bloquear as portas do linux) 
sudo apt-get install iptables-persistent (deixar regras salvas quando o terminal for desligado)        
netfilter-persistent save -- reload (salvar as regras atuais)

## Bloqueando pacotes invalidos com iptables:
iptables -t mangle -A PREROUTING -m conntrack --ctstate INVALID -j DROP (criando regra para conexoes invalidas serem bloqueadas)
iptables -t mangle -A PREROUTING -p tcp ! --syn -m conntrack --ctstate NEW -j DROP (bloquear estados novos) 
iptables -L
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT ( liberar o acesso remoto de forma segura usando uma conexão SSH)
sudo iptables -A INPUT -p tcp --dport X -j ACCEPT (Para aceitar conexões que serão cruciais para um determinado processo)
iptables -t mangle -L (especificar a tabela mangle)
sudo nmap -sX 172.31.10.52 (verificar as portas do host)
iptables -t mangle -L -v (regras de bloqueio com mais detalhes)
iptables -t mangle -D PREROUTING 1 (remover regras de prerouting)

## Criando as regras no IPTables com compartilhamento de Internet no Ubuntu:
vim /usr/local/sbin/firewall.sh (criar um shell script para regras de firewall)
 #!/bin/bash

local(){

iptables -t filter -A INPUT -i enp0s3 -p tcp -m multiport --sports 80,443 -j ACCEPT
iptables -t filter -A INPUT -i enp0s3 -p udp -m multiport --sports 53,123 -j ACCEPT
iptables -t filter -A OUTPUT -o enp0s3 -p tcp -m multiport --dports 80,443 -j ACCEPT
iptables -t filter -A OUTPUT -o enp0s3 -p udp -m multiport --dports 53,123 -j ACCEPT

iptables -t filter -A OUTPUT -o enp0s3 -p icmp --icmp-type 8 -d 0/0 -j ACCEPT
iptables -t filter -A INPUT -i enp0s3 -p icmp --icmp-type 0 -s 0/0 -j ACCEPT

iptables -t filter -A INPUT -i lo -j ACCEPT
iptables -t filter -A OUTPUT -o lo -j ACCEPT

iptables -t filter -A INPUT -i enp0s8 -p tcp -m multiport --dports 22,3128 -s 172.31.10.52/20 -j ACCEPT
iptables -t filter -A OUTPUT -o enp0s8 -p tcp -m multiport --dports 22,3128 -d 172.31.10.52/20 -j ACCEPT
}

forwarding(){
iptables -t filter -A FORWARD -i enp0s3 -p tcp -m multiport --sports 80,443 -d 172.31.10.52/20 -j ACCEPT
iptables -t filter -A FORWARD -i enp0s3 -p udp -m multiport --sports 53,123 -d 172.31.10.52/20 -j ACCEPT
iptables -t filter -A FORWARD -i enp0s8 -p tcp -m multiport --dports 80,443 -s 172.31.10.52/20 -j ACCEPT
iptables -t filter -A FORWARD -i enp0s3 -p udp -m multiport --dports 53,123 -s 172.31.10.52/20 -j ACCEPT

iptables -t filter -A FORWARD -o enp0s3 -p icmp --icmp-type 8 -d 0/0 -s 172.31.10.52/20 -j ACCEPT
iptables -t filter -A FORWARD -i enp0s3 -p icmp --icmp-type 0 -s 0/0 -d 172.31.10.52/20 -j ACCEPT

}

internet(){

sysctl -w net.ipv4.ip_forward=1
iptables -t nat -A POSTROUTING -s 172.31.10.52/20 -o enp0s3 -j MASQUERADE

}

default(){
iptables -t filter -P INPUT DROP
iptables -t filter -P OUTPUT DROP
iptables -t filter -P FORWARD DROP

}

iniciar(){
local
forwarding
default
internet
}

parar(){
iptables -t filter -P INPUT ACCEPT
iptables -t filter -P OUTPUT ACCEPT
iptables -t filter -P FORWARDING ACCEPT
iptables -t filter -F
}

case $1 in
start|START|Start)iniciar;;
stop|STOP|Stop)parar;;
restart|RESTART|Restart)parar;iniciar;;
listar)iptables -t filter -nvL;;
*)echo "execute o script com os parametros start ou stop ou restart";;
esac

cd /usr/local/sbin/(acessar o diretorio)
chmod +x firewall.sh (deixar executável)
./firewall.sh start (chamar o arquivo)
iptables -L -n

## protecao basica de ipv6:
ip6tables -A INPUT -i lo -j ACCEPT (conexao local/host)
ip6tables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT (aceitar conexoes estabelecidas)
ip6tables -A INPUT -p tcp --dport ssh -j ACCEPT (liberar porta ssh)
ip6tables -A INPUT -p tcp --dport 53 -j ACCEPT 
ip6tables -A INPUT -p udp --dport 53 -j ACCEPT
ip6tables -L
ip6tables -A INPUT -p icmpv6 --icmpv6-type 1 -j ACCEPT(destino inalcancavel)
ip6tables -A INPUT -p icmpv6 --icmpv6-type 2 -j ACCEPT
ip6tables -A INPUT -p icmpv6 --icmpv6-type 3 -j ACCEPT
ip6tables -A INPUT -p icmpv6 --icmpv6-type 4 -j ACCEPT
ip6tables -A INPUT -p icmpv6 --icmpv6-type 128 -j ACCEPT
ip6tables -A INPUT -p icmpv6 --icmpv6-type 129 -j ACCEPT
ip6tables -A INPUT -p icmpv6 --icmpv6-type 148 -j ACCEPT
ip6tables -A INPUT -p icmpv6 --icmpv6-type 149 -j ACCEPT
ip6tables -A INPUT -j DROP(liberar as regras)

## Criando regras de systemd:
cd /lib/systemd/system (servicos do linux)
vim firewall.service
============================================================
[Unit]
Description= Regras de IPTables vindos do arquivo Firewall.sh
After=network.target

[Service]
Type=oneshot
ExecStart=/usr/local/sbin/firewall.sh start
ExecStop=/usr/local/sbin/firewall.sh stop
KillMode=process
RemainAfterExit=true

[Install]
WantedBy=multi-user.target
=============================================================
systemctl status firewall.service
systemctl enable firewall.service
ls -l /etc/systemd/system/multi-user.target.wants/ (ver se esta em execucao)
systemctl stop firewall.service
systemctl start firewall.service
systemctl status firewall.service

## Compreendendo o Firewalld
https://firewalld.org/documentation/man-pages/firewalld.zones.html
firewall-cmd --get-services (ver em quais servicos o firewall está cadastrado)
cd /usr/lib/firewalld/services (ver arquivos dos servicos)
cat ftp.xml
firewall-cmd
firewall-cmd --get-default-zone
firewall-cmd --get-services
firewall-cmd --list-services
firewall-cmd --list-all ( lista todas as regras existentes no serviço firewalld)
firewall-cmd --list-all --zone=public ( listar apenas as regras de uma determinada zona)
firewall-cmd --add-service=vnc-server
firewall-cmd --list-all 
systemctl restart firewalld 
firewall-cmd --list-all
firewall-cmd --add-service vnc-server --permanent
firewall-cmd --list-all
firewall-cmd --reload
firewall-cmd --add-port=2022/tcp --permanent
firewall-cmd --reload
firewall-cmd --list-all

## Criando um serviço customizado de Firewall
cp /usr/lib/firewalld/services/ssh.xml /etc/firewalld/services
vim /etc/firewalld/services/ssh.xml 
mv /etc/firewalld/services/ssh.xml /etc/firewalld/services/ssh-custom.xml
firewall-cmd --get-services
firewall-cmd --reload(cadastra de forma permanente)
firewall-cmd --get-services
firewall-cmd --add-service ssh-custom --permanent
firewall-cmd --reload
firewall-cmd --list-services 

## Criando as Rich Rules:
REGRAS:
  *port forwarding
  *masquerading
  *rate limiting
  *info de logging
firewall-cmd --zone=dmz --add-rich-rule='rule family=ipv4 source address=10.0.0.100/32 reject' --timeout=60
firewall-cmd --list-all --zone=dmz
firewall-cmd --add-rich-rule='rule service name=http log limit value=3/m accept' --zone=dmz
firewall-cmd --list-all --zone=dmz
firewall-cmd --add-rich-rule='rule protocol value=icmp accept' --zone=dmz
firewall-cmd --add-rich-rule='rule family=ipv4 source address=10.0.0.0/24 port port=7900-7905 protocol=tcp accept' --zone=dmz
firewall-cmd --list-all --zone=dmz
firewall-cmd --zone=dmz --add-rich-rule='rule service name="ssh" log prefix="SSH ATTEMPT: " level="notice" limit value="3/m" accept'

## Como detectar Denial of Service e se proteger deles:
netstat -an
netstat -ntu | grep ESTAB | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -nr
netstat -lan|grep :80|awk {'print $5'}|cut -d: -f 1|sort|uniq -c|sort -nk 1
netstat -an|grep 149.56.180.254
iptables -I INPUT -s 149.56.180.254 -j DROP
netstat -plan|grep :80|awk {'print $5'}|cut -d: -f 1|sort|uniq -c|sort -nk 1

## Trabalhando com NAT e Port Fowarding:
firewall-cmd --permanent --zone=dmz --add-rich-rule='rule family=ipv4 source address=10.0.0.0/24 masquerade'
firewall-cmd --add-forward-port=port=4404:proto=tcp:toport=22:toaddr=192.168.0.210

## Bloqueando Spoofed Addresses:
iptables -F (limpa as regras atuais)
iptables -A INPUT -i lo -j ACCEPT(permitir o acesso ao loopback)
iptables -N blocked-ip (uma tabela para uso)
iptables -I INPUT 2 -j blocked-ip
iptables -A blocked-ip -s 192.168.1.115 -j DROP (bloquear ip)
vim /etc/hosts.conf (editar ordens de configuracao) 

## Bloqueando o trafego recebido:
iptables -F(limpar regras)
iptables -L (visualizar regras)
iptables -S (ver se tem regras criadas)
iptables -A INPUT -i lo -j ACCEPT (permitir o acesso ao loopback)
iptables -A INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT (nao interromper as conexoes ja em andamento)
iptables -A INPUT -p icmp -m icmp-type 11 -j ACCEPT(aceitar pacotes icmp excedidos pelo tempo)
iptables -A INPUT -p icmp -m icmp-type 3/4 -j ACCEPT (aceitar pacotes icmp com destino nao encontrados)
iptables -A INPUT -p icmp -m icmp-type 8 -j ACCEPT (aceitar request/respond do ping)
iptables -N allowed_ip (criar uma nova cadeia de regras)
iptables -A INPUT -j allowed_ip (encaminhar trafego)
iptables -A allowed_ip -p tcp --dport 22 -j ACCEPT ( permitindo o tráfego TCP na porta 22 (usada pelo SSH) para todos os endereços IP na lista "allowed_ip")
iptables -A INPUT -j REJECT --reject-with icmp-host-unreachable ( bloqueia todo o tráfego de entrada que não corresponde a nenhuma regra anterior na cadeia "INPUT", enviando uma mensagem ICMP de volta ao remetente.)

## Alterando regras de firewall com o cockpit(redhat,fedora,centOS):
systemctl enable --now cockpit.socket (habilitar)
ip a
sudo firewall-cmd --list-services 

## Trabalhando com o Firewall UFW
http://wiki.ubuntu-br.org/UFW
sudo apt install ufw
sudo systemctl enable --now ufw
sudo ufw allow 22/tcp (abrir a porta 22)
sudo ufw enable 
sudo ufw status verbose (ver os status)
iptables -L (visualizar regras de iptables)
iptables -L -n | grep 22 
ip6tables -L -n | grep 22
sudo ufw allow 53 (liberar porta 53)
sudo ufw status verbose
cd /etc/ufw -- ls -l -- vim /etc/ufw/before.rules (arquivos de regras) 

#Mangle table added by Donnie
*mangle
:PREROUTING ACCEPT [0:0]
-A PREROUTING -m conntrack --ctstate INVALID -j DROP
-A PREROUTING -p tcp -m tcp ! --tcp-flags FIN,SYN,RST,ACK SYN -m conntrack --ctstate NEW -j DROP
COMMIT

vim /etc/ufw/before6.rules (regras de ipv6)
sudo ufw reload
iptables -L
iptables -t mangle -L
ip6tables -L
ip6tables -t mangle -L
ufw status

## Manipulando os métodos do SELinux
 cat /etc/sysconfig/selinux ( exibir o conteúdo do arquivo de configuração do SELinux)
 sestatus -v (obterá informações detalhadas sobre a configuração e o estado atual do SELinux)
 getenforce (obter o modo de execução atual do SELinux)
 setenforce 0 ( desativa a aplicação de políticas restritivas do SELinux e define o modo de execução como "Permissive")
 getenforce
 vim /etc/sysconfig/selinux 
 SELINUX=disabled (está desabilitado e não está aplicando políticas de segurança restritivas no sistema)
 init 6 (reiniciar o sistema)
 getenforce
 setenforce 1 (modo de execução do SELinux (Security-Enhanced Linux) como "Enforcing")
 vim /etc/sysconfig/selinux
 SELINUX=enforcing ( está ativado e as políticas de segurança restritivas estão sendo aplicadas)
 init 6
 sestatus -v

## Compreendendo as alterações de contexto e a política
cd / (diretorio raiz)
ls -Z (exibir informações de segurança de arquivos e diretórios usando o SELinux)
yum whatprovides semanage ( encontrar qual pacote do sistema fornece o utilitário "semanage")
yum -y install policycoreutils-python
semanage port -a -t mongod_port_t -p tcp 27017 ls -Z /var/www (adicionar a porta TCP 27017 à política de segurança do SELinux com o tipo de porta mongod_port_t. Isso permitiria que o serviço "mongod" utilize essa porta sem violar as políticas de segurança do SELinux)
semanage fcontext -a -t httpd_sys_content_t "/mydir(/.*)?" (regra especifica que todos os arquivos e diretórios localizados em /mydir e seus subdiretórios (usando a expressão regular (/.*)?) devem ter o tipo de contexto de arquivo httpd_sys_content_t.)
semanage fcontext -a -t httpd_sys_content_t "/web(/.*)?" ( regra especifica que todos os arquivos e diretórios localizados em /web e seus subdiretórios (usando a expressão regular (/.*)?) devem ter o tipo de contexto de arquivo httpd_sys_content_t.)

## Configurando um servidor DHCP:
apt install isc-dhcp-server (instalar dependencias dhcp)
ifconfig
vim /etc/default/isc-dhcp-server (definir interface de rede)
cd /etc/dhcp - ls
mv dhcpd.conf dhcpd.conf.bk (mover)
vim dhcpd.conf (criar arquivo do zero)
 #Arquivo de configuracao do dhcp
 # dhcpd.conf
 #
 log- facility local7;
 authoritative;

 # especificando range para a rede local LAN 192.168.88.0/24
 subnet 192.168.88.8 netmask 255.255.255.0 {
        range 192.168.88.10 192.168.88.254;
        option routers 192.168.88.1;
        option domain-name "seu dominio.com.br";
        option domain-name-servers 8.8.8.8,208.67.222.222;
        deafult-lease-time 1800;
        max-lease-time 7200;
        min-lease-time 1000;
 }

# fixar um ip pelo MAC Address
  host windows10 {
          hardware ethernet 08:00:27:cb:76:cB;
          fixed-address 192.168.88.210;
 }
systemctl restart isc-dhcp-server
systemctl status isc-dhcp-server

## Configuracoes extras:
less /var/lib/dhcp/dhcpd.leases (visualizar ips)
less /etc/default/isc-dhcp-server (visualizar interface de rede configurada)
