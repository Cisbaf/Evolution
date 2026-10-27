# Evolution API (notificações por WhatsApp)

Evolution API v2.3.7 (imagem oficial `evoapicloud/evolution-api`) usada pelo chamadosTI para enviar WhatsApp.
Só envio, sem receber mensagens. MySQL existente (`mysql-container`), sem Redis, rede `cisbaf_network_apps`, sem porta publicada.

## Subir

1. Criar o banco: `CREATE DATABASE evolution CHARACTER SET utf8mb4;` e um usuário só com acesso a ele.
2. `cp .env.example .env && chmod 600 .env` e preencher.
3. `docker compose up -d`
4. Criar a instância `chamadosti` e conectar o número lendo o QR (`GET /instance/connect/chamadosti` ou painel em `/manager`).
   Para o painel, publicar `127.0.0.1:8080:8080` temporariamente e acessar por túnel SSH.
5. Os sistemas usam a **chave da instância** (header `apikey`), nunca a global.

## Enviar

    POST http://evolution-api:8080/message/sendText/chamadosti
    apikey: <chave da instância>
    {"number": "5521999990000" ou "<id>@g.us", "text": "mensagem"}

## Operação

- Estado: `GET /instance/connectionState/chamadosti`. Se estiver `close`, reconectar por QR.
- O `.env` e a chave da instância não ficam no git. Backup: rotina `backup-v2` da VPS (banco `evolution` + volume `evolution_evolution-instances`).
