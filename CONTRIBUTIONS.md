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
| Класи та подібні типи | ≥ 9 | 539 (466 класів, 33 інтерфейси, 13 структур, 27 enum) | ✅ |
| Поля | ≥ 15 | 1072 (915 полів стану, 157 констант і статичних полів) | ✅ |
| Нетривіальні методи | ≥ 25 | 770 | ✅ |
| Ієрархії успадкування | ≥ 2, одна з ≥ 3 класів | 8 (16, 3, 5, 14, 5, 5, 96 і 3 класи) | ✅ |
| Поліморфізм | ≥ 3, серед них статичний і динамічний | 12 з клієнтським кодом (9 динамічних, 3 статичні) | ✅ |

Код: `src/Aggregator/Domain` (PR #3); `src/Aggregator/Application`, `src/Aggregator/Infrastructure`, `src/Aggregator/Api` (PR #6); команди контейнерів і їх авторизація (PR #8); поточний користувач і фільтрація за видимістю (PR #9); пошук контейнера за префіксом id (PR #12); пайплайн деплою, репозиторії й стеки (PR #13); контракти керування користувачами (PR #15); контракт Discord-бота й прив'язки акаунта (PR #16); gRPC-шлюз бота й прив'язка Discord-акаунта (PR #18); домен сповіщень і їх доставка в `bot.proto` (PR #22); ключі Data Protection у пам'яті під час збірки (PR #29); логи контейнерів у ЦА (PR #44); API репозиторіїв, стеків і деплоїв для панелі (PR #45); коміт деплою до читання файлів (PR #46); API каналів, правил та історії сповіщень (PR #47); керування середовищами, enrollment-токенами, відкликання агента й креди registry в API панелі (PR #48); приватні репозиторії з токеном доступу (PR #60); SOPS: age-ключі середовищ і розшифрування в пайплайні (PR #61); відновлення секретів після перезавантаження хоста (PR #62); live-оновлення через SignalR (PR #63); події контейнерів від агента (PR #64; зона агента, але реалізував Петренко Б. О.); деплой після push за webhook (PR #65); підключення до git-серверів, репозиторії для редакторів, захист від SSRF і ліміт git-кешу (ADR-0006); хук деплою для CI (deploy hook); перезапуск стеку після зміни файлів (у ЦА й на агенті: зона агента, але реалізував Петренко Б. О.); видалення стеку, репозиторію й середовища; API-токени користувачів з обмеженнями й `Actor`/`AccessScope` у перевірці прав (ADR-0007); вхід у CLI через браузер (device flow, ADR-0008); CLI `forest` (`src/Cli`, ADR-0008); вхід без пароля з passkey, вхід і прив'язка через Discord OAuth2 (ADR-0009); секрети SOPS у git через git-фільтр CLI, `/api/sops/*` і прапорець `secrets` на API-токенах (ADR-0010).

Не враховано: `DevUserAuthenticationHandler` (dev-автентифікація лише для `Development`), `Program.cs`, згенерований код міграцій і proto.

#### Класи та подібні типи

| Простір імен `ContainerForest.Aggregator.Domain.*` | Типи |
|---|---|
| `Common` | `Entity<TId>` (абстрактний клас), `IStronglyTypedId<TSelf>` (інтерфейс), `EnvironmentId`, `RepositoryId`, `GitConnectionId` (ADR-0006), `StackId`, `DeploymentId`, `UserId`, `RoleAssignmentId`, `NotificationChannelId`, `NotificationRuleId`, `NotificationId`, `AgeKeyId`, `ApiTokenId` (ADR-0007), `CliAuthorizationId` (ADR-0008) (структури), `DomainException` (абстрактний клас), `DomainValidationException`, `DomainConflictException`, `SecretToken`, `OneTimeCode` (ADR-0008) (статичні класи) |
| `Environments` | `DockerEnvironment`, `AgeKey` |
| `Repositories` | `GitRepository`, `GitConnection`, `GitConnectionEnvironment` (ADR-0006), `GitUrl` (статичний клас, ADR-0006) |
| `Stacks` | `Stack`, `StackName`, `StackFilePath`, `CommitSha`, `BranchName`, `StackOperation` (enum) |
| `Deployments` | `Deployment`, `DeploymentLogLine`, `ImageReference`, `DeploymentStatus`, `DeploymentTrigger`, `ImagePullPolicy`, `DeploymentLogLevel` (enum) |
| `Notifications` | `NotificationChannel`, `NotificationRule`, `NotificationEvent`, `Notification`, `NotificationEventKinds`, `NotificationSeverity`, `NotificationChannelKind`, `NotificationStatus` (enum) |
| `Access` | `User`, `DiscordLink`, `RoleAssignment` (абстрактний клас), `EnvironmentRoleAssignment`, `StackRoleAssignment`, `AccessPolicy` (статичний клас), `RolePermissions` (статичний клас), `Role`, `Permission` (enum); ADR-0007: `Actor`, `AccessScope`, `ApiToken`, `ApiTokenEnvironment`; ADR-0008: `CliAuthorization`, `CliAuthorizationEnvironment`, `CliAuthorizationStatus`, `CliPollResult` (enum) |

| Простір імен `ContainerForest.Aggregator.Application.*` | Типи |
|---|---|
| `Common` | `IRepository<TEntity, TId>`, `IUnitOfWork` (інтерфейси), `NotFoundException` |
| `Agents` | `IAgentChannel` (інтерфейс), `ContainerSnapshot`, `ContainerState` (enum), `ContainerLogQuery`, `ContainerLogLine`, `LogStream` (enum), `AgentException` (абстрактний клас), `AgentOfflineException`, `AgentDisconnectedException`, `AgentTimeoutException`, `AgentCommandFailedException`, `AgentErrorKind` (enum), `AgentCredentials`, `AgentEnrollmentService`, `StackBundle`, `BundleFile` |
| `Environments` | `IEnvironmentRepository` (інтерфейс), `EnvironmentService`, `EnrollmentToken`, `EnvironmentCreated`, `EnvironmentSummary` |
| `Access` | `IUserRepository`, `IRoleAssignmentRepository`, `ICurrentUser`, `IDiscordLinkRepository` (інтерфейси), `AccessDeniedException`, `UnauthenticatedException`, `CurrentActor`, `DiscordLinkService`, `DiscordLinkStatus`, `DiscordLinkCode`; ADR-0007: `IApiTokenRepository` (інтерфейс), `ApiTokenService`, `ApiTokenAuthenticator`, `CreatedApiToken`; ADR-0008: `ICliAuthorizationRepository` (інтерфейс), `CliAuthorizationService`, `StartedCliAuthorization`, `CliTokenResult` |
| `Stacks` | `IStackRepository` (інтерфейс), `StackService`, `StackSummary` |
| `Repositories` | `IGitRepositoryRepository`, `IGitConnectionRepository` (інтерфейси), `RepositoryService`, `RepositorySummary`, `RepositoryAccess`, `RepositoryActor`, `GitConnectionService`, `GitConnectionSummary`, `ConnectionEnvironments`, `GitAccessToken` (колишній `RepositoryAccessToken`) (ADR-0006) |
| `Deployments` | `IDeployStep`, `IGitSource`, `IComposeParser`, `IDeploymentQueue`, `IDeploymentRepository` (інтерфейси), `DeployPipeline`, `DeployContext`, `DeploymentService`, `DeploymentService.VisibleStack`, `DeploymentRunner`, `DeploymentOptions`, `DeploymentFailedException`, `SecretRehydrationService` (PR #62), `GitSnapshot`, `GitCredentials`, `GitAccess` (ADR-0006), `SourceFile`, `ComposeFile`, `ComposeService`, `DeploymentSummary` |
| `Notifications` | `INotificationChannelRepository`, `INotificationRuleRepository`, `INotificationRepository` (інтерфейси), `NotificationSettingsService`, `NotificationRuleView`, `NotificationHistoryEntry` |
| `Live` (PR #63) | `ILiveUpdates` (інтерфейс), `NullLiveUpdates` |
| `Webhooks` (PR #65) | `GitWebhookProvider`, `HmacWebhookProvider` (абстрактні класи), `GitHubWebhook`, `GiteaWebhook`, `GitLabWebhook`, `WebhookDelivery`, `WebhookEventKind` (enum), `PushEvent`, `WebhookPayloadException`, `PushDeploymentGate`, `PushDeploymentGate.Releaser`, `WebhookRejectedException`, `WebhookOutcome`, `PushStackOutcome` (enum), `PushStackResult`, `WebhookResult`, `PushWebhookService`, `PushFollowUpObserver`; хук деплою: `DeployHookService`, `DeployHookOutcome` (enum), `DeployHookResult` |
| `Secrets` (PR #61) | `IAgeKeyRepository`, `IAgeKeyGenerator`, `ISopsDecryptor` (інтерфейси), `GeneratedAgeKey`, `SopsDecryptionException`, `SopsFile` (статичний клас), `SopsFile.SopsFormat` (enum), `AgeKeyService`, `AgeKeySummary` |
| `Deployments.Steps` | `SopsDecryptStep` (PR #61), `GitFetchStep`, `ValidateComposeStep`, `BuildBundleStep`, `SendToAgentStep` |
| `Containers` | `IContainerCommand` (інтерфейс), `ContainerCommand` (абстрактний клас), `StartContainerCommand`, `StoppingContainerCommand` (абстрактний клас), `StopContainerCommand`, `RestartContainerCommand`, `AuthorizedCommand`, `ContainerCommandService`, `ContainerCommandContext`, `ContainerAction` (enum), `ContainerQueryService`, `VisibleContainer`, `ContainerLookup` (статичний клас), `ContainerLogService` |
| — | `DependencyInjection` (статичний клас) |

| Простір імен `ContainerForest.Aggregator.Infrastructure.*` | Типи |
|---|---|
| `Persistence` | `AggregatorDbContext`, `StronglyTypedIdConverter<TId>`, `ModelConfigurationBuilderExtensions`, `EfRepository<TEntity, TId>`, `EnvironmentRepository`, `StackRepository`, `UserRepository`, `RoleAssignmentRepository`, `GitRepositoryRepository`, `GitConnectionRepository` (ADR-0006), `DeploymentRepository`, `DiscordLinkRepository`, `NotificationChannelRepository`, `NotificationRuleRepository`, `NotificationRepository`, `DatabaseMigrator`, `DesignTimeDbContextFactory`, `AgeKeyRepository`, `LiveChanges` (PR #63), `ApiTokenRepository` (ADR-0007), `CliAuthorizationRepository` (ADR-0008) |
| `Persistence.Configurations` | `EnvironmentConfiguration`, `RepositoryConfiguration`, `GitConnectionConfiguration` (ADR-0006), `StackConfiguration`, `DeploymentConfiguration`, `UserConfiguration`, `RoleAssignmentConfiguration`, `EnvironmentRoleAssignmentConfiguration`, `StackRoleAssignmentConfiguration`, `DiscordLinkConfiguration`, `NotificationChannelConfiguration`, `NotificationRuleConfiguration`, `NotificationConfiguration`, `AgeKeyConfiguration`, `ApiTokenConfiguration` (ADR-0007), `CliAuthorizationConfiguration` (ADR-0008) |
| `Agents` | `AgentGatewayOptions`, `AgentSession`, `AgentSession.PendingRequest`, `AgentRegistry`, `GrpcAgentChannel`, `ContainerLogTail`, `ContainerEventCoalescer` (PR #64) |
| `Deployments` | `GitOptions`, `LibGit2SharpGitSource`, `LibGit2SharpGitSource.TreeReader`, `GitHostGuard` (ADR-0006), `YamlComposeParser`, `DeploymentQueue`, `DeploymentWorker` |
| `Security` | `InMemoryXmlRepository` |
| `Secrets` (PR #61) | `SopsOptions`, `CliResult`, `CliUnavailableException`, `CliProcess` (статичний клас), `SopsCli`, `AgeKeygenCli` |
| — | `DependencyInjection` (статичний клас) |

| Простір імен `ContainerForest.Agent.*` (PR #64) | Типи |
|---|---|
| `Docker` | `ContainerEventFilter` (статичний клас), `DockerContainerEvent`, `DockerDotNetGateway.ChannelProgress` |
| `Connection` | `ContainerEventForwarder` |

| Простір імен `ContainerForest.Aggregator.Api.*` | Типи |
|---|---|
| `Grpc` | `AgentGatewayService`, `BotGatewayService`, `BotGatewayInterceptor`, `BotGatewayOptions` |
| `ErrorHandling` | `ProblemDetailsExceptionHandler` |
| `Access` | `ClaimsCurrentUser`; ADR-0007: `ApiTokenAuthenticationHandler`, `ApiTokenClaims` (статичний клас), `ReadOnlyTokens` (статичний клас), `ReadOnlyTokens.AllowedMetadata` |
| `Contracts` | `CreateEnvironmentRequest`, `UpdateEnvironmentRequest`, `EnvironmentCreatedResponse`, `EnrollmentTokenResponse`, `SetRegistryCredentialRequest`, `RegistryCredentialResponse` (PR #48), `NotificationChannelResponse`, `CreateNotificationChannelRequest`, `UpdateNotificationChannelRequest`, `NotificationRuleResponse`, `CreateNotificationRuleRequest`, `UpdateNotificationRuleRequest`, `NotificationResponse` (PR #47), `RepositoryResponse`, `CreateRepositoryRequest`, `UpdateRepositoryRequest`, `RepositoryAccessTokenRequest` (PR #60), `AgeKeyResponse` (PR #61), `CreateStackRequest`, `ChangeStackSourceRequest`, `StackResponse`, `StackCommandResponse`, `DeployRequest`, `DeploymentResponse`, `DeploymentDetailsResponse`, `DeploymentLogLineResponse` (PR #45), `ContainerLogsResponse`, `ContainerLogLineResponse`, `LoginRequest`, `MeResponse`, `ChangePasswordRequest`, `UserResponse`, `UserDetailsResponse`, `CreateUserRequest`, `RenameUserRequest`, `SetAdminRequest`, `ResetPasswordRequest`, `RoleAssignmentResponse`, `GrantRoleRequest`, `ChangeRoleRequest`, `RoleScope` (enum), `DiscordLinkResponse`, `DiscordLinkCodeResponse` |
| `Live` (PR #63) | `ILiveClient` (інтерфейс), `LiveHub`, `SignalRLiveUpdates` |
| `Contracts` (PR #65) | `WebhookSecretResponse`, `DeployOnPushRequest`, `WebhookResponse`, `WebhookStackResponse` |
| `Contracts` (ADR-0006) | `GitConnectionResponse`, `CreateGitConnectionRequest`, `UpdateGitConnectionRequest`, `GitAccessTokenRequest` (замість `RepositoryAccessTokenRequest`), `GitConnectionWebhookSecretResponse` |
| `Endpoints` | `MeEndpoints`, `DiscordLinkEndpoints`, `StackEndpoints`, `NotificationEndpoints`, `AgeKeyEndpoints`, `WebhookEndpoints` (PR #65), `GitConnectionEndpoints` (ADR-0006), `ApiTokenEndpoints` (ADR-0007), `CliAuthorizationEndpoints` (ADR-0008) |
| `Contracts` (ADR-0007) | `ApiTokenResponse`, `CreateApiTokenRequest`, `CreatedApiTokenResponse` |
| `Contracts` (ADR-0008) | `StartCliAuthorizationRequest`, `CliAuthorizationStartedResponse`, `CliAuthorizationResponse`, `ApproveCliAuthorizationRequest`, `CliTokenRequest`, `CliTokenResponse` |
| Passkeys (ADR-0009) | `Api.Access`: `PasskeySignIn`, `PasskeyManagement`, `PasskeyView`; `Api.Endpoints`: `PasskeyEndpoints`; `Api.Contracts`: `PasskeyLoginRequest`, `AddPasskeyRequest`, `RenamePasskeyRequest`, `PasskeyResponse`, `SignInMethodsResponse`; CLI: `UserPasskeysCommand`, `UserRemovePasskeyCommand` (11 типів) |
| Discord OAuth2 (ADR-0009) | `Api.Access`: `DiscordAuthOptions`, `DiscordSignIn`; `Api.Endpoints`: `DiscordAuthEndpoints` (3 типи) |
| CLI `ContainerForest.Cli.*` (ADR-0008), без згенерованого клієнта `Api.Generated` | корінь: `ExitCodes` (статичний клас), `CliException`, `CliVersion` (статичний клас); `Configuration`: `CliConfig`, `ContextEntry`, `CliConfigJsonContext`, `ConfigStore`, `ServerUrl` (статичний клас), `CredentialSource` (enum), `Connection`, `CredentialOptions`, `CredentialResolver`; `Api`: `ForestApi`, `ForestApiFactory`, `BearerAuthenticationProvider`, `ApiErrors` (статичний клас), `ServerProblemException`, `ProblemDetailsHandler`, `ContainerAction`, `StackAction` (enum); `Output`: `Table`, `CliOutput`, `OutputFormat` (enum), `LogFollower`, `DeploymentWatcher`; `Login`: `IBrowser` (інтерфейс), `SystemBrowser`, `LoopbackSignal`, `BrowserLogin`; `Commands`: `ICliNode` (інтерфейс), `CommandGroup`, `ForestCommand`, `EnvironmentCommand`, `StackCommand`, `EnvironmentArgumentCommand` (абстрактні класи), `CommandContext`, `CliHost`, `GlobalOptions`, `Confirmation`, `ForestCli`, `DeploymentResult` (статичні класи), `Program`; команди `LoginCommand`, `LogoutCommand`, `WhoAmICommand`, `ContextListCommand`, `ContextUseCommand`, `ContextRemoveCommand`, `ContextSetEnvironmentCommand`, `EnvListCommand`, `EnvShowCommand`, `EnvCreateCommand`, `EnvUpdateCommand`, `EnvRemoveCommand`, `EnvEnrollmentTokenCommand`, `EnvRevokeAgentCommand`, `KeyListCommand`, `KeyCreateCommand`, `KeyRemoveCommand`, `RegistryListCommand`, `RegistrySetCommand`, `RegistryRemoveCommand`, `ContainerListCommand`, `ContainerControlCommand`, `ContainerLogsCommand`, `StackListCommand`, `StackShowCommand`, `StackCreateCommand`, `StackSetSourceCommand`, `StackRemoveCommand`, `StackDeployCommand`, `StackControlCommand`, `StackSwitchCommand`, `StackHookCreateCommand`, `StackHookRemoveCommand`, `DeploymentListCommand`, `DeploymentShowCommand`, `DeploymentLogsCommand` (78 типів) |
| CLI: адміністрування (ADR-0008) | `SecretInput` (статичний клас); `RepositoryCommand`, `GitConnectionCommand`, `UserCommand`, `ChannelCommand` (абстрактні класи), `ConnectionEnvironmentOptions`; команди `RepoListCommand`, `RepoAddCommand`, `RepoUpdateCommand`, `RepoRemoveCommand`, `RepoWebhookCreateCommand`, `RepoWebhookRemoveCommand`, `ConnectionListCommand`, `ConnectionAddCommand`, `ConnectionUpdateCommand`, `ConnectionRemoveCommand`, `ConnectionSetTokenCommand`, `ConnectionRemoveTokenCommand`, `ConnectionWebhookCreateCommand`, `ConnectionWebhookRemoveCommand`, `UserListCommand`, `UserShowCommand`, `UserCreateCommand`, `UserRenameCommand`, `UserActivationCommand`, `UserSetAdminCommand`, `UserResetPasswordCommand`, `UserUnlockCommand`, `RoleGrantCommand`, `RoleChangeCommand`, `RoleRevokeCommand`, `UserTokensCommand`, `UserRevokeTokenCommand`, `ChannelListCommand`, `ChannelAddCommand`, `ChannelUpdateCommand`, `ChannelRemoveCommand`, `RuleListCommand`, `RuleAddCommand`, `RuleUpdateCommand`, `RuleRemoveCommand`, `NotificationHistoryCommand` (43 типи) |
| Git-фільтр для секретів (ADR-0010): ЦА | `Application.Secrets`: `ISopsEncryptor`, `ISopsConfigReader` (інтерфейси), `SopsEncryptionRule`, `SopsConfigException`, `SopsEncryptionException`, `SopsSecretsService`, `GitAttributes`, `GitAttributes.Rule`, `GitAttributesFile`; `Infrastructure.Secrets`: `YamlSopsConfigReader`; `Api.Endpoints`: `SopsEndpoints` (статичний клас); `Api.Contracts`: `SopsDecryptRequest`, `SopsEncryptRequest`, `SopsContentResponse` (14 типів) |
| Git-фільтр для секретів (ADR-0010): CLI | `Secrets`: `SopsFiles`, `PktLine`, `GitFilterSetup` (статичні класи), `PktLineReader`, `PktLineWriter`, `GitWorkTree`, `FilterCache`, `FilterCacheEntry`, `FilterCacheJsonContext`, `SopsBackendException`, `ISopsBackend` (інтерфейс), `AggregatorSops`, `LocalSops`, `FilterResult`, `FilterServices`, `FilterOperation` (абстрактний клас), `SmudgeOperation`, `CleanOperation`, `GitFilterProcess`, `SecretsSession`; `Commands`: `SecretsFiles` (статичний клас), `SecretFileStatus`, `SecretsJsonContext`, команди `SecretsInitCommand`, `SecretsAdoptCommand`, `SecretsAddCommand`, `SecretsStatusCommand`, `SecretsCheckCommand`, `SecretsUpdateKeysCommand`, `GitSetupCommand`, `GitFilterProcessCommand`, `GitTextconvCommand` (32 типи) |
| Git-фільтр CLI: `secrets remove`, `secrets rule`, `git teardown` | `Secrets`: `SopsConfigFile`, `SopsConfigFile.Rule`; `Commands`: `SopsConfigEditing` (статичний клас), `SopsRuleStatus`, `SecretsRuleJsonContext`, команди `SecretsRemoveCommand`, `SecretsRuleListCommand`, `SecretsRuleAddCommand`, `SecretsRuleRemoveCommand`, `GitTeardownCommand` (10 типів) |
| Git-фільтр CLI: закінчення рядків | `Secrets`: `RefreshResult`; `Commands`: `SecretsFixEolCommand` (2 типи) |
| `Contracts` (хук деплою) | `DeployHookTokenResponse`, `DeployHookResponse` |
| Перезапуск після зміни файлів | `StackWriteResult` (агент), `RestartOnFileChangeRequest` |

#### Поля

| Тип | Поля стану | Константи та статичні поля |
|---|---|---|
| `Entity<TId>` | `Id` | |
| 9 структур ID | `Value` у кожній (9) | |
| `StackName`, `StackFilePath`, `CommitSha`, `BranchName` | `Value` у кожному (4) | `StackName.MaxLength`, `StackFilePath.MaxLength`, `BranchName.MaxLength`, `BranchName.ForbiddenChars` |
| `DockerEnvironment` | `Name`, `IsPrivate`, `CreatedAt`, `EnrollmentTokenHash`, `EnrollmentTokenExpiresAt`, `AgentSecretHash`, `EnrolledAt`, `AgentVersion`, `LastBootId`, `LastSeenAt` | `MaxNameLength`, `DefaultEnrollmentTokenLifetime` |
| `GitRepository` | `Name`, `Url`, `ConnectionId` (ADR-0006), `CreatedAt` | `MaxNameLength` |
| `GitConnection` (ADR-0006) | `Name`, `UrlPrefix`, `ProtectedAccessToken`, `AccessUserName`, `AllowsAllEnvironments`, `environments`, `ProtectedWebhookSecret`, `CreatedAt` | `MaxNameLength`, `MaxAccessUserNameLength`, `DefaultAccessUserName` |
| Підключення до git-серверів (ADR-0006): `GitConnectionEnvironment`, `GitConnectionId`, `GitOptions`, `GitHostGuard`, `RepositoryActor`, `RepositorySummary`, `GitConnectionSummary`, `ConnectionEnvironments`, `GitAccess`, `GitCredentials`, `PushEvent`, `DeployContext`, `RepositoryResponse`, `UpdateRepositoryRequest`, `GitConnectionResponse`, `CreateGitConnectionRequest`, `UpdateGitConnectionRequest`, `GitConnectionWebhookSecretResponse` | `EnvironmentId`; `Value`; `MaxCacheBytes`, `AllowPrivateHosts`; `resolve`; `Actor`, `Assignments`, `EditorEnvironments`; `Repository`, `Connection`, `CanManage`; `Connection`, `RepositoryCount`, `EnvironmentIds`; `AllEnvironments`, `EnvironmentIds`; `Credentials`, `IsTrustedHost`; `Scope`; `RepositoryUrls`; `Connection`; `ConnectionId`, `ConnectionName`, `ConnectionHasWebhook`, `CanManage` (замість `HasAccessToken`, `AccessUserName`); `ConnectionId`; 10 полів; 5 полів; 4 поля; `Connection`, `Secret` (46) | `GitAccess.Public` |
| `Stack` | `EnvironmentId`, `Name`, `RepositoryId`, `Branch`, `ComposePath`, `IsSystem`, `CreatedAt`, `DeployedCommit`, `DeployedAt`, `DeployedWithSecrets` | |
| `Deployment` | `StackId`, `Trigger`, `RequestedBy`, `PullPolicy`, `Status`, `Commit`, `QueuedAt`, `StartedAt`, `FinishedAt`, `Error` | `MaxErrorLength` |
| `User` | `UserName`, `IsAdmin`, `IsActive`, `CreatedAt` | `MinUserNameLength`, `MaxUserNameLength` |
| `NotificationEvent`, `NotificationChannel` (PR #22) | `Kind`, `EnvironmentId`, `StackId`, `OccurredAt`; `Name`, `Kind`, `DiscordChannelId`, `IsEnabled`, `CreatedAt` | `NotificationChannel.MaxNameLength` |
| `NotificationRule` | `ChannelId`, `Events`, `EnvironmentId`, `StackId`, `IsEnabled`, `CreatedAt` | |
| `Notification` | `ChannelId`, `RuleId`, `Kind`, `Severity`, `EnvironmentId`, `StackId`, `OccurredAt`, `Summary`, `CreatedAt`, `Status`, `FinishedAt`, `Error` | `MaxSummaryLength`, `MaxErrorLength`, `MaxPendingAge` |
| `DiscordLink` (PR #18) | `DiscordUserId`, `DiscordUserName`, `LinkedAt`, `PendingCodeHash`, `PendingCodeExpiresAt` | `CodeLength`, `MaxDiscordUserNameLength`, `CodeLifetime`, `CodeAlphabet` |
| `DiscordLinkStatus`, `DiscordLinkCode`, `BotGatewayOptions` | 3 + 2 + `TokenHashes` | `BotGatewayOptions.SectionName` |
| `BotGatewayInterceptor` | | `TokenHeader`, `ReasonTrailer`, `InvalidBotToken`, `AccountNotLinked` |
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
| `DiscordLinkResponse`, `DiscordLinkCodeResponse` (PR #16) | `Linked`, `DiscordUserName`, `LinkedAt`; `Code`, `ExpiresAt` | |
| `EnvironmentSummary` (PR #9) | `Role` | |
| `AgentSession` | `writer`, `logger`, `writeLock`, `pending`, `closed`, `closing`, `EnvironmentId`, `SessionId`, `ConnectedAt`, `LastMessageAt` | |
| `AgentRegistry` | `sessions` | |
| `EfRepository<TEntity, TId>` | `Db` | |
| `EnvironmentConfiguration`, `RoleAssignmentConfiguration` | | `Sha256HexLength`; `EnvironmentScope`, `StackScope` |
| `Infrastructure.DependencyInjection`, `AgentGatewayService` | | `ConnectionStringName`; `AgentIdHeader`, `AgentSecretHeader` |
| `ImageReference` (PR #13) | `Name`, `Tag`, `Digest` | `ImplicitTag` |
| `Deployment` (PR #13), `DeploymentLogLine` | `log`; `Sequence`, `At`, `Level`, `Text` | `MaxLogLines`, `MaxLogLineLength` |
| `GitSnapshot`, `SourceFile`, `ComposeFile`, `ComposeService` | 3 + 3 + 1 + 4 | |
| `DeploymentSummary`, `StackSummary`, `DeploymentService.VisibleStack` | 9 + 11 + 4 | `DeploymentService.HistoryLimit` |
| Webhook-деплой (PR #65): `GitRepository`, `Stack`, `StackSummary`, `RepositoryResponse`, `StackResponse`, `WebhookDelivery`, `PushEvent`, `PushDeploymentGate`, `PushDeploymentGate.Releaser`, `PushStackResult`, `WebhookResult`, `WebhookSecretResponse`, `DeployOnPushRequest`, `WebhookResponse`, `WebhookStackResponse` | `ProtectedWebhookSecret`; `DeployOnPush`; `DeployOnPush`; `HasWebhook`; `DeployOnPush`; `Headers`, `Body`; `Branch`, `After`; `mutex`, `pending`; `disposed`; 5; 4; 2; 1; 4; 5 | `GitWebhookProvider.BranchPrefix`, `WebhookEndpoints.MaxBodyBytes` |
| Події контейнерів (PR #64): `DockerContainerEvent`, `ContainerEventForwarder`, `ContainerEventCoalescer`, `ContainerEventFilter` | `ContainerId`, `Action`, `At`; `RetryDelay`; `pending` | `ContainerEventCoalescer.Window`, `ContainerEventFilter.Relevant`, `ContainerEventFilter.HealthPrefix` |
| Live-оновлення (PR #63): `LiveChanges`, `AggregatorDbContext`, `ClaimsCurrentUser` | `stacks`, `deployments`, `environments`; `live`; `principal` | `LiveHub.GroupsKey` |
| SOPS (PR #61): `AgeKey`, `AgeKeyId`, `GeneratedAgeKey`, `AgeKeySummary`, `SourceFile`, `BundleFile`, `SopsOptions`, `CliResult`, `AgeKeyResponse` | `EnvironmentId`, `Recipient`, `ProtectedIdentity`, `CreatedAt`; `Value`; `Recipient`, `Identity`; `Id`, `Recipient`, `CreatedAt`; `IsSecret`; `IsSecret`; `SopsPath`, `AgeKeygenPath`, `Timeout`; `ExitCode`, `Stdout`, `Stderr`; `Id`, `Recipient`, `CreatedAt` | `SopsFile.Marker`, `SopsOptions.SectionName`, `SopsCli.CouldNotRetrieveKey`, `SopsCli.MaxErrorLength`, `AgeKeygenCli.PublicKeyComment`, `AgeKeygenCli.IdentityPrefix` |
| Приватні репозиторії (PR #60): `GitAccessToken` (колишній `RepositoryAccessToken`), `GitCredentials`, `UpdateRepositoryRequest`, `GitAccessTokenRequest` | `UserName`, `Token` у кожному (8) | `GitAccessToken.MaxTokenLength` |
| `DeploymentOptions`, `GitOptions` | `MaxBundleBytes`, `AgentTimeout`, `MaxConcurrentDeployments`; `CacheDirectory` | `SectionName` в обох |
| `DeployContext` | `source`, `bundle`, `Deployment`, `Stack`, `Environment`, `Repository` | |
| `DeployPipeline` | `steps`, `observers` | |
| `StackBundle`, `BundleFile` | `StackName`, `Commit`, `Files`, `Pull`; `Path`, `Content`, `Mode` | `ComposeFileName`; `RegularMode`, `ExecutableMode` |
| `LibGit2SharpGitSource`, `TreeReader` | `locks`; `totalBytes`, `Files`, `Skipped` | `RemoteName` |
| `YamlComposeParser`, `DeploymentRepository` | | `MergeKey`; `Unfinished` |
| `DeploymentQueue`, `AgentSession.PendingRequest` | `channel`; `Completion`, `OnProgress`, `OnLogs` | |
| Керування середовищами (PR #48): DTO, `EnvironmentSummary` | `CreateEnvironmentRequest` 2, `UpdateEnvironmentRequest` 2, `EnvironmentCreatedResponse` 4, `EnrollmentTokenResponse` 2, `SetRegistryCredentialRequest` 3, `RegistryCredentialResponse` 5; `EnrollmentTokenExpiresAt` | |
| Сповіщення в панелі (PR #47): `NotificationRuleView`, `NotificationHistoryEntry`, DTO | 3 + 2; `NotificationChannelResponse` 6, `CreateNotificationChannelRequest` 2, `UpdateNotificationChannelRequest` 3, `NotificationRuleResponse` 9, `CreateNotificationRuleRequest` 3, `UpdateNotificationRuleRequest` 2, `NotificationResponse` 12 | `NotificationSettingsService.DefaultHistoryLimit`, `MaxHistoryLimit`; `NotificationEndpoints.SingleKinds` |
| DTO стеків і деплоїв (PR #45, #60) | `RepositoryResponse` 6, `CreateRepositoryRequest` 3, `CreateStackRequest` 4, `ChangeStackSourceRequest` 3, `StackResponse` 12, `StackCommandResponse` 3, `DeployRequest` 1, `DeploymentResponse` 9, `DeploymentDetailsResponse` 11, `DeploymentLogLineResponse` 3 | |
| Логи контейнерів (PR #44): `ContainerLogQuery`, `ContainerLogLine`, `ContainerLogTail`, `ContainerLogsResponse`, `ContainerLogLineResponse`, `ContainerLogService` | `TailLines`, `Since`; `At`, `Stream`, `Text`; `capacity`, `lines`, `gate`; `Lines`; `At`, `Stream`, `Text` | `DefaultTailLines`, `MaxTailLines` |
| `InMemoryXmlRepository` (PR #29) | `elements` | |
| Хук деплою: `Stack`, `StackSummary`, `StackResponse`, `DeployHookResult`, `DeployHookTokenResponse`, `DeployHookResponse` | `DeployHookTokenHash`; `HasDeployHook`; `HasDeployHook`; `StackId`, `Outcome`, `DeploymentId`; `Stack`, `Token`; `StackId`, `Outcome`, `DeploymentId` (11) | |
| Перезапуск після зміни файлів: `Stack`, `StackBundle`, `StackSummary`, `StackResponse`, `RestartOnFileChangeRequest`, `StackWriteResult` (агент) | `RestartOnFileChange` у перших чотирьох; `Enabled`; `Directory`, `FilesChanged` (7) | `DeployStackHandler.RestartTimeout` |
| Видалення стеку: `StackSummary`, `StackResponse` | `CanRemove` в обох (2) | |
| Видалення репозиторію: `RepositorySummary`, `RepositoryResponse` | `StackCount` в обох (2) | |
| API-токени (ADR-0007): `ApiToken`, `ApiTokenEnvironment`, `ApiTokenId`, `AccessScope`, `Actor`, `CreatedApiToken`, `ApiTokenService`, `ApiTokenAuthenticationHandler`, `ApiTokenClaims` | `UserId`, `Name`, `TokenHash`, `Hint`, `IsReadOnly`, `IsRestricted`, `environments`, `CreatedAt`, `ExpiresAt`, `LastUsedAt`; `EnvironmentId`; `Value`; `environments`, `IsReadOnly`; `User`, `Scope`; `Token`, `Secret` (18) | `ApiToken.Prefix`, `MaxNameLength`, `HintLength`, `LastUsedPrecision`; `AccessScope.Full`; `ApiTokenService.MaxTokensPerUser`; `SchemeName`, `BearerPrefix`; `TokenId`, `ReadOnly`, `Restricted`, `Environment` (12) |
| DTO API-токенів (ADR-0007) | `ApiTokenResponse` 9, `CreateApiTokenRequest` 4, `CreatedApiTokenResponse` 2 (15) | |
| Вхід у CLI (ADR-0008): `CliAuthorization`, `CliAuthorizationEnvironment`, `CliAuthorizationId`, `StartedCliAuthorization`, `CliTokenResult`, `CliAuthorizationService`, `OneTimeCode`, `CliAuthorizationEndpoints` | `DeviceCodeHash`, `UserCodeHash`, `ClientName`, `HostName`, `RequestedFrom`, `CreatedAt`, `ExpiresAt`, `LastPolledAt`, `Status`, `ApprovedBy`, `TokenName`, `TokenReadOnly`, `TokenRestricted`, `tokenEnvironments`, `TokenExpiresAt`; `EnvironmentId`; `Value`; 3; 4 (24) | `UserCodeLength`, `MaxClientNameLength`, `MaxHostNameLength`, `MaxAddressLength`, `Lifetime`, `PollInterval`; `MaxOpenRequests`; `OneTimeCode.Alphabet` (замість `DiscordLink.CodeAlphabet`); `StartPolicy`, `PollPolicy`, `CodePolicy`, `VerificationPath` (11 нових) |
| DTO входу в CLI (ADR-0008) | `StartCliAuthorizationRequest` 2, `CliAuthorizationStartedResponse` 6, `CliAuthorizationResponse` 6, `ApproveCliAuthorizationRequest` 4, `CliTokenRequest` 1, `CliTokenResponse` 4 (23) | |
| CLI (ADR-0008): конфігурація й облікові дані — `CliException`, `CliConfig`, `ContextEntry`, `ConfigStore`, `CredentialResolver`, `Connection`, `CredentialOptions` | `ExitCode`; `CurrentContext`, `Contexts`; `Url`, `Token`, `UserName`, `Environment`; `Directory`; `store`, `getVariable`, `standardInput`; 5; 4 (20) | `ExitCodes` (7), `CliVersion.Current`, `ConfigStore.FileName`, `UrlVariable`, `TokenVariable`, `ContextVariable` (12) |
| CLI: API, вивід і вхід — `BearerAuthenticationProvider`, `ForestApiFactory`, `ForestApi`, `ServerProblemException`, `Table`, `CliOutput`, `LogFollower`, `DeploymentWatcher`, `LoopbackSignal`, `BrowserLogin` | `server`, `token`; `handlerFactory`; `client`, `Server`; `Status`, `Title`, `Detail`; `headers`, `rows`; `Format`, `Out`, `Error`; `atNewest`, `Since`; `api`, `output`, `time`; `listener`, `stopping`, `decision`; `api`, `browser`, `time`, `output` (27) | `RequestTimeout`; `Gap`, `Missing`; `DeploymentWatcher.Interval`; `DonePath`; `SlowDownStep` (6) |
| CLI: каркас і команди — `CliHost`, `CommandGroup`, `ForestCommand`, `CommandContext`, `StackCommand`, `EnvironmentArgumentCommand`, 36 команд | 8; 3; 2; 6; `stackArgument`; `environmentArgument`; аргументи й опції команд (48) (68) | `GlobalOptions` (5), `EnvironmentCommand.EnvironmentOption`, `ContainerLogsCommand.FollowInterval` (7) |
| CLI: адміністрування — базові класи, `ConnectionEnvironmentOptions`, 36 команд, `CliHost.InteractiveTerminal` | аргументи базових класів (4), `Environments`, `All`, аргументи й опції команд (46), `InteractiveTerminal` (53) | `ForestApi.NotificationEvents` (1) |
| Passkeys (ADR-0009): `PasskeyView`, DTO, команда `user passkey remove` | `Id`, `Name`, `CreatedAt`, `IsBackedUp`, `Transports`; `PasskeyLoginRequest` 2, `AddPasskeyRequest` 2, `RenamePasskeyRequest` 1, `PasskeyResponse` 5, `SignInMethodsResponse` 3; `passkey`, `yes` (20) | `PasskeyManagement.MaxNameLength`, `MaxPasskeysPerUser`; `AuthenticationSetup.PasskeyCookieName` (3) |
| Discord OAuth2 (ADR-0009): `DiscordAuthOptions`, `DiscordSignIn` | `ClientId`, `ClientSecret`; `signInManager`, `links`, `currentActor`, `logger` (6) | `DiscordAuthOptions.SectionName`; `Scheme`, `CallbackPath`, `CompletePath`, `PurposeKey`, `RedirectKey`, `UserKey`, `Login`, `Link`, `LoginFailedPath`, `LinkedPath`, `LinkFailedPath` (12) |
| Прапорець `secrets` (ADR-0010): `AccessScope`, `ApiToken`, `CliAuthorization`, DTO токенів і входу в CLI, `ApiTokenClaims` | `CanDecryptSecrets`; `CanDecryptSecrets`; `SecretsRequested`, `TokenCanDecryptSecrets`; `ApiTokenResponse.Secrets`, `CreateApiTokenRequest.Secrets`, `StartCliAuthorizationRequest.Secrets`, `CliAuthorizationResponse.SecretsRequested`, `ApproveCliAuthorizationRequest.Secrets` (9) | `ApiTokenClaims.Secrets` (1) |
| SOPS для git-фільтра (ADR-0010): `SopsEncryptionRule`, `SopsSecretsService`, `GitAttributes`, `GitAttributes.Rule`, `GitAttributesFile`, `GitSnapshot`, DTO, `YamlSopsConfigReader`, `SopsEndpoints`, `LibGit2SharpGitSource` | 8; `currentActor`, `roleAssignments`, `keys`, `protector`, `decryptor`, `encryptor`, `configs`, `logger`; `rules`; `Pattern`, `SetsForest`; `Directory`, `Content`; `OuterAttributes`; `SopsDecryptRequest` 2, `SopsEncryptRequest` 3, `SopsContentResponse` 1 (28) | `MaxContentBytes`, `MaxConfigBytes`; `GitAttributes.FileName`, `Driver`; `RegexTimeout`, `ValueKeys`; `SopsEndpoints.Policy`; `MaxAttributesBytes` (8) |
| Git-фільтр CLI (ADR-0010): `PktLineReader`, `PktLineWriter`, `GitWorkTree`, `FilterCache`, `FilterCacheEntry`, `AggregatorSops`, `LocalSops`, `FilterResult`, `FilterServices`, `FilterOperation`, `GitFilterProcess`, `SecretsSession`, `CliHost`, команди, `SecretFileStatus` | `stream`, `header`; `stream`; `Root`, `GitDirectory`; `directory`, `key`, `entries`, `changed`; `Mac`, `Ciphertext`; `connect`, `root`, `api`; `sopsPath`, `root`; `Success`, `Content`; `Decryptors`, `Encryptor`, `Cache`, `Error`, `ToldReasons`; `Services`; `reader`, `writer`, `operations`; `Tree`, `Services`; `standardInput`, `standardOutput`, `workingDirectory`; опції команд (11: `login --secrets`, `git setup --local-sops`, `textconv` file, 5 опцій `secrets init`, файли `add` і `updatekeys`, `check --staged`); 5 (48) | `PktLine.MaxPacketLength`, `MaxDataLength`; `GitWorkTree.FilterDriver`; `FilterCache.KeyLength`; `LocalSops.Timeout`; `SopsFiles.Marker`; `GitFilterSetup.ContextKey`, `LocalSopsKey`, `SopsPathKey`, `HookMarker`; `SecretsFiles.Attributes`, `SopsConfig`, `FilterAttributes`, `DefaultPatterns` (14) |
| Git-фільтр CLI: `secrets remove`, `secrets rule`, `git teardown`: `SopsConfigFile`, `SopsConfigFile.Rule`, `SopsRuleStatus`, команди | `header`, `rules`, `trailer`; `Lines`, `PathRegex`, `AgeRecipients`, `OtherKeyTypes`, `Indent`; `Files`, `PathRegex`, `Environments`, `OtherAgeKeys`, `OtherKeyTypes`; опції команд (6: файли, `--plain` і `--yes` у `remove`, правило й `--recovery` у `rule add`, префікс у `rule remove`) (19) | |
| Git-фільтр CLI: закінчення рядків: `RefreshResult`, `SecretsFiles` | `Decrypted`, `Total`, `Messages` (3) | `LegacyFilterAttributes` (1) |

#### Нетривіальні методи (738)

| Тип | Методи |
|---|---|
| `Entity<TId>` (1) | `Equals(Entity<TId>)` — порівняння за типом та ідентифікатором |
| `SecretToken` (3) | `Generate`, `Hash`, `Matches` (порівняння за сталий час) |
| `StackName` (2) | `Parse`, `TryParse` |
| `StackFilePath` (3) | `Parse`, `TryParse`, `IsValid` |
| `CommitSha` (2) | `Parse`, `TryParse` |
| `BranchName` (3) | `Parse`, `TryParse`, `IsValid` |
| `DockerEnvironment` (10) | `Create`, `Rename`, `IssueEnrollmentToken`, `TryEnroll`, `VerifyAgentSecret`, `RecordConnection`, `RevokeAgent`, `TouchLastSeen`, `NormalizeName`, `NormalizeHostname` |
| `GitRepository` (7) | `Create`, `Rename`, `ChangeSource` (адреса під префіксом підключення), `IsUsableIn` (де стеки можуть деплоїти з репозиторію), `EnsureOwnConnection`, `ParseUrl`, `NormalizeName` |
| `GitConnection`, `GitUrl` (ADR-0006, 14) | `Create`, `Rename`, `IsAllowedIn`, `Covers`, `ChangeUrlPrefix` (не лишає репозиторії поза префіксом, токен — на своєму хості), `AllowEnvironments`, `SetAccessToken`, `SetWebhookSecret`, `NormalizeAccessUserName`, `NormalizeName`; `GitUrl.Parse` (без облікових даних, query й %-кодування), `Covers` (сегменти без регістру й `.git`), `Key`, `Segments` |
| `Stack` (3) | `EnsureAllows`, `RequiresUpdateHelper`, `RecordDeployment` |
| `Deployment` (8) | `Start`, `ResolveCommit`, `Succeed`, `Fail`, `Cancel`, `TransitionTo`, `EnsureStatus`, `AppendLog` (ліміт рядків і довжини) |
| `ImageReference` (1) | `TryParse`: ім'я, тег, digest, порт registry |
| `RolePermissions` (3) | `MinimumRole`, `Grants`, `RequiredFor` |
| `User` (4) | `Rename`, `GrantAdmin`, `EnsureActive`, `NormalizeUserName` |
| `DiscordLink` (4) | `IssueCode`, `HashCode` (регістр, пробіли й дефіси не важать), `CompleteLink` (строк дії коду), `NormalizeDiscordUserName` |
| `RoleAssignment` та підкласи (8) | `ChangeRole`, `EnsureDefined`; `Covers`, `CoversWholeEnvironment`, `Reveals` в `EnvironmentRoleAssignment` і `StackRoleAssignment` (по 3) |
| `AccessPolicy` (9) | `EditorEnvironments` (ADR-0006; з ADR-0007 звужується до scope токена), `CanSee(Actor, DockerEnvironment, …)`, `EffectiveRole` для стеку, `EffectiveRole` для середовища, `CanPerform` для стеку, `CanPerform` для середовища, `Highest`, `OwnAssignments`, `EnsureSameEnvironment` |
| `EnvironmentService` (7) | `CreateAsync`, `IssueEnrollmentTokenAsync`, `ListVisibleAsync`, `IssueToken`, `UpdateAsync` (унікальність назви без збігу із собою), `RevokeAgentAsync` (забути облікові дані, потім обірвати з'єднання), `Summarize` |
| `AgentEnrollmentService` (4) | `EnrollAsync`, `AuthenticateAsync`, `RecordConnectedAsync`, `RecordDisconnectedAsync` |
| `Application.DependencyInjection` (1) | `AddApplication` |
| `AggregatorDbContext` (2) | `OnModelCreating`, `ConfigureConventions` |
| `ModelConfigurationBuilderExtensions` (1) | `AddStronglyTypedId` |
| 12 конфігурацій EF (12) | `Configure` у кожній |
| `NotificationEvent` (1) | `EnsureSingleKind` |
| `NotificationChannel` (4) | `Rename`, `MoveToDiscordChannel`, `EnsureDiscordId`, `NormalizeName` |
| `NotificationRule` (3) | `ChangeEvents`, `Matches` (вид події й область: система, середовище, стек), `EnsureEvents` |
| `Notification` (5) | `Create` (лише з правила, що збігається), `IsStale`, `MarkFailed`, `Finish` (стан-машина), `NormalizeSummary` |
| Репозиторії сповіщень (5) | `ListAsync`; `ListActiveAsync`, `ListByChannelAsync`; `ListPendingAsync`, `ListRecentAsync` |
| `EfRepository<TEntity, TId>` (2) | `FindAsync`, `GetAsync` |
| `EnvironmentRepository` (3) | `FindByEnrollmentTokenHashAsync`, `ListAsync`, `NameExistsAsync` |
| `StackRepository`, `UserRepository`, `RoleAssignmentRepository` (4) | `FindByNameAsync`, `ListByEnvironmentAsync`, `FindByUserNameAsync`, `ListForUserAsync` |
| `DatabaseMigrator`, `DesignTimeDbContextFactory` (2) | `StartAsync`, `CreateDbContext` |
| `AgentGatewayOptions` (1) | `IsValid` |
| `AgentSession` (7) | `RequestAsync`, `WriteAsync`, `RecordActivity`, `ReportProgress`, `ReportLogs`, `Complete`, `Close` |
| `AgentRegistry` (4) | `Open`, `Remove`, `TryGet`, `Close` |
| `EnvironmentEndpoints` (PR #48, 2) | `MapEnvironmentAdministration`, `MapRegistryCredentials` |
| `GrpcAgentChannel` (11) | `ListContainersAsync`, `StartContainerAsync`, `StopContainerAsync`, `RestartContainerAsync`, `DeployStackAsync`, `ReadContainerLogsAsync`, `SendAsync`, `WaitFor`, `ToSnapshot`, `ToState`, `ToErrorKind` |
| `Infrastructure.DependencyInjection` (1) | `AddInfrastructure` |
| `AgentGatewayService` (9) | `Enroll`, `Connect`, `AuthenticateAsync`, `ReadHelloAsync`, `ReceiveAsync`, `Dispatch`, `RecordDisconnectedAsync`, `WithEnrollmentAsync` (дві версії) |
| `ProblemDetailsExceptionHandler` (2) | `TryHandleAsync`, `StatusFor` |
| `ContainerCommand` (2) | `ExecuteAsync` (шаблонний метод), `EnsureAllowedState` |
| `StartContainerCommand`, `StopContainerCommand`, `RestartContainerCommand` (6) | `IsAllowedIn`, `SendAsync` у кожній |
| `StoppingContainerCommand` (1) | `ValidateTimeout` |
| `AuthorizedCommand` (1) | `ExecuteAsync` |
| `ContainerCommandService` (2) | `ExecuteAsync`, `Create` |
| `CurrentActor` (3) | `GetAsync` (з ADR-0007 — `Actor` зі scope облікових даних), `GetAdministratorAsync` (обмежений токен — не адміністратор), `GetSignedInUserAsync` (ADR-0007: лише сесія, не токен) |
| `ContainerQueryService` (2) | `ListAsync`, `Can` |
| `ContainerLookup` (1) | `Find`: повний id, точне ім'я, унікальний префікс id |
| `ContainerLogService` (1) | `ReadAsync`: межі хвоста, видимість середовища й контейнера (роль на стеку або на середовищі), право на логи до звернення до агента |
| `ContainerLogTail` (3) | `Add` (лише останні рядки, під блокуванням), `ToList`, `ToLine` |
| `DeployPipeline` (3) | `RunAsync`, `Describe`, `NotifyObserversAsync` (спостерігачі після збереження результату; помилка одного не зупиняє інших) |
| Події контейнерів (PR #64, 9) | агент: `ContainerEventFilter.IsRelevant` (без `exec_*` з командним рядком); `DockerDotNetGateway.FindContainerAsync`, `WatchContainerEventsAsync` (потік подій Docker через `Channel` зі збереженням порядку), `TimeOf`; `ContainerEventForwarder.RunAsync` (повтор після збою Docker), `ForwardAsync`, `TryFindAsync`; ЦА: `ContainerEventCoalescer.Report`, `PublishAfterWindowAsync` (одне оновлення на середовище за вікно) |
| Live-оновлення (PR #63, 7) | `LiveChanges.Collect` (зміни з трекера, включно з рядками логу деплою), `PublishAsync`, `EnvironmentOfAsync`; `AggregatorDbContext.SaveChangesAsync` (публікація лише після успішного збереження); `GrpcAgentChannel.ChangeContainersAsync` (і після помилки агента, але не офлайн); `LiveHub.SubscribeAsync` (групи видимих середовищ); `SignalRLiveUpdates.SendAsync` (помилка лише логується) |
| Rehydrate (PR #62, 4) | `Deployment.QueueRehydrate` (коміт закріплений з постановки); `SecretRehydrationService.RehydrateAsync` (лише стеки із секретами, без незавершеного деплою; збереження до постановки в чергу); `GitFetchStep.FetchPinnedAsync` (закріплений коміт, кеш при недоступному git); `AgentGatewayService.RehydrateAsync` (після відкриття сесії, помилка не рве з'єднання) |
| SOPS (PR #61, 18) | `AgeKey.Create` (формат recipient); `SopsFile.IsEncrypted`, `FormatOf`, `HasJsonMetadata` (метадані sops за форматом); `AgeKeyService.ListAsync` (видимість середовища), `CreateAsync`, `DeleteAsync` (лише ключ свого середовища); `SopsDecryptStep.ExecuteAsync` (лише ключі цільового середовища, файли стають секретними), `Unprotect`; `SopsOptions.IsValid`; `CliProcess.RunAsync` (очищене оточення, stdin/stdout без диска, тайм-аут з убиванням дерева процесів), `ReadAllAsync`; `SopsCli.DecryptAsync` (код виходу → повідомлення), `FirstLine`; `AgeKeygenCli.GenerateAsync`, `Parse`; `AgeKeyRepository.ListByEnvironmentAsync`; `AgeKeyEndpoints.MapAgeKeyEndpoints` |
| `GitFetchStep`, `BuildBundleStep`, `SendToAgentStep` (5) | `ExecuteAsync` у кожному; `GitFetchStep.EnsureUsable` (середовище, якому підключення більше не дозволене, не тягне), `GitFetchStep.Access` (розшифрування токена підключення, довірений хост) |
| `ValidateComposeStep` (3) | `ExecuteAsync`, `Check`, `Decode` |
| `DeploymentService` (4) | `QueueAsync`, `ListAsync`, `GetAsync`, `LoadVisibleStackAsync` |
| `DeploymentRunner` (2) | `RunAsync`, `FailInterruptedAsync` |
| `DeploymentOptions` (1) | `IsValid` |
| `RepositoryService`, `StackService` (13) | `CreateAsync` (без токена, під дозволеним підключенням), `ListAsync` (адміністратори — усі, редактори — доступні їхнім середовищам), `UpdateAsync`, `UsableConnectionAsync`, `LoadManagedAsync` (ADR-0006); `CreateAsync`, `ListVisibleAsync`, `GetAsync`, `ChangeSourceAsync` (Editor, не під час деплою), `UsableRepositoryAsync` (репозиторій, дозволений у середовищі стеку, ADR-0006), `Summarize` (права на дії), `RepositoryNamesAsync`, `LoadVisibleAsync` |
| Підключення до git-серверів (ADR-0006, 30) | `RepositoryActor.CanUse`; `RepositoryAccess.GetActorAsync`, `CanManageAsync` (право деплою на кожному стеку репозиторію); `GitConnectionService.ListAsync` (редактор бачить лише свої середовища), `CreateAsync`, `UpdateAsync`, `DeleteAsync` (не з репозиторіями), `CreateWebhookSecretAsync`, `ChangeAsync`, `AllowAsync` (лише наявні середовища), `SetAccessToken`, `EnsureNameIsFreeAsync`; `PushWebhookService.ReceiveForConnectionAsync` (репозиторій за адресою з підписаного тіла), `Authenticate`, `Ignore`, `IgnorePush`, `DeployRepositoryAsync` (`NotAllowed` для недозволеного середовища); `GitWebhookProvider.RepositoryUrls` (віртуальний), `String`, `GitLabWebhook.RepositoryUrls`; `LibGit2SharpGitSource.EnsureCacheSize`, `DeleteClone`; `GitHostGuard.EnsurePublicAsync`, `IsPublic` (приватні, link-local, CGNAT адреси); `GitConnectionRepository.ListAsync`, `NameExistsAsync`, `GitRepositoryRepository.ListByConnectionAsync`, `StackRepository.ListByRepositoryAsync`; `GitConnectionEndpoints.MapGitConnectionEndpoints`, `WebhookEndpoints.ReceiveForConnectionAsync` |
| `StackEndpoints` (2) | `MapStackEndpoints`, `MapStackAction` |
| `NotificationSettingsService` (11) | `ListChannelsAsync`, `CreateChannelAsync`, `UpdateChannelAsync`, `DeleteChannelAsync`, `ListRulesAsync`, `CreateRuleAsync` (область за переданими id, стек чужого середовища), `UpdateRuleAsync`, `DeleteRuleAsync`, `ListHistoryAsync` (назви каналів, межі ліміту), `DescribeAsync`, `ParseDiscordId` |
| `NotificationEndpoints` (3) | `MapNotificationEndpoints`, `Combine`, `Split` (список видів ↔ прапорці) |
| `LibGit2SharpGitSource` (10) | `FetchAsync`, `ReadAsync` (по дві версії), `Fetch`, `Read` (коміт з кешу), `WithLockAsync`, `CredentialsFor` (токен лише під префіксом підключення); `TreeReader.Read`, `TreeReader.ReadBlob` |
| `YamlComposeParser` (2) | `Parse`, `Find` (з merge-ключами) |
| `DeploymentWorker` (3) | `StartAsync`, `ExecuteAsync`, `RunAsync` |
| `InMemoryXmlRepository` (2) | `GetAllElements`, `StoreElement` (копії елементів під блокуванням) |
| `GitRepositoryRepository`, `DeploymentRepository` (4) | `ListAsync`; `HasUnfinishedAsync`, `ListUnfinishedAsync`, `ListByStackAsync` |
| `ClaimsCurrentUser` (1) | `Id` (читає й розбирає claim) |
| `MeEndpoints`, `DiscordLinkEndpoints` (2) | `MapMeEndpoints`, `MapDiscordLinkEndpoints` |
| `DiscordLinkService` (5) | `GetStatusAsync`, `IssueCodeAsync`, `UnlinkAsync`, `LinkAsync` (перенесення прив'язки), `FindLinkedUserAsync` |
| `DiscordLinkRepository` (2) | `FindByDiscordUserAsync`, `FindByCodeHashAsync` |
| `BotGatewayOptions`, `BotGatewayInterceptor` (4) | `IsValid`; `UnaryServerHandler` (перевірка токена), `ToStatus`, `ToStatusCode` |
| `BotGatewayService` (8) | `GetCaller`, `LinkAccount`, `ListEnvironments`, `ListContainers`, `ControlContainer`, `ActAsAsync` (виклик від імені прив'язаного користувача), `ParseEnvironmentId`, `ToState` |
| Webhook-деплой (PR #65) (25) | `GitRepository.SetWebhookSecret`, `Stack.SetDeployOnPush` (не для системного стеку); `GitWebhookProvider.ReadPush` (гілка, видалена гілка, не JSON), `FixedTimeEquals`; `HmacWebhookProvider.IsAuthentic` (HMAC-SHA256 за сталий час); `Recognizes` і `EventKind` у `GitHubWebhook`, `GiteaWebhook`, `GitLabWebhook`, `GitLabWebhook.IsAuthentic`; `PushDeploymentGate.EnterAsync`, `Releaser.Dispose`; `PushWebhookService.ReceiveAsync`, `DeployAsync` (стан деплою стеку → черга, пізніше або нічого); `PushFollowUpObserver.DeploymentFinishedAsync`; `RepositoryService.CreateWebhookSecretAsync`, `RemoveWebhookSecretAsync`; `StackService.SetDeployOnPushAsync`; `StackRepository.ListDeployedOnPushAsync`, `DeploymentRepository.FindUnfinishedAsync`; `WebhookEndpoints.MapWebhookEndpoints`, `ReceiveAsync`, `ReadBodyAsync` (ліміт тіла) |
| Хук деплою (9) | `Stack.SetDeployHook` (лише хеш, не для системного стеку), `AcceptsDeployHook` (за сталий час); `PushDeploymentGate.MarkPending` (вимога оновити образи не губиться за push); `DeployHookService.ReceiveAsync` (токен → черга, після поточного деплою або 503); `StackService.CreateDeployHookAsync`, `RemoveDeployHookAsync`, `LoadDeployableAsync`; `WebhookEndpoints.ReceiveDeployHookAsync`, `BearerToken` |
| Перезапуск після зміни файлів (5) | `Stack.SetRestartOnFileChange` (не для системного стеку); `StackService.SetRestartOnFileChangeAsync`; агент: `StackFileStore.FilesChanged` (порівняння з хостом і маніфестом до запису), `HasContent` (посилання не читається), `DeployStackHandler.RestartIfFilesChangedAsync` |
| Видалення репозиторію й середовища (3) | `RepositoryService.RemoveAsync` (лише без стеків, потім клон із кешу), `EnvironmentService.RemoveAsync` (лише адміністратор і без стеків; каскад ключів, кредів, ролей і правил, агент від'єднується), `LibGit2SharpGitSource.ForgetAsync` (видалення клону під замком репозиторію, помилка лише в лог) |
| Видалення стеку (2) | `Stack.EnsureRemovable` (не системний і не розгорнутий: його контейнери лишилися б без стеку); `StackService.RemoveAsync` (Editor середовища, без незавершеного деплою; історію, ролі й правила сповіщень забирає каскад) |
| API-токени (ADR-0007, 30) | `ApiToken.Create` (секрет, хеш, підказка, межі), `HasTokenShape`, `IsExpired`, `RecordUse` (не частіше разу на хвилину), `NormalizeName`; `AccessScope.Reaches`, `Allows` (лише читання); `Actor.IsAdmin` (обмежений scope знімає права адміністратора); `ApiTokenService.ListMineAsync`, `CreateAsync` (лише сесія, ліміт, лише видимі середовища), `RevokeMineAsync`, `RevokeCurrentAsync`, `ListForUserAsync`, `RevokeForUserAsync`, `RemoveAsync`; `ApiTokenAuthenticator.AuthenticateAsync`; `ApiTokenRepository.FindByHashAsync`, `ListForUserAsync`, `CountForUserAsync`; `ApiTokenConfiguration.Configure`; `ApiTokenAuthenticationHandler.HandleAuthenticateAsync`, `HandleChallengeAsync`, `HasBearer`; `ApiTokenClaims.For`, `TokenIdOf`, `IsReadOnly`, `ScopeOf`; `ReadOnlyTokens.UseReadOnlyTokens` (не-GET запит токена лише для читання — 403); `ApiTokenEndpoints.MapApiTokenEndpoints`, `ToResponse` |
| Вхід у CLI (ADR-0008, 28) | `CliAuthorization.Start` (коди, хеші, очищення назв), `FormatUserCode`, `IsOpen`, `Approve` (перевірка обмежень токена), `Deny`, `Poll` (`SlowDown` при частому опитуванні), `IssueToken`, `EnsureOpen`, `Clean`; `OneTimeCode.Normalize`; `ApiToken.ValidateOptions`; `ApiTokenService.EnsureRoomForTokenAsync`, `EnsureEnvironmentsVisibleAsync`; `CliAuthorizationService.StartAsync` (прибирання прострочених, ліміт відкритих), `GetOpenAsync`, `ApproveAsync` (лише сесія), `DenyAsync`, `ExchangeAsync` (невідомий код — `Expired`), `IssueAsync` (токен лише при видачі; деактивований користувач — `Denied`), `FindOpenAsync`; `CliAuthorizationRepository.FindByDeviceCodeHashAsync`, `FindByUserCodeHashAsync`, `CountUnexpiredAsync`, `RemoveExpiredAsync`; `CliAuthorizationConfiguration.Configure`; `CliAuthorizationEndpoints.AddCliAuthorizationRateLimits`, `MapCliAuthorizationEndpoints`, `PerAddress` |
| CLI: інфраструктура (ADR-0008, 60) | `ConfigStore.Default` (XDG, `~/.config`, `%APPDATA%`), `Load`, `Save` (0600 із створення, атомарно), `CreateDirectory` (0700); `ServerUrl.Parse` (https за замовчуванням, http лише для loopback), `SameServer`, `ContextName`, `IsLoopback`; `CredentialResolver.Resolve` (пріоритет джерел; токен контексту лише на його сервер), `FindContext`, `ResolveToken`, `ReadFile`, `Clean`; `BearerAuthenticationProvider.AuthenticateRequestAsync`; `ForestApiFactory.Create`, `CreateClient` (без повторів, з `ProblemDetailsHandler`); `ApiErrors.Translate` (×3), `Unreachable`; `ForestApi.FindRepositoryAsync`, `ResolveEnvironmentAsync`, `Match`, `FindStackAsync`, `ControlContainerAsync`, `GetContainerLogsAsync`, `ControlStackAsync`, `Required` (×2); `ProblemDetailsHandler.SendAsync`, `Text`; `Table.AddRow`, `Render`, `WriteRow`, `Clean`; `CliOutput.ListAsync`, `ItemAsync`, `Done`, `Time`, `ShortCommit`; `LogFollower.Accept`; `DeploymentWatcher.WatchAsync`, `WriteAsync`, `IsFinished`; `SystemBrowser.TryOpen`; `LoopbackSignal.Start`, `AcceptAsync`, `AnswerAsync`, `Dispose`; `BrowserLogin.RunAsync`, `PollAsync`; `CliHost.RunAsync` (облікові дані, помилки → коди виходу), `FromProcess`; `GlobalOptions.CredentialsFrom`; `CommandGroup.Build`, `ForestCommand.Build`; `EnvironmentCommand.ExecuteAsync`, `StackCommand.ExecuteAsync`, `StackCommand.Configure`; `Confirmation.Require`; `ForestCli.Build` |
| CLI: команди (ADR-0008, 42) | `ExecuteAsync` кожної з 36 команд; `LoginCommand.ServerAddress`, `RevokeReplacedAsync` (повторний вхід відкликає старий токен); `EnvironmentArgumentCommand.ExecuteAsync`, `AgentStatus`; `ContainerLogsCommand.WriteAsync` (stderr контейнера — у stderr); `DeploymentResult.ReportAsync` |
| CLI: адміністрування (ADR-0008, 54) | `ForestApi.FindGitConnectionAsync`, `AddGitConnectionAsync` (усі середовища або перелік, токен за бажанням), `UpdateGitConnectionAsync`, `FindUserAsync`, `SetUserActiveAsync`, `FindNotificationChannelAsync`; `SecretInput.ReadAsync` (stdin або введення без відлуння), `ReadWithoutEcho`; `ExecuteAsync` 36 команд; `RepositoryCommand`, `GitConnectionCommand`, `UserCommand`, `ChannelCommand` — `ExecuteAsync` (знаходять об'єкт за назвою чи id); `RepositoryCommand.ShowWebhookAsync`, `ConnectionEnvironmentOptions.ResolveAsync`, `UserCommand.Status`, `ChannelCommand.Events` (`all` і назви без урахування регістру), `Enabled`; `Program.Main` (код 2 для помилки розбору) |
| Passkeys (ADR-0009, 21) | `PasskeySignIn.SignInAsync` (асерція, lockout, активний доменний користувач, лічильник, «Запам'ятати мене»); `PasskeyManagement.MakeCreationOptionsAsync` (ліміт ключів), `AddAsync` (атестація лише для поточного користувача), `ListMineAsync`, `RenameAsync`, `RemoveMineAsync`, `ListForUserAsync`, `RemoveForUserAsync`, `RemoveAsync`, `ListAsync`, `SignedInAsync` (лише сесія), `AccountOfAsync`, `View`, `CredentialId` (base64url), `NormalizeName`, `Ensure`; `PasskeyEndpoints.MapPasskeyEndpoints`, `ToResponse`; `DesignTimeDbContextFactory.CreateDbContext` (налаштування Identity для міграцій); `UserPasskeysCommand.ExecuteAsync`, `UserRemovePasskeyCommand.ExecuteAsync` |
| Discord OAuth2 (ADR-0009, 15) | `DiscordLink.LinkVerified`, `UpdateDiscordUserName`; `DiscordLinkService.LinkVerifiedAsync` (перенесення прив'язки), `FindUserForSignInAsync` (лише прив'язаний активний користувач, оновлення імені); `DiscordAuthOptions.IsConfigured`; `DiscordSignIn.AddTo` (схема, scope `identify`, PKCE, Lax-cookie потоку, `OnRemoteFailure`), `LoginProperties`, `LinkPropertiesAsync` (лише сесія), `CompleteAsync`, `ContinuePage` (`meta refresh` для SameSite=Strict), `SafePath`, `Item`, `SignInAsync` (без реєстрації, lockout), `LinkAsync`; `DiscordAuthEndpoints.MapDiscordAuthEndpoints` |
| SOPS для git-фільтра: ЦА (ADR-0010, 23) | `SopsFile.AgeRecipients`; `SopsSecretsService.DecryptAsync` (право токена, лише ключі редакторських середовищ, 403 і 422), `EncryptAsync` (не лише читання, вже зашифрований — відмова), `EditableAsync`, `EnsureSize`, `Unprotect`; `GitAttributes` (конструктор: файли від кореня вниз), `IsFiltered` (останнє правило перемагає), `Parse` (`filter=forest`, `-filter`, `!filter`, інший драйвер), `PatternRegex` (шаблони `.gitignore`: `*`, `**`, `?`, класи, прив'язка до каталогу); `SopsDecryptStep.EnsureFilteredFilesEncrypted`; `LibGit2SharpGitSource.OuterAttributes`; `AgeKeyRepository.FindByRecipientsAsync`; `SopsCli.EncryptAsync` (тимчасовий конфіг лише з правила, stderr не логується), `ConfigFor`; `YamlSopsConfigReader.RuleFor`, `Matches` (неретроспективний regex), `Read` (лише age і параметри шифрування), `Recipients` (рядок через кому або список), `IsEmpty`, `Find`; `SopsEndpoints.AddSopsRateLimit`, `MapSopsEndpoints` |
| Git-фільтр CLI (ADR-0010, 63) | `PktLineReader.ReadPacketAsync`, `ReadTextListAsync`, `ReadContentAsync`, `ReadExactlyOrEndAsync`; `PktLineWriter.WriteTextListAsync`, `WriteContentAsync` (поділ на пакети), `WriteFlushAsync`, `WritePacketAsync`; `SopsFiles.IsEncrypted`, `AgeRecipients`, `HasJsonMetadata`; `GitWorkTree.Open`, `GetConfig`, `Filtered` (`check-attr -z`), `WorkTreeFile`, `CheckOutAgain`, `Renormalize`, `GitPath`, `Run`, `RunRaw`; `FilterCache.Open`, `Find` (HMAC за сталий час), `Remember`, `Forget`, `Save` (атомарно, 0600), `Mac`, `LoadOrCreateKey`, `PrivateFile`, `CreatePrivateDirectory`; `AggregatorSops.EncryptAsync`, `CallAsync` (помилки API → причина для терміналу); `LocalSops.RunAsync`; `FilterOperation.RunAsync` (шаблонний метод), `TryDecryptAsync` (бекенди по черзі, причина один раз); `SmudgeOperation.NeedsConversion`, `ConvertAsync` (без доступу — шифротекст); `CleanOperation.NeedsConversion`, `ConvertAsync` (кеш, жодного відкритого тексту при помилці); `GitFilterProcess.RunAsync`, `HandshakeAsync`; `SecretsSession.Open`; `GitFilterSetup.Apply`, `Executable` (лапки для shell), `Refresh`, `InstallHook` (чужий хук не перезаписується); `SecretsFiles.AddAttributes`, `EnsureFilterSetUp`, `InRepository`; `ExecuteAsync` 9 команд; `GitSetupCommand.CurrentContextName`; `SecretsInitCommand.RecipientAsync`, `AppendRule`; `ForestApi.DecryptSopsFileAsync`, `EncryptSopsFileAsync` |
| Git-фільтр CLI: `secrets remove`, `secrets rule`, `git teardown` (22) | `SopsConfigFile.Parse` (рядки блокового списку, коментар — до наступного правила, чужа форма — відмова), `AddPrefixRule` (перед першим правилом, що перекривається), `RemovePrefixRule`, `ToString` (незмінені правила — як були), `RuleLines`, `PrefixRegex` (RE2, перенесено з `SecretsInitCommand.EscapeRegex`); `SopsConfigFile.Rule.Overlaps` (префікси в обидва боки, інший regex — збіг на файлі під префіксом), `Read` (age, path_regex, інші типи ключів), `AsPrefix`, `WithoutComment`, `Unquote`; `SecretsFiles.RemoveAttributes` (свій рядок геть, шаблон — `-filter -diff`, відкат, якщо глибший `.gitattributes` вмикає фільтр), `EnvironmentsByRecipientAsync`; `GitFilterSetup.Remove` (незакомічені зміни — відмова, шифротекст назад, кеш геть), `RemoveHook` (чужий хук лишається); `SopsConfigEditing.Load`, `Describe`; `ExecuteAsync` 5 команд |
| Git-фільтр CLI: закінчення рядків (11) | `SecretsFiles.WithLineEndings` (`eol=lf` для рядків `filter=forest` без власного `eol`, `-text` чи `binary`), `WithoutLineEndings` (файли, які git може віддати з CRLF), `NoteLineEndings`; `SecretsFixEolCommand.ExecuteAsync` (`.gitattributes`, blob без CR в індекс повз робочий файл, повторний checkout); `GitSetupCommand.Report` (причина від фільтра замість загальної підказки); `GitWorkTree.Attribute` (`check-attr` для будь-якого атрибута), `StageContent` (`hash-object` + `update-index`); `SopsFiles.HasCrLf`, `SameButLineEndings` (шифротекст, який git лише перевів у CRLF, перечитується), `WithoutCr`, `PlainForSops` (dotenv і INI шифруються без CR) |

Не враховано як тривіальні: фабрики `Queue*`, `Create*` у `Stack`, `User` і `RoleAssignment`, методи `MakePrivate`, `MakePublic`, `RemoveAccessToken`, `RemoveDeployHook`, `PushDeploymentGate.TakePending`, `ChangeSource`, `RevokeAdmin`, `Activate`, `Deactivate`, методи-делегатори (`RecordHeartbeat`, `CanSee` для стеку, `CanPerform` для операції, `IsActiveAdmin`, `Equals(object)`, `EfRepository.Add`/`Remove`, `AgentRegistry.IsOnline`, `GrpcAgentChannel.IsOnline`, `NullIfEmpty`, `StronglyTypedIdConverter.Create`, `DeployContext.Info`/`Warn`/`Error`, `DeploymentQueue.Enqueue`, `BuildBundleStep.Kib`), обчислювані властивості (`StackFilePath.Directory`/`FileName`, `ImageReference.IsFloatingLatest`, `StackBundle.TotalBytes`, `Deployment.Log`) і методи структур ID.

#### Ієрархії успадкування

1. `Entity<TId>` → `DockerEnvironment`, `GitRepository`, `GitConnection` (ADR-0006), `Stack`, `Deployment`, `User`, `DiscordLink`, `NotificationChannel`, `NotificationRule`, `Notification`, `ApiToken` (ADR-0007), `CliAuthorization` (ADR-0008), `RoleAssignment` → `EnvironmentRoleAssignment`, `StackRoleAssignment`. Це 16 класів на трьох рівнях.
2. `DomainException` → `DomainValidationException`, `DomainConflictException`. Це 3 класи.
3. `AgentException` → `AgentOfflineException`, `AgentDisconnectedException`, `AgentTimeoutException`, `AgentCommandFailedException`. Це 5 класів.
4. `EfRepository<TEntity, TId>` → `EnvironmentRepository`, `StackRepository`, `UserRepository`, `RoleAssignmentRepository`, `GitRepositoryRepository`, `GitConnectionRepository` (ADR-0006), `DeploymentRepository`, `DiscordLinkRepository`, `NotificationChannelRepository`, `NotificationRuleRepository`, `NotificationRepository`, `ApiTokenRepository` (ADR-0007), `CliAuthorizationRepository` (ADR-0008). Це 14 класів.
5. `ContainerCommand` → `StartContainerCommand`, `StoppingContainerCommand` → `StopContainerCommand`, `RestartContainerCommand`. Це 5 класів на трьох рівнях.
6. `GitWebhookProvider` → `HmacWebhookProvider` → `GitHubWebhook`, `GiteaWebhook`; `GitWebhookProvider` → `GitLabWebhook` (PR #65). Це 5 класів на трьох рівнях.
7. Команди CLI (ADR-0008): `ForestCommand` → `LoginCommand`, `LogoutCommand`, `WhoAmICommand`, 4 команди контекстів, `EnvListCommand`, `EnvCreateCommand`, `DeploymentShowCommand`, `DeploymentLogsCommand`, `EnvironmentArgumentCommand` → 5 команд середовища (`show`, `update`, `remove`, `enrollment-token`, `revoke-agent`), `EnvironmentCommand` → 6 команд ключів і кредів registry, 3 команди контейнерів, `StackListCommand`, `StackCreateCommand`, `StackCommand` → 8 команд стеку й `DeploymentListCommand`; `RepositoryCommand` → 4 команди репозиторію, `GitConnectionCommand` → 6 команд підключення, `UserCommand` → 13 команд користувача, `ChannelCommand` → 5 команд каналу й правил, і ще 24 команди прямо від `ForestCommand` (10 адміністративних, 10 команд секретів і 4 команди git-фільтра, ADR-0010). Це 96 класів на чотирьох рівнях.
8. Операції git-фільтра (ADR-0010): `FilterOperation` → `SmudgeOperation`, `CleanOperation`. Це 3 класи.

#### Поліморфізм

| # | Вид | Поліморфний інтерфейс | Клієнтський код |
|---|---|---|---|
| 1 | Динамічний (віртуальні методи) | `RoleAssignment.Covers`, `CoversWholeEnvironment`, `Reveals` | `AccessPolicy.EffectiveRole` і `AccessPolicy.CanSee` працюють зі списком `RoleAssignment`, не знаючи конкретних підкласів ✅ |
| 2 | Статичний (generics, `static abstract`) | `StronglyTypedIdConverter<TId>` викликає `IStronglyTypedId<TSelf>.From` | `AggregatorDbContext.ConfigureConventions` реєструє один конвертер через `AddStronglyTypedId<TId>` для всіх шести типів ID ✅ |
| 3 | Статичний (generics) | `IRepository<TEntity, TId>` і `EfRepository<TEntity, TId>` | `EnvironmentService` і `AgentEnrollmentService` викликають `FindAsync` і `GetAsync`, реалізовані один раз для будь-якої сутності ✅ |
| 4 | Динамічний (декоратор) | `IContainerCommand`: реалізують `ContainerCommand` і `AuthorizedCommand` | `ContainerCommandService` виконує команду через `IContainerCommand`, а `AuthorizedCommand` викликає вкладену команду, не знаючи її конкретного типу ✅ |
| 5 | Динамічний (шаблонний метод) | `ContainerCommand.IsAllowedIn`, `SendAsync`, `AllowedOnSystemStack` | `ContainerCommand.ExecuteAsync` виконує однакові перевірки для старту, зупинки й перезапуску, а відмінності бере з перевизначених методів ✅ |
| 6 | Динамічний (ланцюжок кроків) | `IDeployStep`: реалізують `GitFetchStep`, `ValidateComposeStep`, `BuildBundleStep`, `SendToAgentStep` | `DeployPipeline.RunAsync` виконує кроки зі списку `IDeployStep`, не знаючи їхніх типів; новий етап (SOPS) додається без зміни пайплайну ✅ |
| 7 | Динамічний (абстрактні й віртуальні методи) | `GitWebhookProvider.Recognizes`, `IsAuthentic`, `EventKind`, `ReadPush`, `RepositoryUrls` (ADR-0006); `HmacWebhookProvider` дає спільну перевірку HMAC, підкласи — лише заголовок і префікс підпису (PR #65) | `PushWebhookService.ReceiveAsync` і `ReceiveForConnectionAsync` обирають провайдера зі списку `GitWebhookProvider` і перевіряють підпис, не знаючи, GitHub це, Gitea чи GitLab; адресу репозиторію з тіла дає віртуальний `RepositoryUrls`, який GitLab перевизначає (ADR-0006) ✅ |
| 8 | Динамічний (Composite) | `ICliNode.Build`: реалізують `CommandGroup` і `ForestCommand` (ADR-0008) | `ForestCli.Build` і `CommandGroup.Build` будують дерево команд зі списку `ICliNode`, не розрізняючи груп і команд ✅ |
| 9 | Динамічний (шаблонний метод) | `ForestCommand.ExecuteAsync` → `EnvironmentCommand.ExecuteAsync(context, environment)` → `StackCommand.ExecuteAsync(context, environment, stack)` | `CliHost.RunAsync` виконує будь-яку з 36 команд через `ForestCommand.InvokeAsync`; проміжні класи розв'язують середовище й стек, підкласи лише працюють ✅ |
| 10 | Статичний (generics) | `CliOutput.ListAsync<T>`, `ItemAsync<T>` для `T : IParsable`; `ForestApi.Required<T>` для посилальних типів і `Nullable<T>` | команди виводять списки середовищ, стеків, контейнерів, деплоїв і окремі об'єкти одним кодом таблиці та JSON ✅ |
| 11 | Динамічний (Strategy) | `ISopsBackend`: реалізують `AggregatorSops` і `LocalSops` (ADR-0010) | `FilterOperation.TryDecryptAsync` пробує розшифрувати бекендами зі списку по черзі, а `CleanOperation` шифрує тим, що в `FilterServices.Encryptor`, не знаючи, ЦА це чи локальний sops ✅ |
| 12 | Динамічний (шаблонний метод) | `FilterOperation.NeedsConversion`, `ConvertAsync`: перевизначають `SmudgeOperation` і `CleanOperation` (ADR-0010) | `GitFilterProcess.RunAsync` виконує команду git через словник `FilterOperation` за назвою (`clean`, `smudge`), а `FilterOperation.RunAsync` однаково пропускає файли, що не потребують перетворення ✅ |

#### Тести (не входять в облік)

- `tests/Aggregator/Domain`, 278 тестів (разом із варіантами `Theory`): право розшифровувати секрети лише явно й ніколи для токена лише для читання, його видача через вхід у CLI (ADR-0010); прив'язка через Discord без коду й оновлення імені (ADR-0009); вхід у CLI (ADR-0008): лише хеші кодів, формат і нормалізація коду, опитування (`SlowDown`), підтвердження, відмова, строк, одне рішення, перевірка обмежень токена; API-токени й scope (ADR-0007): лише хеш і підказка, межі середовищ, строк дії, запис використання, обмежений токен адміністратора — Editor у своїх середовищах і не адміністратор, лише читання; value objects, код прив'язки Discord, правила й стан-машина сповіщень, посилання на образ (тег, digest, `latest`), реєстрація агента, стан-машина й лог деплою, правила системних стеків, ефективна роль і видимість середовищ; підключення до git-серверів (ADR-0006): префікс адреси за сегментами, `.git` і регістр, відхилення облікових даних і %-кодування, дозволені середовища, токен лише на своєму хості, репозиторій — лише там, де дозволене його підключення; хук деплою приймає лише свій токен і зберігається хешем.
- `tests/Aggregator/Infrastructure`, 123 тести (разом із варіантами `Theory`): ADR-0010: помилка sops на файлі з CRLF називає причину; вибір правила `.sops.yaml` і відмова на KMS, Vault, не-age отримувачах, непідтримуваному regex і невалідному YAML; шифрування справжнім sops для отримувачів правила (YAML, dotenv, двійковий) і з `encrypted_regex`, помилка без цитування вводу; `.gitattributes` каталогів вище стеку з git; зіставлення запитів і відповідей у `AgentSession`, прогрес команди, закриття сесії, таймаут, заборона паралельного запису в стрім, заміна сесії в `AgentRegistry`; fetch зі справжнього локального git-репозиторію (нові коміти, видалена гілка, права на виконання, символьні посилання, ліміт розміру); розбір compose-файлів з якорями й merge-ключами; токен лише під префіксом підключення (зокрема після перенаправлення), відмова тягнути публічний репозиторій з приватної адреси, видалення клону понад ліміт кешу, класифікація адрес `GitHostGuard` (ADR-0006).
- `tests/Aggregator/Application`, 349 тестів (разом із варіантами `Theory`): ADR-0010: розшифрування лише ключами середовищ, де користувач Editor (і стеку), відмова Viewer'у й Operator'у до запуску sops, адміністратор — усі, токен без права й токен інших середовищ, файл без ключа ЦА, шифрування за правилом і відмова токену лише для читання; шаблони `.gitattributes`; деплой відмовляє на незашифрованому файлі під `filter=forest`; прив'язка й вхід через Discord (перенесення, деактивований користувач, лише прив'язаний активний, ADR-0009); вхід у CLI (ADR-0008): токен видається один раз, відмова, невідомий код, токен не підтверджує вхід, лише видимі середовища, строк, деактивація після підтвердження, прибирання прострочених; API-токени (ADR-0007): автентифікація, відкликання й строк, токен не керує токенами, але відкликає себе, межі лише видимими середовищами, адміністратор відкликає чужі, ліміт кількості; `CurrentActor` зі scope; прив'язка Discord-акаунта (код, строк дії, перенесення, деактивований користувач); пайплайн деплою (порядок кроків, перша помилка зупиняє, приховування внутрішніх помилок), кожен крок деплою, права на деплой, один деплой на стек, офлайн-агент, системний стек; пошук контейнера за id, ім'ям і префіксом; команди контейнерів — дозволені стани, системні стеки, таймаут зупинки, права через роль на середовищі й на стеку, адміністратор, деактивований і анонімний користувач, приховування чужого середовища без звернення до агента; `CurrentActor`; фільтрація середовищ і контейнерів за видимістю; керування середовищами лише для адміністраторів; репозиторії й підключення (ADR-0006): редактор додає репозиторій лише під підключенням свого середовища, зміна — лише з правом деплою на кожному стеку, список для редактора, недоступне підключення — 404, підключення змінює лише адміністратор, префікс не лишає репозиторії поза собою, підключення з репозиторіями не видаляється; webhook організації, `NotAllowed` для недозволеного середовища, адреса репозиторію з тіла GitLab; хук деплою: деплой з оновленням образів, однакова відмова, офлайн-агент, виклик під час деплою й після деплою без оновлення образів.
- `tests/Aggregator/Api`, 137 інтеграційних тестів: `/api/sops/*` (ADR-0010): шифрування й розшифрування Editor'ом, Operator шифрує, але не розшифровує, токен із правом і без, токен лише для читання, непридатні запити (400, 422), прапорець у вході CLI; вхід і прив'язка через Discord OAuth2 з фальшивим Discord (ADR-0009): вхід прив'язаним акаунтом, відмова неприв'язаному, прив'язка із сесії, прив'язка без сесії веде на вхід, адреса повернення лише свого сайту, callback із чужого браузера відхиляється, без налаштувань вимкнено; passkeys (ADR-0009) з програмним автентифікатором `FakeAuthenticator` (ES256, CBOR): реєстрація й вхід без пароля, перейменування й видалення, деактивований користувач, невідомий ключ, відповідь без виклику опцій у цьому браузері, токен не додає ключ, адміністратор видаляє загублений; вхід у CLI через HTTP (ADR-0008): повний потік до запиту з отриманим токеном, підтвердження лише сесією, відмова, невідомий код, ліміт запитів; API-токени через HTTP (ADR-0007): Bearer, токен лише для читання (403 на зміни, але може відкликати себе), обмежений токен не бачить інших середовищ і не адміністратор, токен не міняє пароль і не відкриває live-хаб, деактивований користувач; збереження каналів, правил і сповіщень у SQLite; ЦА в пам'яті, фейковий агент через gRPC, git підмінено заглушкою. Шлюз бота: токен, неприв'язаний акаунт, прив'язка кодом з панелі, перезапуск контейнера від імені прив'язаного користувача, коди статусу для відмови в правах і невідомого середовища. Деплой через агента з прогресом і логом, помилка агента, крок, що зупиняє деплой до агента, конфлікт двох деплоїв, права Viewer. Реєстрація, повторне використання токена, невірний секрет, Hello першим повідомленням, список контейнерів, команда до офлайн-агента, помилка агента, обрив з'єднання, перезапуск контейнера через агента, 403 і 401 для dev-користувачів, `/api/me`, приховане приватне середовище, заборона створювати середовища звичайному користувачу; підключення з токеном, якого немає у відповіді, репозиторій редактора під ним і стек з ним, 404 для середовища поза підключенням, 409 на видалення підключення в користуванні; webhook організації через підключення (ADR-0006); хук деплою: деплой з `PULL_POLICY_ALWAYS`, однаковий 401, права Editor.
- `tests/Cli`, 124 тести (разом із варіантами `Theory`): git-фільтр (ADR-0010): `eol=lf` у нових і старих рядках `.gitattributes`, `secrets fix-eol` (blob без CR, повторний checkout, змінений файл не чіпається), `check` відхиляє шифротекст із CRLF, dotenv із CRLF шифрується без CR, підказка при CRLF; pkt-line (пакети понад 64 КБ, відмова на чужому протоколі), протокол фільтра з боку git (рукостискання, smudge і clean, помилка для одного файла), кеш повертає той самий шифротекст і не містить відкритого тексту, без доступу файл лишається зашифрованим і причина звучить один раз, відкритий текст не проходить при помилці шифрування, `secrets check` на справжньому git (усі файли й `--staged`), `secrets add` без налаштованого фільтра, `secrets remove` (свій рядок `.gitattributes`, `-filter` для шаблону, шифротекст у клоні, відмова для файлів без шифротексту в git, `--plain` лише з `--yes` і лише для розшифрованого), `git teardown` (конфіг, хук, кеш, шифротекст; чужий хук лишається), правила `.sops.yaml` (без змін — той самий текст, місце нового правила, дубль, видалення з коментарем, рукописні правила, відмова на чужій формі); адреса сервера (https за замовчуванням, http лише для loopback), пріоритет джерел облікових даних і заборона надсилати токен контексту на інший сервер, конфігурація (місце за XDG, права 0700/0600, пошкоджений файл), таблиця, повторні рядки логу на межі `since`, loopback-сигнал, розбір командного рядка, довідка кожної команди.
- `tests/Aggregator/Api/CliEndToEndTests.cs`, 11 наскрізних тестів CLI проти ЦА в пам'яті: `secrets init` пише ключ середовища, `.gitattributes`, конфіг і хук, а протокол фільтра шифрує через ЦА (ADR-0010); `secrets rule add`, `list`, `remove` з ключами середовищ і резервним ключем; вхід через браузер, вихід із відкликанням токена, контейнери через фейкового агента, deploy зі стеженням за логом, токен лише для читання, `--yes`, користувач із роллю (пароль зі stdin), репозиторій і правило сповіщень, обмежений токен адміністратора без керування користувачами.

### Лаба 2

| Вимога | Потрібно | Є | Стан |
|---|---|---|---|
| Екрани | ≥ 4 | 10 | ✅ |
| Елементи керування | ≥ 20 | 126 | ✅ |
| Контейнерні елементи | ≥ 1 | 9 (7 таблиць, 2 списки) | ✅ |
| Обробники подій | ≥ 10 | 76 | ✅ |
| Логіка окремо від GUI | так | так | ✅ |

Код: `src/Web` — стеки, деплої й репозиторії (PR #45), приватні репозиторії (PR #60), ключі середовищ (PR #61), webhook і деплой після push (PR #65), підключення до git-серверів (ADR-0006), хук деплою. Діалог підтвердження `src/components/confirm-action.tsx` — Денисенко Л. В., тут лише використаний; його кнопки в облік не йдуть.

- **Екрани:**
  1. стеки середовища (`src/routes/_authed/stacks/index.tsx`, `/stacks`);
  2. стек: дії й редактор джерела (`src/routes/_authed/stacks/$stackId/index.tsx`, `/stacks/$stackId`);
  3. історія деплоїв стеку (`src/routes/_authed/stacks/$stackId/deployments.tsx`, `/stacks/$stackId/deployments`);
  4. деплой із логом (`src/routes/_authed/deployments/$deploymentId.tsx`, `/deployments/$deploymentId`);
  5. репозиторії (`src/routes/_authed/repositories.tsx`, `/repositories`);
  6. ключі середовищ (`src/routes/_authed/keys.tsx`, `/keys`);
  7. підключення до git-серверів (`src/routes/_authed/git-connections.tsx`, `/git-connections`, ADR-0006);
  8. API-токени (`src/routes/_authed/tokens.tsx`, `/tokens`, ADR-0007);
  9. підтвердження входу CLI (`src/routes/_authed/cli/authorize.tsx`, `/cli/authorize`, ADR-0008);
  10. код входу CLI (`src/routes/_authed/cli/device.tsx`, `/cli/device`, ADR-0008).
- **Елементи керування (90):**
  - шапка: посилання «Стеки» й «Репозиторії»;
  - стеки: список «Середовище», кнопка «Новий стек», посилання на стек у кожному рядку, кнопка «Оновити»; діалог «Новий стек» (`src/components/stacks/create-stack-dialog.tsx`): поле назви, список репозиторіїв, поля гілки й compose-файлу, кнопки «Скасувати» й «Створити»;
  - стек: посилання «Усі стеки», кнопки «Деплой», «Деплой з оновленням образів», «Перезапустити», «Down»; редактор джерела: список репозиторіїв, поля гілки й compose-файлу, кнопки «Зберегти» й «Скасувати зміни»; посилання на останні деплої й «Уся історія»;
  - історія: посилання на стек, список «Стан», кнопка «Оновити», посилання на деплой у кожному рядку;
  - деплой: посилання на історію, прапорці «Лише попередження й помилки» і «Стежити за новими рядками»;
  - репозиторії: поле пошуку, кнопки «Підключення» (лише адміністраторам) і «Додати репозиторій»; поля діалогу (`src/components/stacks/repository-fields.tsx`, ADR-0006): назва, список «Підключення», адреса; кнопки «Скасувати» й «Додати» (`add-repository-dialog.tsx`); кнопка «Змінити» в рядку (лише для `canManage`); діалог зміни (`edit-repository-dialog.tsx`): ті самі поля, «Скасувати», «Зберегти»; розділ webhook (`src/components/stacks/webhook-section.tsx`): «Створити секрет / Новий секрет», «Вимкнути webhook», «Копіювати адресу», «Копіювати секрет»;
  - підключення (ADR-0006): кнопки «Репозиторії», «Нове підключення», «Змінити» в рядку; поля підключення (`src/components/git-connections/connection-fields.tsx`): назва, префікс адреси, прапорець «Усі середовища», прапорці середовищ; діалог створення (`create-connection-dialog.tsx`): прапорець «З токеном доступу», поля токена й імені користувача (`access-token-fields.tsx`), «Скасувати», «Створити»; діалог зміни (`edit-connection-dialog.tsx`): «Скасувати», «Зберегти», поля токена й імені, «Задати / Замінити токен», «Видалити токен», кнопки webhook організації, «Видалити підключення»;
  - стек (PR #65): перемикач «Деплоїти після push»;
  - стек, хук деплою (`src/components/stacks/deploy-hook-card.tsx`): «Створити хук / Новий токен», «Вимкнути хук», «Копіювати адресу», «Копіювати токен», «Копіювати команду»;
  - стек: перемикач «Перезапускати після зміни файлів», кнопка «Видалити стек» (з підтвердженням; вимкнена, поки стек розгорнутий);
  - діалог зміни репозиторію: «Видалити репозиторій» (вимкнена, поки з нього деплояться стеки); екран середовища (екран Бояра В. О., кнопку реалізував Петренко Б. О.): «Видалити середовище» (лише адміністраторам, вимкнена, поки є стеки).
  - ключі: посилання «Ключі» в шапці, список «Середовище», кнопка «Новий ключ» (з підтвердженням, якщо ключ уже є), у рядку — «Копіювати» й «Видалити», «Копіювати .sops.yaml».
  - API-токени (ADR-0007): посилання «API-токени» в бічній панелі, кнопка «Новий токен»; діалог (`src/components/tokens/create-token-dialog.tsx`): поле назви, список «Строк дії», прапорці «Лише читання», «Розшифрування секретів» (ADR-0010; вимкнений для токена лише для читання), «Усі середовища» й прапорці середовищ, «Скасувати», «Створити токен», після створення — «Копіювати токен» і «Готово»; «Відкликати» в рядку (`token-table.tsx`) і в картці токенів користувача (`user-tokens-card.tsx`, на екрані користувача Денисенко Л. В.).
  - вхід у CLI (ADR-0008): поле назви токена й поля обмежень (`src/components/tokens/token-options-fields.tsx`, спільні з діалогом токена: «Строк дії», «Лише читання», «Розшифрування секретів» (ADR-0010; позначений, якщо CLI попросив `--secrets`), «Усі середовища», прапорці середовищ), «Дозволити», «Відхилити», посилання «Мої API-токени» й «Ввести код вручну»; поле коду й «Далі» на `/cli/device`.
  - passkeys (ADR-0009): кнопка «Увійти з passkey» на екрані входу (екран Денисенко Л. В.); розділ «Passkeys» в «Обліковому записі» (`src/components/passkeys/passkeys-card.tsx`): поле назви, «Додати passkey», у рядку «Перейменувати» й «Видалити» (`passkey-table.tsx`), діалог перейменування: поле назви, «Скасувати», «Зберегти»; «Видалити» в картці passkeys користувача для адміністратора (`user-passkeys-card.tsx`).
  - Discord OAuth2 (ADR-0009): кнопка «Увійти через Discord» на екрані входу (екран Денисенко Л. В.), «Прив'язати через Discord» на екрані прив'язки (екран Синенка Д. В.); обидві — навігації браузера на ЦА, без обробників.
- **Контейнерні елементи:** таблиця passkeys (ADR-0009; в обліковому записі й у картці користувача), таблиця API-токенів (ADR-0007; і на екрані, і в картці користувача), таблиця стеків, таблиця деплоїв, таблиця репозиторіїв, таблиця підключень (ADR-0006), таблиця ключів середовища; список останніх деплоїв стеку, рядки логу деплою.
- **Обробники подій (51):**
  - стеки: вибір середовища (у URL через `validateSearch`), відкриття й закриття діалогу (скидання форми й помилки), вибір репозиторію, надсилання форми (перехід на сторінку стеку), «Оновити»;
  - стек: «Деплой» і «Деплой з оновленням образів» (перехід на сторінку деплою), підтвердження перезапуску й down (вивід compose під кнопками), вибір репозиторію, надсилання джерела, «Скасувати зміни»;
  - історія: вибір стану (у URL), «Оновити»;
  - деплой: прапорці фільтра й стеження;
  - репозиторії: введення в пошук (у URL), відкриття й закриття діалогу, вибір підключення й введення адреси (на льоту: адреса поза префіксом підключення блокує збереження, ADR-0006), надсилання форми; відкриття й закриття діалогу зміни (форма й мутації скидаються), збереження; створення секрету webhook (з підтвердженням, якщо він уже є; секрет — лише в стані компонента, мутація скидається), підтвердження вимкнення webhook (PR #65);
  - підключення (ADR-0006): відкриття й закриття діалогу створення (форма й мутація з токеном скидаються), прапорець «З токеном доступу», прапорець «Усі середовища» (ховає список), прапорець середовища, надсилання форми; відкриття й закриття діалогу зміни, збереження, збереження токена (форма й мутація очищаються), підтвердження видалення токена, створення й вимкнення webhook організації, підтвердження видалення підключення;
  - стек: перемикач «Деплоїти після push» (PR #65); створення токена хука деплою (з підтвердженням, якщо він уже є; токен — лише в стані компонента, мутація скидається), підтвердження вимкнення хука; перемикач «Перезапускати після зміни файлів»; підтвердження видалення стеку (перехід до списку стеків, потім кеш стеку забувається); підтвердження видалення репозиторію (діалог закривається); підтвердження видалення середовища (перехід до списку середовищ, потім кеш забувається).
  - ключі: вибір середовища (у URL; без нього — перше видиме), «Новий ключ» (скидає помилку видалення), підтвердження видалення ключа.
  - API-токени (ADR-0007): відкриття й закриття діалогу (токен, форма й мутація скидаються), вибір строку дії, прапорці «Лише читання» (знімає «Розшифрування секретів»), «Розшифрування секретів» (ADR-0010), «Усі середовища» (ховає список) і середовища, надсилання форми (токен — лише в стані компонента, мутація скидається одразу), підтвердження відкликання власного токена й токена користувача адміністратором.
  - вхід у CLI (ADR-0008): «Дозволити» (перевірка форми, потім браузер іде на `127.0.0.1:<порт>`, якщо CLI його дав), «Відхилити», вибір строку, прапорці «Лише читання», «Розшифрування секретів» (ADR-0010), «Усі середовища» і середовища; введення коду й «Далі» (нормалізація й перевірка коду, перехід на сторінку підтвердження).
  - passkeys (ADR-0009): «Увійти з passkey» (скасовує церемонію автозаповнення, потім діалог браузера; після входу — перехід далі), церемонія автозаповнення при показі екрана входу (ключі в підказках поля імені), «Додати passkey» (перевірка назви, діалог браузера, скасування не показується як помилка), відкриття, збереження й закриття діалогу перейменування, підтвердження видалення власного ключа й ключа користувача адміністратором.
- **Логіка окремо від GUI:**
  - запити й кеш — `src/api/queries.ts` (`repositoriesQuery`, `gitConnectionsQuery` (ADR-0006), `stacksQuery`, `stackQuery`, `deploymentsQuery`, `deploymentQuery`), мутації з інвалідацією — `src/api/stacks.ts` і `src/api/git-connections.ts`;
  - підключення (ADR-0006): `src/features/git-connections/model.ts` — схеми форм, правила адреси як у ЦА (`parseGitUrl`, `coversUrl`), адреса webhook організації; у `src/features/stacks/model.ts` — `prefixMissedBy` (адреса поза префіксом обраного підключення);
  - правила форм (Zod, як у ЦА: назва стеку, шлях compose-файлу, https-адреса без облікових даних і %-кодування; токен без пробілів, ім'я користувача без двокрапки — для підключень), адреса webhook (`webhookUrl`), стани деплою, тривалість, короткий коміт, помилки ЦА за статусом → перекладені повідомлення — `src/features/stacks/model.ts`;
  - live-оновлення (PR #63): `src/features/live/model.ts` — типи подій хабу, які запити кожна подія робить застарілими, backoff перепідключення, опитування лише без живого з'єднання; з'єднання — `HubProvider` (`src/components/live/hub-provider.tsx`), стан у шапці — `LiveIndicator`; екрани деплоїв не опитують ЦА, поки з'єднання живе;
  - ключі: `src/features/keys/model.ts` — поточний ключ (найновіший), правило `.sops.yaml` для середовища з місцем для власного ключа, команди sops; запит `ageKeysQuery`, мутації — `src/api/keys.ts`;
  - незавершений деплой опитується кожні 2 с (`refetchInterval`), доки не завершиться;
  - API-токени (ADR-0007): `src/features/tokens/model.ts` — схема форми, строк дії → дата, запит, стан токена (діє, скоро спливе, прострочений), назви середовищ обмеженого токена, правило «лише читання не розшифровує секрети» (ADR-0010); запити `apiTokensQuery`, `userApiTokensQuery`, мутації — `src/api/tokens.ts`.
  - вхід у CLI (ADR-0008): `src/features/cli-login/model.ts` — нормалізація й формат коду, схема коду, search params сторінки, адреса loopback-сигналу (лише `127.0.0.1` і порт), назва токена за хостом; запит `cliAuthorizationQuery`, мутації — `src/api/cli-login.ts`.
  - passkeys (ADR-0009): `src/features/passkeys/model.ts` — підтримка браузером, церемонії `create`/`get` через вбудований WebAuthn JSON браузера, значення помилок `DOMException`, схема назви; запити `passkeysQuery`, `userPasskeysQuery`, `signInMethodsQuery`, мутації й вхід — `src/api/passkeys.ts`.

Інтернаціоналізація панелі (PR #36): `src/i18n` (ініціалізація, вибір мови, словники uk/en) і перемикач мови `LanguageSwitcher` — елемент керування з обробником натискання (`src/components/language-switcher.tsx`), у шапці й на екрані входу. Тексти екранів інших учасників перенесено в словники; екрани й елементи лишаються за їхніми авторами.

Каркас панелі `src/Web` (PR #23): Vite, маршрутизація й шар даних, клієнт API з OpenAPI, shadcn/ui на Tailwind CSS, образ nginx, job у CI. Це спільна інфраструктура, а не екрани, тому в облік лаби 2 не входить; згенеровані компоненти `src/components/ui` не зараховуються нікому.

### Лаба 3

| Бібліотека | Що використано | Ретроспектива |
|---|---|---|
| gRPC для .NET (`Grpc.AspNetCore`, `Grpc.Tools`, `Google.Protobuf`), почато в PR #6 | Генерація коду з `.proto` окремо для сервера й клієнта; двонапрямлений стрім `Connect`; автентифікація через metadata; коди статусу (`Unauthenticated`, `InvalidArgument`, `DeadlineExceeded`); `Grpc.Net.Client` з `TestServer` в інтеграційних тестах | [docs/retro/grpc.md](docs/retro/grpc.md) |
| EF Core + SQLite, почато в PR #6 | Мапінг доменних класів без окремих моделей; міграції й застосування при старті; конвертери значень для ID і value objects; конвенції (`ConfigureConventions`); TPH-успадкування з дискримінатором; фільтровані унікальні індекси; колація `NOCASE`; owned-колекція (`OwnsMany`) для логу деплою; проєкції без завантаження логу | [docs/retro/ef-core.md](docs/retro/ef-core.md) |
| LibGit2Sharp, почато в PR #13 | Bare-клон як кеш; fetch однієї гілки з refspec і `Prune`; читання файлів із дерева коміту без checkout (`Tree`, `Blob`, `Mode`); скасування через колбек прогресу; HTTPS-автентифікація токеном через `CredentialsProvider` і `UsernamePasswordCredentials` з перевіркою хоста запиту (PR #60), а з ADR-0006 — префікса шляху підключення | [docs/retro/libgit2sharp.md](docs/retro/libgit2sharp.md) |
| YamlDotNet, почато в PR #13 | Дерево вузлів `YamlStream` / `YamlMappingNode` замість типізованої моделі; якорі й merge-ключі; позиція помилки з `YamlException.Start`; `.sops.yaml` (ADR-0010): `YamlSequenceNode` правил, значення рядком або списком | [docs/retro/yamldotnet.md](docs/retro/yamldotnet.md) |
| TanStack Router, почато в PR #23 | Маршрути з файлів з генерацією дерева; pathless layout `_authed` з `beforeLoad` і redirect на 401; контекст роутера; `validateSearch` зі схемою Zod | [docs/retro/tanstack-router.md](docs/retro/tanstack-router.md) |
| TanStack Query, почато в PR #23 | `queryOptions`-фабрики як єдине джерело ключів; `ensureQueryData` у `beforeLoad`; політика повторів без 401 | [docs/retro/tanstack-query.md](docs/retro/tanstack-query.md) |
| i18next (`i18next`, `react-i18next`, `i18next-browser-languagedetector`), PR #36 | Вбудовані словники з типізованими ключами (`CustomTypeOptions`); `useTranslation`; детектор мови з `localStorage` і браузера; запасна мова; `changeLanguage` і подія `languageChanged` | [docs/retro/i18next.md](docs/retro/i18next.md) |
| SignalR, клієнт `@microsoft/signalr` (PR #63; сервер — частина ASP.NET Core) | `HubConnectionBuilder` з `withAutomaticReconnect` і власною політикою затримок; підписка на події (`on`) і виклик методу хабу (`invoke("Resubscribe")`); `onreconnecting` / `onreconnected` / `onclose` для стану з'єднання; `HttpError` для 401/403; на сервері — типізований `Hub<ILiveClient>`, групи, `IHubContext`, `RequireAuthorization` і `CloseOnAuthenticationExpiration`; .NET-клієнт з long polling в інтеграційних тестах | [docs/retro/signalr.md](docs/retro/signalr.md) |
| ASP.NET Core Identity (passkeys), ADR-0009 | WebAuthn Identity .NET 10: схема версії 3 (`AspNetUserPasskeys`), `MakePasskeyCreationOptionsAsync`, `MakePasskeyRequestOptionsAsync` (discoverable), `PerformPasskeyAttestationAsync`, `PerformPasskeyAssertionAsync`, `AddOrUpdatePasskeyAsync`, `GetPasskeysAsync`, `RemovePasskeyAsync`; стан церемонії в cookie `TwoFactorUserId` | [docs/retro/aspnet-identity.md](docs/retro/aspnet-identity.md) |
| AspNet.Security.OAuth.Discord 10.0.0, ADR-0009 | `AddDiscord` поверх `OAuthHandler`: scope `identify`, PKCE, `SignInScheme` зовнішньої cookie, налаштування correlation cookie, `Events.OnRemoteFailure`, `AuthenticationProperties.Items` у захищеному `state`; `BackchannelHttpHandler` у тестах | [docs/retro/aspnet-security-oauth-discord.md](docs/retro/aspnet-security-oauth-discord.md) |
| Kiota (`Microsoft.Kiota.Bundle`, інструмент `microsoft.openapi.kiota`), ADR-0008 | Генерація клієнта C# із закоміченого OpenAPI 3.1 з виключенням шляхів; перевірка актуальності в CI; `HttpClientRequestAdapter` з власним `IAuthenticationProvider` (Bearer лише для свого сервера); ланцюжок `DelegatingHandler` без `RetryHandler` і з власним обробником problem details; `KiotaJsonSerializer` для `-o json`; сумісність з NativeAOT | [docs/retro/kiota.md](docs/retro/kiota.md) |
| System.CommandLine 2.0, ADR-0008 | Дерево команд, типізовані аргументи й опції (`Guid`, `int?`, `DateTimeOffset?`, enum), `Required`, `AcceptOnlyFromAmong`, рекурсивні глобальні опції, `SetAction` зі скасуванням, `InvocationConfiguration` для тестів, автоматична довідка й `--version` | [docs/retro/system-commandline.md](docs/retro/system-commandline.md) |

## Бояр Володимир Олександрович (@volodaboyar1-star)

Зона: агент (обробники команд `IAgentCommandHandler`, робота з Docker і Docker Compose); у ЦА — креди container registry та їх шифрування, операції restart і down стеку, у Discord-боті — команди стеків.

### Лаба 1

| Вимога | Потрібно | Є | Стан |
|---|---|---|---|
| Класи та подібні типи | ≥ 9 | 102 (89 класів, з них 8 статичних і 13 record; 9 інтерфейсів; 1 структура; 3 enum) | ✅ |
| Поля | ≥ 15 | 220 (171 поле стану, 49 констант і статичних полів) | ✅ |
| Нетривіальні методи | ≥ 25 | 157 | ✅ |
| Ієрархії успадкування | ≥ 2, одна з ≥ 3 класів | 4 (11 класів на трьох рівнях; 14 класів; 3 класи; 3 класи) | ✅ |
| Поліморфізм | ≥ 3, серед них статичний і динамічний | 12 з клієнтським кодом (11 динамічних, 1 статичний) | ✅ |

Код: `src/Agent` (PR #10; обробники стеків — PR #21; скасування команд — PR #24, змерджено як #26); у ЦА — креди registry (PR #21), restart і down стеку в ЦА й Discord-боті (PR #37), API панелі для середовищ і контейнерів (PR #42); читання логів контейнера на агенті (`StreamContainerLogs`, PR #49).

Не враховано: `Program.cs`, згенерований з proto код і міграції, тимчасові `DevRegistryEndpoints` і ендпоінт restart/down у `DevStackEndpoints` (прибрані PR #45 і #48, замість них — API панелі); `EnvironmentResponse` і `ContainerResponse` в `Api/Contracts/EnvironmentContracts.cs` (REST DTO за рішенням команди зараховуються Петренку Б. О.).

#### Класи та подібні типи

| Простір імен `ContainerForest.Agent.*` | Типи |
|---|---|
| — | `AgentInfo` (статичний клас), `FatalAgentException`, `DependencyInjection` (статичний клас) |
| `Configuration` | `AgentOptions` |
| `Enrollment` | `AgentCredentials` (record), `CredentialStore`, `AgentEnroller` |
| `Platform` | `IBootIdProvider` (інтерфейс), `LinuxBootIdProvider`, `UptimeBootIdProvider` |
| `Docker` | `IDockerGateway` (інтерфейс), `DockerDotNetGateway`, `ContainerInfoMapper` (статичний клас), `ContainerLogParser`, `ContainerLogParser.LineBuffer` |
| `Commands` | `IAgentCommandHandler`, `ICommandProgress`, `ILogSink` (інтерфейси), `AgentCommandHandler<TCommand>` (абстрактний клас), `ContainerActionHandler<TCommand>` (абстрактний клас), `ContainerIds` (статичний клас), `ListContainersHandler`, `StartContainerHandler`, `StopContainerHandler`, `RestartContainerHandler`, `StreamContainerLogsHandler`, `LogBatcher`, `StackCommandHandler<TCommand>` (абстрактний клас), `DeployStackHandler`, `RestartStackHandler`, `DownStackHandler`, `StopTimeout` (статичний клас), `CommandDispatcher`, `RunningCommands`, `RunningCommands.RunningCommand`, `AgentCommandException` (абстрактний клас), `InvalidCommandArgumentException`, `ContainerNotFoundException`, `DockerUnavailableException`, `DockerCommandFailedException`, `DockerTimeoutException`, `UnsupportedCommandException`, `UnsupportedOptionException`, `StackNotFoundException`, `StackBusyException`, `ComposeUnavailableException`, `ComposeFailedException`, `ComposeTimeoutException`, `StackFileConflictException` |
| `Stacks` | `StackName` (статичний клас), `StackLayout`, `StackLayoutFile` (record), `StackLocks`, `StackLocks.Lease`, `IStackFileStore` (інтерфейс), `StackFileStore`, `StackFileStore.PlannedFile` (record), `StackManifest` (record), `StackManifestEntry` (record) |
| `Compose` | `ComposeInvocation`, `IComposeRunner` (інтерфейс), `DockerComposeCli`, `RegistryConfigWriter`, `TemporaryRegistryConfig` |
| `Connection` | `AgentStreamWriter`, `StreamCommandProgress`, `ReconnectBackoff`, `AgentSessionRunner`, `AgentWorker` |

| Простір імен `ContainerForest.Aggregator.*` | Типи |
|---|---|
| `Domain.Common` | `RegistryCredentialId` (структура) |
| `Domain.Deployments` | `RegistryHost` (статичний клас) |
| `Domain.Environments` | `RegistryCredential` |
| `Application.Common` | `ISecretProtector` (інтерфейс), `SecretPurpose` (enum), `SecretUnreadableException` |
| `Application.Registries` | `IRegistryCredentialRepository` (інтерфейс), `RegistryCredentialSummary` (record), `RegistryCredentialService` |
| `Application.Agents` | `RegistryLogin` (record) |
| `Application.Deployments.Steps` | `ResolveRegistryAuthStep` |
| `Application.Stacks` | `StackCommand` (абстрактний клас), `RestartStackCommand`, `DownStackCommand`, `StackCommandResult` (record), `StackAction` (enum), `StackCommandService` |
| `Infrastructure.Security` | `SecretProtectionOptions`, `DataProtectionSecretProtector` |
| `Infrastructure.Persistence` | `RegistryCredentialConfiguration`, `RegistryCredentialRepository` |
| `Api.Endpoints` | `EnvironmentEndpoints` (статичний клас) |

| Простір імен `ContainerForest.DiscordBot.*` | Типи |
|---|---|
| `Forest` | `ForestStack`, `ForestStackResult` (record), `StackAction` (enum) |
| `Commands` | `StackList`, `StackActionArgs`, `StackActionOutcome` (record), `StacksCommand`, `StackActionCommand` (абстрактний клас), `RestartStackCommand`, `DownStackCommand` |
| `Interactions` | `StackAutocompleteHandler` |

#### Поля

Поля, що зберігаються з параметрів первинного конструктора, враховано як поля стану.

| Тип | Поля стану | Константи та статичні поля |
|---|---|---|
| `AgentOptions` | `ControllerUrl`, `EnrollmentToken`, `DataDirectory`, `StacksDirectory`, `SecretsDirectory`, `DockerExecutable`, `ComposeTimeout`, `HeartbeatInterval` | `SectionName` |
| `AgentCredentials` | `AgentId`, `AgentSecret` | |
| `CredentialStore` | `options` | `FileName`, `Json` |
| `AgentEnroller` | `store`, `options`, `logger` | |
| `AgentInfo` | | `Version` |
| `LinuxBootIdProvider` | `path` | `DefaultPath` |
| `UptimeBootIdProvider` | `time`, `uptime` | |
| `AgentCommandException` та підкласи | `Code`; `ContainerNotFoundException.ContainerId`; `DockerTimeoutException.Timeout`; `StackNotFoundException.StackName`; `StackBusyException.StackName`; `ComposeFailedException.ExitCode`; `ComposeTimeoutException.Timeout`; `StackFileConflictException.Path` | |
| `DockerDotNetGateway` | `client` | `RequestTimeout` |
| `ContainerInfoMapper` | | `ProjectLabel`, `ServiceLabel`, `SystemLabel` |
| `AgentCommandHandler<TCommand>` | `logger` | `CommandField` |
| `ContainerActionHandler<TCommand>` | `Docker` | |
| `ContainerIds` | | `MaxLength` |
| `ListContainersHandler` | `docker` | |
| `StreamContainerLogsHandler` | `docker`, `time` | |
| `LogBatcher` | `sink`, `time`, `lines`, `bytes`, `firstLineAt`, `SentLines` | `MaxLines`, `MaxBytes`, `MaxDelay` |
| `ContainerLogParser` | `stdout`, `stderr` | `MaxLineBytes`, `TruncationMark`, `MaxTimestampLength`, `TimestampFormats`, `Utf8` |
| `ContainerLogParser.LineBuffer` | `bytes`, `length`, `truncated` | |
| `StackCommandHandler<TCommand>` | `locks` | |
| `DeployStackHandler` | `store`, `compose`, `registryConfigs` | |
| `RestartStackHandler`, `DownStackHandler` | `store`, `compose`, `options` у кожному | |
| `StackName` | | `MaxLength` |
| `StackLayout` | `StackName`, `Files` | `ComposeFileName`, `MaxPathLength`, `RegularMode`, `ExecutableMode` |
| `StackLayoutFile` | `Path`, `Content`, `Mode`, `Secret` | |
| `StackLocks` | `held` | |
| `StackFileStore` | `options`, `logger` | `OwnerOnlyDirectory`, `PathComparer` |
| `StackFileStore.PlannedFile` | `Source`, `Target`, `Staged`, `Secret` | |
| `StackManifest` | `Files` | `Json` |
| `StackManifestEntry` | `Path`, `Secret` | |
| `StackLocks.Lease` | `owner`, `stackName`, `disposed` | |
| `ComposeInvocation` | `Operation`, `Arguments`, `WorkingDirectory`, `Environment` | `NoEnvironment` |
| `DockerComposeCli` | `options`, `logger` | `MaxLineLength`, `AgentVariablePrefix` |
| `RegistryConfigWriter` | `options`, `logger` | `DockerConfigVariable`, `ParentName`, `OwnerOnlyDirectory` |
| `TemporaryRegistryConfig` | `Directory`, `logger` | |
| `CommandDispatcher` | `handlers`, `logger`, `running` | |
| `RunningCommands` | `running` | |
| `RunningCommands.RunningCommand` | `owner`, `requestId`, `source` | |
| `AgentStreamWriter` | `stream`, `writeLock` | |
| `StreamCommandProgress` | `requestId`, `writer` | |
| `ReconnectBackoff` | `random`, `Attempts` | `BaseDelay`, `MaxDelay`, `StableSession`, `MaxExponent` |
| `AgentSessionRunner` | `dispatcher`, `docker`, `bootIds`, `options`, `time`, `logger` | `AgentIdHeader`, `AgentSecretHeader` |
| `AgentWorker` | `enroller`, `credentialStore`, `runner`, `options`, `lifetime`, `time`, `logger`, `backoff` | |
| ЦА: `RegistryCredentialId` | `Value` | |
| ЦА: `RegistryHost` | | `DockerHub`, `DockerHubConfigKey` |
| ЦА: `RegistryCredential` | `EnvironmentId`, `Server`, `Username`, `ProtectedPassword`, `CreatedAt`, `UpdatedAt` | `MaxUsernameLength` |
| ЦА: `SecretUnreadableException` | `Purpose` | |
| ЦА: `RegistryCredentialSummary` | `Id`, `Server`, `Username`, `CreatedAt`, `UpdatedAt` | |
| ЦА: `RegistryCredentialService` | `currentActor`, `environments`, `credentials`, `protector`, `unitOfWork`, `time` | |
| ЦА: `ResolveRegistryAuthStep` | `credentials`, `protector` | |
| ЦА: `RegistryLogin` | `Server`, `Username`, `Password` | |
| ЦА: `SecretProtectionOptions` | `KeyDirectory` | `SectionName` |
| ЦА: `DataProtectionSecretProtector` | `provider` | |
| ЦА: `StackCommand` | `agents`, `StopTimeout` | `DefaultStopTimeout`, `MaxStopTimeout`, `MaxOutputLines` |
| ЦА: `StackCommandResult` | `Error`, `Output` | |
| ЦА: `StackCommandService` | `currentActor`, `environments`, `stacks`, `roleAssignments`, `deployments`, `unitOfWork`, `agents` | |
| Бот: `ForestStack` | `Id`, `Name`, `IsSystem`, `DeployedCommit`, `DeployedAt`, `CanRestart`, `CanTakeDown` | |
| Бот: `ForestStackResult` | `Error`, `Output` | |
| Бот: `StackList`, `StackActionOutcome` | `Environment`, `Stacks`; `Environment`, `Result` | |
| Бот: `StackActionArgs` | `Environment`, `Stack`, `TimeoutSeconds` | |
| Бот: `StacksCommand`, `StackActionCommand` | `forest`, `environments` у кожному | |
| Бот: `StackAutocompleteHandler` | `source` | |
| ЦА: поля, додані в класи Петренка Б. О. | `DeployContext.compose`, `StackBundle.RegistryLogins`, `AgentGatewayOptions.StackCommandTimeout`, `StackSummary.CanRestart` | `AgentSession.CancelWriteTimeout` |
| Бот: поля, додані в класи Синенка Д. В. | `BotOptions.StackActionTimeout` | `ForestModule.StackDescription`, `StackTimeoutDescription` |

#### Нетривіальні методи (157)

| Тип | Методи |
|---|---|
| `AgentOptions` (2) | `DefaultDataDirectory` (залежить від ОС), `IsValid` |
| `AgentCredentials` (1) | `ToString` (не виводить секрет) |
| `CredentialStore` (2) | `LoadAsync` (перевірка вмісту), `SaveAsync` (атомарний запис, права 0600 від створення) |
| `AgentEnroller` (1) | `GetCredentialsAsync` |
| `AgentInfo` (1) | `ReadVersion` |
| `LinuxBootIdProvider`, `UptimeBootIdProvider` (2) | `GetBootId` у кожному |
| `AgentCommandException` (1) | `ToCommandError` |
| `DockerDotNetGateway` (9) | `GetVersionAsync`, `ListContainersAsync`, `StartContainerAsync`, `StopContainerAsync`, `RestartContainerAsync`, `ReadContainerLogsAsync` (бюджет лише на відкриття, читання до кінця потоку з розбором), `FormatSince` (межа з мілісекундами), `CallAsync` (бюджет часу й трансляція помилок Docker.DotNet), `DescribeApiError` |
| `ContainerLogParser` (7) | `Append` (окремий незавершений рядок для stdout і stderr), `Flush`, `ParseLine` (час, `
` TTY, обрізання), `TryParseTimestamp` (наносекунди), `WholeCharacters` (обрізання без половини символу UTF-8), `LineBuffer.Write` (ліміт 16 КБ), `LineBuffer.TakeLine` |
| `ContainerInfoMapper` (5) | `ToContainerInfo`, `ToState`, `ToName`, `Label`, `ToUnixMilliseconds` |
| `AgentCommandHandler<TCommand>` (1) | `HandleAsync` (шаблонний метод) |
| `ContainerActionHandler<TCommand>` (1) | `ExecuteAsync` |
| `ContainerIds` (1) | `Validate` |
| `ListContainersHandler` (1) | `ExecuteAsync` |
| `StreamContainerLogsHandler` (1) | `ExecuteAsync` (перевірки до звернення до Docker, `follow` не підтримується, вміст логів не логується) |
| `LogBatcher` (2) | `AddAsync` (пачка за кількістю, розміром або часом), `FlushAsync` |
| `StopContainerHandler`, `RestartContainerHandler` (2) | `RunAsync` у кожному |
| `StackCommandHandler<TCommand>` (1) | `ExecuteAsync` (перевірка імені, один запуск на стек) |
| `DeployStackHandler` (3) | `RunAsync` (усі перевірки до першого запису, секрети після успішного up), `ValidateEnvironment`, `Describe` |
| `RestartStackHandler`, `DownStackHandler` (2) | `RunAsync` у кожному |
| `StopTimeout` (1) | `FromSeconds` |
| `StackName` (1) | `Validate` |
| `StackLayout` (4) | `Create` (дублікати, файл і каталог з одним шляхом, compose.yaml), `ResolveIn` (перевірка через `Path.GetFullPath`), `ValidatePath`, `ToMode` |
| `StackLocks` (2) | `TryAcquire`, `Lease.Dispose` (одноразове звільнення) |
| `StackFileStore` (12) | `WriteAsync` (порядок кроків, проміжний маніфест), `CheckTarget`, `LinkedOrFileAncestor` (посилання, підкладені контейнером), `StageAsync` (секрети на tmpfs і посилання), `WriteFileAsync` (режим незалежно від umask), `BackUpAsync`, `RemoveStale`, `TryResolveOwnFile`, `RemoveEmptyParents`, `ReadManifestAsync`, `ReplaceDirectory`, `DeleteSecrets` |
| `StackManifest` (4) | `Of`, `Union`, `ReadAsync`, `WriteAsync` (атомарний запис) |
| `ComposeInvocation` (4) | `Up`, `Restart`, `Down`, `WithEnvironment` |
| `DockerComposeCli` (5) | `RunAsync` (тайм-аут, скасування, код виходу), `CreateStartInfo` (`ArgumentList`, фільтр `CF_*`), `PumpAsync`, `Kill`, `DrainAsync` |
| `RegistryConfigWriter` (4) | `WriteAsync`, `Validate`, `WriteConfigAsync` (формат `docker login`, 0600), `ParentDirectory` |
| `TemporaryRegistryConfig` (1) | `Dispose` |
| `CommandDispatcher` (3) | конструктор (таблиця обробників, перевірка дублікатів), `DispatchAsync` (реєстрація й скасування команд), `Unsupported` |
| `RunningCommands` (3) | `Begin` (відмова для дубліката), `Cancel` (гонка із завершенням), `RunningCommand.Dispose` (видаляє лише свою реєстрацію) |
| `AgentStreamWriter` (1) | `WriteAsync` |
| `StreamCommandProgress` (2) | `ReportAsync`, `SendAsync` (`LogBatch` з request_id) |
| `ReconnectBackoff` (2) | `NextDelay`, `SessionEnded` |
| `AgentSessionRunner` (5) | `RunAsync`, `CreateHelloAsync`, `SendHeartbeatsAsync`, `RunCommandAsync`, `IsSessionCancellation` |
| `AgentWorker` (5) | `ExecuteAsync`, `EnrollAsync`, `KeepConnectedAsync`, `CreateChannel`, `Describe` |
| `DependencyInjection` (1) | `AddAgent` |
| ЦА: `RegistryHost` (4) | `Of` (правило Docker для хоста образу), `TryParse`, `ConfigKey`, `Normalize` |
| ЦА: `RegistryCredential` (6) | `Create`, `ParseServer`, `ChangeLogin`, `Serves`, `ValidateUsername`, `ValidatePassword` |
| ЦА: `RegistryCredentialService` (3) | `ListAsync`, `SetAsync` (створення або заміна логіну), `DeleteAsync` |
| ЦА: `ResolveRegistryAuthStep` (3) | `ExecuteAsync`, `Select` (лише потрібні образам креди), `Decrypt` |
| ЦА: `RegistryLogin` (1) | `ToString` (не виводить пароль) |
| ЦА: `DataProtectionSecretProtector` (2) | `Unprotect` (переклад `CryptographicException`), `Protector` (окремий purpose) |
| ЦА: `RegistryCredentialConfiguration` (1) | `Configure` |
| ЦА: `RegistryCredentialRepository` (2) | `ListByEnvironmentAsync`, `FindByServerAsync` |
| ЦА: `StackCommand` (3) | `ExecuteAsync` (заборони для стеку, вивід compose і помилка compose як результат, без скасування викликачем), `Collect` (останні рядки виводу), `ValidateTimeout` |
| ЦА: `EnvironmentEndpoints` (3) | `MapEnvironmentEndpoints`, `MapContainerAction` (окремий ендпоінт на дію, тайм-аут зупинки лише для stop і restart), `RunAsync` |
| ЦА: `StackCommandService` (4) | `ExecuteAsync` за id і `ExecuteAsync` за назвою в середовищі (видимість, однакова відповідь для відсутнього й невидимого стеку), `RunAsync` (роль для операції, незавершений деплой, коміт забувається лише після успішного down), `Create` |
| ЦА: методи, додані в класи Петренка Б. О. (6) | `Stack.RecordTakenDown` (заборонено для системного стеку); `AgentSession.CancelOnAgentAsync`; `GrpcAgentChannel.RestartStackAsync`, `DownStackAsync`; `BotGatewayService.ListStacks` (дії без системних стеків), `ControlStack` |
| Бот: `StacksCommand` (1) | `CallForestAsync` (середовище за назвою чи id, стеки) |
| Бот: `StackActionCommand` (2) | `CallForestAsync`, `Format` (помилка compose як відповідь-помилка з виводом) |
| Бот: `StackAutocompleteHandler` (1) | `GenerateSuggestionsAsync` (дія з назви команди) |
| Бот: методи, додані в класи Синенка Д. В. (9) | `GrpcForestClient.ListStacksAsync`, `ControlStackAsync`, `ToStack`; `ReplyFormatter.Stacks`, `StackLine`, `WithOutput` (кінець виводу в блоці коду в межах 2000 символів, захист від закриття блоку); `AutocompleteSource.StacksAsync`, `SuggestStacks`, `Allows` |

Не враховано як тривіальні: `ContainerIdOf` і `StackNameOf` в обробниках, `StartContainerHandler.RunAsync` і `ContainerActionHandler.ToStopTimeout` (лише делегують), `ReconnectBackoff.Reset`, `DockerDotNetGateway.Seconds` і `Dispose`, `AgentStreamWriter.Dispose`, `StackLocks.IsHeld`, `RunningCommands.Count`, `CommandDispatcher.RunningCount`, `StackFileStore.Exists`, `DiscardPreviousSecrets`, `DeleteDirectory` і шляхи (`StackDirectory`, `SecretsDirectory`, `ManifestPath`), `DataProtectionSecretProtector.Protect`, `RegistryCredentialSummary.Of`, `SendAsync` у `RestartStackCommand` і `DownStackCommand` (лише делегують), `StackCommandResult.Succeeded`, `EnvironmentEndpoints.WithAgentProblems` (лише перелік відповідей), `ForestStackResult.Succeeded`, `StacksCommand.Format`, перевизначення `Name`, `Action`, `Done` у командах стеків бота, методи `/stacks`, `/stack-restart`, `/stack-down` у `ForestModule` (лише передають опції), властивості `Handles`, `FilePath`, `SupportedCommands`, `StacksRoot`, `RegularFiles`, `SecretFiles`, `DeployContext.Compose`.

#### Ієрархії успадкування

1. `AgentCommandHandler<TCommand>` → `ListContainersHandler`, `StreamContainerLogsHandler`, `ContainerActionHandler<TCommand>` → `StartContainerHandler`, `StopContainerHandler`, `RestartContainerHandler`, `StackCommandHandler<TCommand>` → `DeployStackHandler`, `RestartStackHandler`, `DownStackHandler`. Це 11 класів на трьох рівнях (плюс інтерфейс `IAgentCommandHandler`).
2. `AgentCommandException` → `InvalidCommandArgumentException`, `ContainerNotFoundException`, `DockerUnavailableException`, `DockerCommandFailedException`, `DockerTimeoutException`, `UnsupportedCommandException`, `UnsupportedOptionException`, `StackNotFoundException`, `StackBusyException`, `ComposeUnavailableException`, `ComposeFailedException`, `ComposeTimeoutException`, `StackFileConflictException`. Це 14 класів.
3. ЦА: `StackCommand` → `RestartStackCommand`, `DownStackCommand`. Це 3 класи.
4. Бот: `StackActionCommand` → `RestartStackCommand`, `DownStackCommand` (під `BotCommand<TArgs, TResult>` Синенка Д. В., як і `StacksCommand`). Це 3 класи.

#### Поліморфізм

| # | Вид | Поліморфний інтерфейс | Клієнтський код |
|---|---|---|---|
| 1 | Динамічний (інтерфейс) | `IAgentCommandHandler.HandleAsync` | `CommandDispatcher.DispatchAsync` виконує будь-яку команду через таблицю обробників, не знаючи їхніх типів ✅ |
| 2 | Статичний (generics) | `AgentCommandHandler<TCommand>`: поле `oneof` і тип команди визначаються з `TCommand` | `HandleAsync` один раз реалізує витяг команди, `request_id` і перетворення помилок для команд контейнерів і стеків ✅ |
| 3 | Динамічний (шаблонний метод) | `ContainerActionHandler<TCommand>.ContainerIdOf`, `RunAsync` | `ContainerActionHandler.ExecuteAsync` однаково перевіряє id контейнера для старту, зупинки й перезапуску ✅ |
| 4 | Динамічний (стратегія) | `IBootIdProvider`: `LinuxBootIdProvider`, `UptimeBootIdProvider` | `AgentSessionRunner.CreateHelloAsync` бере boot_id, не знаючи ОС ✅ |
| 5 | Динамічний (адаптер) | `IDockerGateway`: `DockerDotNetGateway` (і фейк у тестах) | обробники команд і `AgentSessionRunner` працюють із Docker без залежності від Docker.DotNet ✅ |
| 6 | Динамічний (шаблонний метод) | `StackCommandHandler<TCommand>.StackNameOf`, `RunAsync` | `StackCommandHandler.ExecuteAsync` однаково перевіряє ім'я стеку й не пускає дві операції на один стек для deploy, restart і down ✅ |
| 7 | Динамічний (адаптер) | `IComposeRunner`: `DockerComposeCli` (і фейк у тестах) | обробники стеків запускають compose, не знаючи про процеси й CLI ✅ |
| 8 | Динамічний (адаптер) | ЦА: `ISecretProtector`: `DataProtectionSecretProtector` (і фейк у тестах) | `RegistryCredentialService` шифрує, а `ResolveRegistryAuthStep` розшифровує паролі без залежності від Data Protection ✅ |
| 9 | Динамічний (адаптер) | `IStackFileStore`: `StackFileStore` (і фейк у тестах) | обробники стеків пишуть файли й прибирають секрети, не знаючи про маніфест, tmpfs і посилання ✅ |
| 10 | Динамічний (шаблонний метод) | ЦА: `StackCommand.Operation`, `Name`, `SendAsync` | `StackCommandService.ExecuteAsync` перевіряє роль і запускає restart і down через базовий клас, а `StackCommand.ExecuteAsync` однаково перевіряє заборони для стеку й збирає вивід ✅ |
| 11 | Динамічний (шаблонний метод) | Бот: `StackActionCommand.Action`, `Done` | `BotCommand.ExecuteAsync` виконує `/stack-restart` і `/stack-down` через `CallForestAsync` і `Format` базового `StackActionCommand`, які беруть дію й текст відповіді з підкласу ✅ |
| 12 | Динамічний (адаптер) | `ILogSink`: `StreamCommandProgress` (і фейк у тестах) | `LogBatcher` і `StreamContainerLogsHandler` надсилають пачки рядків логу, не знаючи про gRPC-стрім і `AgentStreamWriter` ✅ |

#### Тести (не входять в облік)

- `tests/Agent`, 209 тестів (частина лише на Linux): зіставлення міток compose і станів контейнерів; обробники — `request_id` у результаті, коди помилок, передача таймауту зупинки, відхилення небезпечних id контейнерів; логи — пачки `LogBatch` з request_id перед результатом, `tail` і `since` доходять до Docker, `follow` — `UNSUPPORTED`, `NOT_FOUND`, `DOCKER_UNAVAILABLE` без деталей, скасування читання; пачки за кількістю, часом і розміром; розбір логів — час із наносекундами й зсувом, рядок без часу, окремі незавершені рядки stdout і stderr, символ UTF-8 на межі шматків, невалідні байти, `
` TTY, обрізання до 16 КБ без половини символу; межа `since` з мілісекундами; диспетчер — непідтримувані й невідомі команди, `CancelRequest` без результату; backoff — межі, jitter, скидання після довгої сесії; послідовний запис у стрім; сесія — облікові дані в metadata, Hello першим, відповіді на команди, heartbeat, `Unauthenticated`; збереження облікових даних і реєстрація; boot_id; стеки — ім'я стеку, шляхи поза каталогом стеку, дублікати, режими файлів, аргументи compose без `-f` для restart/down, `BUSY`, `NOT_FOUND`, секрети не потрапляють у прогрес і помилки, тимчасовий `DOCKER_CONFIG`; справжній процес (`/bin/sh` замість docker) — вивід обох потоків, код виходу, тайм-аут, скасування, фільтр `CF_*`; запис файлів — дані контейнерів лишаються, застарілі файли й порожні каталоги прибираються, `.prev`, режими 0644/0755, секрет лежить у каталозі секретів, а в каталозі стеку — посилання на нього, вміст секретів не потрапляє на диск, `.prev` секретів до успішного up, посилання після перезавантаження, підкладене контейнером посилання не використовується для запису й видалення; скасування — `CancelRequest` зупиняє команду з `ERROR_CODE_CANCELLED`, дублікат request_id, скасований деплой лишає `.prev` секретів і звільняє стек.
- `tests/Aggregator`: правило хоста registry для образів і нормалізація адрес, `RegistryCredential`, сервіс кредів (лише адміністратор, заміна логіну, ізоляція середовищ), крок `ResolveRegistryAuthStep` (лише потрібні креди, `${VAR}`, нерозшифровуваний пароль), Data Protection (перезапуск, пошкоджене значення, втрачений key ring), інтеграційний тест: креди через dev-ендпоінт доходять до агента в `DeployStack`; `AgentSession` надсилає `CancelRequest` після тайм-ауту й скасування викликача, але не після результату чи закриття сесії; restart і down стеку — роль Operator для restart і Editor для down, невидимий стек — 404 без звернення до агента, системний стек і незавершений деплой — 409, межі тайм-ауту зупинки, помилка compose як результат із виводом, лише останні рядки виводу, скасування викликача не доходить до агента, стек за назвою в середовищі (невідома, некоректна назва й стек іншого середовища — 404), успішний down забуває розгорнутий коміт, невдалий down і restart — ні, `Stack.RecordTakenDown` для системного стеку — помилка; інтеграційні тести: `RestartStack`/`DownStack` з тайм-аутом доходять до агента, вивід і помилка compose у відповіді, `NOT_FOUND` агента — 404, після деплою й down у списку стеків немає розгорнутого коміту; шлюз бота — `ListStacks` з правами на дії, `ControlStack` доходить до агента й повертає помилку compose з виводом, Viewer — `PERMISSION_DENIED`, невідомий стек — `NOT_FOUND`.
- `tests/DiscordBot`: `/stack-restart` і `/stack-down` — defer до виклику, назва стеку й тайм-аут у запиті, вивід compose в блоці коду, помилка compose як відповідь-помилка, відмова ЦА; `/stacks` — рядки зі станом деплою й доступними діями; підказки стеків за дією; вивід у межах 2000 символів зі зміщенням до останніх рядків, compose не може закрити блок коду; `GrpcForestClient` — перетворення `ListStacks`, поля `ControlStack`, тайм-аут не задано, статус → `ForestException`.

### Лаба 2

| Вимога | Потрібно | Є | Стан |
|---|---|---|---|
| Екрани | ≥ 4 | 4 | ✅ |
| Елементи керування | ≥ 20 | 54 | ✅ |
| Контейнерні елементи | ≥ 1 | 4 (таблиці контейнерів, середовищ і кредів registry; список рядків логу) | ✅ |
| Обробники подій | ≥ 10 | 38 | ✅ |
| Логіка окремо від GUI | так | так | ✅ |

Код: `src/Web` і API панелі в ЦА (PR #42): `GET /api/environments`, `GET /api/environments/{id}/containers`, `POST …/containers/{container}/start|stop|restart` (`Api/Endpoints/EnvironmentEndpoints.cs`, контракт — `Api/Contracts/EnvironmentContracts.cs`). Екран логів контейнера — на готовому `GET /api/environments/{id}/containers/{container}/logs` (PR #44, Петренко Б. О.), PR #49. Екрани середовищ і реєстрації агента — на API адміністрування середовищ (PR #48, Петренко Б. О.), PR #51. Спільні `ConfirmAction` і `form-messages` (інших учасників) не враховано. Згенеровані `src/components/ui/{table,select,badge}.tsx` не враховано.

- **Екрани:** контейнери середовища (`src/routes/_authed/containers.tsx`, `/containers`); логи контейнера (`src/routes/_authed/containers_.$environmentId.$containerId.logs.tsx`, `/containers/<середовище>/<контейнер>/logs`); середовища й статус агентів (`src/routes/_authed/environments/index.tsx`, `/environments`); середовище й реєстрація агента з кредами registry (`src/routes/_authed/environments/$environmentId.tsx`, `/environments/<id>`).
- **Елементи керування:** вибір середовища; фільтри «Стек» і «Стан»; поле пошуку за назвою, образом або id; кнопки «Скинути фільтри» й «Оновити»; у кожному рядку кнопки «Запустити», «Зупинити», «Перезапустити»; у діалозі підтвердження — поле тайм-ауту зупинки, «Скасувати» й кнопка дії; посилання «Контейнери» в шапці (`src/routes/_authed.tsx`); у кожному рядку таблиці — кнопка «Логи». Екран логів: посилання «До контейнерів»; вибір кількості рядків; фільтр потоку (stdout/stderr); поле пошуку в тексті; кнопки «Скинути фільтри», «Оновити» й «Копіювати»; перемикачі «Автооновлення», «Переносити рядки», «Показувати час». Середовища: посилання «Середовища» в шапці; фільтр стану агента; поле пошуку за назвою або хостом; кнопки «Скинути фільтри», «Оновити», «Нове середовище»; у рядку — посилання на середовище, кнопка «Змінити середовище», посилання «Агент і креди»; у формах створення й зміни — поле назви, прапорець «Приватне середовище», «Скасувати», «Створити», «Зберегти»; після створення — поле з токеном, «Копіювати токен», поля домену ЦА й назви хоста, «Копіювати .env», «Копіювати compose.yaml», «Відкрити середовище», «Готово». Середовище: посилання «До середовищ»; кнопки «Новий токен» і «Відкликати агента»; поля registry, користувача й пароля, «Зберегти»; у рядку кредів — «Видалити».
- **Контейнерні елементи:** таблиця контейнерів (назва й короткий id, стек і сервіс, стан і статус Docker, образ, час створення, дії); список рядків логу (`role="log"`: час, stderr виділено, перенесення або горизонтальна прокрутка); таблиця середовищ (назва й приватність, стан агента й виданого токена, хост, версія, коли бачили востаннє, дії); таблиця кредів registry (registry, користувач, коли змінено, видалення).
- **Обробники подій:** зміна середовища (фільтр стеку скидається), стеку, стану; введення в пошук; «Скинути фільтри»; «Оновити»; натискання «Запустити» (одразу), «Зупинити» й «Перезапустити» (відкривають підтвердження); введення тайм-ауту (перевірка 0–300 на льоту); підтвердження (`onSubmit` форми діалогу); закриття діалогу (`onOpenChange`: Esc, «Скасувати»). Екран логів: зміна кількості рядків (новий запит хвоста), потоку; введення в пошук; «Скинути фільтри»; «Оновити» (дозавантаження з `since`); «Копіювати» (видимі рядки в буфер обміну, повідомлення про результат); перемикачі автооновлення, перенесення й часу; прокрутка списку (`onScroll`: нові рядки тримають вид унизу, лише поки користувач не прокрутив угору). Середовища: зміна фільтра стану; введення в пошук; «Скинути фільтри»; «Оновити»; відкриття й закриття діалогів створення та зміни (`onOpenChange`: форма й помилка скидаються, а токен створеного середовища забувається); `onSubmit` створення (токен переходить у стан діалогу, мутація скидається); `onSubmit` зміни (зайнята назва — окреме повідомлення); введення домену й назви хоста (`.env` перебудовується на льоту, обидва поля перевіряються); «Копіювати» (`CopyButton`: буфер обміну й позначка на 2 с); фокус на полі токена (виділення); «Готово». Середовище: «Новий токен» (з підтвердженням, якщо є виданий токен або агент); «Відкликати агента» (з підтвердженням); `onSubmit` кредів (пароль прибирається з форми й мутації після збереження); «Видалити» креди (з підтвердженням).
- **Логіка окремо від GUI:** `src/features/containers/model.ts` — схема search params, фільтрація й сортування, варіанти фільтра стеків, доступність дії за станом (ті самі правила, що в `ContainerCommand` ЦА), розбір тайм-ауту, ключ перекладу для помилки за статусом; `src/features/environments/model.ts` — стан агента (онлайн / офлайн / не зареєстровано) і виданого токена, фільтр і схема search params, схеми форм середовища й кредів (Zod, ті самі обмеження, що в домені ЦА), здогадка налаштувань агента (домен ЦА — адреса панелі, назва хоста з назви середовища за RFC 1123) і їхня перевірка, `compose.yaml` агента (сам `deploy/agent/compose.yaml`, вбудований під час збірки) і `.env` з токеном, ключ перекладу помилки; мутації — `src/api/environments.ts` (інвалідація `environmentsQuery` і `registryCredentialsQuery`, `gcTime: 0` для мутацій із токеном або паролем); запити — `src/api/queries.ts` (`environmentsQuery`, `containersQuery`) і `src/api/containers.ts` (`useContainerActionMutation` з інвалідацією списку). Фільтри й вибране середовище лежать у URL (`validateSearch`), список оновлюється кожні 15 с, офлайн-агент не опитується. Логи: `src/features/container-logs/model.ts` — схема search params і типові значення, межа `since` (найпізніший час серед рядків, бо stdout і stderr не впорядковані між собою), злиття нових рядків без повторів на межі `since` (повтор, який програма справді надрукувала двічі, лишається) і з обмеженням 5000 рядків, фільтр за потоком і текстом, текст для копіювання, правило повтору запиту (404, 501, 503 не повторюються), ключ перекладу для помилки (404 — контейнера немає, 501 — застарілий агент, 503 — агент офлайн); запит — `containerLogsQuery` у `src/api/queries.ts`: перше завантаження — хвіст, кожне наступне (кнопка або `refetchInterval` 5 с) — лише рядки з `since`, дописані до наявних.


### Лаба 3

| Бібліотека | Що використано | Ретроспектива |
|---|---|---|
| Docker.DotNet (`Docker.DotNet.Enhanced`) | `DockerClientBuilder` з визначенням endpoint як у docker CLI; `System.GetVersionAsync`; `Containers.ListContainersAsync`, `StartContainerAsync`, `StopContainerAsync`, `RestartContainerAsync` з `WaitBeforeKillSeconds`; `Containers.GetContainerLogsAsync` (`ContainerLogsParameters`: `Timestamps`, `Tail`, `Since`) і `MultiplexedStream.ReadOutputAsync` з `TargetStream` для stdout/stderr; типізовані винятки (`DockerContainerNotFoundException`, `DockerApiException`) | [docs/retro/docker-dotnet.md](docs/retro/docker-dotnet.md) |
| gRPC для .NET (`Grpc.Net.Client`, `Grpc.Tools`, `Google.Protobuf`) | Клієнт двонапрямленого стріму `Connect`; metadata виклику; коди статусу (`Unauthenticated`, `Unavailable`); HTTP/2 keepalive-пінги через `SocketsHttpHandler`; дескриптори protobuf для зіставлення типу команди з полем `oneof`; фейковий клієнт у тестах через віртуальні методи згенерованого класу | [docs/retro/grpc.md](docs/retro/grpc.md) |
| Discord.Net | Slash-команди `/stacks`, `/stack-restart`, `/stack-down` (`[SlashCommand]`, `[Summary]`, `[MinValue]`, `[MaxValue]`), власний `AutocompleteHandler` для стеків, що читає вже вибране середовище й назву команди з `IAutocompleteInteraction.Data` | [docs/retro/discord-net.md](docs/retro/discord-net.md) |
| EF Core | Конфігурація сутності `RegistryCredential` (зовнішній ключ з каскадним видаленням, унікальний індекс «середовище + registry»), міграція `RegistryCredentials` | [docs/retro/ef-core.md](docs/retro/ef-core.md) |

## Синенко Дмитро Вячеславович (@Dmyt-S)

Зона: Discord-бот, система сповіщень.

### Лаба 1

| Вимога | Потрібно | Є | Стан |
|---|---|---|---|
| Класи та подібні типи | ≥ 9 | 111 (97 класів, з них 40 record і 9 статичних; 5 інтерфейсів; 1 структура; 8 enum) | ✅ |
| Поля | ≥ 15 | 350 (284 поля стану, 66 констант і статичних полів) | ✅ |
| Нетривіальні методи | ≥ 25 | 159 | ✅ |
| Ієрархії успадкування | ≥ 2, одна з ≥ 3 класів | 5 (9 класів на трьох рівнях; 8 класів; 7 record; 6 record; 3 record) | ✅ |
| Поліморфізм | ≥ 3, серед них статичний і динамічний | 11, усі з клієнтським кодом (9 динамічних, 2 статичних) | ✅ |

Код: `src/DiscordBot` — клієнт ЦА по gRPC (PR #19); Discord-частина: slash-команди, автодоповнення, прив'язка акаунта (PR #20); Docker-образ бота (PR #25). Сповіщення (PR #38): у ЦА — диспетчер, доставка через бота, деталі сповіщення; у боті — споживач потоку й повідомлення в Discord. Меню `/dashboard` на Components V2 (PR «feat(bot): /dashboard with stacks and redeploy»): у боті — маршрути в `custom_id`, екрани, рендерер; у ЦА — `DeployStack` і `GetDeployment` у шлюзі бота. Сповіщення про агента офлайн і знову онлайн (PR «feat(aggregator): notify when an agent goes offline and comes back»): у ЦА — сторож з'єднань агентів із затримкою й джерело подій. Сповіщення про падіння й unhealthy контейнерів (PR «feat(aggregator): notify when a container crashes or turns unhealthy»): у ЦА — деталі контейнера, сторож подій Docker і джерело подій. Кнопка «Restart» під сповіщенням про контейнер (PR «feat(bot): restart button under container notifications»): у боті — маршрут кнопки в `custom_id`, обробник натискання, кодування GUID, спільне з `/dashboard`; у ЦА — прапорець системного контейнера в деталях.

Не враховано: `Program.cs`, згенерований з proto код і міграції.

#### Класи та подібні типи

| Простір імен `ContainerForest.DiscordBot.*` | Типи |
|---|---|
| — | `DependencyInjection` (статичний клас) |
| `Configuration` | `BotOptions`, `DiscordOptions` |
| `Forest` | `IForestClient` (інтерфейс), `ForestCaller`, `ForestEnvironment`, `ForestContainer` (record), `ContainerState`, `ContainerAction` (enum), `ForestException` (абстрактний клас), `AccountNotLinkedException`, `ForestAccessDeniedException`, `ForestNotFoundException`, `ForestRequestRejectedException`, `AgentUnavailableException`, `AgentFailedException`, `ForestUnavailableException`, `RpcErrorMapper` (статичний клас), `GrpcForestClient` |
| `Commands` | `ICommandInteraction` (інтерфейс), `BotUser`, `BotReply`, `ContainerList`, `ContainerActionArgs`, `Suggestion` (record), `NoArgs` (структура), `BotCommand<TArgs, TResult>` (абстрактний клас), `EnvironmentsCommand`, `ContainersCommand`, `ContainerActionCommand` (абстрактний клас), `StartContainerCommand`, `StopContainerCommand`, `RestartContainerCommand`, `LinkCommand`, `EnvironmentResolver`, `AutocompleteSource`, `ReplyFormatter`, `Markdown` (статичні класи) |
| `Interactions` | `DiscordBotService`, `ForestModule`, `DiscordInteraction`, `EnvironmentAutocompleteHandler`, `ContainerAutocompleteHandler`, `NotificationEmbed` (статичний клас), `DiscordNotificationPoster` |
| `Forest` (сповіщення) | `ForestNotification`, `DeploymentEvent`, `ContainerEvent` (record), `NotificationKind`, `NotificationSeverity` (enum), `INotificationSource` (інтерфейс), `GrpcNotificationSource` |
| `Notifications` | `DeliveryResult` (record), `INotificationPoster` (інтерфейс), `ReconnectBackoff`, `NotificationWatcher` |
| `Forest` (деплої) | `ForestDeployment` (record), `DeploymentStatus` (enum) |
| `Dashboard` | `DashboardRoute` (абстрактний record), `EnvironmentsRoute`, `StacksRoute`, `StackRoute`, `ConfirmRoute`, `RunRoute`, `DeploymentRoute` (record), `DashboardRoute.PartReader`, `StackOperation`, `NoticeKind` (enum), `PageOf<T>`, `ContainerCount`, `Notice`, `StackRow` (record), `DashboardScreen` (абстрактний record), `EnvironmentsScreen`, `StacksScreen`, `StackScreen`, `ConfirmScreen`, `ErrorScreen` (record), `StackOperations` (статичний клас), `DashboardService` |
| `Interactions` (`/dashboard`) | `DashboardModule`, `DashboardRenderer` (статичний клас) |
| `Notifications`, `Interactions`, `Commands`, `Forest` (кнопки «Restart» і «Logs» під сповіщенням) | `ContainerButton`, `ContainerLogs`, `ForestLogLine` (record), `ContainerButtonAction` (enum), `NotificationModule`, `ContainerLogsCommand`, `CompactGuid` (статичний клас) |

| Простір імен `ContainerForest.Aggregator.*` (сповіщення) | Типи |
|---|---|
| `Domain.Notifications` | `NotificationDetails` (абстрактний record), `DeploymentNotificationDetails`, `ContainerNotificationDetails` (record) |
| `Application.Deployments` | `IDeploymentObserver` (інтерфейс), `DeploymentOutcome` (record) |
| `Application.Notifications` | `NotificationFeed`, `NotificationFeed.Subscription`, `NotificationDispatcher`, `DeploymentNotifier`, `OutgoingNotification` (record), `NotificationDeliveryService`, `NotificationOptions`, `AgentConnectivityNotifier`, `AgentConnectivityWatch`, `AgentConnectivityWatch.Outage`, `ContainerObservation` (record), `ContainerIncidentNotifier`, `ContainerIncidentWatch`, `ContainerIncidentWatch.PendingDeath` |
| `Infrastructure.Persistence` | `NotificationDetailsConverter`, `NotificationDetailsConverter.StoredDetails` (record) |
| `Api.Grpc` | `NotificationMessages` (статичний клас) |

#### Поля

Поля, що зберігаються з параметрів первинного конструктора, враховано як поля стану.

| Тип | Поля стану | Константи та статичні поля |
|---|---|---|
| `BotOptions` | `ControllerUrl`, `ServiceToken`, `QueryTimeout`, `ActionTimeout` | `SectionName` |
| `DiscordOptions` | `Token`, `DevGuildId` | `SectionName` |
| `ForestCaller` | `UserId`, `UserName`, `IsAdmin`, `IsActive` | |
| `ForestEnvironment` | `Id`, `Name`, `Online` | |
| `ForestContainer` | `Id`, `Name`, `State`, `Image`, `StackName`, `ServiceName`, `StatusText`, `CanControl` | |
| `ForestUnavailableException` | `Diagnostic` | |
| `RpcErrorMapper` | | `ReasonTrailer`, `InvalidBotTokenReason`, `AccountNotLinkedReason` |
| `GrpcForestClient` | `gateway`, `options`, `time`, `logger` | `BotTokenHeader` |
| `BotUser`, `BotReply` | `Id`, `UserName`; `Content`, `IsError` | |
| `BotCommand<TArgs, TResult>` | `logger` | `UnexpectedErrorMessage` |
| `EnvironmentsCommand`, `LinkCommand` | `forest` у кожному | |
| `ContainersCommand`, `ContainerActionCommand` | `forest`, `environments` у кожному | |
| `ContainerList`, `ContainerActionArgs`, `Suggestion` | `Environment`, `Containers`; `Environment`, `Container`, `TimeoutSeconds`; `Name`, `Value` | |
| `EnvironmentResolver` | `forest` | |
| `AutocompleteSource` | `forest`, `logger` | `MaxSuggestions`, `MaxLength`, `Budget` |
| `ReplyFormatter` | | `MaxMessageLength` |
| `DiscordInteraction` | `interaction`, `User` | |
| `NotificationModule` | `restart`, `logs`, `lifetime`, `time`, `logger` | `OutdatedButtonMessage` |
| `ContainerButton`, `CompactGuid` | `Action`, `EnvironmentId`, `ContainerName` | `Prefix`, `MaxCustomIdLength`, `RestartKind`, `LogsKind`, `Separator`; `Length` |
| `ContainerLogsCommand`, `ContainerLogs`, `ForestLogLine` | `forest`, `environments`; `Environment`, `Lines`; `At`, `IsStderr`, `Text` | `TailLines`, `MaxLineLength` |
| `ForestModule` | `environments`, `containers`, `start`, `stop`, `restart`, `link`, `lifetime` | `EnvironmentDescription`, `ContainerDescription`, `TimeoutDescription` |
| `EnvironmentAutocompleteHandler`, `ContainerAutocompleteHandler` | `source` у кожному | |
| `DiscordBotService` | `client`, `interactions`, `services`, `options`, `environment`, `lifetime`, `logger`, `commandsRegistered` | |
| `ForestNotification` | `Id`, `DiscordChannelId`, `Kind`, `Severity`, `OccurredAt`, `EnvironmentId`, `EnvironmentName`, `Summary`, `Deployment`, `Container` | |
| `DeploymentEvent`, `ContainerEvent` | `DeploymentId`, `StackId`, `StackName`, `CommitSha`, `PulledImages`, `Error`; `ContainerId`, `ContainerName`, `StackName`, `ServiceName`, `ExitCode`, `IsSystem` | |
| `GrpcNotificationSource` | `gateway`, `options`, `time`, `logger` | |
| `DeliveryResult` | `Error` | `Delivered` |
| `ReconnectBackoff` | `failures`, `Initial`, `Max` | `HealthyStreamAge` |
| `NotificationWatcher` | `source`, `poster`, `backoff`, `time`, `logger` | `BotFailureError` |
| `NotificationEmbed` | | `MaxFieldLength`, `LastActionField` |
| `DiscordNotificationPoster` | `client`, `ready` | `UnknownChannel`, `MissingAccess`, `CannotSendInNonTextChannel`, `MissingPermissions` |
| `ForestDeployment` | `Status`, `Commit`, `Error`, `QueuedAt`, `FinishedAt` | |
| `DashboardRoute`, `PartReader` | `parts`, `next` | `Prefix`, `MaxCustomIdLength`, `MaxPage`, `Separator` |
| `EnvironmentsRoute`, `StacksRoute`, `StackRoute`, `ConfirmRoute`, `RunRoute`, `DeploymentRoute` | `Page`; `EnvironmentId`, `Page`; `EnvironmentId`, `StackId`, `StacksPage`; `Operation`, `EnvironmentId`, `StackId`, `StacksPage` (у двох); `EnvironmentId`, `StackId`, `DeploymentId`, `StacksPage` | `Name` у кожному |
| `PageOf<T>`, `ContainerCount`, `Notice`, `StackRow` | `Items`, `Number`, `Count`, `Total`; `Running`, `Total`; `Kind`, `Text`, `Output`; `Stack`, `Containers` | |
| `EnvironmentsScreen`, `StacksScreen`, `StackScreen`, `ConfirmScreen`, `ErrorScreen` | `Environments`; `Environment`, `Stacks`; `Environment`, `Stack`, `Containers`, `StacksPage`, `Notice`, `DeploymentId`, `Busy`; `Environment`, `Stack`, `Operation`, `StacksPage`; `Message`, `Back` | |
| `DashboardService` | `forest`, `time`, `logger` | `PageSize`, `MaxContainerLines`, `FollowInterval`, `FollowLimit` |
| `DashboardRenderer` | | `Accent`, `ErrorAccent`, `MaxTextLength`, `MaxNoticeLength`, `NoPreviousId`, `NoNextId` |
| `DashboardModule` | `dashboard`, `lifetime` | |
| Поля, додані в чужі класи для `/dashboard` | `ForestStack.CanDeploy` (Бояр В. О.); ЦА: `BotGatewayService.deployments` (Петренко Б. О.) | |
| ЦА: `DeploymentNotificationDetails` | `DeploymentId`, `StackId`, `StackName`, `Commit`, `PulledImages`, `Error` | |
| ЦА: `ContainerNotificationDetails` | `ContainerId`, `ContainerName`, `StackName`, `ServiceName`, `ExitCode`, `OutOfMemory`, `IsSystem` | |
| ЦА: `ContainerObservation` | `EnvironmentId`, `ContainerId`, `Action`, `OccurredAt`, `ContainerName`, `StackName`, `ServiceName`, `StatusText`, `ReportedExitCode`, `OutOfMemory`, `IsSystem` | |
| ЦА: `ContainerIncidentNotifier` | `dispatcher`, `environments`, `stacks`, `logger` | |
| ЦА: `ContainerIncidentWatch`, `PendingDeath` | `scopeFactory`, `options`, `time`, `logger`, `gate`, `deaths`, `lastNotified`, `stopping`, `disposed`, `requestedStops`, `outOfMemory`, `unhealthy`; `Observation`, `Waiting`, `WaitOver` | `DieAction`, `StopAction`, `DestroyAction`, `OutOfMemoryAction`, `UnhealthyAction`, `HealthyAction` |
| ЦА: `DeploymentOutcome` | `Deployment`, `Stack`, `Environment` | |
| ЦА: `NotificationFeed`, `Subscription` | `gate`, `current`; `feed`, `ended`, `signals`, `closed`, `WasReplaced` | |
| ЦА: `NotificationDispatcher` | `rules`, `notifications`, `unitOfWork`, `feed`, `time` | |
| ЦА: `DeploymentNotifier` | `dispatcher`, `logger` | |
| ЦА: `NotificationOptions` | `AgentOfflineDelay`, `ContainerStopWait`, `ContainerRepeatInterval` | `SectionName` |
| ЦА: `AgentConnectivityNotifier` | `dispatcher`, `environments`, `logger` | |
| ЦА: `AgentConnectivityWatch`, `Outage` | `scopeFactory`, `options`, `time`, `logger`, `gate`, `outages`, `deliveries`, `stopping`, `disposed`; `Waiting`, `DelayOver`, `Notified` | |
| ЦА: `OutgoingNotification` | `Notification`, `DiscordChannelId`, `EnvironmentName` | |
| ЦА: `NotificationDeliveryService` | `notifications`, `channels`, `environments`, `unitOfWork`, `time`, `logger` | `ChannelDeletedError`, `ChannelDisabledError`, `UnknownBotError` |
| ЦА: `NotificationDetailsConverter`, `StoredDetails` | `Type`, `DeploymentId`, `StackId`, `StackName`, `Commit`, `PulledImages`, `Error`, `ContainerId`, `ContainerName`, `ServiceName`, `ExitCode` | `DeploymentType`, `ContainerType`, `Options` |
| ЦА: поля, додані в класи Петренка Б. О. | `Notification.Details`, `NotificationEvent.Details`; `BotGatewayService.notificationDelivery`, `notificationFeed`, `scopeFactory`, `containerLogs`; `AgentGatewayService.connectivity`, `containerIncidents` | `BotGatewayService.NotificationRescanInterval` |
| Агент: поле, додане в тип Бояра В. О. | `DockerContainerEvent.ExitCode` | |

#### Нетривіальні методи (159)

| Тип | Методи |
|---|---|
| `BotOptions`, `DiscordOptions` (2) | `IsValid` у кожному (для `DiscordOptions` — з урахуванням оточення) |
| `ForestException` (1) | `DetailOr` (повідомлення ЦА або типовий текст) |
| `RpcErrorMapper` (1) | `ToForestException` (коди статусу й трейлер `x-cf-reason` → винятки; відрізняє недоступний ЦА від офлайн-агента) |
| `GrpcForestClient` (9) | `GetCallerAsync`, `LinkAccountAsync`, `ListEnvironmentsAsync`, `ListContainersAsync`, `ControlContainerAsync`, `CallAsync` (токен, дедлайн, скасування, перетворення помилок, лог помилок конфігурації), `ToContainer`, `ToState`, `ToProto` |
| `DependencyInjection` (3) | `AddDiscordBot`, `AddForestClient`, `CreateChannel` |
| `BotCommand<TArgs, TResult>` (1) | `ExecuteAsync` (шаблонний метод: defer → виклик ЦА → форматування → обробка помилок) |
| `ContainersCommand` (1) | `CallForestAsync` (пошук середовища й список контейнерів) |
| `ContainerActionCommand` (2) | `CallForestAsync`, `Format` |
| `EnvironmentResolver` (2) | `ResolveAsync`, `Find` (id, точне ім'я, ім'я без урахування регістру, неоднозначність) |
| `ReplyFormatter` (6) | `Environments`, `Containers` (групування за стеками), `ContainerLine`, `StateIcon`, `Status`, `WithinLimit` (ліміт 2000 символів із підсумком пропущеного) |
| `Markdown` (1) | `Escape` |
| `AutocompleteSource` (8) | `EnvironmentsAsync`, `ContainersAsync`, `SuggestEnvironments`, `SuggestContainers`, `Fits` (стани, доречні для дії), `IsIdPrefix`, `Truncate`, `WithinBudgetAsync` (бюджет 2 с, помилка → без підказок) |
| `DiscordInteraction` (2) | `DeferAsync` (кнопка — окрема ephemeral-відповідь, а не редагування повідомлення з нею), `RespondAsync` (без згадок користувачів) |
| `EnvironmentAutocompleteHandler`, `ContainerAutocompleteHandler` (2) | `GenerateSuggestionsAsync` у кожному |
| `DiscordBotService` (4) | `ExecuteAsync` (вхід, невірний токен), `RegisterCommandsAsync` (тестовий сервер або глобально, один раз), `ReportFailureAsync`, `LogAsync` |
| `GrpcNotificationSource` (6) | `WatchAsync` (потік без дедлайну, заміна іншим потоком — тихе завершення), `ReportAsync`, `Translate`, `ToNotification` (деталі з `oneof`), `ToKind`, `ToSeverity` |
| `ReconnectBackoff` (1) | `Next` (подвоєння до межі, jitter, скидання після здорового потоку) |
| `NotificationWatcher` (3) | `ExecuteAsync` (очікування Discord, повторне відкриття потоку), `WatchOnceAsync` (помилки потоку в лог), `DeliverAsync` (помилка бота — звіт про невдачу, невдалий звіт не зупиняє доставку) |
| `NotificationEmbed` (6) | `Build` (поля за видом деталей, екранування), `WithRestart` (поле «Last action» замінює попереднє, решта embed без змін), `Buttons` («Restart» лише там, де він є), `Title`, `ColorOf`, `CodeBlock` (ліміт поля 1024, захист від закриття блоку) |
| `DiscordNotificationPoster` (2) | `PostAsync` (канал не існує або не текстовий, відмова Discord як причина), `Describe` (коди помилок Discord → підказка про права) |
| ЦА: `DeploymentNotificationDetails` (2) | `Of` (лише успішний чи невдалий деплой свого стеку), `Fits` |
| ЦА: `ContainerNotificationDetails` (1) | `Fits` (код виходу й OOM лише в падінні) |
| ЦА: `ContainerObservation` (2) | `StatusExitCode` (код виходу зі статусу Docker; `ExitCode` спершу бере код із події агента), `DisplayName` (коротке id контейнера, якого вже немає) |
| ЦА: `ContainerIncidentNotifier` (3) | `NotifyAsync` (стек — лише керований стек із тим самим іменем, видалене середовище — без сповіщення), `Summarize`, `SignalName` (код понад 128 — сигнал 128 + N) |
| ЦА: `ContainerIncidentWatch` (9) | `Observe` (розбір дій), `Died` (stop щойно перед die — запитана зупинка, інакше чекання), `Requested` (stop або destroy скасовує чекання або запам'ятовується для наступного die), `RanOutOfMemory` (oom позначає падіння, що чекає, або запам'ятовується для наступного die), `Remember` (запис події з прибиранням старіших за чекання), unhealthy — одразу, healthy — «знову здоровий» лише після надісланого unhealthy і без інтервалу повтору, `NotifyUnlessStoppedAsync` (код 0 — не падіння, крім OOM), `Raise` (не частіше разу за інтервал для контейнера й виду, прибирання старих записів; чи надіслано), `NotifyAsync` (власний DI-scope, помилки лише в лог), `Dispose` |
| ЦА: метод, доданий у клас Петренка Б. О. (1) | `AgentGatewayService.ToObservation` (подія агента → спостереження, час події або поточний) |
| ЦА: `NotificationFeed`, `Subscription` (3) | `Subscribe` (новий підписник завершує попереднього), `Leave`, `WaitAsync` (сигнал або тайм-аут) |
| ЦА: `NotificationDispatcher` (2) | `DispatchAsync` (правила → одне сповіщення на канал → збереження → пробудження потоку), `Specificity` |
| ЦА: `DeploymentNotifier` (2) | `DeploymentFinishedAsync` (подія з деталями, помилки лише в лог), `Summarize` (один рядок) |
| ЦА: `NotificationOptions` (1) | `IsValid` |
| ЦА: `AgentConnectivityNotifier` (2) | `NotifyAsync` (лише події агента, видалене середовище — без сповіщення), `Summarize` |
| ЦА: `AgentConnectivityWatch` (6) | `Connected` (кінець простою, «знову онлайн» лише після «офлайн»), `Disconnected` (початок простою, повторний розрив не перезапускає очікування), `NotifyWhenStillOfflineAsync` (затримка, перевірка, що простій той самий), `Enqueue` (черга сповіщень середовища: «онлайн» не випереджає «офлайн»), `NotifyAsync` (власний DI-scope, помилки лише в лог), `Dispose` (зупинка ЦА скасовує очікування) |
| ЦА: `NotificationDeliveryService` (2) | `TakePendingAsync` (пропуск надісланих, прострочені → Expired, видалений чи вимкнений канал → Failed, назва середовища), `ReportAsync` (повторний звіт ігнорується) |
| ЦА: `NotificationDetailsConverter` (2) | `Serialize`, `Deserialize` |
| ЦА: `NotificationMessages` (3) | `ToMessage`, `ToKind`, `ToSeverity` |
| ЦА: методи, додані в класи Петренка Б. О. (3) | `NotificationEvent.EnsureValid` (деталі відповідають виду й стеку); `BotGatewayService.WatchNotifications` (скан у власному DI-scope, пробудження, заміна потоку — `Aborted`), `ReportDelivery` |
| `GrpcForestClient` (2, `/dashboard`) | `DeployStackAsync`, `GetDeploymentAsync` (статуси, порожні поля → null) |
| `DashboardRoute` (3) | `ToCustomId` (перевірка ліміту 100 символів), `TryParse` (лише id, які бот сам записав), `Encode` для операції |
| `DashboardRoute.PartReader` (2) | `Page` (лише канонічне число в межах), `Operation` |
| `CompactGuid` (2) | `Encode` (GUID у 22 символи base64url), `TryDecode` (лише 16 байтів) |
| `ContainerButton` (5) | `For` (лише падіння чи unhealthy; «Restart» не для системного контейнера, «Logs» завжди; ім'я за правилом Docker, ліміт 100 символів), `ToCustomId`, `TryParse`, `KindOf`, `ActionOf` |
| `ContainerLogsCommand` (3) | `CallForestAsync`, `Format` (кінець логу в межах повідомлення, порожній лог), `Readable` (без ANSI й керівних символів, обрізання довгого рядка) |
| `GrpcForestClient` (1, логи) | `GetContainerLogsAsync` (час 0 → null) |
| `NotificationModule` (2) | `ClickAsync` (невідомий id — підказка, «Logs» — `ContainerLogsCommand`, «Restart» — `RestartContainerCommand` від імені того, хто натиснув), `NoteRestartAsync` (успішний restart — позначка в самому сповіщенні, помилка лише в лог) |
| `PageOf<T>`, `ContainerCount`, `StackScreen` (3) | `PageOf.Of` (сторінка в межах списку, що зменшився), `ContainerCount.Of`, `StackScreen.Operations` (дії за правами) |
| `DashboardService` (12) | `ShowAsync` (маршрут → екран, помилки ЦА → екран помилки з дорогою назад), `BackFrom`, `EnvironmentsAsync`, `StacksAsync` (кількість контейнерів кожного стеку), `StackAsync`, `RunAsync` (redeploy у чергу; restart і down — екран прогресу, дія, свіжі дані), `FollowAsync` (стан деплою оновлюється в тому самому повідомленні, лише коли змінився; межа 13 хв), `FollowDeploymentAsync`, `DescribeDeployment`, `FindEnvironmentAsync`, `FindStackAsync`, `TryListContainersAsync` (офлайн-агент — без кількості, а не помилка) |
| `DashboardRenderer` (11) | `Environments`, `Stacks`, `Stack` (кнопки за правами, вимкнені для офлайн-агента й під час дії), `Confirm`, `Error`, `StackSummary`, `Deployed`, `ContainersLine`, `NoticeText` (кінець виводу compose в межах ліміту тексту), `TrimToEnd`, `Pager` (вимкнені кнопки з унікальними id) |
| `DashboardModule` (2) | `NavigateAsync` (невідомий id → список середовищ), `DrawAsync` (Components V2, без згадок) |
| ЦА: методи, додані в `BotGatewayService` Петренка Б. О. (3) | `DeployStack` (стек за назвою серед видимих, далі `DeploymentService.QueueAsync`), `GetDeployment`, `GetContainerLogs` (через `ContainerLogService` з його правами) |
| Агент: метод, доданий у `DockerDotNetGateway` Бояра В. О. (1) | `ExitCodeOf` (атрибут `exitCode` лише події `die`, некоректне значення — немає коду) |

Не враховано як тривіальні: `CallForestAsync` і `Format` у `EnvironmentsCommand` і `LinkCommand` (лише делегують), перевизначення `Action`, `Done`, `StopTimeout`, `Name`, `BotReply.Success` і `Error`, `Markdown.Code`, `ReplyFormatter.Count`, `AutocompleteSource.Matches`, `DependencyInjection.AddCommands` (лише реєстрація), `DiscordInteraction.DeferAsync`, `DiscordBotService.HandleInteractionAsync`, методи slash-команд у `ForestModule` (лише передають опції команді), `DeliveryResult.Failed` і `IsDelivered`, `DiscordNotificationPoster.WaitUntilReadyAsync`, `NotificationFeed.Publish`, `Subscription.Signal`, `Replace`, `Close` і `Dispose`, `DeploymentNotificationDetails.Succeeded`, перевизначення в `BotGatewayInterceptor` для потокових викликів (лише передають у спільну перевірку токена); для `/dashboard` — `Kind` і `Parts` маршрутів, `PartReader.Take`, `PageOf.HasPrevious` і `HasNext`, `StackScreen.CanAct`, `ForestDeployment.IsFinished`, методи `StackOperations` (`NeedsConfirmation`, `IsQueued`, `Label`, `Explanation`), `DashboardService.ConfirmAsync` і `ShortCommit`, `DashboardRenderer.Render` (лише вибір за типом екрана) та його будівельники (`Button`, `Container`, `Text`, `Section`, `Divider`, `Build`, `Emoji`, `Style`), `DashboardModule.OpenAsync` і `ShowAsync`.

#### Ієрархії успадкування

1. `ForestException` → `AccountNotLinkedException`, `ForestAccessDeniedException`, `ForestNotFoundException`, `ForestRequestRejectedException`, `AgentUnavailableException`, `AgentFailedException`, `ForestUnavailableException`. Це 8 класів.
2. `BotCommand<TArgs, TResult>` → `EnvironmentsCommand`, `ContainersCommand`, `LinkCommand`, `ContainerLogsCommand`, `ContainerActionCommand` → `StartContainerCommand`, `StopContainerCommand`, `RestartContainerCommand`. Це 9 класів на трьох рівнях.
3. ЦА: `NotificationDetails` → `DeploymentNotificationDetails`, `ContainerNotificationDetails` (3 record).
4. `DashboardRoute` → `EnvironmentsRoute`, `StacksRoute`, `StackRoute`, `ConfirmRoute`, `RunRoute`, `DeploymentRoute`. Це 7 record.
5. `DashboardScreen` → `EnvironmentsScreen`, `StacksScreen`, `StackScreen`, `ConfirmScreen`, `ErrorScreen`. Це 6 record.

#### Поліморфізм

| # | Вид | Поліморфний інтерфейс | Клієнтський код |
|---|---|---|---|
| 1 | Динамічний (шаблонний метод) | `BotCommand.CallForestAsync`, `Format` | `BotCommand.ExecuteAsync` однаково відкладає відповідь, обробляє помилки й відповідає для всіх шести команд ✅ |
| 2 | Статичний (generics) | `BotCommand<TArgs, TResult>`: типи опцій і результату виклику ЦА | `ExecuteAsync` реалізовано один раз для `NoArgs`, `string`, `ContainerActionArgs` і різних результатів; `ForestModule` викликає його для кожної команди ✅ |
| 3 | Динамічний (адаптер) | `IForestClient`: `GrpcForestClient` (і stub у тестах) | команди, `EnvironmentResolver` і `AutocompleteSource` працюють із ЦА, не знаючи про gRPC ✅ |
| 4 | Динамічний (адаптер) | `ICommandInteraction`: `DiscordInteraction` (і запис взаємодії в тестах) | `BotCommand.ExecuteAsync` відповідає користувачу, не знаючи про Discord.Net ✅ |
| 5 | Динамічний (віртуальні члени) | `ContainerActionCommand.Action`, `Done`, `StopTimeout` | `ContainerActionCommand.CallForestAsync` і `Format` однакові для старту, зупинки й перезапуску; старт не надсилає таймаут зупинки ✅ |
| 6 | Динамічний (адаптер) | `INotificationSource`: `GrpcNotificationSource` (і фейк у тестах) | `NotificationWatcher` отримує сповіщення й звітує про них, не знаючи про gRPC ✅ |
| 7 | Динамічний (адаптер) | `INotificationPoster`: `DiscordNotificationPoster` (і фейк у тестах) | `NotificationWatcher` публікує сповіщення, не знаючи про Discord.Net ✅ |
| 8 | Динамічний (абстрактний член) | ЦА: `NotificationDetails.Fits` | `NotificationEvent.EnsureValid` перевіряє, що деталі описують подію, для будь-якого виду деталей; через нього — `NotificationDispatcher` і `Notification.Create` ✅ |
| 9 | Динамічний (спостерігач) | ЦА: `IDeploymentObserver`: `DeploymentNotifier` | `DeployPipeline` після збереження результату повідомляє всі зареєстровані `IDeploymentObserver`, не знаючи їхніх типів (виклик — Петренко Б. О., PR #40) ✅ |
| 10 | Динамічний (шаблонний метод) | `DashboardRoute.Kind`, `Parts` | `DashboardRoute.ToCustomId` однаково записує й перевіряє id для всіх шести маршрутів; `DashboardRenderer` дає кнопкам маршрути, не знаючи їхнього формату ✅ |
| 11 | Статичний (generics) | `PageOf<T>` | одна логіка сторінок для середовищ (`PageOf<ForestEnvironment>`) і стеків (`PageOf<StackRow>`); `DashboardRenderer.Pager<T>` малює гортання для будь-якої сторінки ✅ |

#### Тести (не входять в облік)

- `tests/DiscordBot`, 197 тестів (увесь проєкт тестів бота, разом із тестами команд стеків Бояра В. О.).
  - `/dashboard`, 63 тести: маршрути (кожен переживає `custom_id`, найдовший уміщається в 100 символів, чужі й зіпсовані id відхиляються); сервіс із заглушкою ЦА (сортування й сторінки, сторінка за межею — остання, кількість контейнерів стеку, офлайн-агент без звернення до нього, зниклий стек — помилка з дорогою назад, неприв'язаний акаунт, непередбачена помилка в лог, restart спершу показує прогрес і потім свіжі дані, помилка compose з виводом, redeploy і redeploy with pull ставлять деплой у чергу, а стан оновлюється в тому самому повідомленні лише при зміні, з вимкненими кнопками; довгий деплой — до межі 13 хв; «Refresh» стежить знову; стани деплою); рендерер (повна сторінка в межах 40 компонентів і 4000 символів, унікальні id, кнопки за правами, вимкнені для офлайн-агента й під час дії, підтвердження, екранування); маршрутизація натискань справжнім `InteractionService` Discord.Net.
  - Нові виклики `GrpcForestClient`, 6 тестів: `can_deploy`, `DeployStack` з `pull`, офлайн-агент → `AgentUnavailableException`, `GetDeployment` для завершеного й незавершеного деплою.
  - Кнопка «Logs», 10 тестів: команда (defer до виклику, блок коду, порожній лог, кінець довгого логу в межах 2000 символів, помилка ЦА), прибирання кольорів ANSI й керівних символів, обрізання рядка; кнопки «Restart» і «Logs» (у системного контейнера — лише «Logs») і маршрутизація обох; у ЦА — `GetContainerLogs` для Viewer через фейкового агента й ліміт рядків.
  - Кнопка «Restart» під сповіщенням, 18 тестів (позначка «Last action»: одна, з останнім користувачем, решта embed без змін): є під падінням і unhealthy, немає під системним контейнером, іншими видами, задовгим іменем і без читабельного середовища; `custom_id` переживає запис і читання, зіпсовані й чужі id відхиляються; кнопка в повідомленні; натискання `ntf:*` і `dash:*` потрапляють кожне у свій модуль (`SearchComponentCommand`).
  - Сповіщення: `GrpcNotificationSource` проти сервера в пам'яті (поля й деталі з `oneof`, токен, тихе завершення при заміні потоку, відхилений токен, звіти); `NotificationWatcher` з фейками (публікація й звіт, очікування Discord, помилка бота як невдача, повторне відкриття потоку, невдалий звіт не зупиняє доставку); embed (поля, екранування, обрізання помилки до ліміту поля, кольори); backoff.
  - `GrpcForestClient` проти справжнього gRPC-сервера `BotGateway` у пам'яті (`TestServer`) із заготовленими відповідями: токен у metadata `x-bot-token`, поля запитів, перетворення відповідей, кожен код помилки ЦА → свій виняток; невірний токен і недоступний ЦА — у лог, користувачу загальне повідомлення, токен не потрапляє ні в лог, ні у виняток; скасування.
  - Команди зі stub-реалізацією `IForestClient`: defer до виклику ЦА, пошук середовища за ім'ям і id, дії з таймаутом і без, помилки ЦА → читабельна відповідь, непередбачена помилка → у лог і загальне повідомлення, скасування без відповіді, `/link`.
  - Форматування: групування за стеками, ліміт 2000 символів, екранування markdown. Автодоповнення: фільтр за введеним текстом і станом, ліміти 25 варіантів і 100 символів, бюджет часу, помилки ЦА.
  - Конфігурація: `ValidateOnStart` для ЦА й Discord, тестовий сервер у `Development`, токен Discord лише з ASCII без пробілів.
- `tests/Aggregator`, 33 тести сповіщень: деталі деплою (лише успішний і невдалий деплой свого стеку, відповідність виду й стеку); диспетчер (одне сповіщення на канал від найконкретнішого правила, вимкнені канали й інші середовища, пробудження потоку); `DeploymentNotifier` (підсумок в один рядок, скасований деплой без сповіщення, помилка не доходить до пайплайну); доставка (порядок, переміщений канал, без повторного надсилання, прострочені, видалений і вимкнений канал, видалене середовище, звіти); `NotificationFeed` (сигнал, тайм-аут, заміна); інтеграційні тести шлюзу: потік без токена відхиляється, збережені й нові сповіщення з деталями, звіти, повторне надсилання новим потоком і `Aborted` для старого. Агент (12 тестів): затримка перед «офлайн», швидке перепідключення без сповіщень, «знову онлайн» лише після «офлайн», повторний розрив не зсуває початок простою, зупинка ЦА скасовує очікування, правило стеку й видалене середовище; наскрізні тести з агентом, що від'єднується від ЦА в пам'яті. Контейнери (26 тестів): «знову здоровий» один раз і лише після надісланого unhealthy, не стримується попереднім «знову здоровий», мовчить після приглушеного unhealthy; die без stop, stop перед die, stop або destroy після die, давній stop не ховає падіння, код 0, контейнер, уже перезапущений Docker (без коду від старого агента, з кодом із події, код 0 із події — не падіння), unhealthy одразу, цикл перезапусків — одне сповіщення за інтервал, зупинка ЦА; керований і некерований стек, контейнер поза compose, код виходу зі статусу; наскрізні тести від події агента до сповіщення в потоці бота з `ContainerDetails` і до запису з кодом із події. Агент (`tests/Agent`, 2 нові тести й перевірки в 2 наявних): `ExitCodeOf` (лише `die`, некоректне значення й відсутній атрибут), `ContainerEventForwarder` пересилає код і не ставить його без коду.
- `tests/Aggregator`, 2 тести шлюзу для `/dashboard`: адміністратор деплоїть стек через бота (команда агенту з `pull`, другий деплой під час першого — `FAILED_PRECONDITION`, `GetDeployment` до `Succeeded` з комітом); Viewer не бачить `can_deploy` і отримує `PERMISSION_DENIED`, невідомий стек і деплой — `NOT_FOUND`, некоректний id — `INVALID_ARGUMENT`.

### Лаба 2

| Вимога | Потрібно | Є | Стан |
|---|---|---|---|
| Екрани | ≥ 4 | 4 | ✅ |
| Елементи керування | ≥ 20 | 41 | ✅ |
| Контейнерні елементи | ≥ 1 | 3 (таблиці) | ✅ |
| Обробники подій | ≥ 10 | 24 | ✅ |
| Логіка окремо від GUI | так | так | ✅ |

Код: `src/Web` — прив'язка Discord (PR #39), сповіщення (PR «feat(web): notification channels, rules and history»). Згенеровані компоненти `src/components/ui` (зокрема доданий `switch`) і чужі спільні компоненти (`confirm-action.tsx`, `copy-button.tsx`) в облік не йдуть.

- **Екрани:**
  1. прив'язка Discord (`src/routes/_authed/discord.tsx`, `/discord`);
  2. канали сповіщень (`src/routes/_authed/notifications/channels/index.tsx`, `/notifications/channels`);
  3. правила каналу (`src/routes/_authed/notifications/channels/$channelId.tsx`, `/notifications/channels/$channelId`);
  4. історія сповіщень (`src/routes/_authed/notifications/history.tsx`, `/notifications/history`).

  Над екранами сповіщень — вкладки «Канали й правила» / «Історія» (`src/routes/_authed/notifications/route.tsx`).
- **Елементи керування (41):**
  - прив'язка Discord: кнопки «Відв'язати», «Так, відв'язати», «Скасувати», «Отримати код» / «Новий код», «Копіювати команду»;
  - шапка (`src/routes/_authed.tsx`): посилання «Discord», посилання «Сповіщення» (лише для адміністраторів); вкладки «Канали й правила» й «Історія»;
  - канали: кнопка «Новий канал»; у кожному рядку — посилання на правила, кнопка «Копіювати» id, перемикач «Увімкнено», кнопка «Змінити», кнопка «Видалити»; діалог каналу (`src/components/notifications/channel-dialog.tsx`, створення й зміна): поле назви, поле id Discord-каналу (лише цифри), кнопки «Скасувати» й «Створити» / «Зберегти»;
  - правила: посилання «До каналів», кнопка «Нове правило»; у кожному рядку — перемикач «Увімкнено», кнопка «Змінити події», кнопка «Видалити»; діалог «Нове правило» (`src/components/notifications/rule-dialogs.tsx`): шість прапорців подій (`src/components/notifications/event-checkboxes.tsx`), список середовищ, список стеків вибраного середовища, кнопки «Скасувати» й «Додати»; діалог «Події правила»: кнопки «Скасувати» й «Зберегти» (прапорці ті самі);
  - історія: списки «Стан» (з кількістю записів), «Канал» (разом із видаленими), «Скільки» (50–500), кнопки «Скинути фільтри» й «Оновити».
- **Контейнерні елементи:** таблиця каналів, таблиця правил каналу, таблиця історії сповіщень.
- **Обробники подій (24):**
  - прив'язка Discord: натискання «Відв'язати» (показ підтвердження), «Так, відв'язати» (відв'язка й оновлення стану), «Скасувати», «Отримати код» (новий код, запам'ятовується поточний стан прив'язки), «Копіювати команду» (буфер обміну, позначка «Скопійовано» на 2 с);
  - канали: перемикач увімкнення (поки запит іде, показує новий стан), підтвердження видалення (прибирає правила з кешу й перезапитує історію), відкриття й закриття діалогу каналу (форма заповнюється поточними значеннями каналу), введення id (лишає тільки цифри, бо скопійований id часто приходить із пробілами), надсилання форми каналу;
  - правила: відкриття й закриття діалогу нового правила, вибір середовища (скидає стек), вибір стеку, зміна прапорця події (порядок подій як у ЦА), надсилання нового правила, перемикач увімкнення правила, підтвердження видалення, відкриття й закриття діалогу подій, збереження подій (невідомі панелі види подій зберігаються як були);
  - історія: вибір стану, каналу й кількості (усе в URL через `validateSearch`, `replace`), «Скинути фільтри», «Оновити».
- **Логіка окремо від GUI:**
  - запити — `src/api/queries.ts` (`discordLinkQuery`, `notificationChannelsQuery`, `notificationRulesQuery`, `notificationsQuery`); мутації з інвалідацією кешу — `src/api/discord.ts` і `src/api/notifications.ts`;
  - `src/features/notifications/model.ts` без React: схеми Zod форм каналу й правила (id Discord лише рядком із 17–20 цифр, хоча б одна подія), перетворення форми правила на запит, область правила, фільтри й підрахунок історії, канали для фільтра (разом із видаленими), кольори станів, помилки ЦА за статусом → перекладені повідомлення;
  - відлік часу дії коду — `src/hooks/use-seconds-left.ts`. Поки код дійсний, стан прив'язки перезапитується кожні 3 с; поки в історії є сповіщення в очікуванні — кожні 5 с.

### Лаба 3

| Бібліотека | Що використано | Ретроспектива |
|---|---|---|
| Discord.Net 3.20.1 | `DiscordSocketClient` лише з інтентом `Guilds`; `InteractionService` з модулями через DI (`InteractionModuleBase`, `[SlashCommand]`, `[Summary]`, `[MinValue]`, `[MaxValue]`); автодоповнення (`AutocompleteHandler`, `[Autocomplete]`); `RunMode.Async`; реєстрація команд на тестовому сервері або глобально; відкладені ephemeral-відповіді (`DeferAsync`, `ModifyOriginalResponseAsync`), `AllowedMentions.None`; події `Log`, `Ready`, `InteractionCreated`, `InteractionExecuted`; сповіщення — `GetChannelAsync`, `IMessageChannel.SendMessageAsync` з embed (`EmbedBuilder`), коди помилок Discord у `HttpException.DiscordCode`; меню `/dashboard` на Components V2 (`ComponentBuilderV2`, `ContainerBuilder`, `SectionBuilder` з кнопкою-accessory, `TextDisplayBuilder`, `SeparatorBuilder`, `MessageFlags.ComponentsV2`), натискання кнопок через `[ComponentInteraction]` із шаблоном `custom_id`, редагування повідомлення після `DeferAsync` компонента | [docs/retro/discord-net.md](docs/retro/discord-net.md) |
| gRPC для .NET (`Grpc.Net.Client`, `Grpc.Tools`, `Google.Protobuf`; `Grpc.AspNetCore` у тестах) | Генерація клієнта з `bot.proto`; metadata виклику (`x-bot-token`); дедлайни й скасування через `CallOptions`; коди статусу й трейлери (`x-cf-reason`); `Status.DebugException`, щоб відрізнити помилку клієнта від відповіді сервера; сервер у пам'яті на `TestServer` у тестах, розведення згенерованих типів через `extern alias`; server streaming `WatchNotifications` з обох боків (сервер у ЦА, клієнт у боті), HTTP/2 keepalive-пінги, перехоплювач (`Interceptor`) для потокових викликів | [docs/retro/grpc.md](docs/retro/grpc.md) |

## Денисенко Любов Володимирівна (@denysenkoliubov)

Зона: вхід у систему, керування користувачами й журнал аудиту — бекенд у ЦА й екрани панелі.

### Лаба 1

| Вимога | Потрібно | Є | Стан |
|---|---|---|---|
| Класи та подібні типи | ≥ 9 | 66 (38 класів, з них 8 статичних і 3 абстрактні; 18 record, з них 1 абстрактний; 1 record struct; 4 інтерфейси; 5 переліків) | ✅ |
| Поля | ≥ 15 | 154 (131 поле стану, 23 константи й статичні поля) | ✅ |
| Нетривіальні методи | ≥ 25 | 60 | ✅ |
| Ієрархії успадкування | ≥ 2, одна з ≥ 3 класів | 3 (8 класів на трьох рівнях; 4 класи; 8 record на двох рівнях) | ✅ |
| Поліморфізм | ≥ 3, серед них статичний і динамічний | 8 з клієнтським кодом (6 динамічних, 2 статичні) | ✅ |

Код: вхід за паролем у ЦА (PR #28); керування користувачами в ЦА (PR #31); правила паролів і узагальнені ендпоінти змін користувача (PR #33); журнал аудиту (PR #90); ліміт спроб входу й строк зберігання журналу (PR «feat: sign-in rate limit and audit retention»).

Не враховано: зміни в `Program.cs`, `AggregatorDbContext`, `ProblemDetailsExceptionHandler` і `UserRepository` (спільний код; у `UserRepository` — однорядкові запити `ListAsync` і `CountActiveAdministratorsAsync`), міграції `Accounts` і `AuditLog`, згенеровані `schema.gen.ts` і `routeTree.gen.ts`. Для аудиту — виклики журналу, додані в сервіси інших учасників (`ApiTokenService`, `ContainerCommandService`, `StackCommandService`, `StackService`, `DeploymentService`, `EnvironmentService`, `SopsSecretsService`, `DiscordLinkService`, `PasskeySignIn`, `DiscordSignIn`): ці класи й методи лишаються за їхніми авторами, окремо враховано лише новий метод `ApiTokenService.LimitsOf`.

#### Класи та подібні типи

| Простір імен `ContainerForest.Aggregator.*` | Типи |
|---|---|
| `Infrastructure.Accounts` | `AccountUser`, `AccountPolicy` (статичний клас), `BootstrapOptions`, `AdminBootstrapper`, `IdentityAccountStore`, `PasswordRule` (абстрактний клас), `MaximumLengthRule`, `RepetitiveOrSequentialRule`, `ContextWordsRule` |
| `Application.Access` | `PasswordRejectedException`, `IAccountStore` (інтерфейс), `UserActionContext` (record), `UserAccountAction`, `AdministratorRemovingAction` (абстрактні класи), `DeactivateUserAction`, `RevokeAdminAction`, `ActivateUserAction`, `GrantAdminAction`, `RenameUserAction`, `ResetPasswordAction`, `UnlockUserAction`, `UserManagementService`, `UserSummary`, `RoleAssignmentView`, `UserDetails` (record) |
| `Api.Access` | `AuthenticationSetup` (статичний клас), `PasswordSignIn` |
| `Api.Endpoints` | `AuthEndpoints`, `UserEndpoints` (статичні класи) |
| `Domain.Audit` (аудит) | `AuditEntry`, `AuditActor` (record), `AuditActions` (статичний клас), переліки `AuditAction`, `AuditCategory`, `AuditOutcome`, `AuditChannel`, `AuditTargetKind`; `AuditTarget` (абстрактний record) і його нащадки `UserAuditTarget`, `RoleAssignmentAuditTarget`, `ApiTokenAuditTarget`, `EnvironmentAuditTarget`, `StackAuditTarget`, `ContainerAuditTarget`, `FileAuditTarget` |
| `Domain.Common` | `AuditEntryId` (record struct) |
| `Application.Audit` | `IAuditLog`, `IRequestOrigin`, `IAuditTrail` (інтерфейси), `AuditFilter`, `AuditPage`, `AuditPerson` (record), `AuditDraft`, `AuditTrail`, `AuditedCommand`, `AuditService`, `AuditOptions`, `AuditTrailExtensions`, `AuditOutcomes` (статичні класи) |
| `Infrastructure.Persistence` | `AuditLog`, `AuditEntryConfiguration`, `AuditRetention` |
| `Api` | `HttpRequestOrigin`, `AuditEndpoints` (статичний клас), `AuditEntryResponse`, `AuditPageResponse` (record) |

#### Поля

Поля, що зберігаються з параметрів первинного конструктора, враховано як поля стану.

| Тип | Поля стану | Константи та статичні поля |
|---|---|---|
| `AccountPolicy` | | `MinPasswordLength`, `MaxFailedSignIns`, `LockoutDuration` |
| `BootstrapOptions` | `AdminUserName`, `AdminPassword` | `SectionName` |
| `AdminBootstrapper` | `scopeFactory`, `options`, `time`, `logger` | |
| `PasswordRejectedException` | `Reasons` | |
| `AuthenticationSetup` | | `SessionCookieName`, `DevOrCookieScheme` |
| `PasswordSignIn` | `signInManager`, `users`, `currentActor`, `currentUser`, `audit` | `IncorrectPassword`, `DecoyAccount`, `decoyHash` |
| `IdentityAccountStore` | `userManager`, `db` | |
| `UserActionContext` | `Users`, `Accounts` | |
| `RenameUserAction`, `ResetPasswordAction` | `userName`; `newPassword` | |
| `UserManagementService` | `currentActor`, `users`, `roleAssignments`, `environments`, `stacks`, `accounts`, `unitOfWork`, `audit`, `time` | |
| `UserSummary` | `User`, `LockedOutUntil` | |
| `RoleAssignmentView` | `Id`, `EnvironmentId`, `EnvironmentName`, `StackId`, `StackName`, `Role`, `GrantedAt` | |
| `UserDetails` | `User`, `RoleAssignments` | |
| `MaximumLengthRule` | | `MaxLength` |
| `RepetitiveOrSequentialRule` | | `MaxRepeatedChunk`, `KeyboardRows` |
| `ContextWordsRule` | | `MinUserNameLength`, `ServiceNames` |
| `AuditEntry` | `OccurredAt`, `Action`, `Category`, `Outcome`, `ActorId`, `ActorName`, `Channel`, `ApiTokenId`, `Address`, `TargetKind`, `TargetId`, `TargetName`, `EnvironmentId`, `StackId`, `Details` | `MaxNameLength`, `MaxDetailsLength`, `MaxAddressLength` |
| `AuditEntryId` | `Value` | |
| `AuditActor` | `UserId`, `UserName`, `Channel`, `ApiTokenId`, `Address` | |
| `UserAuditTarget`, `ApiTokenAuditTarget`, `EnvironmentAuditTarget` | `UserId`, `UserName`; `TokenId`, `TokenName`; `Environment`, `EnvironmentName` | |
| `RoleAssignmentAuditTarget` | `AssignmentId`, `UserName`, `OnEnvironment`, `OnStack` | |
| `StackAuditTarget`, `ContainerAuditTarget`, `FileAuditTarget` | `Stack`, `StackName`, `InEnvironment`; `ContainerName`, `InEnvironment`, `InStack`; `Path` | |
| `AuditDraft` | `Action`, `Target`, `Details`, `Failure` | |
| `AuditTrail` | `currentUser`, `users`, `origin`, `log`, `time`, `logger` | |
| `AuditPerson` | `UserId`, `UserName`, `Channel` | |
| `AuditFilter` | `Category`, `Action`, `Outcome`, `ActorId`, `EnvironmentId`, `Search`, `From`, `To` | |
| `AuditPage` | `Entries`, `Total` | |
| `AuditedCommand` | `inner`, `action`, `audit` | |
| `AuditService` | `currentActor`, `log` | `DefaultPageSize`, `MaxPageSize` |
| `AuditLog` | `db`, `scopes` | |
| `AuditOptions` | `RetentionDays` | `SectionName` |
| `AuditRetention` | `scopes`, `options`, `time`, `logger` | `Interval` |
| `AuthEndpoints` | | `SignInPolicy` |
| `HttpRequestOrigin` | `httpContextAccessor` | `BotAuthenticationType` |
| `AuditEntryResponse` | `Id`, `OccurredAt`, `Action`, `Category`, `Outcome`, `ActorId`, `ActorName`, `Channel`, `Address`, `TargetKind`, `TargetId`, `TargetName`, `EnvironmentId`, `StackId`, `Details` | |
| `AuditPageResponse` | `Entries`, `Total`, `Page`, `PageSize` | |

#### Нетривіальні методи (60)

| Тип | Методи |
|---|---|
| `AccountPolicy` (1) | `Configure` (політика паролів і lockout) |
| `BootstrapOptions` (1) | `IsValid` (ім'я й пароль задаються разом або жодне) |
| `AdminBootstrapper` (1) | `StartAsync` (перший адміністратор лише в порожній базі, атомарно з обліковим записом) |
| `AuthenticationSetup` (1) | `AddSessionAuthentication` (cookie `cf_session`, 401/403 замість редіректів, security stamp, policy-схема з dev-заголовком) |
| `PasswordSignIn` (3) | `SignInAsync` (перевірка пароля з lockout, деактивований користувач, decoy-хеш для невідомого імені, запис успіху й причини невдачі в аудит), `ChangeOwnPasswordAsync` (зміна пароля, завершення інших сесій, перевидача поточної), `SignOutAsync` (запис в аудит до виходу, поки запит ще знає користувача) |
| `AuthEndpoints` (2) | `MapAuthEndpoints`, `AddSignInRateLimit` (ліміт спроб входу й зміни пароля на адресу клієнта: lockout не зупиняє один пароль на багатьох іменах) |
| `IdentityAccountStore` (6) | `RenameAsync` (без зміни security stamp), `SetPasswordAsync` (перевірка політики до зміни хешу, обліковий запис для користувача без нього, завершення сесій), `EndSessionsAsync`, `UnlockAsync`, `ListLockoutsAsync`, `ThrowIfFailed` (помилки пароля → `PasswordRejectedException`) |
| `UserAccountAction` (1) | `ExecuteAsync` (шаблонний метод: перевірка → дія → завершення сесій) |
| `AdministratorRemovingAction` (1) | `CheckAsync` (останній активний адміністратор лишається) |
| `RenameUserAction` (2) | `CheckAsync` (зайняте ім'я без урахування регістру, крім власного), `ApplyAsync` (доменне ім'я й ім'я облікового запису) |
| `UserManagementService` (12) | `ListAsync`, `GetAsync` (ролі за середовищем і стеком), `CreateUserAsync` (зайняте ім'я, відкат при відхиленому паролі), `GrantRoleCoreAsync` (стек того самого середовища, одна роль на область), `GrantRoleAsync`, `ChangeRoleAsync`, `RevokeRoleAsync` (запис в аудит із роллю й областю до зміни), `RunAsync` (виконання дії з перевіркою адміністратора під дією аудиту самої дії), `SummarizeAsync`, `GetAssignmentAsync` (чуже призначення — 404), `ViewAsync` (назви середовища й стеку), `NameOfAsync` (ім'я або id видаленого користувача) |
| `UserEndpoints` (4) | `MapUserEndpoints` (12 ендпоінтів), `MapUserChange<TRequest>` (ендпоінт зміни користувача з тілом будь-якого типу), `MapUserChange` (без тіла), `ToResponse` для призначення (область за наявністю стеку) |
| `PasswordRule` (1) | `ValidateAsync` (шаблонний метод: перевірка правила → помилка Identity з кодом `Password…`) |
| `MaximumLengthRule` (1) | `Check` |
| `RepetitiveOrSequentialRule` (4) | `Check`, `IsRepetitive` (повтор шматка до 4 символів), `IsSequential` (послідовні цифри з переходом 9 → 0 або літери), `IsKeyboardRow` |
| `ContextWordsRule` (1) | `Check` (ім'я користувача від 3 символів, назва системи) |
| `AuditActions` (1) | `CategoryOf` (розділ кожної дії) |
| `AuditEntry` (2) | `Record` (перевірка переліків, плоский запис із будь-якої цілі), `Clip` (один рядок, межа довжини з «…») |
| `AuditTrail` (4) | `RunAsync<T>` (виконання з записом результату: виконано, відмовлено, не вдалося; повторний кидок винятку), `WriteAsync` (без токена скасування; помилка запису не ламає дію), `CurrentActorAsync` (канал: сесія, токен, бот, без входу), `Join` |
| `AuditOutcomes` (1) | `Of` (виняток → результат і безпечна причина; невідомі винятки — без повідомлення) |
| `AuditedCommand` (2) | `ExecuteAsync` (декоратор: запис будь-якої спроби, зокрема відмовленої авторизацією), `ActionFor` |
| `AuditService` (1) | `ListAsync` (лише адміністратор, межі сторінки) |
| `AuditLog` (3) | `AppendAsync` (окремий контекст, щоб не зберегти напівзроблені зміни запиту), `ListAsync` (кількість і сторінка, порядок за часом і id v7), `Apply` (фільтри; пошук `LIKE` з екрануванням `%` і `_`) |
| `AuditRetention` (2) | `ExecuteAsync` (одразу після старту, далі раз на добу; 0 днів — вимкнено), `RemoveExpiredAsync` (окремий scope, помилка лише в лог) |
| `AuditEndpoints` (1) | `MapAuditEndpoints` |
| `ApiTokenService` (1) | `LimitsOf` (обмеження токена одним рядком для аудиту, без самого токена) |

Не враховано як тривіальні: `PasswordSignIn.ChangePasswordAsync`, `RecordSignInAsync` і `RecordFailureAsync` (обгортки виклику аудиту), `AuthenticationSetup.WriteStatus`, `BootstrapOptions.IsConfigured`, `AdminBootstrapper.StopAsync`, конструктор `AccountUser`, `IdentityAccountStore.CreateAsync` і `FindAsync`, `ApplyAsync` дій, що лише викликають один доменний метод чи метод `IAccountStore`, однорядкові `RenameAsync`, `DeactivateAsync`, `ActivateAsync`, `SetAdminAsync`, `ResetPasswordAsync`, `UnlockAsync` сервісу, `UserEndpoints.ToResponse` для користувача, властивості `Code` правил паролів, `RepetitiveOrSequentialRule.Steps`, `UserManagementService.CreateAsync` і `TargetOf`, `ScopeOf`, властивості цілей аудиту, `AuditDraft.Fail`, `AuditTrailExtensions.RunAsync`, `AuditTrail.RecordAsync` і `AddressFor`, властивості `HttpRequestOrigin`, `AuditEndpoints.ToResponse`, `AuditEntryConfiguration.Configure`, однорядковий запит `AuditLog.RemoveOlderThanAsync`, `AuditOptions.IsValid`, `UserAccountAction.AuditAction` і `AuditDetails` дій.

#### Ієрархії успадкування

1. `UserAccountAction` → `ActivateUserAction`, `GrantAdminAction`, `RenameUserAction`, `ResetPasswordAction`, `UnlockUserAction`, `AdministratorRemovingAction` → `DeactivateUserAction`, `RevokeAdminAction`. Це 8 класів на трьох рівнях.
2. `PasswordRule` → `MaximumLengthRule`, `RepetitiveOrSequentialRule`, `ContextWordsRule`. Це 4 класи.
3. `AuditTarget` (абстрактний record) → `UserAuditTarget`, `RoleAssignmentAuditTarget`, `ApiTokenAuditTarget`, `EnvironmentAuditTarget`, `StackAuditTarget`, `ContainerAuditTarget`, `FileAuditTarget`. Це 8 типів.

`AccountUser : IdentityUser<Guid>` успадковує клас бібліотеки й власною ієрархією не вважається.

#### Поліморфізм

| # | Вид | Поліморфний інтерфейс | Клієнтський код |
|---|---|---|---|
| 1 | Динамічний (шаблонний метод) | `UserAccountAction.CheckAsync`, `ApplyAsync`, `EndsSessions` | `UserAccountAction.ExecuteAsync` і `UserManagementService.RunAsync` однаково виконують сім різних дій над користувачем ✅ |
| 2 | Динамічний (адаптер) | `IAccountStore`: `IdentityAccountStore` (і `FakeAccountStore` у тестах) | `UserManagementService`, дії й `AdminBootstrapper` керують обліковими записами, не знаючи про ASP.NET Identity ✅ |
| 3 | Динамічний (віртуальний метод) | `PasswordRule.Check` через `IPasswordValidator<AccountUser>` | `PasswordRule.ValidateAsync` однаково перетворює відмову будь-якого правила на помилку Identity; `IdentityAccountStore.SetPasswordAsync` перебирає всі валідатори, не знаючи, які це правила ✅ |
| 4 | Статичний (generics) | `MapUserChange<TRequest>`: тип тіла запиту — параметр типу | `MapUserEndpoints` будує ним ендпоінти з `RenameUserRequest` і `SetAdminRequest`; обробник, відповідь і 404 описані один раз ✅ |
| 5 | Динамічний (абстрактні властивості) | `AuditTarget.Kind`, `Id`, `Name`, `EnvironmentId`, `StackId` | `AuditEntry.Record` однаково записує будь-яку ціль: користувача, роль, токен, середовище, стек, контейнер, файл; фільтр за середовищем працює для всіх ✅ |
| 6 | Динамічний (декоратор) | `IContainerCommand`: `AuditedCommand` навколо `AuthorizedCommand` навколо команди | `ContainerCommandService` виконує обгорнуту команду через той самий інтерфейс, а запис у журнал і перевірка прав не знають, яка це команда ✅ |
| 7 | Динамічний (віртуальна властивість) | `UserAccountAction.AuditAction`, `AuditDetails` | `UserManagementService.RunAsync` записує кожну з семи дій під її власною дією аудиту, не розрізняючи їх ✅ |
| 8 | Статичний (generics) | `IAuditTrail.RunAsync<T>`: тип результату дії — параметр типу | сервіси записують дії з різними результатами (`UserSummary`, `RoleAssignmentView`, `CreatedApiToken`, `StackCommandResult`, `byte[]`) одним методом, результат повертається без приведення типів ✅ |

#### Тести (не входять в облік)

- `tests/Aggregator/Api/AuthTests.cs`, 12 тестів: вхід і `/api/me`, атрибути cookie, `RememberMe`, неправильний пароль, невідоме ім'я з тією самою відповіддю, деактивований користувач, lockout, logout, зміна пароля завершує інші сесії й лишає поточну, неправильний поточний пароль, закороткий новий пароль із причиною, ліміт спроб входу з однієї адреси. Адміністратора тестів створює `AdminBootstrapper`.
- `tests/Aggregator/Application/Access/UserManagementServiceTests.cs`, 20 тестів: лише адміністратори, список із блокуваннями, створення й відкат при відхиленому паролі, зайняте ім'я, останній активний адміністратор, які дії завершують сесії, перейменування, розблокування, ролі на середовищі й стеку, одна роль на область, чужий стек і чуже призначення.
- `tests/Aggregator/Infrastructure/Accounts/PasswordRulesTests.cs`, 19 тестів: повтори, послідовності й рядки клавіатури, звичайні паролі проходять, ім'я користувача й назва системи, короткі імена не шукаються, межа довжини, відмова правила → помилка Identity з кодом `Password…`.
- `tests/Aggregator/Domain/Audit/AuditEntryTests.cs`, 7 тестів: розділ кожної дії, копія актора, область цілі стеку й контейнера, деталі одним рядком і з межею довжини, порожні деталі, невідомі значення відхиляються.
- `tests/Aggregator/Application/Audit/AuditTrailTests.cs`, 8 тестів: успіх із ціллю, знайденою під час дії, відмова й повторний кидок, невідома помилка без повідомлення, невдача як результат дії, неавтентифікований запит не записується, канали токена й бота (бот без адреси), вхід без сесії, недоступний журнал не ламає дію. Нові тести в `UserManagementServiceTests` (кожна зміна під своєю дією, відмова), `ContainerCommandServiceTests` (відмовлена спроба з назвою контейнера) і `StackCommandServiceTests` (невдалий compose — «не вдалося»).
- `tests/Aggregator/Api/AuditTests.cs`, 7 тестів через HTTP: входи й невдалі входи з наданим ім'ям і без паролів у журналі, хто керував користувачами, відмовлена спроба, журнал лише для адміністраторів, фільтри, пошук і сторінки, токен у журналі без самого токена й канал запиту з токеном, видалення лише записів, старших за межу.
- `tests/Aggregator/Api/UserManagementTests.cs`, 12 тестів через HTTP: легкий для вгадування пароль відхиляється з усіма причинами, створений користувач входить, 403 для не-адміністратора, 409 і 400 при створенні, останній адміністратор, деактивація й скидання пароля завершують сесію, перейменування, блокування й розблокування, роль відкриває приватне середовище і після відкликання ховає його.

### Лаба 2

| Вимога | Потрібно | Є | Стан |
|---|---|---|---|
| Екрани | ≥ 4 | 5 | ✅ |
| Елементи керування | ≥ 20 | 61 | ✅ |
| Контейнерні елементи | ≥ 1 | 3 (таблиці) | ✅ |
| Обробники подій | ≥ 10 | 46 | ✅ |
| Логіка окремо від GUI | так | так | ✅ |

Код: `src/Web` — вхід (PR #30), керування користувачами й власний обліковий запис (PR #41), форма видачі ролі (PR #58), журнал аудиту, хвойна тема, сезонна анімація й PWA (PR «feat(web): conifer theme, seasonal backdrop and PWA»). Згенеровані компоненти `src/components/ui` (`input`, `label`, `checkbox`, `card`, `table`, `dialog`, `alert-dialog`, `select`, `badge`) в облік не йдуть.

- **Екрани:**
  1. вхід (`src/routes/login.tsx`, `/login`);
  2. користувачі (`src/routes/_authed/users/index.tsx`, `/users`);
  3. картка користувача з ролями (`src/routes/_authed/users/$userId.tsx`, `/users/$userId`);
  4. мій обліковий запис (`src/routes/_authed/account.tsx`, `/account`);
  5. журнал аудиту (`src/routes/_authed/audit.tsx`, `/audit`).
- **Елементи керування (61):**
  - вхід: поле імені, поле пароля, прапорець «Запам'ятати мене», кнопка «Увійти»;
  - шапка (`src/routes/_authed.tsx`): кнопка «Вийти», посилання «Користувачі» (лише для адміністраторів), посилання на власний обліковий запис;
  - користувачі: поле пошуку, список «Стан», кнопка «Новий користувач», посилання на картку в кожному рядку; діалог «Новий користувач» (`src/components/users/create-user-dialog.tsx`): поле імені, поле пароля, прапорець «Адміністратор», кнопки «Скасувати» й «Створити»;
  - картка: посилання «Усі користувачі», поле імені й кнопка «Зберегти», кнопки «Розблокувати», «Деактивувати» / «Активувати», «Зробити адміністратором» / «Зняти права адміністратора», «Скинути пароль»; діалог скидання пароля (`src/components/users/reset-password-dialog.tsx`): поле нового пароля, кнопки «Скасувати» й «Задати пароль»; діалог підтвердження (`src/components/confirm-action.tsx`): кнопки підтвердження й «Скасувати»; у таблиці ролей — список ролі й кнопка «Відкликати» в кожному рядку; форма «Видати роль» (`src/components/users/grant-role-form.tsx`): списки «Середовище», «Область» (усе середовище або стек) і «Роль», кнопка «Видати»;
  - мій обліковий запис: поля поточного пароля, нового пароля й повтору, кнопка «Змінити пароль»; у картці «Вигляд» (`src/components/seasons/season-card.tsx`) — список «Сезонна анімація» (за календарем, один із сезонів, вимкнено); у картці «Застосунок» (`src/components/pwa/install-card.tsx`) — кнопка «Встановити»;
  - екран «Немає з'єднання» (`src/components/layout/connection-error.tsx`): кнопка «Спробувати знову»;
  - журнал аудиту: посилання «Журнал аудиту» в бічній панелі (лише адміністраторам), списки «Розділ», «Результат», «Хто», «Середовище», поля дат «З дня» й «По день», поле пошуку й кнопка «Знайти», «Скинути фільтри», «Оновити», ім'я автора в рядку (фільтр за ним), посилання на об'єкт у рядку (користувач, середовище, стек, логи контейнера), «Попередні» й «Наступні»;
  - навігація й дизайн панелі (`src/components/layout/app-sidebar.tsx`, `src/routes/_authed.tsx`; PR «feat(web): sidebar navigation, clickable lists, stack quick actions»): перемикач середовища в бічній панелі, кнопка «Меню» на телефоні; швидкі дії стеку в списку стеків (`src/components/stacks/stack-quick-actions.tsx`) — «Розгорнути», «Зупинити» (down), «Перезапустити». Посилання бічної панелі замінили посилання шапки, які інші учасники вже зарахували собі, тому тут вони не рахуються.
- **Контейнерні елементи:** таблиця користувачів, таблиця ролей користувача, таблиця журналу аудиту.
- **Обробники подій (46):**
  - вхід: надсилання форми, зміна прапорця, «Вийти»;
  - користувачі: введення в пошук і вибір стану (фільтри в URL через `validateSearch`, `replace`), відкриття й закриття діалогу (скидання форми й помилки), надсилання форми нового користувача (перехід на його картку);
  - картка: надсилання перейменування, підтвердження деактивації, «Активувати», підтвердження зняття прав адміністратора, «Зробити адміністратором», «Розблокувати», відкриття й закриття діалогу скидання пароля, надсилання нового пароля, вибір ролі (зміна ролі), підтвердження відкликання ролі; у формі видачі ролі — вибір середовища (підвантажує його стеки й скидає область), вибір області, вибір ролі, надсилання (роль з'являється в таблиці, форма очищається);
  - мій обліковий запис: надсилання форми, зміна будь-якого поля (ховає попередній результат), вибір сезонної анімації (фон змінюється одразу, вибір запам'ятовується в браузері), «Встановити» (діалог встановлення браузера; відмова показує підказку про меню браузера);
  - екран «Немає з'єднання»: «Спробувати знову» і повернення мережі (подія `online` — сторінка перезавантажується сама);
  - журнал аудиту: вибір розділу, результату, користувача й середовища, зміна дати «з» і «по» (фільтри в URL, сторінка скидається на першу), введення в пошук і надсилання пошуку (запит до ЦА лише на Enter або «Знайти»), «Скинути фільтри» (очищає й поле пошуку), «Оновити», клік на ім'я автора (фільтр за ним), «Попередні» й «Наступні» (сторінка в URL, попередня сторінка лишається на екрані, доки вантажиться наступна);
  - навігація й дизайн: зміна середовища в бічній панелі (запам'ятовується, відкривається той самий розділ іншого середовища), відкриття й закриття меню на телефоні, вибір пункту меню (закриває меню), швидкий деплой стеку (рядок результату з посиланням на деплой), підтвердження down і restart стеку (результат над списком, список контейнерів оновлюється).
- **Логіка окремо від GUI:**
  - запити й робота з кешем — `src/api/queries.ts` (`usersQuery`, `userQuery`; у формі видачі ролі — наявні `environmentsQuery` і `stacksQuery`), `src/api/users.ts` (мутації користувача й ролей, зокрема `useGrantRoleMutation`, з інвалідацією списку й картки), `src/api/auth.ts` (`useChangePasswordMutation`);
  - стан користувача й фільтрація — `src/lib/users.ts` (`userStatus`, `filterUsers`, `takenScopes` — області, де роль уже є, правило імені як у ЦА), формат дат — `src/lib/format.ts`;
  - правила форм — схеми Zod, помилки ЦА за статусом → перекладені повідомлення, причини відхилення пароля — `src/components/form-messages.tsx`;
  - журнал аудиту — `src/features/audit/model.ts`: схема фільтрів у URL, запит до ЦА з фільтрів (дні — локальні, «по день» включно), діапазон сторінки, посилання на об'єкт запису, вигляд результату; `auditQuery` у `src/api/queries.ts`; `formatTimestamp` у `src/lib/format.ts`;
  - навігація — `src/features/navigation/model.ts`: розділ сторінки, середовище сторінки (параметр шляху → search param → середовище стеку), вибір поточного середовища (сторінка → запам'ятоване → перше), збереження в `localStorage`; клікабельні рядки — класи `src/lib/row-link.ts`;
  - дизайн — хвойна палітра світлої й темної тем (`src/styles.css`), логотип `src/components/layout/brand.tsx`; сезонний фон — `src/features/seasons/model.ts`: сезон за датою, вибір у `localStorage`, вигляд частинок кожного сезону, їх поява й рух; малює їх `src/components/seasons/seasonal-backdrop.tsx` на canvas позаду вмісту (без кліків, `aria-hidden`, пауза в прихованій вкладці, вимкнено за `prefers-reduced-motion`); PWA — `src/features/pwa/model.ts`: стан встановлення (встановлено, можна встановити, iOS, лише меню браузера), подія `beforeinstallprompt`, реєстрація service worker; сам service worker — `pwa/sw.js` з плагіном збірки в `vite.config.ts`.


### Лаба 3

| Бібліотека | Що використано | Ретроспектива |
|---|---|---|
| React Hook Form 7.89.0 (з `@hookform/resolvers`) | `useForm` з `zodResolver`, `register` для полів (зокрема з власним `onChange`), `Controller` для прапорця Radix (не нативний `input`), `formState.errors` і `isDirty`, `handleSubmit`, `reset` після закриття діалогу й успішної зміни пароля | [docs/retro/react-hook-form.md](docs/retro/react-hook-form.md) |
| Zod 4.6.5 | Схеми форм входу, нового користувача, перейменування, скидання й зміни пароля (`z.object`, `trim`, `min`, `regex` з правилом імені як у ЦА, `refine` для повтору пароля з `path`), повідомлення — ключі перекладу; `z.enum` і `catch` для search params списку користувачів; `z.infer` для типів форм | [docs/retro/zod.md](docs/retro/zod.md) |
| TanStack Query | `placeholderData: keepPreviousData` для сторінок журналу аудиту; `useMutation` для входу, виходу, зміни пароля й усіх змін користувача; `setQueryData` і `removeQueries` для кешу поточного користувача; `useSuspenseQuery` з `queryOptions`-фабриками (`usersQuery`, `userQuery(id)`); `invalidateQueries` за префіксом ключа, щоб зміна користувача оновлювала і список, і картку | [docs/retro/tanstack-query.md](docs/retro/tanstack-query.md) |
| TanStack Router | `loaderDeps` журналу аудиту (запит залежить від search params), `Route.useSearch` для `redirect` і фільтрів, `validateSearch` зі схемою Zod, `useNavigate` (`href`, `replace`, функція оновлення `search`), `loader` з `ensureQueryData`, параметри маршруту (`$userId`), `errorComponent` для 403 і 404 | [docs/retro/tanstack-router.md](docs/retro/tanstack-router.md) |
| ASP.NET Core Identity (`Microsoft.AspNetCore.Identity.EntityFrameworkCore`) | `IdentityUserContext<TUser, TKey>` у наявному `DbContext` без таблиць ролей; `UserManager` (створення облікового запису, зміна пароля, `UpdateAsync` для імені й розблокування, `UpdateSecurityStampAsync` для завершення сесій, `PasswordValidators` і `PasswordHasher` для пароля від адміністратора); власні `IPasswordValidator` (`AddPasswordValidator`) для правил NIST; `SignInManager` (`CheckPasswordSignInAsync` з lockout, `SignInAsync` з постійною cookie, `RefreshSignInAsync`, `SignOutAsync`); `PasswordHasher`; `IdentityOptions` (політика паролів, lockout); security stamp і `SecurityStampValidatorOptions`; `ConfigureApplicationCookie` | [docs/retro/aspnet-identity.md](docs/retro/aspnet-identity.md) |
| EF Core | Міграції `Accounts` для таблиць Identity і `AuditLog`; конфігурація журналу (`IEntityTypeConfiguration`, переліки як рядки `HasConversion<string>`, складені індекси, без зовнішніх ключів); запит журналу (`AsNoTracking`, `CountAsync`, `Skip`/`Take`, `EF.Functions.Like` з символом екранування); запис у власному `DbContext` з `IServiceScopeFactory` | [docs/retro/ef-core.md](docs/retro/ef-core.md) |
