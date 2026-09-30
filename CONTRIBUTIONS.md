# Облік елементів лабораторних робіт

Кожен учасник веде свій розділ і оновлює його в тому самому PR, де додає код. Класи, методи, ієрархії, екрани й обробники (лаби 1–2) зараховуються лише одному учаснику. Бібліотеки (лаба 3) — ні: кожен, хто з бібліотекою працював, описує своє використання й пише свій розділ ретроспективи.

Що рахується (за умовою курсу):

- **Лаба 1.** Класи (або подібні типи), поля, нетривіальні методи, ієрархії успадкування, випадки поліморфізму з клієнтським кодом. Тривіальними вважаються геттери й сеттери, що лише читають або пишуть поле, і конструктори, що лише ініціалізують поля. Такі методи в списки не потрапляють.
- **Лаба 2.** Екрани, елементи керування, контейнерні елементи, обробники подій вебпанелі.
- **Лаба 3.** Зовнішні бібліотеки й використані можливості; ретроспектива — у `docs/retro/<бібліотека>.md`.

Тести в облік не входять, але для довідки вказано, що вони покривають.

## Петренко Богдан Олександрович (@lepterion)

Зона: контракти, доменна модель ЦА, ієрархія команд ЦА, пайплайн деплою, SOPS, авторизація.

### Лаба 1

| Вимога | Потрібно | Є | Стан |
|---|---|---|---|
| Класи та подібні типи | ≥ 9 | 151 (118 класів, 16 інтерфейсів, 6 структур, 11 enum) | ✅ |
| Поля | ≥ 15 | 240 (207 полів стану, 33 константи й статичні поля) | ✅ |
| Нетривіальні методи | ≥ 25 | 181 | ✅ |
| Ієрархії успадкування | ≥ 2, одна з ≥ 3 класів | 5 (9, 3, 5, 7 і 5 класів) | ✅ |
| Поліморфізм | ≥ 3, серед них статичний і динамічний | 6 з клієнтським кодом (4 динамічні, 2 статичні) | ✅ |

Код: `src/Aggregator/Domain` (PR #3); `src/Aggregator/Application`, `src/Aggregator/Infrastructure`, `src/Aggregator/Api` (PR #6); команди контейнерів і їх авторизація (PR #8); поточний користувач і фільтрація за видимістю (PR #9); пошук контейнера за префіксом id (PR #12); пайплайн деплою, репозиторії й стеки (PR #13); контракти керування користувачами (PR #15).

Не враховано: тимчасові `DevEnvironmentEndpoints`, `DevAccessEndpoints`, `DevStackEndpoints` і `DevUserAuthenticationHandler` (прибираються, коли з'явиться API панелі), `Program.cs`, згенерований код міграцій і proto.

#### Класи та подібні типи

| Простір імен `ContainerForest.Aggregator.Domain.*` | Типи |
|---|---|
| `Common` | `Entity<TId>` (абстрактний клас), `IStronglyTypedId<TSelf>` (інтерфейс), `EnvironmentId`, `RepositoryId`, `StackId`, `DeploymentId`, `UserId`, `RoleAssignmentId` (структури), `DomainException` (абстрактний клас), `DomainValidationException`, `DomainConflictException`, `SecretToken` (статичний клас) |
| `Environments` | `DockerEnvironment` |
| `Repositories` | `GitRepository` |
| `Stacks` | `Stack`, `StackName`, `StackFilePath`, `CommitSha`, `BranchName`, `StackOperation` (enum) |
| `Deployments` | `Deployment`, `DeploymentLogLine`, `ImageReference`, `DeploymentStatus`, `DeploymentTrigger`, `ImagePullPolicy`, `DeploymentLogLevel` (enum) |
| `Access` | `User`, `RoleAssignment` (абстрактний клас), `EnvironmentRoleAssignment`, `StackRoleAssignment`, `AccessPolicy` (статичний клас), `RolePermissions` (статичний клас), `Role`, `Permission` (enum) |

| Простір імен `ContainerForest.Aggregator.Application.*` | Типи |
|---|---|
| `Common` | `IRepository<TEntity, TId>`, `IUnitOfWork` (інтерфейси), `NotFoundException` |
| `Agents` | `IAgentChannel` (інтерфейс), `ContainerSnapshot`, `ContainerState` (enum), `AgentException` (абстрактний клас), `AgentOfflineException`, `AgentDisconnectedException`, `AgentTimeoutException`, `AgentCommandFailedException`, `AgentErrorKind` (enum), `AgentCredentials`, `AgentEnrollmentService`, `StackBundle`, `BundleFile` |
| `Environments` | `IEnvironmentRepository` (інтерфейс), `EnvironmentService`, `EnrollmentToken`, `EnvironmentCreated`, `EnvironmentSummary` |
| `Access` | `IUserRepository`, `IRoleAssignmentRepository`, `ICurrentUser` (інтерфейси), `AccessDeniedException`, `UnauthenticatedException`, `CurrentActor` |
| `Stacks` | `IStackRepository` (інтерфейс), `StackService`, `StackSummary` |
| `Repositories` | `IGitRepositoryRepository` (інтерфейс), `RepositoryService` |
| `Deployments` | `IDeployStep`, `IGitSource`, `IComposeParser`, `IDeploymentQueue`, `IDeploymentRepository` (інтерфейси), `DeployPipeline`, `DeployContext`, `DeploymentService`, `DeploymentService.VisibleStack`, `DeploymentRunner`, `DeploymentOptions`, `DeploymentFailedException`, `GitSnapshot`, `SourceFile`, `ComposeFile`, `ComposeService`, `DeploymentSummary` |
| `Deployments.Steps` | `GitFetchStep`, `ValidateComposeStep`, `BuildBundleStep`, `SendToAgentStep` |
| `Containers` | `IContainerCommand` (інтерфейс), `ContainerCommand` (абстрактний клас), `StartContainerCommand`, `StoppingContainerCommand` (абстрактний клас), `StopContainerCommand`, `RestartContainerCommand`, `AuthorizedCommand`, `ContainerCommandService`, `ContainerCommandContext`, `ContainerAction` (enum), `ContainerQueryService`, `VisibleContainer`, `ContainerLookup` (статичний клас) |
| — | `DependencyInjection` (статичний клас) |

| Простір імен `ContainerForest.Aggregator.Infrastructure.*` | Типи |
|---|---|
| `Persistence` | `AggregatorDbContext`, `StronglyTypedIdConverter<TId>`, `ModelConfigurationBuilderExtensions`, `EfRepository<TEntity, TId>`, `EnvironmentRepository`, `StackRepository`, `UserRepository`, `RoleAssignmentRepository`, `GitRepositoryRepository`, `DeploymentRepository`, `DatabaseMigrator`, `DesignTimeDbContextFactory` |
| `Persistence.Configurations` | `EnvironmentConfiguration`, `RepositoryConfiguration`, `StackConfiguration`, `DeploymentConfiguration`, `UserConfiguration`, `RoleAssignmentConfiguration`, `EnvironmentRoleAssignmentConfiguration`, `StackRoleAssignmentConfiguration` |
| `Agents` | `AgentGatewayOptions`, `AgentSession`, `AgentSession.PendingRequest`, `AgentRegistry`, `GrpcAgentChannel` |
| `Deployments` | `GitOptions`, `LibGit2SharpGitSource`, `LibGit2SharpGitSource.TreeReader`, `YamlComposeParser`, `DeploymentQueue`, `DeploymentWorker` |
| — | `DependencyInjection` (статичний клас) |

| Простір імен `ContainerForest.Aggregator.Api.*` | Типи |
|---|---|
| `Grpc` | `AgentGatewayService` |
| `ErrorHandling` | `ProblemDetailsExceptionHandler` |
| `Access` | `ClaimsCurrentUser` |
| `Contracts` | `LoginRequest`, `MeResponse`, `ChangePasswordRequest`, `UserResponse`, `UserDetailsResponse`, `CreateUserRequest`, `RenameUserRequest`, `SetAdminRequest`, `ResetPasswordRequest`, `RoleAssignmentResponse`, `GrantRoleRequest`, `ChangeRoleRequest`, `RoleScope` (enum) |
| `Endpoints` | `MeEndpoints` |

#### Поля

| Тип | Поля стану | Константи та статичні поля |
|---|---|---|
| `Entity<TId>` | `Id` | |
| 6 структур ID | `Value` у кожній (6) | |
| `StackName`, `StackFilePath`, `CommitSha`, `BranchName` | `Value` у кожному (4) | `StackName.MaxLength`, `StackFilePath.MaxLength`, `BranchName.MaxLength`, `BranchName.ForbiddenChars` |
| `DockerEnvironment` | `Name`, `IsPrivate`, `CreatedAt`, `EnrollmentTokenHash`, `EnrollmentTokenExpiresAt`, `AgentSecretHash`, `EnrolledAt`, `AgentVersion`, `LastBootId`, `LastSeenAt` | `MaxNameLength`, `DefaultEnrollmentTokenLifetime` |
| `GitRepository` | `Name`, `Url`, `ProtectedAccessToken`, `CreatedAt` | `MaxNameLength` |
| `Stack` | `EnvironmentId`, `Name`, `RepositoryId`, `Branch`, `ComposePath`, `IsSystem`, `CreatedAt`, `DeployedCommit`, `DeployedAt` | |
| `Deployment` | `StackId`, `Trigger`, `RequestedBy`, `PullPolicy`, `Status`, `Commit`, `QueuedAt`, `StartedAt`, `FinishedAt`, `Error` | `MaxErrorLength` |
| `User` | `UserName`, `IsAdmin`, `IsActive`, `CreatedAt` | `MinUserNameLength`, `MaxUserNameLength` |
| `RoleAssignment` та підкласи | `UserId`, `Role`, `GrantedAt`; `EnvironmentRoleAssignment.EnvironmentId`; `StackRoleAssignment.StackId`, `StackRoleAssignment.EnvironmentId` | |
| `SecretToken` | | `TokenBytes` |
| `DockerEnvironment` (PR #6) | `AgentHostname` | `MaxHostnameLength` |
| `NotFoundException` | `EntityName`, `Id` | |
| `ContainerSnapshot` | `Id`, `Name`, `State`, `Image`, `StackName`, `ServiceName`, `StatusText`, `CreatedAt` | |
| `AgentException` та підкласи | `EnvironmentId`; `AgentTimeoutException.Timeout`; `AgentCommandFailedException.Kind` | |
| `EnrollmentToken`, `EnvironmentCreated`, `EnvironmentSummary`, `AgentCredentials` | 2 + 3 + 8 + 2 | |
| `AgentGatewayOptions` | `CommandTimeout`, `HelloTimeout`, `IdleTimeout` | `SectionName` |
| `ContainerCommandContext` | `Actor`, `Environment`, `Container`, `Stack` | |
| `StoppingContainerCommand` | `StopTimeout` | `DefaultStopTimeout`, `MaxStopTimeout` |
| `CurrentActor` | `actor` | |
| `VisibleContainer` | `Container`, `CanControl` | |
| `LoginRequest`, `MeResponse` | `UserName`, `Password`, `RememberMe`; `Id`, `UserName`, `IsAdmin` | |
| DTO керування користувачами (PR #15) | `ChangePasswordRequest` 2, `UserResponse` 6, `UserDetailsResponse` 2, `CreateUserRequest` 3, `RenameUserRequest` 1, `SetAdminRequest` 1, `ResetPasswordRequest` 1, `RoleAssignmentResponse` 8, `GrantRoleRequest` 3, `ChangeRoleRequest` 1 | |
| `EnvironmentSummary` (PR #9) | `Role` | |
| `AgentSession` | `writer`, `logger`, `writeLock`, `pending`, `closed`, `closing`, `EnvironmentId`, `SessionId`, `ConnectedAt`, `LastMessageAt` | |
| `AgentRegistry` | `sessions` | |
| `EfRepository<TEntity, TId>` | `Db` | |
| `EnvironmentConfiguration`, `RoleAssignmentConfiguration` | | `Sha256HexLength`; `EnvironmentScope`, `StackScope` |
| `Infrastructure.DependencyInjection`, `AgentGatewayService` | | `ConnectionStringName`; `AgentIdHeader`, `AgentSecretHeader` |
| `ImageReference` (PR #13) | `Name`, `Tag`, `Digest` | `ImplicitTag` |
| `Deployment` (PR #13), `DeploymentLogLine` | `log`; `Sequence`, `At`, `Level`, `Text` | `MaxLogLines`, `MaxLogLineLength` |
| `GitSnapshot`, `SourceFile`, `ComposeFile`, `ComposeService` | 3 + 3 + 1 + 4 | |
| `DeploymentSummary`, `StackSummary`, `DeploymentService.VisibleStack` | 9 + 9 + 4 | `DeploymentService.HistoryLimit` |
| `DeploymentOptions`, `GitOptions` | `MaxBundleBytes`, `AgentTimeout`, `MaxConcurrentDeployments`; `CacheDirectory` | `SectionName` в обох |
| `DeployContext` | `source`, `bundle`, `Deployment`, `Stack`, `Environment`, `Repository` | |
| `DeployPipeline` | `steps` | |
| `StackBundle`, `BundleFile` | `StackName`, `Commit`, `Files`, `Pull`; `Path`, `Content`, `Mode` | `ComposeFileName`; `RegularMode`, `ExecutableMode` |
| `LibGit2SharpGitSource`, `TreeReader` | `locks`; `totalBytes`, `Files`, `Skipped` | `RemoteName` |
| `YamlComposeParser`, `DeploymentRepository` | | `MergeKey`; `Unfinished` |
| `DeploymentQueue`, `AgentSession.PendingRequest` | `channel`; `Completion`, `OnProgress` | |

#### Нетривіальні методи (181)

| Тип | Методи |
|---|---|
| `Entity<TId>` (1) | `Equals(Entity<TId>)` — порівняння за типом та ідентифікатором |
| `SecretToken` (3) | `Generate`, `Hash`, `Matches` (порівняння за сталий час) |
| `StackName` (2) | `Parse`, `TryParse` |
| `StackFilePath` (3) | `Parse`, `TryParse`, `IsValid` |
| `CommitSha` (2) | `Parse`, `TryParse` |
| `BranchName` (3) | `Parse`, `TryParse`, `IsValid` |
| `DockerEnvironment` (10) | `Create`, `Rename`, `IssueEnrollmentToken`, `TryEnroll`, `VerifyAgentSecret`, `RecordConnection`, `RevokeAgent`, `TouchLastSeen`, `NormalizeName`, `NormalizeHostname` |
| `GitRepository` (6) | `Create`, `Rename`, `ChangeUrl`, `SetAccessToken`, `ParseUrl`, `NormalizeName` |
| `Stack` (3) | `EnsureAllows`, `RequiresUpdateHelper`, `RecordDeployment` |
| `Deployment` (8) | `Start`, `ResolveCommit`, `Succeed`, `Fail`, `Cancel`, `TransitionTo`, `EnsureStatus`, `AppendLog` (ліміт рядків і довжини) |
| `ImageReference` (1) | `TryParse`: ім'я, тег, digest, порт registry |
| `RolePermissions` (3) | `MinimumRole`, `Grants`, `RequiredFor` |
| `User` (4) | `Rename`, `GrantAdmin`, `EnsureActive`, `NormalizeUserName` |
| `RoleAssignment` та підкласи (8) | `ChangeRole`, `EnsureDefined`; `Covers`, `CoversWholeEnvironment`, `Reveals` в `EnvironmentRoleAssignment` і `StackRoleAssignment` (по 3) |
| `AccessPolicy` (8) | `CanSee(User, DockerEnvironment, …)`, `EffectiveRole` для стеку, `EffectiveRole` для середовища, `CanPerform` для стеку, `CanPerform` для середовища, `Highest`, `OwnAssignments`, `EnsureSameEnvironment` |
| `EnvironmentService` (4) | `CreateAsync`, `IssueEnrollmentTokenAsync`, `ListVisibleAsync`, `IssueToken` |
| `AgentEnrollmentService` (4) | `EnrollAsync`, `AuthenticateAsync`, `RecordConnectedAsync`, `RecordDisconnectedAsync` |
| `Application.DependencyInjection` (1) | `AddApplication` |
| `AggregatorDbContext` (2) | `OnModelCreating`, `ConfigureConventions` |
| `ModelConfigurationBuilderExtensions` (1) | `AddStronglyTypedId` |
| 8 конфігурацій EF (8) | `Configure` у кожній |
| `EfRepository<TEntity, TId>` (2) | `FindAsync`, `GetAsync` |
| `EnvironmentRepository` (3) | `FindByEnrollmentTokenHashAsync`, `ListAsync`, `NameExistsAsync` |
| `StackRepository`, `UserRepository`, `RoleAssignmentRepository` (4) | `FindByNameAsync`, `ListByEnvironmentAsync`, `FindByUserNameAsync`, `ListForUserAsync` |
| `DatabaseMigrator`, `DesignTimeDbContextFactory` (2) | `StartAsync`, `CreateDbContext` |
| `AgentGatewayOptions` (1) | `IsValid` |
| `AgentSession` (6) | `RequestAsync`, `WriteAsync`, `RecordActivity`, `ReportProgress`, `Complete`, `Close` |
| `AgentRegistry` (3) | `Open`, `Remove`, `TryGet` |
| `GrpcAgentChannel` (10) | `ListContainersAsync`, `StartContainerAsync`, `StopContainerAsync`, `RestartContainerAsync`, `DeployStackAsync`, `SendAsync`, `WaitFor`, `ToSnapshot`, `ToState`, `ToErrorKind` |
| `Infrastructure.DependencyInjection` (1) | `AddInfrastructure` |
| `AgentGatewayService` (9) | `Enroll`, `Connect`, `AuthenticateAsync`, `ReadHelloAsync`, `ReceiveAsync`, `Dispatch`, `RecordDisconnectedAsync`, `WithEnrollmentAsync` (дві версії) |
| `ProblemDetailsExceptionHandler` (2) | `TryHandleAsync`, `StatusFor` |
| `ContainerCommand` (2) | `ExecuteAsync` (шаблонний метод), `EnsureAllowedState` |
| `StartContainerCommand`, `StopContainerCommand`, `RestartContainerCommand` (6) | `IsAllowedIn`, `SendAsync` у кожній |
| `StoppingContainerCommand` (1) | `ValidateTimeout` |
| `AuthorizedCommand` (1) | `ExecuteAsync` |
| `ContainerCommandService` (2) | `ExecuteAsync`, `Create` |
| `CurrentActor` (2) | `GetAsync`, `GetAdministratorAsync` |
| `ContainerQueryService` (2) | `ListAsync`, `Can` |
| `ContainerLookup` (1) | `Find`: повний id, точне ім'я, унікальний префікс id |
| `DeployPipeline` (2) | `RunAsync`, `Describe` |
| `GitFetchStep`, `BuildBundleStep`, `SendToAgentStep` (3) | `ExecuteAsync` у кожному |
| `ValidateComposeStep` (3) | `ExecuteAsync`, `Check`, `Decode` |
| `DeploymentService` (4) | `QueueAsync`, `ListAsync`, `GetAsync`, `LoadVisibleStackAsync` |
| `DeploymentRunner` (2) | `RunAsync`, `FailInterruptedAsync` |
| `DeploymentOptions` (1) | `IsValid` |
| `RepositoryService`, `StackService` (4) | `CreateAsync`, `ListAsync`; `CreateAsync`, `ListVisibleAsync` |
| `LibGit2SharpGitSource` (5) | `FetchAsync` (дві версії), `Fetch`; `TreeReader.Read`, `TreeReader.ReadBlob` |
| `YamlComposeParser` (2) | `Parse`, `Find` (з merge-ключами) |
| `DeploymentWorker` (3) | `StartAsync`, `ExecuteAsync`, `RunAsync` |
| `GitRepositoryRepository`, `DeploymentRepository` (5) | `ListAsync`, `NameExistsAsync`; `HasUnfinishedAsync`, `ListUnfinishedAsync`, `ListByStackAsync` |
| `ClaimsCurrentUser` (1) | `Id` (читає й розбирає claim) |
| `MeEndpoints` (1) | `MapMeEndpoints` |

Не враховано як тривіальні: фабрики `Queue*`, `Create*` у `Stack`, `User` і `RoleAssignment`, методи `MakePrivate`, `MakePublic`, `RemoveAccessToken`, `ChangeSource`, `RevokeAdmin`, `Activate`, `Deactivate`, методи-делегатори (`RecordHeartbeat`, `CanSee` для стеку, `CanPerform` для операції, `IsActiveAdmin`, `Equals(object)`, `EfRepository.Add`/`Remove`, `AgentRegistry.IsOnline`, `GrpcAgentChannel.IsOnline`, `NullIfEmpty`, `StronglyTypedIdConverter.Create`, `DeployContext.Info`/`Warn`/`Error`, `DeploymentQueue.Enqueue`, `BuildBundleStep.Kib`), обчислювані властивості (`StackFilePath.Directory`/`FileName`, `ImageReference.IsFloatingLatest`, `StackBundle.TotalBytes`, `Deployment.Log`) і методи структур ID.

#### Ієрархії успадкування

1. `Entity<TId>` → `DockerEnvironment`, `GitRepository`, `Stack`, `Deployment`, `User`, `RoleAssignment` → `EnvironmentRoleAssignment`, `StackRoleAssignment`. Це 9 класів на трьох рівнях.
2. `DomainException` → `DomainValidationException`, `DomainConflictException`. Це 3 класи.
3. `AgentException` → `AgentOfflineException`, `AgentDisconnectedException`, `AgentTimeoutException`, `AgentCommandFailedException`. Це 5 класів.
4. `EfRepository<TEntity, TId>` → `EnvironmentRepository`, `StackRepository`, `UserRepository`, `RoleAssignmentRepository`, `GitRepositoryRepository`, `DeploymentRepository`. Це 7 класів.
5. `ContainerCommand` → `StartContainerCommand`, `StoppingContainerCommand` → `StopContainerCommand`, `RestartContainerCommand`. Це 5 класів на трьох рівнях.

#### Поліморфізм

| # | Вид | Поліморфний інтерфейс | Клієнтський код |
|---|---|---|---|
| 1 | Динамічний (віртуальні методи) | `RoleAssignment.Covers`, `CoversWholeEnvironment`, `Reveals` | `AccessPolicy.EffectiveRole` і `AccessPolicy.CanSee` працюють зі списком `RoleAssignment`, не знаючи конкретних підкласів ✅ |
| 2 | Статичний (generics, `static abstract`) | `StronglyTypedIdConverter<TId>` викликає `IStronglyTypedId<TSelf>.From` | `AggregatorDbContext.ConfigureConventions` реєструє один конвертер через `AddStronglyTypedId<TId>` для всіх шести типів ID ✅ |
| 3 | Статичний (generics) | `IRepository<TEntity, TId>` і `EfRepository<TEntity, TId>` | `EnvironmentService` і `AgentEnrollmentService` викликають `FindAsync` і `GetAsync`, реалізовані один раз для будь-якої сутності ✅ |
| 4 | Динамічний (декоратор) | `IContainerCommand`: реалізують `ContainerCommand` і `AuthorizedCommand` | `ContainerCommandService` виконує команду через `IContainerCommand`, а `AuthorizedCommand` викликає вкладену команду, не знаючи її конкретного типу ✅ |
| 5 | Динамічний (шаблонний метод) | `ContainerCommand.IsAllowedIn`, `SendAsync`, `AllowedOnSystemStack` | `ContainerCommand.ExecuteAsync` виконує однакові перевірки для старту, зупинки й перезапуску, а відмінності бере з перевизначених методів ✅ |
| 6 | Динамічний (ланцюжок кроків) | `IDeployStep`: реалізують `GitFetchStep`, `ValidateComposeStep`, `BuildBundleStep`, `SendToAgentStep` | `DeployPipeline.RunAsync` виконує кроки зі списку `IDeployStep`, не знаючи їхніх типів; новий етап (SOPS) додається без зміни пайплайну ✅ |

#### Тести (не входять в облік)

- `tests/Aggregator/Domain`, 126 тестів: value objects, посилання на образ (тег, digest, `latest`), реєстрація агента, стан-машина й лог деплою, правила системних стеків, ефективна роль і видимість середовищ.
- `tests/Aggregator/Infrastructure`, 28 тестів: зіставлення запитів і відповідей у `AgentSession`, прогрес команди, закриття сесії, таймаут, заборона паралельного запису в стрім, заміна сесії в `AgentRegistry`; fetch зі справжнього локального git-репозиторію (нові коміти, видалена гілка, права на виконання, символьні посилання, ліміт розміру); розбір compose-файлів з якорями й merge-ключами.
- `tests/Aggregator/Application`, 70 тестів: пайплайн деплою (порядок кроків, перша помилка зупиняє, приховування внутрішніх помилок), кожен крок деплою, права на деплой, один деплой на стек, офлайн-агент, системний стек; пошук контейнера за id, ім'ям і префіксом; команди контейнерів — дозволені стани, системні стеки, таймаут зупинки, права через роль на середовищі й на стеку, адміністратор, деактивований і анонімний користувач, приховування чужого середовища без звернення до агента; `CurrentActor`; фільтрація середовищ і контейнерів за видимістю; керування середовищами лише для адміністраторів.
- `tests/Aggregator/Api`, 21 інтеграційний тест: ЦА в пам'яті, фейковий агент через gRPC, git підмінено заглушкою. Деплой через агента з прогресом і логом, помилка агента, крок, що зупиняє деплой до агента, конфлікт двох деплоїв, права Viewer. Реєстрація, повторне використання токена, невірний секрет, Hello першим повідомленням, список контейнерів, команда до офлайн-агента, помилка агента, обрив з'єднання, перезапуск контейнера через агента, 403 і 401 для dev-користувачів, `/api/me`, приховане приватне середовище, заборона створювати середовища звичайному користувачу.

### Лаба 2

Ще немає. Заплановано: список стеків, редактор стеку, історія деплоїв, ключі середовищ.

### Лаба 3

| Бібліотека | Що використано | Ретроспектива |
|---|---|---|
| gRPC для .NET (`Grpc.AspNetCore`, `Grpc.Tools`, `Google.Protobuf`), почато в PR #6 | Генерація коду з `.proto` окремо для сервера й клієнта; двонапрямлений стрім `Connect`; автентифікація через metadata; коди статусу (`Unauthenticated`, `InvalidArgument`, `DeadlineExceeded`); `Grpc.Net.Client` з `TestServer` в інтеграційних тестах | [docs/retro/grpc.md](docs/retro/grpc.md), чернетка |
| EF Core + SQLite, почато в PR #6 | Мапінг доменних класів без окремих моделей; міграції й застосування при старті; конвертери значень для ID і value objects; конвенції (`ConfigureConventions`); TPH-успадкування з дискримінатором; фільтровані унікальні індекси; колація `NOCASE`; owned-колекція (`OwnsMany`) для логу деплою; проєкції без завантаження логу | [docs/retro/ef-core.md](docs/retro/ef-core.md), чернетка |
| LibGit2Sharp, почато в PR #13 | Bare-клон як кеш; fetch однієї гілки з refspec і `Prune`; читання файлів із дерева коміту без checkout (`Tree`, `Blob`, `Mode`); скасування через колбек прогресу | [docs/retro/libgit2sharp.md](docs/retro/libgit2sharp.md), чернетка |
| YamlDotNet, почато в PR #13 | Дерево вузлів `YamlStream` / `YamlMappingNode` замість типізованої моделі; якорі й merge-ключі; позиція помилки з `YamlException.Start` | [docs/retro/yamldotnet.md](docs/retro/yamldotnet.md), чернетка |

## Бояр Володимир Олександрович (@volodaboyar1-star)

Зона: агент (обробники команд `IAgentCommandHandler`, робота з Docker і Docker Compose).

### Лаба 1

| Вимога | Потрібно | Є | Стан |
|---|---|---|---|
| Класи та подібні типи | ≥ 9 | 34 (30 класів, з них 3 статичні й 1 record; 4 інтерфейси) | ✅ |
| Поля | ≥ 15 | 58 (42 поля стану, 16 констант і статичних полів) | ✅ |
| Нетривіальні методи | ≥ 25 | 47 | ✅ |
| Ієрархії успадкування | ≥ 2, одна з ≥ 3 класів | 2 (6 класів на трьох рівнях; 7 класів) | ✅ |
| Поліморфізм | ≥ 3, серед них статичний і динамічний | 5 з клієнтським кодом (4 динамічні, 1 статичний) | ✅ |

Код: `src/Agent` (PR «агент: реєстрація, з'єднання, команди контейнерів»).

Не враховано: `Program.cs`, згенерований з proto код.

#### Класи та подібні типи

| Простір імен `ContainerForest.Agent.*` | Типи |
|---|---|
| — | `AgentInfo` (статичний клас), `FatalAgentException`, `DependencyInjection` (статичний клас) |
| `Configuration` | `AgentOptions` |
| `Enrollment` | `AgentCredentials` (record), `CredentialStore`, `AgentEnroller` |
| `Platform` | `IBootIdProvider` (інтерфейс), `LinuxBootIdProvider`, `UptimeBootIdProvider` |
| `Docker` | `IDockerGateway` (інтерфейс), `DockerDotNetGateway`, `ContainerInfoMapper` (статичний клас) |
| `Commands` | `IAgentCommandHandler`, `ICommandProgress` (інтерфейси), `AgentCommandHandler<TCommand>` (абстрактний клас), `ContainerActionHandler<TCommand>` (абстрактний клас), `ListContainersHandler`, `StartContainerHandler`, `StopContainerHandler`, `RestartContainerHandler`, `CommandDispatcher`, `AgentCommandException` (абстрактний клас), `InvalidCommandArgumentException`, `ContainerNotFoundException`, `DockerUnavailableException`, `DockerCommandFailedException`, `DockerTimeoutException`, `UnsupportedCommandException` |
| `Connection` | `AgentStreamWriter`, `StreamCommandProgress`, `ReconnectBackoff`, `AgentSessionRunner`, `AgentWorker` |

#### Поля

Поля, що зберігаються з параметрів первинного конструктора, враховано як поля стану.

| Тип | Поля стану | Константи та статичні поля |
|---|---|---|
| `AgentOptions` | `ControllerUrl`, `EnrollmentToken`, `DataDirectory`, `HeartbeatInterval` | `SectionName` |
| `AgentCredentials` | `AgentId`, `AgentSecret` | |
| `CredentialStore` | `options` | `FileName`, `Json` |
| `AgentEnroller` | `store`, `options`, `logger` | |
| `AgentInfo` | | `Version` |
| `LinuxBootIdProvider` | `path` | `DefaultPath` |
| `UptimeBootIdProvider` | `time`, `uptime` | |
| `AgentCommandException` та підкласи | `Code`; `ContainerNotFoundException.ContainerId`; `DockerTimeoutException.Timeout` | |
| `DockerDotNetGateway` | `client` | `RequestTimeout` |
| `ContainerInfoMapper` | | `ProjectLabel`, `ServiceLabel` |
| `AgentCommandHandler<TCommand>` | `logger` | `CommandField` |
| `ContainerActionHandler<TCommand>` | `Docker` | `MaxContainerIdLength` |
| `ListContainersHandler` | `docker` | |
| `CommandDispatcher` | `handlers`, `logger` | |
| `AgentStreamWriter` | `stream`, `writeLock` | |
| `StreamCommandProgress` | `requestId`, `writer` | |
| `ReconnectBackoff` | `random`, `Attempts` | `BaseDelay`, `MaxDelay`, `StableSession`, `MaxExponent` |
| `AgentSessionRunner` | `dispatcher`, `docker`, `bootIds`, `options`, `time`, `logger` | `AgentIdHeader`, `AgentSecretHeader` |
| `AgentWorker` | `enroller`, `credentialStore`, `runner`, `options`, `lifetime`, `time`, `logger`, `backoff` | |

#### Нетривіальні методи (47)

| Тип | Методи |
|---|---|
| `AgentOptions` (2) | `DefaultDataDirectory` (залежить від ОС), `IsValid` |
| `AgentCredentials` (1) | `ToString` (не виводить секрет) |
| `CredentialStore` (2) | `LoadAsync` (перевірка вмісту), `SaveAsync` (атомарний запис, права 0600 від створення) |
| `AgentEnroller` (1) | `GetCredentialsAsync` |
| `AgentInfo` (1) | `ReadVersion` |
| `LinuxBootIdProvider`, `UptimeBootIdProvider` (2) | `GetBootId` у кожному |
| `AgentCommandException` (1) | `ToCommandError` |
| `DockerDotNetGateway` (7) | `GetVersionAsync`, `ListContainersAsync`, `StartContainerAsync`, `StopContainerAsync`, `RestartContainerAsync`, `CallAsync` (бюджет часу й трансляція помилок Docker.DotNet), `DescribeApiError` |
| `ContainerInfoMapper` (5) | `ToContainerInfo`, `ToState`, `ToName`, `Label`, `ToUnixMilliseconds` |
| `AgentCommandHandler<TCommand>` (1) | `HandleAsync` (шаблонний метод) |
| `ContainerActionHandler<TCommand>` (3) | `ExecuteAsync`, `ToStopTimeout`, `ValidateContainerId` |
| `ListContainersHandler` (1) | `ExecuteAsync` |
| `StopContainerHandler`, `RestartContainerHandler` (2) | `RunAsync` у кожному |
| `CommandDispatcher` (3) | конструктор (таблиця обробників, перевірка дублікатів), `DispatchAsync`, `Unsupported` |
| `AgentStreamWriter` (1) | `WriteAsync` |
| `StreamCommandProgress` (1) | `ReportAsync` |
| `ReconnectBackoff` (2) | `NextDelay`, `SessionEnded` |
| `AgentSessionRunner` (5) | `RunAsync`, `CreateHelloAsync`, `SendHeartbeatsAsync`, `RunCommandAsync`, `IsSessionCancellation` |
| `AgentWorker` (5) | `ExecuteAsync`, `EnrollAsync`, `KeepConnectedAsync`, `CreateChannel`, `Describe` |
| `DependencyInjection` (1) | `AddAgent` |

Не враховано як тривіальні: `ContainerIdOf` в обробниках, `StartContainerHandler.RunAsync` (лише делегує), `ReconnectBackoff.Reset`, `DockerDotNetGateway.Seconds` і `Dispose`, `AgentStreamWriter.Dispose`, властивості `Handles`, `FilePath`, `SupportedCommands`.

#### Ієрархії успадкування

1. `AgentCommandHandler<TCommand>` → `ListContainersHandler`, `ContainerActionHandler<TCommand>` → `StartContainerHandler`, `StopContainerHandler`, `RestartContainerHandler`. Це 6 класів на трьох рівнях (плюс інтерфейс `IAgentCommandHandler`).
2. `AgentCommandException` → `InvalidCommandArgumentException`, `ContainerNotFoundException`, `DockerUnavailableException`, `DockerCommandFailedException`, `DockerTimeoutException`, `UnsupportedCommandException`. Це 7 класів.

#### Поліморфізм

| # | Вид | Поліморфний інтерфейс | Клієнтський код |
|---|---|---|---|
| 1 | Динамічний (інтерфейс) | `IAgentCommandHandler.HandleAsync` | `CommandDispatcher.DispatchAsync` виконує будь-яку команду через таблицю обробників, не знаючи їхніх типів ✅ |
| 2 | Статичний (generics) | `AgentCommandHandler<TCommand>`: поле `oneof` і тип команди визначаються з `TCommand` | `HandleAsync` один раз реалізує витяг команди, `request_id` і перетворення помилок для `ListContainers`, `StartContainer`, `StopContainer`, `RestartContainer` ✅ |
| 3 | Динамічний (шаблонний метод) | `ContainerActionHandler<TCommand>.ContainerIdOf`, `RunAsync` | `ContainerActionHandler.ExecuteAsync` однаково перевіряє id контейнера для старту, зупинки й перезапуску ✅ |
| 4 | Динамічний (стратегія) | `IBootIdProvider`: `LinuxBootIdProvider`, `UptimeBootIdProvider` | `AgentSessionRunner.CreateHelloAsync` бере boot_id, не знаючи ОС ✅ |
| 5 | Динамічний (адаптер) | `IDockerGateway`: `DockerDotNetGateway` (і фейк у тестах) | обробники команд і `AgentSessionRunner` працюють із Docker без залежності від Docker.DotNet ✅ |

#### Тести (не входять в облік)

- `tests/Agent`, 62 тести (2 з них лише на Linux): зіставлення міток compose і станів контейнерів; обробники — `request_id` у результаті, коди помилок, передача таймауту зупинки, відхилення небезпечних id контейнерів; диспетчер — непідтримувані й невідомі команди, `CancelRequest` без результату; backoff — межі, jitter, скидання після довгої сесії; послідовний запис у стрім; сесія — облікові дані в metadata, Hello першим, відповіді на команди, heartbeat, `Unauthenticated`; збереження облікових даних і реєстрація; boot_id.

### Лаба 2

Ще немає. Заплановано: середовища та статус агентів, реєстрація агента, контейнери, логи контейнера.

### Лаба 3

| Бібліотека | Що використано | Ретроспектива |
|---|---|---|
| Docker.DotNet (`Docker.DotNet.Enhanced`) | `DockerClientBuilder` з визначенням endpoint як у docker CLI; `System.GetVersionAsync`; `Containers.ListContainersAsync`, `StartContainerAsync`, `StopContainerAsync`, `RestartContainerAsync` з `WaitBeforeKillSeconds`; типізовані винятки (`DockerContainerNotFoundException`, `DockerApiException`) | [docs/retro/docker-dotnet.md](docs/retro/docker-dotnet.md), чернетка |
| gRPC для .NET (`Grpc.Net.Client`, `Grpc.Tools`, `Google.Protobuf`) | Клієнт двонапрямленого стріму `Connect`; metadata виклику; коди статусу (`Unauthenticated`, `Unavailable`); HTTP/2 keepalive-пінги через `SocketsHttpHandler`; дескриптори protobuf для зіставлення типу команди з полем `oneof`; фейковий клієнт у тестах через віртуальні методи згенерованого класу | [docs/retro/grpc.md](docs/retro/grpc.md), чернетка |

## Синенко Дмитро Вячеславович (@Dmyt-S)

Зона: Discord-бот, система сповіщень (роль остаточно не визначена).

Ще немає.

## Денисенко Любов Володимирівна (@denysenkoliubov)

Зона: API для вебпанелі, аудит (роль остаточно не визначена).

Ще немає.
