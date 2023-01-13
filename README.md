<h1>Servidor OpenVPN utilizando TLS Crypt v2</h1> 

O objetivo desta passo a passo é explicar minuciosamente como é feita uma VPN e também, explicar os termos e tecnologias que serão usadas. Rodando OpenVPN em um Ubuntu Server vamos utilizar o TLS Crypt v2, que vai disponibilizar uma chave tls-crypt para cada usuário, a principal intenção do uso do TLS Crypt é reduzir os danos caso a chave tls-crypt seja comprometida.

<h2>Instalando o OpenVPN e suas dependências</h2>
<h3>OpenVPN</h3>

<p>Primeiro vamos atualizar os pacotes e no mesmo comando vamos instalar o OpenVPN.</p>
<code>sudo apt-get update && sudo apt-get install openvpn </code>
<p>
<p>É necessário verificar se foi instalado a versão OpenVPN 
2.5.5, pois o TLS Crypt v2 funcionara apenas a partir desta versão. Caso não tenho instalado esta versão será necessário atualizar o mesmo.</p>
<p>Para verificar a versão:</p>
<code>openvpn --version</code>

<h3>easy-rsa</h3>

<p>Uma dependência importante que precisamos é o easy-rsa, e em uma breve explicação o easy-rsa é um utilitário CLI para construir e gerenciar uma PKI CA. Em termos leigos, isso significa criar uma autoridade de certificação raiz e solicitar e assinar certificados, incluindo CAs intermediárias e listas de certificados revogados (CRL).</p>
<p>Sendo assim, primeira etapa ao configurar o OpenVPN é criar uma Public Key Infrastructure (PKI). Isso consiste em:

<ol>

- Um certificado mestre público de autoridade de certificação (CA) e uma chave privada.

- Um certificado público separado e um par de chaves privadas para cada servidor.

- Um certificado público separado e um par de chaves privadas para cada cliente.
</ol>

<p>Pode-se pensar na autenticação baseada em chave em termos semelhantes aos de como as chaves SSH funcionam com a camada adicional de uma autoridade de assinatura (CA). OpenVPN conta com uma estratégia de autenticação bidirecional, então o cliente deve autenticar o certificado do servidor e em paralelo, o servidor deve autenticar o certificado do cliente.
<p>Isso é feito pela assinatura da terceira parte (CA) nos certificados do cliente e do servidor. Uma vez estabelecido, outras verificações são realizadas antes que a autenticação seja concluída.
<p>Comando de instalação:</p>
<code>sudo apt install easy-rsa</code> 
<h3>Configurando firewall com ufw</h3>
<p>O UFW, ou Uncomplicated Firewall (Firewall Descomplicado), é uma interface para iptables desenvolvida para simplificar o processo de configuração de um firewall. Assim de forma simples, podemos criar regras básicas para tanto proteger nosso servidor, como também configurar de forma simples algumas regras da VPN.
<p>Comando de instalação:</p>
<code>sudo apt install ufw</code>
<p>
<p>Após instalar precisamos configurar algumas regras antes de começar o serviço de VPN. Começando com a ativação do ufw:</p>
<code>sudo ufw enable</code>
<p>
<p>E para ver se o ufw realmente está ativado podemos usar o comando:</p>
<code>sudo ufw status verbose</code>
<p>
<p>Com o firewall funcionando precisamos atribuir a regra para liberar as portas de acesso do SSH e SFTP no servidor:</p>
<code>sudo ufw allow 22/tcp</code>

<code>sudo ufw allow 21/tcp</code>

<strong>--- EM DESENVOLVIMENTO ---</strong>
