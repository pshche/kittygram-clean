FROM node:18 as build

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci

COPY . .
RUN npm run build

# Второй этап: используем nginx для отдачи статики
FROM nginx:alpine

# Копируем собранные файлы из первого этапа
COPY --from=build /app/build /usr/share/nginx/html

# Если внутри build есть папка static, поднимаем её содержимое на уровень выше
RUN if [ -d "/usr/share/nginx/html/static" ]; then \
      cp -rf /usr/share/nginx/html/static/* /usr/share/nginx/html/ && \
      rm -rf /usr/share/nginx/html/static; \
    fi

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
