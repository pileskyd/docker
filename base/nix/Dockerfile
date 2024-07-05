FROM nixos/nix:latest

# Установка bash и других необходимых пакетов
RUN nix-env -iA nixpkgs.bash \
  nixpkgs.tzdata \
  nixpkgs.sudo \
  nixpkgs.curl \
  nixpkgs.xz \
  nixpkgs.git \
  nixpkgs.ca-certificates \
  nixpkgs.openssl \
  nixpkgs.make \
  nixpkgs.pkg-config

# Установка временной зоны Владивосток
RUN cp /etc/zoneinfo/Asia/Vladivostok /etc/localtime && \
  echo "Asia/Vladivostok" > /etc/timezone

# Определение переменных окружения
ENV ENV=/etc/profile \
  USER=root \
  PATH=/nix/var/nix/profiles/default/bin:/nix/var/nix/profiles/default/sbin:/bin:/sbin:/usr/bin:/usr/sbin \
  GIT_SSL_CAINFO=/etc/ssl/certs/ca-certificates.crt \
  NIX_SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt \
  NIX_PATH=/nix/var/nix/profiles/per-user/root/channels \
  NIXPKGS_ALLOW_BROKEN=1 \
  NIXPKGS_ALLOW_UNFREE=1 \
  NIXPKGS_ALLOW_INSECURE=1 \
  LD_LIBRARY_PATH=/usr/lib \
  CPATH=/root/.nix-profile/include:$CPATH \
  LIBRARY_PATH=/root/.nix-profile/lib:$LIBRARY_PATH \
  QTDIR=/root/.nix-profile:$QTDIR \
  NIXPACKS_PATH=/nix/var/nix/profiles/default/bin
