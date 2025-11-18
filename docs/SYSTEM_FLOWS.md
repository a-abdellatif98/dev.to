# System Flow Diagrams

This document provides visual flow diagrams for major systems in the Forem/DEV.to codebase.

## Table of Contents

1. [User Authentication Flow](#user-authentication-flow)
2. [Article Publishing Flow](#article-publishing-flow)
3. [Comment Creation Flow](#comment-creation-flow)
4. [Notification System Flow](#notification-system-flow)
5. [User Deletion Flow (GDPR)](#user-deletion-flow-gdpr)
6. [Search Flow](#search-flow)
7. [Moderation Flow](#moderation-flow)
8. [Email Digest Flow](#email-digest-flow)

---

## User Authentication Flow

```mermaid
flowchart TD
    Start([User Clicks Sign In]) --> Provider{Choose Provider}
    Provider -->|GitHub| GitHub[GitHub OAuth]
    Provider -->|Twitter| Twitter[Twitter OAuth]
    Provider -->|Google| Google[Google OAuth]
    Provider -->|Apple| Apple[Apple OAuth]

    GitHub --> OAuthCallback[OAuth Callback]
    Twitter --> OAuthCallback
    Google --> OAuthCallback
    Apple --> OAuthCallback

    OAuthCallback --> OmniauthController[OmniauthCallbacksController]
    OmniauthController --> Authenticator[Authentication::Authenticator]

    Authenticator --> CheckUser{User Exists?}
    CheckUser -->|No| CreateUser[Create New User]
    CheckUser -->|Yes| UpdateUser[Update Existing User]

    CreateUser --> CreateIdentity[Create Identity Record]
    UpdateUser --> UpdateIdentity[Update Identity Record]
    CreateIdentity --> CheckSuspended{User Suspended?}
    UpdateIdentity --> CheckSuspended

    CheckSuspended -->|Yes| Error[Suspended User Error]
    CheckSuspended -->|No| SignIn[Sign In via Devise]

    SignIn --> SetSession[Set Session]
    SetSession --> Redirect{First Time?}
    Redirect -->|Yes| Onboarding[Onboarding Flow]
    Redirect -->|No| Dashboard[User Dashboard]

    Onboarding --> Dashboard

    style CreateUser fill:#4ecdc4
    style SignIn fill:#95e1d3
    style Error fill:#ff6b6b
```

**Key Components**:
* `OmniauthCallbacksController` - Handles OAuth callbacks
* `Authentication::Authenticator` - Processes auth payload
* `Authentication::Providers::*` - Provider-specific logic
* `Users::CreateFromAuthentication` - User creation logic

---

## Article Publishing Flow

```mermaid
flowchart TD
    Start([User Creates Article]) --> Form[Article Form Submission]
    Form --> ArticlesController[ArticlesController#create]
    ArticlesController --> RateLimit{Rate Limit OK?}
    RateLimit -->|No| RateLimitError[429 Too Many Requests]
    RateLimit -->|Yes| ArticlesCreator[Articles::Creator]

    ArticlesCreator --> Validate{Valid?}
    Validate -->|No| ValidationError[Show Validation Errors]
    Validate -->|Yes| CreateDraft[Create Draft Article]

    CreateDraft --> Publish{Publish Now?}
    Publish -->|No| SaveDraft[Save as Draft]
    Publish -->|Yes| EnqueueWorker[Enqueue Articles::PublishWorker]

    SaveDraft --> Success[Success Response]

    EnqueueWorker --> WorkerStart[PublishWorker Starts]
    WorkerStart --> SpamCheck[Spam Detection Check]
    SpamCheck --> SpamResult{Spam?}

    SpamResult -->|Yes| MarkSpam[Mark as Spam]
    SpamResult -->|No| PublishArticle[Publish Article]

    PublishArticle --> IndexAlgolia[Index to Algolia]
    IndexAlgolia --> BustCache[Enqueue Cache Bust Workers]
    BustCache --> SendNotifications[Send Notifications to Followers]
    SendNotifications --> UpdateFeed[Update Feed Events]
    UpdateFeed --> Success

    MarkSpam --> ModerationQueue[Add to Moderation Queue]
    ModerationQueue --> Success

    style PublishArticle fill:#4ecdc4
    style MarkSpam fill:#ff6b6b
    style Success fill:#95e1d3
```

**Key Components**:
* `ArticlesController` - HTTP endpoint
* `Articles::Creator` - Creation logic
* `Articles::PublishWorker` - Async publishing
* `Articles::BustCacheWorker` - Cache invalidation
* `Notifications::Create` - Notification creation

---

## Comment Creation Flow

```mermaid
flowchart TD
    Start([User Submits Comment]) --> CommentsController[CommentsController#create]
    CommentsController --> RateLimit{Rate Limit OK?}
    RateLimit -->|No| RateLimitError[429 Error]
    RateLimit -->|Yes| CommentCreator[CommentCreator Service]

    CommentCreator --> Validate{Valid?}
    Validate -->|No| ValidationError[Show Errors]
    Validate -->|Yes| CreateComment[Create Comment Record]

    CreateComment --> SpamCheck[Spam Detection]
    SpamCheck --> SpamResult{Spam?}

    SpamResult -->|Yes| HandleSpam[Handle Spam Comment]
    SpamResult -->|No| PublishComment[Publish Comment]

    HandleSpam --> ModerationQueue[Add to Moderation]
    ModerationQueue --> NotifyModerators[Notify Moderators]

    PublishComment --> CalculateScore[Calculate Comment Score]
    CalculateScore --> BustCache[Enqueue Cache Bust]
    BustCache --> SendNotifications[Send Notifications]

    SendNotifications --> NotifyAuthor[Notify Article Author]
    SendNotifications --> NotifyMentions[Notify Mentioned Users]
    SendNotifications --> NotifySubscribers[Notify Subscribers]

    NotifyAuthor --> Success[Success Response]
    NotifyMentions --> Success
    NotifySubscribers --> Success

    style PublishComment fill:#4ecdc4
    style HandleSpam fill:#ff6b6b
    style Success fill:#95e1d3
```

**Key Components**:
* `CommentsController` - HTTP endpoint
* `CommentCreator` - Comment creation service
* `Comments::CalculateScore` - Score calculation
* `Comments::BustCacheWorker` - Cache invalidation
* `Notifications::Create` - Notification creation

---

## Notification System Flow

```mermaid
flowchart TD
    Event([Event Occurs]) --> EventType{Event Type}

    EventType -->|Reaction| ReactionWorker[Notifications::NewReactionWorker]
    EventType -->|Comment| CommentWorker[Notifications::MentionWorker]
    EventType -->|Follow| FollowWorker[Notifications::NewFollowerWorker]
    EventType -->|Article| ArticleWorker[Notifications::NotifiableActionWorker]
    EventType -->|Badge| BadgeWorker[Notifications::NewBadgeAchievementWorker]

    ReactionWorker --> NotificationsCreate[Notifications::Create]
    CommentWorker --> NotificationsCreate
    FollowWorker --> NotificationsCreate
    ArticleWorker --> NotificationsCreate
    BadgeWorker --> NotificationsCreate

    NotificationsCreate --> CheckPreferences{User Preferences?}
    CheckPreferences -->|Disabled| Skip[Skip Notification]
    CheckPreferences -->|Enabled| CreateRecord[Create Notification Record]

    CreateRecord --> CheckEmail{Email Enabled?}
    CheckEmail -->|Yes| EnqueueEmail[Enqueue Email Worker]
    CheckEmail -->|No| CheckPush{Push Enabled?}

    EnqueueEmail --> EmailWorker[Emails::SendNotificationWorker]
    EmailWorker --> SendEmail[Send Email]

    CheckPush -->|Yes| EnqueuePush[Enqueue Push Worker]
    CheckPush -->|No| Complete[Complete]

    EnqueuePush --> PushWorker[PushNotifications::DeliverWorker]
    PushWorker --> SendPush[Send Push Notification]

    SendEmail --> Complete
    SendPush --> Complete
    Skip --> Complete

    style CreateRecord fill:#4ecdc4
    style Complete fill:#95e1d3
```

**Key Components**:
* `Notifications::Create` - Notification creation service
* `Notifications::*Worker` - Various notification workers
* `Emails::SendNotificationWorker` - Email notifications
* `PushNotifications::DeliverWorker` - Push notifications
* `Notification` model - Notification records

---

## User Deletion Flow (GDPR)

See `GDPR_DELETE_REQUEST_FLOW.md` for detailed documentation.

```mermaid
flowchart TD
    Start([User Requests Deletion]) --> RequestDestroy[UsersController#request_destroy]
    RequestDestroy --> GenerateToken[Generate Deletion Token]
    GenerateToken --> SendEmail[Send Confirmation Email]
    SendEmail --> UserConfirms[User Confirms Deletion]

    UserConfirms --> EnqueueWorker[Enqueue Users::DeleteWorker]
    EnqueueWorker --> WorkerStart[DeleteWorker Starts]

    WorkerStart --> UsersDelete[Users::Delete.call]
    UsersDelete --> DeleteComments[Delete Comments]
    DeleteComments --> DeleteArticles[Delete Articles]
    DeleteArticles --> DeleteActivity[Delete User Activity]
    DeleteActivity --> CancelStripe[Cancel Stripe Subscriptions]
    CancelStripe --> RemoveMailchimp[Remove from Mailchimp]
    RemoveMailchimp --> DestroyUser[Destroy User Record]

    DestroyUser --> CreateGDPR[Create GDPRDeleteRequest]
    CreateGDPR --> SendDeletionEmail[Send Deletion Confirmation]
    SendDeletionEmail --> AdminPhase[Admin Confirmation Phase]

    AdminPhase --> AdminView[Admin Views Requests]
    AdminView --> AdminConfirm[Admin Confirms Deletion]
    AdminConfirm --> DestroyGDPR[Destroy GDPRDeleteRequest]
    DestroyGDPR --> CreateAuditLog[Create AuditLog]
    CreateAuditLog --> Complete[Complete]

    style DestroyUser fill:#ff6b6b
    style CreateGDPR fill:#4ecdc4
    style Complete fill:#95e1d3
```

---

## Search Flow

```mermaid
flowchart TD
    Start([User Searches]) --> SearchController[SearchController]
    SearchController --> SearchType{Search Type}

    SearchType -->|Articles| ArticleSearch[Article Search]
    SearchType -->|Users| UserSearch[User Search]
    SearchType -->|Tags| TagSearch[Tag Search]

    ArticleSearch --> AlgoliaQuery[Query Algolia API]
    UserSearch --> AlgoliaQuery
    TagSearch --> DatabaseQuery[Query PostgreSQL]

    AlgoliaQuery --> AlgoliaAPI[Algolia Search API]
    AlgoliaAPI --> Results[Search Results]

    DatabaseQuery --> PgSearch[PgSearch Full Text]
    PgSearch --> Results

    Results --> FormatResults[Format Results]
    FormatResults --> Render[Render Results Page]

    style AlgoliaQuery fill:#4ecdc4
    style Results fill:#95e1d3
```

**Key Components**:
* `SearchController` - Search endpoint
* `AlgoliaSearchable` concern - Algolia integration
* `PgSearch` - PostgreSQL full-text search
* `ArticleApiIndexService` - Algolia indexing

---

## Moderation Flow

```mermaid
flowchart TD
    Content([Content Created/Updated]) --> AutoMod[Auto-Moderation Check]
    AutoMod --> SpamCheck[Spam Detection Service]
    SpamCheck --> Result{Spam Detected?}

    Result -->|Yes| MarkSpam[Mark as Spam]
    Result -->|No| UserFlag{User Flagged?}

    UserFlag -->|Yes| CreateReport[Create FeedbackMessage]
    UserFlag -->|No| Publish[Publish Content]

    MarkSpam --> ModerationQueue[Add to Moderation Queue]
    CreateReport --> ModerationQueue

    ModerationQueue --> ModeratorView[Moderator Views Queue]
    ModeratorView --> ModeratorAction{Moderator Action}

    ModeratorAction -->|Approve| Approve[Approve Content]
    ModeratorAction -->|Reject| Reject[Reject Content]
    ModeratorAction -->|Ban User| BanUser[Ban User]

    Approve --> NotifyUser[Notify User]
    Reject --> NotifyUser
    BanUser --> BanUserWorker[Moderator::BanishUserWorker]

    BanUserWorker --> DeleteContent[Delete User Content]
    DeleteContent --> NotifyUser

    NotifyUser --> AuditLog[Create AuditLog]
    AuditLog --> Complete[Complete]

    Publish --> Complete

    style MarkSpam fill:#ff6b6b
    style Approve fill:#4ecdc4
    style Complete fill:#95e1d3
```

**Key Components**:
* `ModerationsController` - Moderation interface
* `Moderator::*` services - Moderation actions
* `Spam::*` services - Spam detection
* `FeedbackMessage` model - User reports
* `Moderator::BanishUserWorker` - User banning

---

## Email Digest Flow

```mermaid
flowchart TD
    Schedule([Scheduled: Daily/Weekly]) --> EnqueueDigest[Enqueue Digest Worker]
    EnqueueDigest --> DigestWorker[Emails::EnqueueDigestWorker]

    DigestWorker --> GetUsers[Get Users for Digest]
    GetUsers --> FilterUsers{Filter Users}

    FilterUsers --> EmailEnabled{Email Enabled?}
    FilterUsers --> DigestEnabled{Digest Enabled?}
    FilterUsers --> RecentActivity{Recent Activity?}

    EmailEnabled -->|No| Skip[Skip User]
    DigestEnabled -->|No| Skip
    RecentActivity -->|No| Skip

    EmailEnabled -->|Yes| CollectArticles[Collect Articles]
    DigestEnabled -->|Yes| CollectArticles
    RecentActivity -->|Yes| CollectArticles

    CollectArticles --> ArticleCollector[EmailDigestArticleCollector]
    ArticleCollector --> GetFeed[Get User Feed]
    GetFeed --> FilterArticles[Filter & Rank Articles]
    FilterArticles --> SelectTop[Select Top Articles]

    SelectTop --> EnqueueEmail[Enqueue Email Worker]
    EnqueueEmail --> SendDigestWorker[Emails::SendUserDigestWorker]

    SendDigestWorker --> RenderEmail[Render Email Template]
    RenderEmail --> SendEmail[Send via Mailer]
    SendEmail --> TrackOpen[Track Email Open]
    TrackOpen --> Complete[Complete]

    Skip --> Complete

    style CollectArticles fill:#4ecdc4
    style SendEmail fill:#95e1d3
    style Complete fill:#95e1d3
```

**Key Components**:
* `Emails::EnqueueDigestWorker` - Digest scheduling
* `Emails::SendUserDigestWorker` - Individual digest sending
* `EmailDigestArticleCollector` - Article collection logic
* `EmailDigest` service - Digest generation

---

## Key Takeaways

### Common Patterns Across Flows

1. **Rate Limiting**: Most user actions check rate limits first
2. **Spam Detection**: Content creation includes spam checks
3. **Async Processing**: Heavy operations use background workers
4. **Cache Busting**: Content changes trigger cache invalidation
5. **Notifications**: Events trigger notification creation
6. **Error Handling**: Comprehensive error handling throughout
7. **Audit Logging**: Important actions create audit logs

### Performance Considerations

* **Background Jobs**: Heavy operations are async
* **Caching**: Aggressive caching at multiple layers
* **Database Optimization**: Efficient queries and indexing
* **CDN**: Edge caching for static content
* **Rate Limiting**: Prevents abuse and overload

### Security Considerations

* **Authentication**: OAuth and token-based auth
* **Authorization**: Policy-based access control
* **Rate Limiting**: Prevents abuse
* **Spam Detection**: Content filtering
* **Input Validation**: Comprehensive validation
* **Audit Logging**: Track important actions

---

## How to Use These Diagrams

1. **Understanding Flow**: Use these diagrams to understand how features work end-to-end
2. **Debugging**: Trace through flows when debugging issues
3. **Onboarding**: Help new developers understand the system
4. **Architecture Decisions**: Reference when making architectural changes
5. **Documentation**: Keep diagrams updated as code changes

---

## Updating These Diagrams

When making changes to flows:
1. Update the relevant diagram
2. Update the component list
3. Update the key takeaways if patterns change
4. Keep diagrams synchronized with code
