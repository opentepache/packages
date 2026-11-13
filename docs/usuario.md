# Repositorio APT de debfactory

debfactory publica versiones recientes de herramientas de desarrollo que no están al día en
Debian ni en Ubuntu, empaquetadas como `.deb` firmados para amd64 y arm64. Cada paquete pasó
lintian y una prueba de instalación real en cada distribución antes de publicarse.

| Paquete | Qué es | No disponible en |
|---|---|---|
| `fnm` | gestor de versiones de Node.js | — |
| `bob` | gestor de versiones de Neovim | bookworm (requiere glibc ≥ 2.39; bookworm tiene 2.36) |
| `buf` | herramientas para Protocol Buffers | — |
| `aqua` | gestor declarativo de herramientas CLI | — |

**Llave del repositorio** (llave maestra; compruébala antes de confiar en el repositorio):

```text
Fingerprint: 7267 1CD2 2168 4BF7 FDCD  FB39 65B2 7897 5D4F E04B
```

## Suites

Elige el valor de `<suite>` según tu distribución. Escríbelo tal cual: en Debian sid,
`/etc/os-release` dice `forky`, pero la suite es `sid`.

| Distribución | `<suite>` |
|---|---|
| Debian 12 | `bookworm` |
| Debian 13 | `trixie` |
| Debian sid (inestable) | `sid` |
| Ubuntu 24.04 LTS | `noble` |
| Ubuntu 26.04 LTS | `resolute` |

## Agregar el repositorio

Copia el bloque, cambia `<suite>` por el valor de la tabla y ejecútalo:

```sh debfactory:add
sudo apt-get install -y curl ca-certificates
sudo curl -fsSLo /etc/apt/keyrings/debfactory.gpg https://apt.opentepache.org/debfactory.gpg
sudo tee /etc/apt/sources.list.d/debfactory.sources >/dev/null <<'EOF'
Types: deb
URIs: https://apt.opentepache.org/
Suites: <suite>
Components: main
Architectures: amd64 arm64
Signed-By: /etc/apt/keyrings/debfactory.gpg
EOF
sudo apt-get update
```

La llave solo se usa para este repositorio (`Signed-By`): no afecta a la confianza en los
repositorios de tu distribución.

## Verificar la llave (opcional)

Requiere `gnupg` (`sudo apt-get install -y gnupg`):

```sh debfactory:verify
gpg --show-keys --with-fingerprint /etc/apt/keyrings/debfactory.gpg
```

El fingerprint de la llave maestra (`pub`) debe coincidir con el de arriba. Si no coincide,
quita el repositorio y no instales nada.

## Instalar

```sh debfactory:install
sudo apt-get install -y fnm buf aqua bob
```

En bookworm, sin `bob`.

## Actualizar

```sh debfactory:upgrade
sudo apt-get update
sudo apt-get upgrade -y
```

## Volver a una versión anterior

El repositorio conserva las últimas 3 versiones de cada paquete. Para ver cuáles hay y volver a
una de ellas:

```sh
apt-cache policy fnm
```

```sh debfactory:downgrade
sudo apt-get install -y --allow-downgrades fnm=<version>
```

Si una versión se retira por un defecto, `apt upgrade` no te la cambia sola: usa este mismo
comando con la versión anterior. Para que `apt upgrade` no vuelva a subirla hasta que quieras,
retenla con `sudo apt-mark hold fnm` (y `sudo apt-mark unhold fnm` para soltarla).

## Cambio de llave

Si se anuncia un cambio de llave, vuelve a descargar el keyring con la segunda línea del
bloque de "Agregar el repositorio":

```sh
sudo curl -fsSLo /etc/apt/keyrings/debfactory.gpg https://apt.opentepache.org/debfactory.gpg
```

El archivo `.sources` no cambia.

## Quitar el repositorio

```sh debfactory:remove
sudo rm -f /etc/apt/sources.list.d/debfactory.sources /etc/apt/keyrings/debfactory.gpg
sudo apt-get update
```

Los paquetes instalados se conservan. Para desinstalarlos también:

```sh
sudo apt-get purge fnm bob buf aqua
```
