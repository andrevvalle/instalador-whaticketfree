# Instalador Whaticket SaaS - Redis em Docker

Neste instalador o pm2 é no usuário root.

* Atualização pelo terminal parece não funcionar.

```bash
sudo apt -y update && apt -y upgrade
```

FAZENDO DOWNLOAD DO INSTALADOR & INICIANDO A PRIMEIRA INSTALAÇÃO (USAR SOMENTE PARA PRIMEIRA INSTALAÇÃO):

```bash
sudo apt install -y git && git clone https://github.com/launcherbr/instalador.git instalador && sudo chmod -R 777 instalador  && cd instalador  && sudo ./install_primaria
```

## Requisitos

| --- | Mínimo | Recomendado |
| --- | --- | --- |
| Node JS | 20.x | 20.x |
| Ubuntu | 20.x | 20.x |
| Memória RAM | 4Gb | 8Gb |  



74% of storage used … If you run out, you can't create, edit, and upload files.
========================================================================================================================================================
INSTALAÇÃO UBUNTU 22 COM DOCKER
========================================================================================================================================================
Servidor>>
IP: 
IPV6: 
Usuário: root
senha: 

Front = endereço principal do chatbot: [[app.dominio.com]]
Backend/API = endereço da api interna: [[api.dominio.com]]
app: celtainfo [[nome do app, nome curto - sem letras maiúscula e caracteres especiais]]

Senha deploy: [[senha do user deploy, somente letras e números, sem letras maiúscula e caracteres especiais; recomendado 8 caracteres]]

Usuário super-admin: admin@admin.com
Senha: 123456

*** Nunca delete esse usuário, e ao editar os dados não se esqueça de preencher a senha atual ou uma nova senha. 

Github - Código
https://github.com/launcherbr/whaticketsaasfree.git

========================================================================================================================================================

ATUALIZAR A VPS
sudo apt update && sudo apt -y upgrade

INSTALAR O NODE E POSTGRES

sudo su root
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
apt-get install -y nodejs
npm install -g npm@latest

sudo sh -c 'echo "deb https://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc|sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg
sudo apt update -y
sudo apt-get update -y && sudo apt-get -y install postgresql-16
sudo timedatectl set-timezone America/Sao_Paulo
	
INTALAR O DOCKER

sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
apt-cache policy docker-ce
sudo apt install docker-ce
	
INSTALAR O PUPPETEER
apt-get install -y libxshmfence-dev libgbm-dev wget unzip fontconfig locales gconf-service libasound2 libatk1.0-0 libc6 libcairo2 libcups2 libdbus-1-3  libexpat1 libfontconfig1 libgcc1 libgconf-2-4 libgdk-pixbuf2.0-0 libglib2.0-0 libgtk-3-0 libnspr4 libpango-1.0-0 libpangocairo-1.0-0 libstdc++6 libx11-6 libx11-xcb1 libxcb1 libxcomposite1 libxcursor1 libxdamage1 libxext6 libxfixes3 libxi6 libxrandr2 libxrender1 libxss1 libxtst6 ca-certificates fonts-liberation libappindicator1 libnss3 lsb-release xdg-utils

INSTALAR O PM2
npm install -g pm2
	
INSTALAR O SNAPD
apt install -y snapd
snap install core
snap refresh core
	
INSTALAR O NGINX
apt install -y nginx
rm /etc/nginx/sites-enabled/default
service nginx restart
sudo nano /etc/nginx/nginx.conf
client_max_body_size 100M;
underscores_in_headers on;
		
INSTALAR O CERTBOT
apt-get remove certbot
snap install --classic certbot
ln -s /snap/bin/certbot /usr/bin/certbot
		
CRIAR USUARIO DEPLOY = 
adduser deploy
usermod -aG sudo deploy

Prencher apenas senha, enter para campos de dados pessoais.
	
==============================================================================================================================================
INSTALAR USANDO O INSTALADOR
========================================================================================================================================================
cd /home
ls
sudo apt install -y git && git clone https://github.com/launcherbr/instalador.git instalador && sudo chmod -R 777 instalador  && cd instalador  && sudo ./install_primaria

======================================================================================================================================================== 
Siga o passo-a passo do terminal exibido - Tenha atenção ao copiar e colar para não ter espaços no final da expressão:

[0] Instalar WhatsPainel/Whaticket

Insira senha para o usuário Deploy e Banco de Dados (Não utilizar caracteres especiais ou letras maiúsculas)
[[senhadeploy]]

Insira o link do GITHUB do seu Whaticket que deseja instalar:
https://github.com/launcherbr/whaticketsaasfree.git

Informe um nome para a Instancia/Empresa que será instalada (Não utilizar espaços, caracteres especiais ou letras maiúsculas, utilize somente letras minúsculas;):
[[nomedoapp]]

Informe a Qt. de de Conexões/Whats que a poderá cadastrar: 1 a 9999
9999

Informe a Qt. de Usuários/Atendentes que a poderá cadastrar: de 1 a 9999
9999

Digite o domínio do FRONTEND/PAINEL; Ex: appbot.cloud
[[app.dominio.com]]

Digite o domínio do BACKEND/API; Ex: api.appbot.cloud
[[api.dominio.com]]

Digite a porta do FRONTEND para a appbot; Ex: 3000 A 3999
3000

Digite a porta do BACKEND para esta instancia; Ex: 4000 A 4999
4000

Digite a porta do REDIS/AGENDAMENTO MSG para a appbot; Ex: 5000 A 5999
5000

Confira se há necessidade de liberar portas do firewall, habitualmente a própria instalação faz a liberação das portas.
======================================================================================================================================================== 
======================================================================================================================================================== 
======================================================================================================================================================== 

INSTALAÇÃO MANUAL
======================================================================================================================================================== 
BACKEND
========================================================================================================================================================
***** CRIAR O REDIS E BANCO POSTGRES
Senha do banco fica entre '' (aspa única)

sudo su root
usermod -aG docker deploy
docker run --name redis-[[nomeredis]] -p 5000:6379 --restart always --detach redis redis-server --requirepass [[senharedis]]
sudo su - postgres
createdb [[nomebanco]];
psql
CREATE USER [[userbanco]] SUPERUSER INHERIT CREATEDB CREATEROLE;
ALTER USER [[userbanco]] PASSWORD '[[senhauser]]';
\q
exit
========================================================================================================================================================	
**** CRIAR VARIAVEL DE AMBIENTE

---- Utilizando usuario deploy no diretório backend ----

cd /home/deploy/[[nome do app]]/backend
ls 
sudo nano .env

------------ Editar o arquivo com os dados abaixo --------------
https://github.com/launcherbr/saascodigo/edit/master/backend/.env.example

NODE_ENV=
BACKEND_URL=https://[[dominio do backend]]
FRONTEND_URL=https://[[dominio do frontend]]
PROXY_PORT=443
PORT=4000

DB_DIALECT=postgres
DB_HOST=localhost
DB_TIMEZONE=-03:00
DB_PORT=5432
DB_USER=[[userbanco]]
DB_PASS=[[senhauser]]
DB_NAME=[[nomebanco]]

rootPath=/var/www/backend

JWT_SECRET=kZaOTd+YZpjRUyyuQUpigJaEMk4vcW4YOymKPZX0Ts8=
JWT_REFRESH_SECRET=dBSXqFg9TaNUEDXVp6fhMTRLBysP+j2DSqf7+raxD3A=

REDIS_URI=redis://[[senharedis]]@127.0.0.1:5000
REDIS_OPT_LIMITER_MAX=1
REDIS_OPT_LIMITER_DURATION=3000

USER_LIMIT=10000
CONNECTIONS_LIMIT=100000
CLOSED_SEND_BY_ME=true

========================================================================================================================================================
INSTALAÇÃO DAS DEPENDENCIAS
---- Utilizando usuario deploy no diretório backend ----
cd /home/deploy/[[nome do app]]/backend
npm install

========================================================================================================================================================
	
COMPILANDO O CÓDIGO DO BACKEND
---- Utilizando usuario deploy no diretório backend ----
cd /home/deploy/[[nome do app]]/backend
npm run build

========================================================================================================================================================	
CRIANDO AS TABELAS NO BANCO
----Utilizando usuario deploy no diretório backend----
cd /home/deploy/[[nome do app]]/backend
npx sequelize db:migrate

PUPULANDO AS TABELAS DO BANCO
----Utilizando usuario deploy no diretório backend----
cd /home/deploy/[[nome do app]]/backend
npx sequelize db:seed:all
	
INICIANDO O SERVIÇO PM2
----Utilizando usuario deploy no diretório backend----
cd /home/deploy/[[nome do app]]/backend
pm2 start dist/server.js --name [[nome do app]]-backend
pm2 save --force

CONFIGURAÇÃO DO NGINX
----Utilizando usuario root no diretório etc----
sudo nano /etc/nginx/sites-available/[[nome do app]]-backend

server {
  server_name [[dominio do backend]];
  location / {
    proxy_pass http://127.0.0.1:4000;
    proxy_http_version 1.1;
    proxy_set_header Upgrade \$http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host \$host;
    proxy_set_header X-Real-IP \$remote_addr;
    proxy_set_header X-Forwarded-Proto \$scheme;
    proxy_set_header X-Forwarded-For \$proxy_add_x_forwarded_for;
    proxy_cache_bypass \$http_upgrade;
  }
}

---- Gerando link simbólico ----
ln -s /etc/nginx/sites-available/[[nome do app]]-backend /etc/nginx/sites-enabled

========================================================================================================================================================
FRONTEND
========================================================================================================================================================
CRIAR VARIAVEL DE AMBIENTE
---- Utilizando usuario deploy no diretório frontend----
cd /home/deploy/[[nome do app]]/frontend
sudo nano .env

REACT_APP_BACKEND_URL=https://[[api.dominio.com]]
REACT_APP_HOURS_CLOSE_TICKETS_AUTO = 24

INSTALANDO DEPENDENCIAS DO FRONTEND
---- Utilizando usuario deploy no diretório frontend ----
cd /home/deploy/[[nome do app]]/frontend
npm install
	
COMPILANDO O CODIGO DO FRONTEND
---- Utilizando usuario deploy no diretório frontend ----
cd /home/deploy/[[nome do app]]/frontend
npm run build
	
INICIANDO O PM2
---- Utilizando usuario deploy no diretório frontend ----
cd /home/deploy/[[nome do app]]/frontend
pm2 start server.js --name [[nome do app]]-frontend
pm2 save --force

----Configuração para o PM2 iniciar com o Ubuntu----
pm2 startup
sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u deploy --hp /home/deploy
	
CONFIGURAÇÃO DO NGINX
---- Utilizando usuario root no diretório etc ----
sudo nano /etc/nginx/sites-available/[[nome do app]]-frontend
	
server {
  server_name [[dominio do frontend]];
  location / {
    proxy_pass http://127.0.0.1:3000;
    proxy_http_version 1.1;
    proxy_set_header Upgrade \$http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host \$host;
    proxy_set_header X-Real-IP \$remote_addr;
    proxy_set_header X-Forwarded-Proto \$scheme;
    proxy_set_header X-Forwarded-For \$proxy_add_x_forwarded_for;
    proxy_cache_bypass \$http_upgrade;
  }
}
	
---- Gerando link simbólico ----
sudo ln -s /etc/nginx/sites-available/[[nome do app]]-frontend /etc/nginx/sites-enabled
	
RODAR O CERTBOT PARA GERAR O SSL
certbot -m seuemail@gmail.com --nginx --agree-tos --non-interactive --domains [[dominio do frontend]],[[dominio do backend]]

======================================================================================================================================================== 
======================================================================================================================================================== 
======================================================================================================================================================== 

* fix, caso pm2 status não liste os apps depois de um reboot:

sudo su - deploy
cd /home/deploy/[[nome do app]]/frontend
sudo pm2 start server.js --name [[nome do app]]-frontend
sudo pm2 save --force

sudo su - deploy
cd /home/deploy/[[nome do app]]/backend
sudo pm2 start dist/server.js --name [[nome do app]]-backend
sudo pm2 save --force

* caso salvo acima, e não aparece após o novo reboot, repetir e incluir estes comandos:
sudo su - root
pm2 startup
sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u deploy --hp /home/deploy
======================================================================================================================================================== 
Fazer login:
usuário padrão
admin@admin.com
123456

Form. de Cadastro
/signup

**** INFORMAÇÕES IMPORTANTES:
Não exclua o usuário superadmin, id 1 - quando for trocar o e-mail ou nome não esquecer de preencher o campo senha, se não ele salva a senha em branco no banco de dados.

Não exclua a empresa principal de id 1, faça a alteração dos dados.

Vá em configurações e empresas para habilitar o envio de campanhas. Após ativar faça logout e login para o menu aparecer.

O Plano 1 pode ser excluído, ele não aceita troca de nome, más ao excluir vincule um novo plano a empresa 1 se não conseguira adicionar conexões, filas e novos usuários.

Na nossa área de base de conhecimento tem as instruções que fiz sobre o chatbot, no Google Drive no arquivo de texto tem um compilado de vídeos do YouTube de Whaticket em geral tanto do gratuito como do SAAS.

Em tipo de chatbot, somente texto. O whatsapp não permite o funcionamento de botões e listas que não seja pela api oficial do Whatsapp Business que é paga.

Ao mudar gerenciamento de horários, esvazie os campos prenchidos da opção atual, exemplo ao mudar de um gerencimento de horário por fila para por empresa, vá as horários das filas e deixe os horários em branco (para que as informações dos horários sejam apagadas do banco de dados), faça o mesmo se for trocar um gerenciamento de horário de empresa para fila, limpando os campos de horários da empresa ou se desativar o horário limpando os campos atualmente ativos.

Esvaziar midias com mais de 30 dias:
find /home/deploy/*/backend/public -type f -mtime +30 -delete
======================================================================================================================================================== 
Comando para build/re-build
sudo npm run build
======================================================================================================================================================== 
Comando para atualizar bibliotecas
npm update --force
======================================================================================================================================================== 
Comando para reiniciar pm2
pm2 restart all
======================================================================================================================================================== 
Gerencianet, atual Efi
Para retorno automático é necessário uma aplicação no PC, Insomnia ou usar o Postman:

backend / .env
agrega estas linhas

GERENCIANET_SANDBOX=false
GERENCIANET_CLIENT_ID=Client_Id_Gerencianet
GERENCIANET_CLIENT_SECRET=Client_Secret_Gerencianet
GERENCIANET_PIX_CERT=certificado-Gerencianet
GERENCIANET_PIX_KEY=chave pix gerencianet

Em backend\certs
Salvar o certificado no formato .p12

Atualizar
./update

(dentro do diretório do app, de permissão 777 antes através do terminal ssh com o comando chmod 777 update)

======================================================================================================================================================== 

Ajuda (Diversos Canais)
*não são específicos desse código
https://help.whaticket.com/pt-br
https://whaticket.online/
https://www.youtube.com/@whaticketapp
https://www.youtube.com/@multiconversaoficial
https://www.youtube.com/@melissatreinamentos
https://www.youtube.com/@astraonlineweb
https://www.youtube.com/@equipechat
https://blog.melissatreinamentos.tec.br/
  
========================================================================================================================================================
========================================================================================================================================================
