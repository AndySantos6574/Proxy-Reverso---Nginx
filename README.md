# Proxy-Reverso---Nginx
Configurar um proxy reverso no Nginx na porta 80 apontando para a aplicação Flask em Python.

Primeiro vamos acessar o nosso arquivo de configuração do nginx:

/etc/nginx/sites-available

Ao acessar o local de instalação (/etc/nginx), mostra uma estrutura parecida com esta:

├── conf.d
├── fastcgi.conf
├── fastcgi_params
├── mime.types
├── modules-available
├── modules-enabled
├── nginx.conf
├── proxy_params
├── sites-available
│   ├── default
├── sites-enabled
│   ├── default -> /etc/nginx/sites-available/default
├── snippets
│   ├── fastcgi-php.conf


sites-available/
Nesta pasta, ficam as configurações dos servidores virtuais.

sites-enabled/
Nesta pasta, ficam os servidores virtuais ativos. Caso o arquivo de configuração esteja em sites-available, mas não esteja em sites-enabled, o Nginx não irá carregá-lo.

conf.d/
Pasta com as configurações extras do Nginx. Nela, é possível criar um arquivo de configuração que será incluído automaticamente nas configurações gerais.


Os dois diretórios em que trabalharemos são sites-availablee sites-enabled.


crie um novo arquivo de configuração de bloco de servidor no diretório do Nginx sites-available

No meu caso criei com o editor de texto vi:

sudo vi reverse-proxy.conf

logo abaixo fiz as seguintes configurações:

server {

    listen       80;
    server_name  app.piercloud.com.br;


        proxy_set_header Host $host;
              proxy_set_header X-Real-IP $remote_addr;
              proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
              add_header Test_header jay;


    location / {
        proxy_pass http://127.0.0.1:8080;

    }

}


Proxy_pass significa passar uma solicitação para um servidor proxy HTTP, a diretiva proxy_pass é especificada dentro de um local.

No meu caso estou indicando a porta 80 que deixei na minha aplicação do Flask que fiz anteriormente.Este bloco também será usado para solicitações de nome de domínio ou endereço IP do nosso servidor, no meu caso como podemos ver a cima o meu nome de dominio deixei dessa forma > app.piercloud.com.br:


Em seguida vincule o arquivo ao diretório habilitado para sites para habilitar o bloco de servidor Nginx que acabamos de criar. A sintaxe é a seguinte:
Então, habilite ambos os sites criando links simbólicos para o diretório sites-enabled:

sudo ln -s /etc/nginx/sites-available/reverse-proxy.conf /etc/nginx/sites-enabled/reverse-proxy.conf

Teste os erros de sintaxe digitando:

nginx -t 

Se não houver problemas, reinicie o processo Nginx para ler nossa nova configuração:

systemctl reload nginx.service 

Agora vamos para o nome de domínio ou endereço IP do seu servidor em seu navegador web, seu aplicativo está em execução.

app.piercloud.com.br;

Veremos abaixo a sua aplicação que foi desenvolvida no Flask anteriormente com sucesso!

Hello World!

















