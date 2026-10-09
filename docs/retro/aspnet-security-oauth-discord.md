# Ретроспектива: AspNet.Security.OAuth.Discord

- Пакет: `AspNet.Security.OAuth.Discord` (aspnet-contrib)

Кожен учасник, що працював із бібліотекою, пише свій розділ за питаннями з умови лаби 3.

## Петренко Б. О.

### Які задачі розв'язує бібліотека

- Вхід у панель Discord-акаунтом і прив'язка Discord-акаунта без коду (ADR-0009): перенаправлення на сторінку Discord, обмін коду на токен, отримання користувача Discord (`/users/@me`) і claims `NameIdentifier`, `Name`.
- Захист потоку: PKCE, `state` під Data Protection, correlation cookie, що прив'язує потік до браузера.

### Чому саме ця бібліотека

Вбудований `OAuthHandler` ASP.NET Core вимагає вручну задати адреси Discord і мапінг полів користувача на claims. Пакет aspnet-contrib робить саме це й більше нічого, підтримується разом з ASP.NET Core (версія 10.0.0 для .NET 10). Власна реалізація OAuth2 означала б повторити state, PKCE і correlation cookie — найнебезпечніші місця протоколу.

### Документація

README пакета короткий, але досить, бо пакет — тонка надбудова над `OAuthHandler`, задокументованим у ASP.NET Core. Неочевидне — у поведінці cookie: що correlation cookie за замовчуванням `SameSite=None` (браузер відкидає її без `Secure` на http у розробці) і що зовнішня cookie має пережити повернення з чужого сайту.

### Проблеми та як їх розв'язали

1. **SameSite=Strict сесії.** Cookie, виставлена в ланцюжку редиректів, що почався на discord.com, на наступному кроці не надсилається. Останній крок відповідає сторінкою з `meta refresh`: наступна навігація йде з нашого сайту.
2. **Прив'язка без сесії.** З тієї самої причини на зворотному шляху немає `cf_session`; користувач, що почав прив'язку, їде в `AuthenticationProperties.Items`, тобто в захищеному `state`.
3. **Cookie потоку.** Correlation і зовнішня cookie — `SameSite=Lax` (переживають повернення з Discord), `Secure` поза розробкою, 5 хвилин.
4. **Тести без Discord.** `BackchannelHttpHandler` опцій підміняє token- і user-ендпоінти Discord; тест сам грає роль браузера: бере `state` з редиректу, викликає callback і перевіряє, що чужий браузер без correlation cookie відхиляється.

### Плюси й мінуси

Плюси:

- мінімум коду: адреси й мапінг claims готові;
- уся безпека потоку — з `OAuthHandler` ASP.NET Core;
- легко тестувати через `BackchannelHttpHandler`.

Мінуси:

- поведінка cookie за замовчуванням не підходить для SameSite=Strict-сесії й http у розробці;
- обробник веде потік через свою зовнішню cookie, тож кінцевий крок усе одно пишемо самі.

### Що змінили б

Так само: пакет поверх `OAuthHandler` і `DiscordLink` як єдина ідентичність замість зовнішніх логінів Identity. Інакше: від початку налаштувати проксі Vite з `X-Forwarded-Host`, щоб у розробці Discord повертав браузер на панель, а не на ЦА.

### Використані ресурси

- Репозиторій: https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers
- OAuth2 Discord: https://discord.com/developers/docs/topics/oauth2
- Cookie SameSite в ASP.NET Core: https://learn.microsoft.com/aspnet/core/security/samesite
