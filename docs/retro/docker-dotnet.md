# Ретроспектива: Docker.DotNet

- Пакет: `Docker.DotNet.Enhanced` 4.3.3 (форк Docker.DotNet від Testcontainers)

Кожен учасник, що працював із бібліотекою, пише свій розділ за питаннями з умови лаби 3.

## Бояр В. О.

Статус: **чернетка.** Факти про задачі й проблеми заповнено; розділи з оцінками — дописати автору.

### Які задачі розв'язує бібліотека

- Агент звертається до Docker Engine на своєму хості: версія Docker для `Hello`, список контейнерів, запуск, зупинка й перезапуск контейнера (`src/Agent/Docker/DockerDotNetGateway.cs`).
- Бібліотека сама знаходить endpoint Docker так само, як docker CLI: `DOCKER_HOST`, потім поточний Docker-контекст, потім типовий сокет (`/var/run/docker.sock` на Linux, named pipe на Windows). Шлях до сокета в агенті не захардкоджено.
- Далі: події контейнерів (live-оновлення панелі) і стрімінг логів.

### Чому саме ця бібліотека

Альтернативи: викликати `docker` CLI і розбирати його вивід або писати HTTP-запити до Docker Engine API вручну. Docker.DotNet дає типізовані моделі API й типізовані винятки. `docker compose` агент однаково викликатиме через CLI (там бібліотека не допоможе), але прості операції з контейнерами зручніше робити через API.

Оригінальний пакет `Docker.DotNet` (3.125.15) оновлюється рідко. Форк `Docker.DotNet.Enhanced` підтримує Testcontainers: регулярні релізи (останній — червень 2026), збірка під .NET 10, Docker API v29. Обрали форк.

### Документація

_Дописати автору: наскільки легко було знайти відповіді, чого бракувало._

Факти, від яких можна відштовхнутися: окремої документації в форку майже немає, API довелося з'ясовувати з XML-коментарів пакета й перевіряти невеликою програмою (`dotnet run probe.cs` з директивою `#:package`). Приклади з інтернету написані для оригінального пакета, де клієнт створюється через `DockerClientConfiguration`; у версії 4.x форку його замінив `DockerClientBuilder`.

### Проблеми та як їх розв'язали

1. **Інший API створення клієнта.** Приклади для `Docker.DotNet` (`new DockerClientConfiguration().CreateClient()`) не компілюються з форком 4.x. Правильно — `new DockerClientBuilder().Build()`.
2. **Таймаут клієнта 100 с.** ЦА дозволяє час коректної зупинки контейнера до 5 хв, а Docker відповідає на `stop` лише після зупинки. Глобальний таймаут вимкнено (`WithTimeout(Timeout.InfiniteTimeSpan)`), а кожен виклик отримує власний бюджет: 30 с для швидких запитів, 30 с + час зупинки для stop/restart. Коли бюджет вичерпано, повертається `ERROR_CODE_TIMEOUT`.
3. **Як виглядає «Docker недоступний».** На Windows з вимкненим Docker Desktop підключення до named pipe чекає 10 с і кидає `TimeoutException`; на Linux це `HttpRequestException` / `SocketException`. Усі такі винятки агент перетворює на `ERROR_CODE_DOCKER_UNAVAILABLE`, і панель показує 502 з текстом «Docker is not reachable on this host.».
4. **«Вже запущений» і «вже зупинений».** Docker відповідає 304, а бібліотека повертає `false` зі `StartContainerAsync` / `StopContainerAsync`. Для агента це успіх, бо бажаний стан уже досягнуто.
5. **Помилки Docker для користувача.** `DockerApiException.Message` містить службовий текст разом із JSON-тілом. Агент бере з `ResponseBody` лише поле `message` (наприклад, «cannot restart container…») і передає в панель тільки його.
6. **Id контейнера стає частиною шляху запиту.** Щоб рядок з `/` чи `..` не звернувся до іншого endpoint'а Docker API, id перевіряється регулярним виразом `^[a-zA-Z0-9][a-zA-Z0-9_.-]*\z` ще до виклику бібліотеки.
7. **Час створення контейнера.** `ContainerListResponse.Created` — це `DateTime`, тож для unix-мілісекунд у протоколі `Kind` явно приводиться до UTC.

### Плюси й мінуси

_Дописати автору._

Факти: типізовані моделі й винятки (`DockerContainerNotFoundException`) замість розбору тексту; endpoint визначається автоматично на Linux і Windows; мало документації для форку, API змінюється між мажорними версіями; `docker compose` бібліотека не покриває.

### Що змінили б

_Дописати автору._

### Використані ресурси

- Репозиторій форку: https://github.com/testcontainers/Docker.DotNet
- Пакет на NuGet: https://www.nuget.org/packages/Docker.DotNet.Enhanced
- Docker Engine API: https://docs.docker.com/reference/api/engine/
