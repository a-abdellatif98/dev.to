# Forem/DEV.to Documentation Index

Complete index of all documentation files in the codebase.

## Table of Contents

1. [Architecture & System Documentation](#architecture--system-documentation)
2. [Learning & Development Guides](#learning--development-guides)
3. [Feature-Specific Documentation](#feature-specific-documentation)
4. [Performance & Optimization](#performance--optimization)
5. [Internationalization](#internationalization)
6. [Project Documentation](#project-documentation)

---

## Architecture & System Documentation

### [docs/SYSTEM_ARCHITECTURE.md](./docs/SYSTEM_ARCHITECTURE.md)
**Comprehensive system architecture documentation**

* System overview and technology stack
* Architectural patterns (Service Objects, Workers, Policies, etc.)
* Main components and modules breakdown
* Request flow diagrams
* Key systems and flows
* Areas for senior Rails developer growth
* Code organization principles

**Use this when**: You need to understand the overall architecture, patterns used, or how components interact.

---

### [docs/SYSTEM_FLOWS.md](./docs/SYSTEM_FLOWS.md)
**Visual flow diagrams for major systems**

* User Authentication Flow
* Article Publishing Flow
* Comment Creation Flow
* Notification System Flow
* User Deletion Flow (GDPR)
* Search Flow
* Moderation Flow
* Email Digest Flow

**Use this when**: You need to understand how a specific feature works end-to-end or trace through a request flow.

---

### [docs/GDPR_DELETE_REQUEST_FLOW.md](./docs/GDPR_DELETE_REQUEST_FLOW.md)
**Complete GDPR deletion flow documentation**

* Two-phase deletion process (automatic + admin confirmation)
* Detailed step-by-step flow
* How records are affected
* Visual Mermaid flow diagram
* Design decisions and rationale

**Use this when**: You need to understand user deletion, GDPR compliance, or see an example of comprehensive flow documentation.

---

## Learning & Development Guides

### [docs/SENIOR_RAILS_LEARNING_PATH.md](./docs/SENIOR_RAILS_LEARNING_PATH.md)
**Structured learning path for Rails developers**

* 8-week structured learning path
* Deep dive topics with study guides
* Practical exercises
* Code review checklist
* Resources and next steps

**Use this when**: You're new to the codebase, want to improve your Rails skills, or need a structured approach to learning.

---

## Feature-Specific Documentation

### [docs/github_repo_recap_service.md](./docs/github_repo_recap_service.md)
**GitHub repository recap service documentation**

* Service implementation details
* Usage examples
* API documentation

**Use this when**: Working with GitHub repository features or the recap service.

---

### [docs/survey_resubmission.md](./docs/survey_resubmission.md)
**Survey resubmission feature documentation**

* Survey resubmission logic
* Implementation details
* Usage patterns

**Use this when**: Working with survey features or user resubmission logic.

---

## Performance & Optimization

### [docs/feed_query_optimization.md](./docs/feed_query_optimization.md)
**Feed query optimization documentation**

* Query optimization strategies
* Performance improvements
* Database indexing strategies

**Use this when**: Optimizing feed queries or improving database performance.

---

### [docs/feed_partial_index_optimization.md](./docs/feed_partial_index_optimization.md)
**Feed partial index optimization**

* Partial index implementation
* Performance gains
* Index strategies

**Use this when**: Working with feed indexes or optimizing database queries.

---

### [docs/moderations_performance_optimizations.md](./docs/moderations_performance_optimizations.md)
**Moderation system performance optimizations**

* Moderation query optimizations
* Performance improvements
* Caching strategies

**Use this when**: Optimizing moderation features or improving moderation performance.

---

### [docs/README_READ_ONLY_DATABASE.md](./docs/README_READ_ONLY_DATABASE.md)
**Read-only database setup and usage**

* Read replica configuration
* Read-only database setup
* Usage patterns and best practices

**Use this when**: Setting up read replicas or optimizing read-heavy operations.

---

## Internationalization

### [docs/i18n_implementation_guide.md](./docs/i18n_implementation_guide.md)
**Internationalization implementation guide**

* i18n setup and configuration
* Translation patterns
* Locale management

**Use this when**: Adding new translations, setting up i18n, or working with multiple languages.

---

### [docs/locale_analysis.md](./docs/locale_analysis.md)
**Locale analysis documentation**

* Locale usage analysis
* Translation coverage
* Locale-specific features

**Use this when**: Analyzing locale usage or planning new locale support.

---

### [docs/portuguese_internationalization_summary.md](./docs/portuguese_internationalization_summary.md)
**Portuguese internationalization summary**

* Portuguese translation implementation
* Locale-specific features
* Translation patterns

**Use this when**: Working with Portuguese translations or adding Portuguese-specific features.

---

## Project Documentation

### [README.md](./README.md)
**Main project README**

* Project overview
* Getting started guide
* Installation instructions
* Contributing guidelines
* Community information

**Use this when**: Starting with the project, setting up development environment, or understanding project basics.

---

### [SECURITY.md](./SECURITY.md)
**Security policy and vulnerability disclosure**

* Security policy
* Vulnerability reporting process
* Security best practices

**Use this when**: Reporting security vulnerabilities or understanding security policies.

---

### [CODE_OF_CONDUCT.md](./CODE_OF_CONDUCT.md)
**Code of conduct**

* Community guidelines
* Expected behavior
* Reporting process

**Use this when**: Understanding community standards or reporting issues.

---

### [CHANGELOG.md](./CHANGELOG.md)
**Project changelog**

* Version history
* Feature additions
* Bug fixes
* Breaking changes

**Use this when**: Checking what changed in recent versions or understanding version history.

---

### [docs/README.md](./docs/README.md)
**Documentation directory README**

* Documentation organization
* How to contribute documentation
* Documentation standards

**Use this when**: Understanding documentation structure or contributing new documentation.

---

## Additional Documentation

### Model-Specific Documentation

* [app/models/articles/feeds/README.md](./app/models/articles/feeds/README.md) - Article feeds implementation
* [config/feed-variants/README.md](./config/feed-variants/README.md) - Feed variants configuration

### License & Legal

* [LICENSE.md](./LICENSE.md) - Project license
* [CLA.md](./CLA.md) - Contributor License Agreement

---

## Quick Reference Guide

### I want to...

**Understand the overall architecture**
→ Read [docs/SYSTEM_ARCHITECTURE.md](./docs/SYSTEM_ARCHITECTURE.md)

**See how a feature works end-to-end**
→ Read [docs/SYSTEM_FLOWS.md](./docs/SYSTEM_FLOWS.md)

**Learn Rails patterns from this codebase**
→ Read [docs/SENIOR_RAILS_LEARNING_PATH.md](./docs/SENIOR_RAILS_LEARNING_PATH.md)

**Understand GDPR deletion flow**
→ Read [docs/GDPR_DELETE_REQUEST_FLOW.md](./docs/GDPR_DELETE_REQUEST_FLOW.md)

**Optimize database queries**
→ Read [docs/feed_query_optimization.md](./docs/feed_query_optimization.md)

**Set up internationalization**
→ Read [docs/i18n_implementation_guide.md](./docs/i18n_implementation_guide.md)

**Set up read replicas**
→ Read [docs/README_READ_ONLY_DATABASE.md](./docs/README_READ_ONLY_DATABASE.md)

**Get started with the project**
→ Read [README.md](./README.md)

**Report a security issue**
→ Read [SECURITY.md](./SECURITY.md)

---

## Documentation Standards

When adding new documentation:

1. **Place in appropriate location**:
   * Architecture/system docs → `docs/` directory
   * Feature-specific → `docs/` directory
   * Model-specific → Near the model file

2. **Follow naming conventions**:
   * Use descriptive names: `FEATURE_NAME_FLOW.md`
   * Use UPPERCASE for important docs: `SYSTEM_ARCHITECTURE.md`
   * Use lowercase for feature docs: `feature_name.md`

3. **Include in this index**:
   * Add your new documentation to this index
   * Categorize appropriately
   * Add a brief description

4. **Structure**:
   * Table of contents
   * Clear sections
   * Code examples where relevant
   * Visual diagrams (Mermaid) for flows

---

## Contributing to Documentation

1. **Read existing docs** to understand style and structure
2. **Update this index** when adding new documentation
3. **Use Mermaid** for flow diagrams
4. **Include examples** and code references
5. **Keep it updated** as code changes

---

## Last Updated

This index was last updated: 2024

---

## Questions?

If you can't find what you're looking for:
1. Check the [README.md](./README.md) for project overview
2. Search the codebase for related code
3. Ask in GitHub discussions
4. Check existing issues/PRs

---

**Happy coding! 🚀**

