# Bitácora — tienda-laravel

Registro de qué se hizo y por qué, para poder retomar el desarrollo sin
perder contexto. Repo hermano de `plataforma-tienda` (la infraestructura
k3s), pero con su propia bitácora porque son historias independientes.

## Fase 0 (implícita) — PHP/Composer en la VM (2026-07-26)

No estaban instalados pese al contexto inicial. Instalado en la misma VM
del clúster (`plataforma-tienda`, ver su bitácora para el detalle de
paquetes RHEL):
- `php8.4` (PHP 8.4.21) — se eligió por sobre el `php` default (8.3.31)
  porque ambos proveen `/usr/bin/php` y son mutuamente excluyentes; 8.4
  es la apuesta más segura para Laravel 13.
- Extensiones: `pdo_pgsql`, `pecl-redis6`, `mbstring`, `xml`, `gd`,
  `bcmath`, `opcache`, `pecl-zip`.
- `unzip` (para Composer).
- `composer` 2.10.2, instalado con el instalador oficial de
  getcomposer.org, verificado contra su hash SHA-384 publicado.
- De yapa se instalaron `httpd` y `php8.4-fpm` (dependencias del paquete
  `php8.4`, no pedidas a mano) — verificado que quedan `disabled`/
  `inactive`, no arrancan solos, sin conflicto con Traefik en el puerto 80.

## Fase 1 — Scaffolding (2026-07-26)

- `composer create-project laravel/laravel tienda-laravel` en
  `~/tienda-laravel` (carpeta hermana a `~/plataforma-tienda`).
  Versión resultante: **Laravel 13.22.0**.
- Se eligió `composer create-project` en vez de instalar
  `laravel/installer` globalmente — un solo proyecto, no vale la pena la
  herramienta extra.
- Se borró `database/database.sqlite` (lo generó el scaffolding por
  defecto) porque el proyecto va a usar Postgres, no SQLite.
- `.gitignore` de Laravel (viene por defecto) ya excluye `.env`,
  `/vendor`, `/node_modules` — no hizo falta tocarlo.
- Repo git inicializado, primer commit (56 archivos), remoto
  `git@github.com:fnicolella/tienda-laravel.git` (SSH, reutilizando la
  clave `~/.ssh/id_ed25519_github` ya autorizada en la cuenta `fnicolella`
  — misma cuenta/host que `plataforma-tienda`, no hacía falta una clave
  nueva). Privado. Push inicial hecho.

**Fase 1 cerrada.**
