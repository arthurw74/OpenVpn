<h1 align="center">OpenVpn</h1>
Requisitos:
Ubuntu Linux 20.04
privilégios Root

Vou dividir este tutorial em alguns passos.

<h3> 1°passo</h3>
<h4>atualize seu sistema para evitar problemas de segurança com o comando "apt"</h4>
sudo apt update
sudo apt upgrade

<h3> 2° passo</h3>
<h4>encontre seu endereço ip, ele será usado para conexão, este comando mostra seu ip logo após a palavra "inet"</h4>
ip a 
Caso esteja utilizando uma vm use o seguinte comando.
dig +short myip.opendns.com @resolver1.opendns.com

O script detecta automaticamente sua configuração de rede. Tudo o que você precisa fazer é fornecer um endereço IP correto quando solicitado.</h4>

<h3>Passo 3 </h3>
 Baixe e execute o script openvpn-install.sh</h4>

 <h4>Vou usar o comando wget da seguinte forma:</h4>

wget https://git.io/vpn -O openvpn-ubuntu-install.sh

<h4>Agora baixamos o script e é hora de torná-lo executável. Portanto, configure as permissões usando o comando chmod: </h4>
<h4>Pode-se visualizar o script usando um editor de texto como nano/vim ou acessando o link https://git.io/vpn</h4>
<h4>As permissões são "0644 (rw-r--r--) para 0755 (rwxr-xr-x)".</h4>

chmod -v +x openvpn-ubuntu-install.sh

<h4>Execute o script openvpn-ubuntu-install.sh para instalar o servidor OpenVPN.</h4>
sudo ./openvpn-ubuntu-install.sh

<h4>Agora é só seguir as instruções do script.</h4>
<h4>Recomendo usar as configurações padrão, lembre-se de abrir a porta UDP 1194 de seu roteador ou VM.</h4>
<h4>Eu sugiro fortemente que você sempre escolha a opção de servidor DNS como 1.1.1.1 ou Google DNS ou qualquer outro serviço DNS desde que você confie de acordo com suas necessidades</h4>

<h4>Como faço para iniciar/parar/reiniciar o servidor OpenVPN no Ubuntu 20.04 LTS?</h4>
<h4>Precisamos usar o comando systemctl da seguinte forma:</h4>

<h4>Pare o servidor OpenVPN</h4>
sudo systemctl stop openvpn-server@server.service
## ou ##
sudo systemctl stop openvpn@server.service

<h4>Inicie o servidor OpenVPN</h4>
sudo systemctl start openvpn-server@server.service
## ou ##
sudo systemctl start openvpn@server.service

<h4>Reinicie o servidor OpenVPN após alterar as opções de configuração</h4>
sudo systemctl restart openvpn-server@server.service
## ou ##
sudo systemctl restart openvpn@server.service

<h4>Mostrar status do servidor OpenVPN</h4>
sudo systemctl status openvpn-server@server.service
## ou ##
sudo systemctl status openvpn@server.service

<h3>4° passo </h3>
<h4>Como adicionar ou remover um novo usuário VPN com um certificado</h4>
<h4>Você precisa executar o mesmo script novamente para adicionar ou remover um novo usuário VPN ao certificado TLS. Por exemplo:</h4>
sudo ./openvpn-ubuntu-install.sh

<h4>Você verá o menu desta forma:</h4>

OpenVPN is already installed.

Select an option:
   1) Add a new client
   2) Revoke an existing client
   3) Remove OpenVPN
   4) Exit
Option: 

<h4>Escolha a opção nº 1 para adicionar um novo cliente/usuário VPN e a opção nº 2 para remover o cliente e usuário VPN existente. Vamos adicionar um novo cliente/usuário nome.ovpn </h4>
<h4>os novos clientes são salvos na pasta /root/nome.ovpn</h4>

<h4>Você pode mover para a pasta do usuário com o comando abaixo, no meu caso o usuário é ubuntu.</h4>
sudo su
mv /root/*.ovpn /home/ubuntu/

<h4>A regra de firewall está configurada corretamente em seu servidor? </h4>
<h4>Use o comando cat para ver as regras: Config:</h4>
sudo cat /etc/systemd/system/openvpn-iptables.service
## ou  ##
sudo cat /etc/systemd/system/iptables-openvpn.service

<h4>Outra opção é executar os comandos iptables command e sysctl command para verificar a configuração da regra NAT em seu servidor:</h4>

sudo iptables -t nat -L -n -v
sysctl net.ipv4.ip_forward
sudo cat /etc/sysctl.d/30-openvpn-forward.conf
## ou ##
sudo cat /etc/sysctl.d/99-openvpn.conf

<h4>Insira as regras se não forem inseridas usando o seguinte comando: </h4>
<h4>O servidor OpenVPN está em execução e a porta está aberta? </h4>
<h4>se o comando ss ou o comando netstat e o comando pidof /ps:</h4>

sudo systemctl start openvpn-iptables.service
## Ou ##
sudo systemctl start iptables-openvpn.service
sudo sysctl -w net.ipv4.ip_forward=1
sudo sysctl -p -f /etc/sysctl.d/30-openvpn-forward.conf
## Ou ##
sudo sysctl -p -f /etc/sysctl.d/99-openvpn.conf

## 1194 é a porta do servidor openvpn ##
netstat -tulpen | grep :1194
## 1194 é a porta do servidor openvpn  ##
ss -tulpen | grep :1194
## o servidor openvpn está rodando? ##
ps aux | grep openvpn
## o servidor openvpn está rodando?  ##
ps -C openvpn
## encontre o PID do servidor openvpn ##
pidof openvpn
Se não estiver executando, reinicie o servidor OpenVPN:

sudo systemctl restart openvpn-server@server.service

<h4>Conclusão</h4>
<h4>arabéns, Você configurou um servidor OpenVpn no Ubuntu Linux 20.04 LTS</h4>

