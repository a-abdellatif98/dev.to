# Senior Rails Developer Learning Path

This document provides a structured learning path for developers who want to grow their Rails expertise by studying the Forem/DEV.to codebase.

## Table of Contents

1. [Learning Philosophy](#learning-philosophy)
2. [Prerequisites](#prerequisites)
3. [Learning Path](#learning-path)
4. [Deep Dive Topics](#deep-dive-topics)
5. [Practical Exercises](#practical-exercises)
6. [Code Review Checklist](#code-review-checklist)

---

## Learning Philosophy

### How to Approach This Codebase

1. **Start Small**: Begin with simple features and work your way up
2. **Read, Don't Just Code**: Spend time reading existing code before writing new code
3. **Follow the Flow**: Trace requests from controller → service → model → database
4. **Ask Why**: Understand the reasoning behind architectural decisions
5. **Write Tests**: Writing tests helps you understand how code works
6. **Refactor**: Improve existing code to learn patterns
7. **Review PRs**: Study pull requests to see how others solve problems

### Learning Mindset

* **Curiosity**: Always ask "why is it done this way?"
* **Patience**: Complex systems take time to understand
* **Practice**: Apply what you learn by building features
* **Teaching**: Explain concepts to others to solidify understanding

---

## Prerequisites

### Required Knowledge

* **Ruby**: Strong understanding of Ruby syntax and idioms
* **Rails**: Solid grasp of Rails MVC, ActiveRecord, routing
* **SQL**: Understanding of database queries and optimization
* **Git**: Version control basics
* **HTTP**: Understanding of web protocols

### Recommended Knowledge

* **Design Patterns**: Service objects, decorators, strategies
* **Background Jobs**: Sidekiq or similar
* **Caching**: Redis, CDN concepts
* **Testing**: RSpec, FactoryBot
* **API Design**: REST principles

---

## Learning Path

### Phase 1: Foundation (Weeks 1-2)

**Goal**: Understand the basic structure and patterns

#### Week 1: Project Setup & Exploration

**Tasks**:
1. Set up the development environment
2. Explore the directory structure
3. Read `README.md` and `SYSTEM_ARCHITECTURE.md`
4. Run the test suite
5. Browse the codebase structure

**Key Files to Read**:
* `config/routes.rb` - Understand routing structure
* `app/controllers/application_controller.rb` - Base controller
* `app/models/user.rb` - Core user model
* `app/models/article.rb` - Core article model

**Learning Objectives**:
* Understand project structure
* Identify main components
* Navigate the codebase efficiently
* Run tests and understand test structure

#### Week 2: Basic Patterns

**Tasks**:
1. Study service object pattern
2. Study policy pattern (Pundit)
3. Study worker pattern (Sidekiq)
4. Read 5-10 service objects
5. Read 3-5 policies
6. Read 3-5 workers

**Key Files to Study**:
* `app/services/application_service.rb` - Base service
* `app/services/users/delete.rb` - Complex service example
* `app/policies/application_policy.rb` - Base policy
* `app/workers/users/delete_worker.rb` - Worker example

**Learning Objectives**:
* Understand service object pattern
* Understand authorization patterns
* Understand background job patterns
* Recognize when to use each pattern

**Practical Exercise**:
Create a simple service object for a new feature (e.g., `Users::UpdateProfile`)

---

### Phase 2: Core Features (Weeks 3-4)

**Goal**: Understand how core features work

#### Week 3: Authentication & User Management

**Tasks**:
1. Study authentication flow
2. Read OAuth integration code
3. Study user creation/update flows
4. Understand role management (Rolify)
5. Study user deletion flow (GDPR)

**Key Files to Study**:
* `app/controllers/omniauth_callbacks_controller.rb`
* `app/services/authentication/authenticator.rb`
* `app/services/users/delete.rb`
* `app/workers/users/delete_worker.rb`
* `GDPR_DELETE_REQUEST_FLOW.md`

**Learning Objectives**:
* Understand OAuth integration
* Understand user lifecycle management
* Understand GDPR compliance
* Understand role-based access control

**Practical Exercise**:
Add a new OAuth provider or implement a user feature

#### Week 4: Content Management

**Tasks**:
1. Study article creation flow
2. Study comment system
3. Understand content moderation
4. Study search implementation
5. Understand caching strategies

**Key Files to Study**:
* `app/services/articles/creator.rb`
* `app/services/articles/updater.rb`
* `app/services/comment_creator.rb`
* `app/services/edge_cache/bust.rb`
* `app/models/concerns/algolia_searchable.rb`

**Learning Objectives**:
* Understand content creation workflows
* Understand moderation systems
* Understand search implementation
* Understand caching strategies

**Practical Exercise**:
Add a new content type or improve search functionality

---

### Phase 3: Advanced Patterns (Weeks 5-6)

**Goal**: Master advanced architectural patterns

#### Week 5: Background Jobs & Async Processing

**Tasks**:
1. Study worker organization
2. Understand job queues and priorities
3. Study retry strategies
4. Understand job dependencies
5. Study error handling in workers

**Key Files to Study**:
* `app/workers/articles/publish_worker.rb`
* `app/workers/notifications/*` - Various notification workers
* `config/sidekiq.yml` - Sidekiq configuration
* `lib/sidekiq/*` - Custom Sidekiq extensions

**Learning Objectives**:
* Understand async processing patterns
* Understand job prioritization
* Understand error handling and retries
* Understand job idempotency

**Practical Exercise**:
Create a new worker for a complex async operation

#### Week 6: Caching & Performance

**Tasks**:
1. Study caching layers (Redis, Fastly, Memory)
2. Understand cache invalidation strategies
3. Study database query optimization
4. Understand N+1 prevention
5. Study performance monitoring

**Key Files to Study**:
* `app/services/edge_cache/bust.rb`
* `app/lib/memory_first_cache.rb`
* `app/models/` - Counter caches, scopes
* `app/controllers/concerns/caching_headers.rb`

**Learning Objectives**:
* Understand multi-layer caching
* Understand cache invalidation
* Understand query optimization
* Understand performance monitoring

**Practical Exercise**:
Optimize a slow query or improve caching for a feature

---

### Phase 4: Specialized Topics (Weeks 7-8)

**Goal**: Deep dive into specialized areas

#### Week 7: API Design & Versioning

**Tasks**:
1. Study API structure
2. Understand API versioning strategy
3. Study serialization patterns
4. Understand rate limiting
5. Study API documentation

**Key Files to Study**:
* `app/controllers/api/v0/`
* `app/controllers/api/v1/`
* `app/serializers/`
* `app/services/rate_limit_checker.rb`

**Learning Objectives**:
* Understand RESTful API design
* Understand API versioning
* Understand serialization
* Understand rate limiting

**Practical Exercise**:
Add a new API endpoint or version an existing endpoint

#### Week 8: Multi-tenancy & Subforems

**Tasks**:
1. Study subforem architecture
2. Understand request context management
3. Study domain-based routing
4. Understand data isolation
5. Study subforem-specific features

**Key Files to Study**:
* `app/models/subforem.rb`
* `app/controllers/application_controller.rb` - Subforem detection
* RequestStore usage throughout codebase

**Learning Objectives**:
* Understand multi-tenancy patterns
* Understand request context management
* Understand domain-based routing
* Understand data isolation strategies

**Practical Exercise**:
Add a subforem-specific feature or improve subforem handling

---

## Deep Dive Topics

### 1. Service Object Architecture

**Why Important**: 
Service objects are the backbone of business logic in this codebase. Understanding them is crucial.

**Study Path**:
1. Read `app/services/application_service.rb`
2. Study 10+ service objects
3. Identify patterns and anti-patterns
4. Understand when to use services vs. model methods
5. Study service composition patterns

**Key Questions to Answer**:
* When should logic go in a service vs. a model?
* How do services handle errors?
* How are services tested?
* How do services interact with each other?

**Recommended Reading**:
* `app/services/users/delete.rb` - Complex deletion logic
* `app/services/articles/creator.rb` - Multi-step creation
* `app/services/authentication/authenticator.rb` - Strategy pattern

**Practice**: Refactor a controller action into a service object

---

### 2. Background Job Patterns

**Why Important**:
Background jobs handle async processing, which is critical for performance and user experience.

**Study Path**:
1. Understand Sidekiq basics
2. Study worker organization patterns
3. Understand queue priorities
4. Study retry strategies
5. Understand job idempotency
6. Study error handling patterns

**Key Questions to Answer**:
* When should work be done async?
* How do you handle job failures?
* How do you prevent duplicate jobs?
* How do you test workers?

**Recommended Reading**:
* `app/workers/users/delete_worker.rb` - Complex async operation
* `app/workers/articles/publish_worker.rb` - Publishing pipeline
* `config/sidekiq.yml` - Queue configuration

**Practice**: Create a worker for a new async operation

---

### 3. Authorization & Security

**Why Important**:
Security is critical. Understanding authorization patterns helps you build secure features.

**Study Path**:
1. Understand Pundit policies
2. Study role-based access control (Rolify)
3. Understand policy scopes
4. Study security best practices
5. Understand rate limiting

**Key Questions to Answer**:
* How do policies work?
* How are roles managed?
* How do you test authorization?
* How do you prevent security vulnerabilities?

**Recommended Reading**:
* `app/policies/application_policy.rb` - Base policy
* `app/policies/article_policy.rb` - Resource policy
* `app/services/rate_limit_checker.rb` - Rate limiting

**Practice**: Create a policy for a new resource

---

### 4. Caching Strategies

**Why Important**:
Caching is essential for performance at scale. Understanding caching patterns helps optimize applications.

**Study Path**:
1. Understand caching layers (Fastly, Redis, Memory)
2. Study cache invalidation strategies
3. Understand cache keys
4. Study cache warming
5. Understand when to cache and when not to

**Key Questions to Answer**:
* What should be cached?
* How do you invalidate caches?
* How do you prevent cache stampedes?
* How do you test caching?

**Recommended Reading**:
* `app/services/edge_cache/bust.rb` - Cache invalidation
* `app/lib/memory_first_cache.rb` - Multi-layer caching
* `app/controllers/concerns/caching_headers.rb` - Cache headers

**Practice**: Implement caching for a slow endpoint

---

### 5. Database Optimization

**Why Important**:
Database performance is often the bottleneck. Understanding optimization techniques is crucial.

**Study Path**:
1. Understand query optimization
2. Study N+1 prevention
3. Understand database indexes
4. Study counter caches
5. Understand read replicas

**Key Questions to Answer**:
* How do you identify slow queries?
* How do you prevent N+1 queries?
* When should you use counter caches?
* How do you optimize complex queries?

**Recommended Reading**:
* `app/models/` - Model associations and scopes
* `app/queries/` - Query objects
* Counter cache usage throughout models

**Practice**: Optimize a slow query or fix an N+1 issue

---

### 6. Testing Patterns

**Why Important**:
Good tests enable confident refactoring and prevent regressions.

**Study Path**:
1. Understand RSpec patterns
2. Study factory usage
3. Understand test organization
4. Study integration tests
5. Understand E2E testing (Cypress)

**Key Questions to Answer**:
* How do you structure tests?
* How do you write maintainable tests?
* How do you test services?
* How do you test workers?
* How do you test policies?

**Recommended Reading**:
* `spec/services/` - Service specs
* `spec/workers/` - Worker specs
* `spec/policies/` - Policy specs
* `cypress/e2e/` - E2E tests

**Practice**: Write comprehensive tests for a new feature

---

## Practical Exercises

### Exercise 1: Create a Service Object

**Task**: Refactor a controller action into a service object

**Steps**:
1. Pick a controller action with business logic
2. Extract logic into a service object
3. Write tests for the service
4. Update controller to use service
5. Ensure tests still pass

**Example**: Create `Users::UpdateProfile` service

---

### Exercise 2: Add a Background Worker

**Task**: Create a worker for an async operation

**Steps**:
1. Identify an operation that should be async
2. Create a worker class
3. Configure queue and retry options
4. Write tests for the worker
5. Integrate into the application

**Example**: Create `Articles::GenerateSocialImageWorker`

---

### Exercise 3: Implement Authorization

**Task**: Add authorization to a new feature

**Steps**:
1. Create a policy class
2. Add authorization checks to controller
3. Write tests for the policy
4. Update views to use policy helpers
5. Test authorization scenarios

**Example**: Create `PodcastPolicy` for podcast management

---

### Exercise 4: Optimize a Query

**Task**: Optimize a slow database query

**Steps**:
1. Identify a slow query (use query logs or PGHero)
2. Analyze the query execution plan
3. Add indexes if needed
4. Refactor query to be more efficient
5. Measure performance improvement

**Example**: Optimize article feed query

---

### Exercise 5: Add Caching

**Task**: Implement caching for a slow endpoint

**Steps**:
1. Identify a slow endpoint
2. Determine what should be cached
3. Implement caching logic
4. Add cache invalidation
5. Test caching behavior

**Example**: Cache user dashboard data

---

### Exercise 6: Write Comprehensive Tests

**Task**: Write tests for a complex feature

**Steps**:
1. Identify a feature without tests
2. Write unit tests for models
3. Write tests for services
4. Write tests for controllers
5. Write integration tests
6. Aim for high coverage

**Example**: Write tests for article moderation feature

---

## Code Review Checklist

When reviewing code (yours or others'), check for:

### Architecture

- [ ] Is business logic in services, not controllers?
- [ ] Are services single-purpose and focused?
- [ ] Is authorization handled in policies?
- [ ] Are background jobs used for heavy operations?
- [ ] Is caching used appropriately?

### Code Quality

- [ ] Is code DRY (Don't Repeat Yourself)?
- [ ] Are methods small and focused?
- [ ] Is naming clear and descriptive?
- [ ] Are there any code smells?
- [ ] Does code follow project conventions?

### Performance

- [ ] Are database queries optimized?
- [ ] Are N+1 queries prevented?
- [ ] Is caching used where appropriate?
- [ ] Are background jobs used for heavy operations?
- [ ] Are rate limits considered?

### Security

- [ ] Is authorization checked?
- [ ] Is input validated?
- [ ] Are SQL injections prevented?
- [ ] Are XSS vulnerabilities prevented?
- [ ] Are CSRF protections in place?

### Testing

- [ ] Are there tests for new code?
- [ ] Do tests cover edge cases?
- [ ] Are tests maintainable?
- [ ] Do tests run quickly?
- [ ] Is test coverage adequate?

### Documentation

- [ ] Is code self-documenting?
- [ ] Are complex parts commented?
- [ ] Is API documentation updated?
- [ ] Are README files updated?

---

## Resources

### Internal Documentation

* `SYSTEM_ARCHITECTURE.md` - System architecture overview
* `SYSTEM_FLOWS.md` - Flow diagrams
* `GDPR_DELETE_REQUEST_FLOW.md` - Example flow documentation
* `docs/` - Additional technical documentation

### External Resources

* [Rails Guides](https://guides.rubyonrails.org/)
* [Sidekiq Best Practices](https://github.com/mperham/sidekiq/wiki/Best-Practices)
* [Pundit Documentation](https://github.com/varvet/pundit)
* [Forem Developer Docs](https://developers.forem.com/)

### Books

* "Practical Object-Oriented Design in Ruby" by Sandi Metz
* "Refactoring" by Martin Fowler
* "Design Patterns in Ruby" by Russ Olsen
* "The Rails Way" by Obie Fernandez

---

## Next Steps

1. **Choose a Focus Area**: Pick one area to deep dive into
2. **Set Learning Goals**: Define what you want to learn
3. **Create a Study Plan**: Break down learning into manageable chunks
4. **Practice Regularly**: Apply what you learn through exercises
5. **Seek Feedback**: Get code reviews and ask questions
6. **Teach Others**: Explain concepts to solidify understanding
7. **Contribute**: Submit PRs and contribute to the codebase

---

## Conclusion

This learning path provides a structured approach to mastering Rails development through the Forem/DEV.to codebase. Remember:

* **Learning is a journey**: Take your time and be patient
* **Practice is essential**: Apply what you learn
* **Ask questions**: Don't hesitate to seek help
* **Contribute**: The best way to learn is by doing

Good luck on your journey to becoming a senior Rails developer! 🚀

