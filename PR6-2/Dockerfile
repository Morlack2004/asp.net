# Используйте официальный образ .NET SDK как базовый образ
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build-env

# Установите рабочую директорию в контейнере
WORKDIR /app

# Скопируйте csproj и восстановите в зависимости
COPY /PR6-2.csproj ./
RUN dotnet restore

# Скопируйте все остальное и постройте
COPY . ./
RUN dotnet publish -c Release -o out

# Создайте среду выполнения из образа .NET
FROM mcr.microsoft.com/dotnet/aspnet:6.0

WORKDIR /app
COPY --from=build-env /app/out .

# Копируем исходные CSS-файлы внутрь контейнера
COPY /wwwroot/css /app/wwwroot/css

# Создаем копии CSS-файлов с префиксом "копия"
RUN for file in /app/wwwroot/css/*.css; do cp "$file" "/app/wwwroot/css/$(basename $file) -копия"; done

# Откройте порт 80
EXPOSE 80

# Запустите приложение
ENTRYPOINT ["dotnet", "PR6-2.dll"]
