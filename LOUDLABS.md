# GLL Viewer в Loudlabs

`upstream` указывает на `CWBudde/gll-tools`, `origin` — на форк
`khvan-innokenty/loudlabs-gll`. Ветка `main` повторяет `upstream/main`.
Рабочая ветка форка — `loudlabs`.

## Обновление из оригинала

Из чистой рабочей копии выполните:

```sh
git fetch upstream main
git switch main
git merge --ff-only upstream/main
git push origin main
gh pr create --repo khvan-innokenty/loudlabs-gll --base loudlabs --head main \
  --title "Sync upstream main" --body "Merge the latest upstream changes into loudlabs."
```

Дождитесь проверки **Build and smoke-test viewer**. Для слияния используйте
**Create a merge commit**, чтобы сохранить историю оригинала и изменения Loudlabs.
При конфликте создайте временную ветку от `loudlabs`, влейте в неё `main`,
разрешите конфликт и откройте PR из временной ветки в `loudlabs`.

## Публикация

Workflow **Loudlabs Viewer** собирает WebAssembly, запускает браузерный smoke-тест
и сохраняет файлы сайта в артефакт `gll-viewer`. Для публикации запустите
workflow на ветке `loudlabs` с флагом `publish`. После успешной сборки и теста
отдельная задача скачает артефакт и загрузит его по SSH через rsync.
Затем она сверит опубликованные HTML и WASM с артефактом.

В окружении GitHub `production` задайте переменные `DEPLOY_HOST`, `DEPLOY_USER`,
`DEPLOY_PATH`, `DEPLOY_URL` и при необходимости `DEPLOY_PORT` (по умолчанию 22).
`DEPLOY_PATH` — отдельный существующий каталог сайта с правом записи,
`DEPLOY_URL` — его публичный HTTPS-адрес. На сервере нужен rsync.
В секретах окружения задайте `DEPLOY_SSH_KEY` и `DEPLOY_KNOWN_HOSTS`.
