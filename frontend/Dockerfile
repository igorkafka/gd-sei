# Build: contexto frontend/
# Imagem do Docker Hub (esteira não aceita GHCR). CI usa Flutter 3.47.2; 3.41.6
# é a versão mais recente disponível nesta imagem no momento desta alteração.
FROM instrumentisto/flutter:3.41.6 AS build
WORKDIR /src
COPY pubspec.* ./
RUN flutter pub get
COPY . .
# URL vazia: Flutter usa /api na mesma origem, via proxy Nginx.
RUN flutter build web --release --dart-define=API_BASE_URL=

# Nginx unprivileged já suporta OpenShift (porta não privilegiada).
FROM nginxinc/nginx-unprivileged:stable-alpine
COPY --from=build /src/build/web/ /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 8080
