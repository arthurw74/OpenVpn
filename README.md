# OpenVpn
Requisotos:
Ubuntu Linux 20.04
privilegios Root

Vou dividir este tutorial em alguns passos.

1°passo
atualise seu sistema para evitar problemas de segurança com o comando "apt"
sudo apt update
sudo apt upgrade

2° passo
encontre seu endereço ip, ele sera usado para conecsão, este comando mosta seu ip logo apos a palavra "inet"
ip a 
caso esteja utilisando uma vm use o seguinte comando.
dig +short myip.opendns.com @resolver1.opendns.com

O script detectará automaticamente sua configuração de rede. Tudo o que você precisa fazer é fornecer um endereço IP correto quando solicitado.

Passo 3 – Baixe e execute o script openvpn-install.sh

Vou usar o comando wget da seguinte forma:

wget https://git.io/vpn -O openvpn-ubuntu-install.sh

Agora baixamos o script e é hora de torná-lo executável. Portanto, configure as permissões usando o comando chmod: 
Pode-se visualizar o script usando um editor de texto como nano/vim ou acessando o link https://git.io/vpn
as permisoes são "0644 (rw-r--r--) para 0755 (rwxr-xr-x)".

chmod -v +x openvpn-ubuntu-install.sh

Execute o script openvpn-ubuntu-install.sh para instalar o servidor OpenVPN.
sudo ./openvpn-ubuntu-install.sh

agora é só seguir as istruçoes do script.
recomendo usar as configurações padrão, lembrese de abrir a porta UDP 1194 de seu roteador ou VM.
Eu sugiro fortemente que você sempre escolha a opção de servidor DNS como 1.1.1.1 ou Google DNS ou qualquer outro serviço DNS desde que você confie de acordo com suas necessidades

Como faço para iniciar/parar/reiniciar o servidor OpenVPN no Ubuntu 20.04 LTS?
Precisamos usar o comando systemctl da seguinte forma:

Pare o servidor OpenVPN
sudo systemctl stop openvpn-server@server.service
## ou ##
sudo systemctl stop openvpn@server.service

Inicie o servidor OpenVPN
sudo systemctl start openvpn-server@server.service
## ou ##
sudo systemctl start openvpn@server.service

Reinicie o servidor OpenVPN após alterar as opções de configuração
sudo systemctl restart openvpn-server@server.service
## ou ##
sudo systemctl restart openvpn@server.service

Mostrar status do servidor OpenVPN
sudo systemctl status openvpn-server@server.service
## ou ##
sudo systemctl status openvpn@server.service

4° passo 
Como adicionar ou remover um novo usuário VPN com um certificado
Você precisa executar o mesmo script novamente para adicionar ou remover um novo usuário VPN ao certificado TLS. Por exemplo:
sudo ./openvpn-ubuntu-install.sh

Você verá o menu desta forma:

OpenVPN is already installed.

Select an option:
   1) Add a new client
   2) Revoke an existing client
   3) Remove OpenVPN
   4) Exit
Option: 

Escolha a opção nº 1 para adicionar um novo cliente/usuário VPN e a opção nº 2 para remover o cliente e usuário VPN existente. Vamos adicionar um novo cliente/usuário nome.ovpn 
os novos clientes são salvos na pasta /root/nome.ovpn

voce pode mover para a pasta do usuario com o comando abaixo, no meu caso o usuario é ubuntu.
sudo du
mv /root/*.ovpn /home/ubuntu/

A regra de firewall está configurada corretamente em seu servidor? 
Use o comando cat para ver as regras: Config:
sudo cat /etc/systemd/system/openvpn-iptables.service
## ou  ##
sudo cat /etc/systemd/system/iptables-openvpn.service

Outra opção é executar os comandos iptables command e sysctl command para verificar a configuração da regra NAT em seu servidor:

sudo iptables -t nat -L -n -v
sysctl net.ipv4.ip_forward
sudo cat /etc/sysctl.d/30-openvpn-forward.conf
## ou ##
sudo cat /etc/sysctl.d/99-openvpn.conf

Insira as regras se não forem inseridas usando o seguinte comando: 
O servidor OpenVPN está em execução e a porta está aberta? U
se o comando ss ou o comando netstat e o comando pidof /ps:

sudo systemctl start openvpn-iptables.service
## Ou ##
sudo systemctl start iptables-openvpn.service
sudo sysctl -w net.ipv4.ip_forward=1
sudo sysctl -p -f /etc/sysctl.d/30-openvpn-forward.conf
## Ou ##
sudo sysctl -p -f /etc/sysctl.d/99-openvpn.conf

## 1194 é a porta do servidor openvpn ##
netstat -tulpn | grep :1194
## 1194 é a porta do servidor openvpn  ##
ss -tulpn | grep :1194
## o servidor openvpn está rodando? ##
ps aux | grep openvpn
## o servidor openvpn está rodando?  ##
ps -C openvpn
## encontre o PID do servidor openvpn ##
pidof openvpn
Se não estiver executando, reinicie o servidor OpenVPN:

sudo systemctl restart openvpn-server@server.service

Conclusão
Parabéns, Voçê configurou um servidor OpenVpn no Ubuntu Linux 20.04 LTS em execução na nuvem.
Neste tutorial foi utilizado a Vm Oracle.
