# Релиз «Отвычки» в Google Play — что делаю я и что делаешь ты

## Что уже сделано (мной)
- ✅ PWA собран и живёт: https://nnenoix.github.io/otvychka/
- ✅ Android-приложение (TWA) собрано и подписано: `app-release-bundle.aab` (это и грузится в Play) + `app-release-signed.apk` (для проверки на телефоне).
- ✅ Ключ подписи создан: `otvychka-upload.keystore` (пароль — в `KEY-INFO.txt`). **Не теряй этот файл и пароль.**
- ✅ Привязка домена к приложению (Digital Asset Links): https://nnenoix.github.io/.well-known/assetlinks.json — чтобы приложение открывалось без адресной строки браузера.
- ✅ Политика конфиденциальности: https://nnenoix.github.io/otvychka/privacy.html
- ✅ Иконка, баннер, 4 скриншота, тексты карточки — в папке `store/`.

## Что можешь сделать только ты (аккаунт и личность)
1. **Аккаунт разработчика Google Play** — https://play.google.com/console/signup
   - Разовый взнос **$25**.
   - Верификация личности по документу (паспорт/права). Занимает от нескольких часов до пары дней.
   - Тип аккаунта: «Личный» (Personal).

2. **Загрузка приложения.** В Play Console:
   - Create app → язык: русский, тип: App, бесплатно.
   - Test and release → закрытое тестирование (Closed testing) → создать релиз → загрузить `app-release-bundle.aab`.
   - Заполнить карточку из `store/store-listing.md`, залить графику из `store/`.
   - Data safety: «данные не собираются» (по `store-listing.md`).
   - Политика конфиденциальности: вставить URL выше.

3. **Обязательное закрытое тестирование (для новых личных аккаунтов).**
   Google требует: минимум **12 тестеров** (раньше было 20), которые держат приложение установленным **14 дней подряд**, прежде чем откроется публикация в проде. Соберём список email (можно друзей/родню), добавим их в Closed testing.

4. **После первой загрузки .aab — пришли мне два отпечатка.**
   Play Console → Test and release → App integrity → App signing. Там два SHA-256: «App signing key» и «Upload key».
   Пришли оба — я допишу их в `assetlinks.json`, иначе в приложении сверху покажется адресная строка браузера (Play App Signing переподписывает приложение своим ключом, и его отпечаток надо добавить к привязке домена).

## Проверить приложение прямо сейчас, до всякого Play
Скинь `app-release-signed.apk` на Android-телефон и установи (разрешив «установку из этого источника»). Так увидишь ровно то, что попадёт в стор.

## Как пересобрать (если менял приложение)
```bash
cd D:/otvychka/android
export BUBBLEWRAP_KEYSTORE_PASSWORD='<пароль из KEY-INFO.txt>'
export BUBBLEWRAP_KEY_PASSWORD='<тот же пароль>'
export JAVA_HOME='C:\Users\yegor\twa-tools\jdk17\jdk-17.0.20.1+1'
bubblewrap update --skipVersionUpgrade   # подтянуть изменения из twa-manifest.json
bubblewrap build --skipPwaValidation     # собрать .aab + .apk
```
При каждом новом релизе в Play поднимай `appVersionCode` (и по желанию `appVersion`) в `twa-manifest.json`.

Примечание: сам PWA (index.html) обновляется мгновенно через GitHub Pages — пользователям приложения новый контент прилетает без переустановки, потому что TWA открывает живой сайт. Пересборка .aab нужна только при смене иконки, имени, версии или настроек оболочки.
