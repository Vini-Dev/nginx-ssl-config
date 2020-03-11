# Helpers

Verificar se configuração está correta

```shell
	nginx -t
```

Restartar o serviço do Nginx

```shell
	systemctl restart nginx
```

# Criando uma configuração simples

Em */etc/nginx/sites-available* crie um arquivo com o nome do domínio

```shell
	nano example.com
```

Escreva a seguinte configuração. Você pode adicionar subdominios em outros arquivos ou no mesmo.

```
server {
  listen 80;
  listen [::]:80;
  server_name example.com;

  root /var/html;

  index index.html;

  location / {
    try_files $uri $uri/ =404;
  }
}
```

Com subdomínios

```
server {
  listen 80;
  listen [::]:80;
  server_name example.com www.example.com;

  root /var/html;

  index index.html;

  location / {
    try_files $uri $uri/ =404;
  }
}
```

# Link simbólico

```shell
ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/example.com
```

# Instalando o Certbot

Adicionando o repositório
```shell
	add-apt-repository ppa:certbot/certbot
```
Atualizando os pacotes
```shell
	apt update
```

Pacote Nginx do Certbot
```shell
	apt update
```

# Instalando o certificado com Let's Encrypt

```shell
	certbot --nginx -d example.com
```

Selecione criar um novo certificado e depois selecione a opção de redirecionar todo o tráfego para https (2)

# Caso queira o arquivo formatado

```
server {
	server_name example.com;

	location / {
		root /var/www/production/tourism-client/prod;
		index index.html;
		try_files $uri $uri/ =404;
	}

	location /admin {
		root /var/www/html;
		index index.html;
		try_files $uri $uri/ =404;
	}

	listen [::]:443 ssl ipv6only=on; # managed by Certbot
	listen 443 ssl; # managed by Certbot
	ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem; # managed by Certbot
	ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem; # managed by Certbot
	include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
	ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}

server {
	if ($host = example.com) {
		return 301 https://$host$request_uri;
	} # managed by Certbot

	listen 80;
	listen [::]:80;
	server_name example.com;
	return 404; # managed by Certbot
}

```
