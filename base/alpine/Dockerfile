FROM alpine:latest

# Установка bash и других необходимых пакетов
RUN apk update && \
  apk add --no-cache bash tzdata sudo curl xz git ca-certificates openssl make pkgconfig musl libc6-compat && \
  update-ca-certificates

# Установка временной зоны Владивосток
RUN cp /usr/share/zoneinfo/Asia/Vladivostok /etc/localtime && \
  echo "Asia/Vladivostok" > /etc/timezone && \
  apk del tzdata

# Настройка nix
RUN mkdir -m 0755 /nix && mkdir -m 0755 /etc/nix && \
  addgroup -S nixbld && chown root /nix && \
  printf 'sandbox = false \nfilter-syscalls = false\n' > /etc/nix/nix.conf && \
  printf 'experimental-features = nix-command\n' >> /etc/nix/nix.conf && \
  for n in $(seq 1 10); do adduser -S -G nixbld -H -h /var/empty -D -s /sbin/nologin "nixbld$n"; done

# Установка nix
SHELL ["/bin/bash", "-c"]
RUN set -o pipefail && curl -L https://nixos.org/nix/install | bash && \
  /nix/var/nix/profiles/default/bin/nix-channel --remove nixpkgs && \
  /nix/var/nix/profiles/default/bin/nix-collect-garbage --delete-old && \
  printf 'if [ -d $HOME/.nix-profile/etc/profile.d ]; then\n for i in $HOME/.nix-profile/etc/profile.d/*.sh; do\n if [ -r $i ]; then\n . $i\n fi\n done\n fi\n' >> /root/.profile && \
  printf 'PATH=$NIXPACKS_PATH:$PATH' >> /root/.profile

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
