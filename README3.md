1. Установка Docker в Ubuntu
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y

sudo systemctl start docker
sudo systemctl enable docker
2. Подготовка веб-приложения для Docker
# Используем официальный образ Node.js
FROM node:18

# Рабочая директория в контейнере
WORKDIR /app

# Копируем package.json и устанавливаем зависимости
COPY package*.json ./
RUN npm install

# Копируем остальные файлы
COPY . .

# Открываем порт (например, 3000)
EXPOSE 3000

# Команда для запуска приложения
CMD ["node", "app.js"]
3. Сборка Docker образа
В директории с Dockerfile выполните:
docker build -t my-web-app .
4. Запуск контейнера с веб-приложением
Запустите контейнер, пробросив порт на хост (например, 3000):
docker run -d -p 3000:3000 --name web-app-container my-web-app
5. Проверка работы
Откройте в браузере на Ubuntu (или машине, где запускается Docker):
http://localhost:3000

http://<IP-адрес_вашего_сервера>:3000

6. Использование docker-compose (если у вас несколько сервисов)
Создайте файл docker-compose.yml с примером:
version: '3'
services:
  web:
    build: .
    ports:
      - "3000:3000"
Запустите:
docker-compose up -d
