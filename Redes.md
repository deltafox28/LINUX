## fundamentos de rede
```sh
: ip (configurar e monitorar)
: ip addr (info basica)
: ip a s (mais informacoes da placa de rede)
: ip route (tipo de rota do gateway)
: ip link (verificar o estado do link da rede)
: ip -s link (informacoes da placa de rede)
: ip route show (informacoes de rota)
: ss (quais portas estao abertas, visualizar) -- ss -lt (somente portas abertas)
```

## Configurando IP e Netmask no ubuntu
```sh
:  cd /etc/netplan --  ls -- vim 00-installer-config.yaml
: vim /etc/netplan
: "NetworkManager"
: "networkd"
: This file describes the network interfaces available on your system
: For more information, see netplan(5).
: network:
   version: 2
   renderer: networkd
   ethernets:
   enp0s8:
   dhcp4: yes
   dhcp6: no
: This file describes the network interfaces available on your system
: For more information, see netplan(5).
: network:
 version: 2
 renderer: networkd
 ethernets:
 enp0s3:
 dhcp4: no
 dhcp6: no
 addresses: [192.168.0.32/24]
 gateway4: 192.168.0.1
 nameservers:
 addresses: [8.8.8.8,208.67.222.222]
: sudo netplan apply
```

## Configurando hosts e hostname
```sh
: vim /etc/hosts (localizar hostnames)
: IP FQDN HOSTNAME ALIAS 
: Ex:192.168.0.23 ubuntu.vmzsolutions.srv master
: vim /etc/hostname (alterar nome da maquina)
: hostname -i (saber o endereco ip)
: hostname -f  (nome atual do hostname)
: hostname -d  (saber se tem dominio)
: hostname -v (outras opcoes)
```

## Trabalhando com nmtui e nmcli no RHEL/centOS
```sh
: nmcli (configurar assuntos relacionados a rede)
: nmcli con show (verificar tipos de placa de rede conectadas)
: nmcli con show enp0s3 (especifica uma placa de rede, infos detalhadas)
: nmcli dev status (status das conexoes)
: nmcli con add con-name "dhcp" type ethernet ifname enp0s8 (criar conexao)
: nmcli con add con-name "static" ifname enp0s8 autoconnect no type ethernet ip4 10.0.0.10/24 gw4 10.0.0.1 (adicionar interface de rede)
: nmcli con show 
: nmcli con up "static" (subir conexao)
: nmcli con up "dhcp" (subir conexao)
: man nmcli-examples (verificar info sobre esse comando)
: nmtui (interface grafica para editar conexoes)
: hostnamectl status (visualizar nome completo da maquina)
: cat /etc/hosts ( arquivo usado pelo sistema operacional para converter nomes de host em endereços IP)
```

## Verificando sua rede na linha de comando
```sh
ip addr show (informacoes sobre o ip)
 ip a s (info sobre ip)
 ip (mostra as opcoes)
 ifconfig (inutilizado)
 ifconfig enp0s3 (inutilizado)
 route (rotas de saida)
 traceroute google.com (visualizar as rotas para um dominio) 
 hostname (mostra o nome do host atual) 
 dnsdomainname
```

## Arquivos de interface de rede no RHEL/CentOS
```sh
: cd /etc/sysconfig/network-scripts (onde ficam as interfaces de rede)
: vim /etc/sysconfig/network (configurar o gateway)
: GATEWAY=192.168.0.1
: vim /etc/resolv.conf (configurar servidor dns)
: vim /etc/authselect/user-nsswitch.conf (onde ficam as configuracoes de rede)
: authselect apply-changes (aplicar mudancas)
: host redhat.com (endereco dns)
: dig redhat.com (endereco dns, arquivos locais)
```

## Verificando a sua placa de rede
```sh
: lspci | grep -i ether (verificar quais sao as interfaces de rede)
: mii-tool (opcoes)
: mii-tool -w enp0s3 (visualizar status do link)
: ethtool enp0s3 (visualizar status do link)
: mii-tool -F 100baseTx-HD enp0s3 (alterar para halfduplex)
: mii-tool -r enp0s3 (entrada e saida)
: mii-tool -F 100baseTx-FD enp0s3 (alterar para fullduplex)
: mii-tool -w enp0s3
: ethtool  -h (opcoes)
: ethtool enp0s3 
: ethtool -S enp0s3 (verificar estatisticas de pacotes)
: ethtool -S enp0s3 | grep -i erro 
```

## comandos ping e fping
```sh
: ping 192.168.0.1 (gerenciar o status de conectividade da rede com um dispositivo através de um IP)
: ping fb.com 
: Tem mais teoria nesse link:https://web.archive.org/web/20200810185445/https://www.cloudflare.com/pt-br/learning/cdn/glossary/time-to-live-ttl/
: ping -c 1 fb.com -4 (mais info) 
: ping -I enp0s3 fb.com -4 (especificar interface de rede origem)
: sudo apt install fping  [On Debian/Ubuntu] ( digitalizar e encontrar as máquinas conectadas e administradas em uma rede)
: sudo dnf install fping  [On CentOS/RHEL] 
: fping 192.168.0.1 ()
: fping -b 12 -C3 192.168.0.1 (especificar numero de tamanho de pacotes)
: fping -b 10000 -C3 192.168.0.1 (especificar numero de tamanho de pacotes)
: fping -c 1 -g 192.168.0.0/24 (testar a subrede)
: sudo apt install hping3  [Debian/Ubuntu] 
: sudo dnf install hping3  [CentOS/RHEL] 
: hping3 -c 100 -i u1000 192.168.0.1 (permite um maior numero de pacotes icmp por segundo)
: hping3 -2 -c 2 192.168.0.1 ()
```

## Trabalhando com Roteamento
```sh
: netstat -nr (ver o roteamento)
: netstat -nr -6 (para ipv6)
: route -n  (visualizar rotas de saida)
: route -n -6  (para ipv6)
: traceroute fb.com  (visualizar as rotas para um dominio) 
: traceroute fb.com -6  (para ipv6)
: tcpdump -lni enp0s3 host fb.com (analisar como o pacote trabalha)
: ping fb.com -4 ou -6  (status de conectividade)
: route add default gw 192.168.0.1 dev enp0s3 (adicionar rota estatica)
: route (mostra configuracoes de roteamento)
: route del default gw 192.168.0.1 dev enp0s3 (remove)
: route 
: ip -6 route (mostra o endereco ipv6, gerencia rotas estáticas na tabela de roteamento)
: ip -6 route add ::/0 via 2804:5990:c002:7301:: dev enp0s3 (adicionar rota estatica)
: ip -6 route 
: ip -6 route del ::/0 via 2804:5990:c002:7301:: dev enp0s3 (remove)
: ip -6 route
```

## Monitoramento da rede
```sh
: yum install arpwatch (analisar enderecos ips)
: apt install arpwatch 
: systemctl start arpwatch (ativar)
: systemctl enable arpwatch (habilitar)
: systemctl start arpwatch@enp0s3 (especificar interface)
: tail /var/log/messages (RHEl/CentOS)
: tail /var/lib/arpwatch/enp0s3.dat (Ubuntu) (ver os logs)
: apt install iptraf-ng 
: yum install iptraf-ng 
: iptraf-ng (interface grafica para trafego de ip)
: apt install darkstat (mini platafoma web, graficos)
: darkstat -i enp0s3 (configura a placa de rede)
: vim /etc/darkstat/init.cfg (configurar pelo arquivo para ficar automatico)  
: http://192.168.1.6:667/ ()
: https://etherape.sourceforge.io/ 
: apt install etherape (via desktop,é uma ferramenta de monitoramento de tráfego de rede/sniffer de pacotes, desenvolvida para Unix)
: etherape (chamar o programa)
: https://etherape.sourceforge.io/download.html#sources 
```

## Monitoramento da rede Parte 2
```sh
: https://iperf.fr/ (aplicativo para saber o tempo de resposta ente maquina e servidor)
: apt install iperf 
: yum install iperf3 
: iperf -c fb.com -p443 (medir a taxa de transferencia)
: iperf -c vmzsolutions.com.br -p443 (medir a taxa de transferencia)
: iperf -c 31.13.85.36 -p443 (por ip)
: iperf -c fb.com -p53 -u ()
: iperf -s (servidor)
: iperf -c 192.168.0.17 (cliente)
: apt install dnstop (analise de trafego dns)
: yum install dnstop 
: dnstop enp0s3 (monitorar)
: ab -n 100 -c10 https://www.facebook.com.br/ (simular uma conexao)
: apt install apache2-utils (webservidor)
: yum install httpd-tools (centOS) 
```

## Comandos atualizados de rede no Linux
```sh
: https://man7.org/linux/man-pages/man8/ip.8.html
: ifconfig -a (substituido pelo ip)
: ip a (mostra informacoes de ip)
: ifconfig eth0 down (substituido pelo ip)
: ifconfig eth0 up (substituido pelo ip)
: ip link set eth0 up (abre a interface)
: ip link set eth0 down (derruba a interface)
: ifconfig eth0 192.168.15.10 (substituido pelo ip)
: ifconfig eth0 netmask 255.255.255.0 (substituido pelo ip)
: ip addr add 192.168.15.10/24 dev eth0 (adicionar enderecoa interface de rede)
: netstat (lista as portas)
: netstat -tulpn (mostra as conexoes ativas)
: netstat -neopa (mostra informacao porta por porta)
: netstat -g (lista enderecos ipv6,ipv4)
: ss (mesmo resultado do netstat)
: ss -tulpn 
: ss -neopa
: ip maddr (lista todas as conexoes)
: route (rotas)
: route add -net 192.168.15.0 netmask 255.255.255.0 dev eth0 (adicionar rota) X
: route add default gw 192.168.0.1 (acionar gateway) X
: ip r (adicionar rota)
: ip -6 r (adicionar rota)
: ip route add 1921.168.15.0/24 dev eth0 (adicionar rota)
: ip route add default via 192.168.15.1 (acionar rota)
: arp (estabelece uma ligação entre o endereço físico (MAC Address) da placa de rede e o endereço lógico (Endereço IP))
: arp -a (Exibe as entradas dos hosts especificados.Exibe as entradas dos hosts especificados)
: arp -v (exibe info)
: ip neigh (listagem mais completa)
```

## Acesso remoto do servidor e host usando SSH
```sh
: apt-get install openssh-server (conjunto de utilitários de rede relacionado à segurança que provém a criptografia em sessões de comunicações em uma rede de computadores usando o protocolo SSH)
: yum –y install openssh-server openssh-clients 
: ssh user1@192.168.1.210 (se conectar a um usuario)
: scp arquivo user@192.168.0.23:/destino (transferencia de arquivos entre linuxs)
: scp -r diretório user@192.168.0.23:/destino (copiar um diretorio todo)
: Da máquina REMOTA para a LOCAL:
: De um arquivo: scp usuario@IP_DE_ORIGEM(REMOTO):/arquivo /destino 
: De um diretório: scp usuario@IP_DE_ORIGEM(REMOTO):/arquivo /destino

## Trabalhando com chaves Assimétricas
```sh
: ssh-keygen -b 1024 -t rsa (gerar chave)
: cat .ssh/id_rsa (visualizar a chave privada)
: cat .ssh/id_rsa.pub (visualizar a chave publica)
: ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.1.4 (se conectar a outra maquina)
```

## Segurança em seu SSH
```sh
: vim /etc/ssh/sshd_config (arquivo de configuracao do ssh)
```

## Criando um Servidor Seguro de SSH no centOS
```sh
: yum install -y openssh openssh-server
: vim /etc/ssh/sshd_config (arquivo de configuracao)
: semanage port -a -t ssh_port_t -p tcp 2022 (configurar as portas alteradas)
: firewall-cmd --add-port=2022/tcp (configurar as portas alteradas)
: firewall-cmd --add-port=2022/tcp --permanent (configurar as portas alteradas)
: systemctl status -l sshd (ver se esta rodando)
```

## Habilitando o log do sistema com o rsyslog
```sh
: $ journalctl (verificar as mensagens de log do kernel)
: vim /etc/rsyslog.conf (configuracao de logs,modulos)
: vim /etc/rsyslog.d/50-default.conf (regras padroes de log)
: vim /etc/rsyslog.conf 
: tail /var/log/syslog (Ubuntu/Debian) (mensagens de log do sistema)
: tail /var/log/messages (RHEL/CentOS) 
```

## Trabalhando com rsync
```sh
: rsync -avl root@192.168.0.25:/usr/share/man/man1/ /tmp/ ()
: rsync -avl root@192.168.0.25:/usr/share/man/man1 /tmp ()
: ls -l /usr/share/man/man1/batch* /tmp/man1/batch* ()
: rsync --help ()
: cp /etc/services /usr/share/man/man1 ()
: rsync -avl root@192.168.0.25:/usr/share/man/man1 /tmp ()
: sudo rm /usr/share/man/man1/services ()
: rsync -avl root@192.168.0.25:/usr/share/man/man1 /tmp ()
: rsync -avl /usr/share/man/man1 localhost:/tmp ()
: rsync -avl --delete root@192.168.0.25:/usr/share/man/man1 /tmp ()
```

## usando o wrapper tcp
```sh
: which sshd (verificar se o programa suporta wrapper tcp)
: /etc/hosts.allow (permitir 1) -- /etc/hosts.deny(negar 2)
: ldd /usr/sbin/sshd | grep libwrap (verificar compatibilidade) -- libwrap.so
: vim/etc/hosts.deny (restringir o acesso de uma maquina a outra) -- sshd 192.168.1.210(maquina que nao vai mais acessar) -- ssh root@192.168.1.194(testar e vai dar errado)
: vim/etc/hosts.allow (mesma ideia do comando acima)
: sshd 192.168.1.210 : spawn /bin/echo '/bin/date' from  %h > /conn.log : deny (bloquear e disparar um log)
: cd / -- ls -- cat conn.log (verificar)
```
