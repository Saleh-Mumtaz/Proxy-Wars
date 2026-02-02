docker installation
docker container ports will bypass ufw so do not change ufw rules
```
sudo apt-get update && \
sudo apt-get install -y ca-certificates curl && \
sudo install -m 0755 -d /etc/apt/keyrings && \
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc && \
sudo chmod a+r /etc/apt/keyrings/docker.asc && \
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null && \
sudo apt-get update && \
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin && \
sudo docker run hello-world
```
---
telegram bot
```
mkdir /etc/tgbot && cd /etc/tgbot && vim compose.yml
```
conf
replace * in TELEGRAM_BOT_TOKEN WHITELISTED_IDS ADMIN_ID TELEGRAM_API_ID TELEGRAM_API_HASH with your own
```
name: telegram-ytdl

services:
  telegram-bot:
    image: ghcr.io/vaaski/telegram-ytdl:latest
    restart: unless-stopped
    volumes:
      - ./vaaski-telegram-ytdl:/app/storage
    environment:
      # get your bot token from @BotFather
      TELEGRAM_BOT_TOKEN: "*"

      # comma separated list of telegram user ids, leave empty to allow all users
      WHITELISTED_IDS: "*"

      # admin user id
      ADMIN_ID: "*"
      ALLOW_GROUPS: 'false'

      TELEGRAM_API_ROOT: "http://telegram-bot-api:8081"
      TELEGRAM_WEBHOOK_PORT: "8443"
      TELEGRAM_WEBHOOK_URL: "http://telegram-bot:8443"

      # whether to automatically update yt-dlp
      # defaults to "true", set to "false" to disable
      YTDL_AUTOUPDATE: "true"

      # provide your OpenAI API key to enable auto-translation (optional)
      OPENAI_API_KEY: ""

      # provide your cobalt instance url (optional, can be left as-is for the default compose.yml)
      COBALT_INSTANCE_URL: "http://cobalt:9000"

    depends_on:
      telegram-bot-api:
        condition: service_healthy
      cobalt:
        condition: service_healthy

  telegram-bot-api:
    image: ghcr.io/bots-house/docker-telegram-bot-api:latest
    restart: unless-stopped
    environment:
      # get these values from https://core.telegram.org/api/obtaining_api_id
      TELEGRAM_API_ID: "*"
      TELEGRAM_API_HASH: "*"
    command:
      - --dir=/var/lib/telegram-bot-api
      - --local
    volumes:
      - telegram-bot-server-data:/var/lib/telegram-bot-api
    healthcheck:
      test: ["CMD-SHELL", "nc -z 127.0.0.1 8081 || exit 1"]
      interval: 5s
      timeout: 30s
      retries: 3

  cobalt:
    image: ghcr.io/imputnet/cobalt:10
    restart: unless-stopped
    environment:
      - API_URL=http://cobalt:9000
    healthcheck:
      test: ["CMD", "wget", "--spider", "-q", "http://localhost:9000"]
      interval: 5s
      timeout: 30s
      retries: 3

volumes:
  telegram-bot-server-data:
    driver: local

```
starting the bot
```
docker compose up -d
```
for cookies use seal android downloader to generate them
```
vim /etc/tgbot/vaaski-telegram-ytdl/cookies.txt
```
to delete everything related to all containers
```
docker kill $(docker ps -q) && docker rm $(docker ps -a -q) && docker volume rm $(docker volume ls -q) && docker rmi $(docker images -q)
```
