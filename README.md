Aplicação pronta que envia somente os alertas dos links COMEP e NETWAY para o seu celular via Telegram.

O que ela faz


Conecta na API do Zabbix

Monitora apenas os hosts/links que você configurar

Envia notificação no Telegram quando aparece problema

Filtra por severidade (padrão: Média, Alta e Desastre)

Roda 24h em qualquer servidor Linux

Instalação rápida (5 minutos)

1. Criar o bot no Telegram

Abra o Telegram e procure @BotFather

Digite /newbot

Escolha um nome e um username (ex: meuzabbix_bot)

Copie o token que ele te der

2. Descobrir seu Chat ID

Mande qualquer mensagem para o bot que você acabou de criar

Abra no navegador (substitua pelo seu token):

https://api.telegram.org/botSEU_TOKEN_AQUI/getUpdates

Procure a linha "chat":{"id": 123456789} → esse número é o seu Chat ID

3. Configurar a aplicação

cd zabbix-telegram-bot

# Copiar arquivo de configuração
cp .env.example .env

# Editar com seus dados
nano .env

Preencha:

ZABBIX_URL=http://IP_DO_SEU_ZABBIX/api_jsonrpc.php
ZABBIX_USER=Admin
ZABBIX_PASS=sua_senha

TELEGRAM_TOKEN=123456:ABC-DEF...
TELEGRAM_CHAT_ID=123456789

HOSTS_INTERESSE=COMEP,NETWAY

4. Instalar e rodar

# Instalar dependências
pip3 install -r requirements.txt

# Rodar
python3 bot.py

Você receberá uma mensagem no Telegram confirmando que o bot iniciou.

Rodar 24 horas (recomendado)

Opção A – screen (simples)

screen -S zabbixbot
python3 bot.py
# Pressione Ctrl+A e depois D para sair

Para voltar: screen -r zabbixbot

Opção B – systemd (profissional)

Crie o arquivo /etc/systemd/system/zabbix-telegram-bot.service:

[Unit]
Description=Zabbix Telegram Alert Bot
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/caminho/para/zabbix-telegram-bot
ExecStart=/usr/bin/python3 /caminho/para/zabbix-telegram-bot/bot.py
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target

Depois:

systemctl daemon-reload
systemctl enable zabbix-telegram-bot
systemctl start zabbix-telegram-bot
systemctl status zabbix-telegram-bot



Personalização

No arquivo .env você pode alterar:

Variável

Descrição

Exemplo

HOSTS_INTERESSE

Links que devem notificar


COMEP,NETWAY



SEVERIDADE_MINIMA


0 a 5 (3 = Média)



4 (só Alta e Desastre)





INTERVALO


Segundos entre verificações


30


Requisitos


Python 3.8+


Acesso à API do Zabbix (usuário com permissão de leitura)



Internet para o Telegram

Pronto! Agora você recebe no celular somente os alertas dos links que interessam.
