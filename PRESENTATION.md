---
marp: true
theme: default
paginate: true
backgroundColor: #fff
backgroundImage: url('https://marp.app/assets/hero-background.svg')
header: 'CI/CD Workshop - Jenkins & Go'
footer: 'EPAM Systems | Workshop Day 1'
---

<!-- _class: lead -->

# 🚀 CI/CD Workshop
## Jenkins & Go Pipeline

**Progressive Learning Approach**
6 Phases from Git Checkout to Production-Ready CI/CD

<!--
PRESENTER NOTES:
- Привітайтесь з аудиторією / Greet the audience warmly
- Представтесь та представте воркшоп / Introduce yourself and the workshop
- Згадайте, що це практичний workshop з прогресивним підходом / Mention this is hands-on with progressive approach
- Орієнтовна тривалість: 2-3 години / Expected duration: 2-3 hours
- Буде багато практики! / Lots of hands-on practice!

SAY: "Welcome everyone! Today we're building a complete CI/CD pipeline from scratch. This isn't just slides and theory - you'll be hands-on throughout. We start simple and progressively add features until we have a production-ready system. By the end, you'll have built something you can actually use in your projects."
-->

---

## 📋 Workshop Agenda

### Part 1: Theory (45 min)
1. **What is CI/CD?**
2. **Quality Gates & Code Quality**
3. **Best Practices**
4. **Common Mistakes to Avoid**

### Part 2: Practice (90+ min)
5. **Project Overview & Setup**
6. **Phase 1-6: Building the Pipeline**
7. **Hands-on Lab**
8. **Q&A**

<!--
PRESENTER NOTES:
- Спочатку розглянемо теорію - важливо розуміти "чому" / First we cover theory - it's crucial to understand the "why"
- Потім практика - будемо будувати реальний pipeline / Then practice - we'll build a real pipeline
- Задавайте питання в будь-який момент / Ask questions at any time
- Після теорії буде невелика перерва (5-10 хв) / Short break after theory (5-10 min)

SAY: "We've structured this in two parts. Part one - 45 minutes of theory. I know, I know, you want to jump into coding. But understanding WHY we do things is crucial. It's the difference between blindly following a recipe versus understanding cooking. Then we break for 10 minutes, grab coffee, and spend 90 minutes building an actual pipeline, hands-on. Questions are welcome anytime - interrupt me, this is interactive."
-->

---

<!-- _class: lead -->

# Part 1: Theory
## Understanding CI/CD

<!--
PRESENTER NOTES:
- Наголосіть що theory - це фундамент
- Без розуміння "чому" практика буде механічною
- Теорія допоможе приймати правильні рішення в майбутньому
-->

---

<!-- _class: lead -->

# What is CI/CD?

<!--
PRESENTER NOTES:
- Почніть з базового визначення / Start with basic definition
- Запитайте аудиторію: хто вже використовує CI/CD? / Ask audience: who already uses CI/CD?
- Які інструменти використовують? (Jenkins, GitLab CI, GitHub Actions, etc.) / What tools do they use?

SAY: "Before we dive in, quick poll - raise your hand if you're already using some form of CI/CD at work. Great! What tools? Jenkins? GitHub Actions? GitLab CI? Keep those in mind - the principles we discuss today apply to all of them."
-->

---

## Continuous Integration / Continuous Delivery

### **Continuous Integration (CI)**
- Automate code integration from multiple developers
- Run automated tests on every commit
- Catch bugs early in development
- Maintain code quality standards
- Faster feedback loop for developers

### **Continuous Delivery (CD)**
- Automate deployment process
- Create deployable artifacts consistently
- Release software faster and more reliably
- Reduce manual errors and human mistakes
- Enable rapid iterations

<!--
PRESENTER NOTES:
- CI = про інтеграцію коду та тестування / CI is about code integration and testing
- CD = про доставку і deployment / CD is about delivery and deployment
- Підкресліть: автоматизація - ключ до успіху / Emphasize: automation is the key to success
- Згадайте, що CD може означати Continuous Delivery АБО Continuous Deployment / Mention CD can mean Delivery OR Deployment
- Delivery = готово до deployment, Deployment = автоматично задеплоєно / Delivery = ready to deploy, Deployment = auto-deployed

SAY: "CI - Continuous Integration - means frequently integrating code from multiple developers and automatically testing it. Every commit triggers tests. Fast feedback. CD extends this. Now here's a trick question - CD can mean TWO things. Continuous Delivery means you're always READY to deploy - one button away. Continuous Deployment means every passing build AUTOMATICALLY goes to production. Big difference! Most companies do Delivery, not Deployment. The keyword for both? Automate."
-->

---

## Why CI/CD Matters

### Without CI/CD
- ❌ Manual testing and building
- ❌ "It works on my machine" syndrome
- ❌ Late bug discovery (expensive fixes)
- ❌ Slow release cycles
- ❌ Fear of deployments

### With CI/CD
- ✅ Automated everything
- ✅ Consistent environments
- ✅ Early bug detection (cheap fixes)
- ✅ Fast, frequent releases
- ✅ Confidence in deployments

<!--
PRESENTER NOTES:
- Розкажіть історію: як виглядало без CI/CD / Tell a story: what it was like without CI/CD
- Integration hell - коли всі зливають код в кінці спринту / Integration hell - when everyone merges at sprint end
- Приклад: bug знайдений через 2 тижні vs 2 хвилини після commit / Example: bug found after 2 weeks vs 2 minutes
- Ціна виправлення bug зростає експоненційно з часом / Bug fix cost grows exponentially with time

SAY: "Let me paint a picture of the 'before times'. Picture this: five developers working in isolation for two weeks. Friday afternoon, everyone tries to merge. Conflicts everywhere. Integration takes all weekend. This is 'integration hell'. Even worse - bugs discovered two weeks later cost 10-100x more to fix than if caught immediately."

PAUSE for effect.

SAY: "With CI/CD? You commit. Five minutes later, you know if you broke something. That's the difference between finding a typo while typing versus after the book is printed. One is free, the other is expensive."

ASK: "Who's experienced integration hell? How long did it take to resolve?" [Wait for responses]
-->

---

## CI/CD Pipeline Flow

```
Developer Commits Code
         ↓
   Source Control (Git)
         ↓
   Trigger CI Pipeline
         ↓
┌─────────────────────┐
│   Build & Compile   │
└─────────────────────┘
         ↓
┌─────────────────────┐
│   Run Unit Tests    │
└─────────────────────┘
         ↓
┌─────────────────────┐
│  Static Analysis    │ ← Quality Gates
└─────────────────────┘
         ↓
┌─────────────────────┐
│  Create Artifacts   │
└─────────────────────┘
         ↓
    Ready for Deploy
```

<!--
PRESENTER NOTES:
- Це базовий flow, який ми будемо будувати сьогодні
- Кожен крок - це quality gate
- Якщо щось fails - pipeline зупиняється
- Це захищає production від поганого коду
- Згадайте: швидкий feedback critical!
-->

---

<!-- _class: lead -->

# Quality Gates
## Guardians of Code Quality

<!--
PRESENTER NOTES:
- Quality gates = автоматичні перевірки якості
- Це checkpoints в pipeline
- Не пускають поганий код далі
-->

---

## What are Quality Gates?

### Definition
**Automated checks that code must pass before proceeding to the next stage**

### Purpose
- Enforce quality standards automatically
- Catch issues early (shift-left testing)
- Prevent bad code from reaching production
- Maintain consistent quality across team
- Build confidence in releases

<!--
PRESENTER NOTES:
- Quality gate = checkpoint в pipeline / Quality gate is a checkpoint in the pipeline
- Якщо не пройшов - pipeline fails / If it fails - pipeline stops
- Приклад з життя: metal detector в аеропорту - не пускає далі поки не ок / Real-life example: airport metal detector
- "Shift-left" = перевіряємо раніше в процесі, не в кінці / "Shift-left" means check earlier, not at the end

SAY: "Quality gates are automated checkpoints. Think of airport security - you don't board the plane until you pass through the metal detector. Same here. Code doesn't proceed to the next stage until it passes the gate."

EXPLAIN: "'Shift-left testing' means moving quality checks earlier in the process. Traditional approach: write code for weeks, then test at the end. Modern approach: check quality at every step. Find issues when they're fresh in your mind and cheap to fix."

EMPHASIZE: "Quality gates aren't bureaucracy - they're guardrails. They prevent bad code from reaching production. Automate the checks, enforce the standards."
-->

---

## Types of Quality Gates

### 1. **Build Quality Gates**
- Code compiles successfully
- No syntax errors
- Dependencies resolved

### 2. **Test Quality Gates**
- Unit tests pass (100% expected)
- Code coverage threshold (e.g., >70%)
- Integration tests pass
- Performance tests within limits

### 3. **Code Quality Gates**
- Static analysis passes (no critical issues)
- Code formatting consistent
- No security vulnerabilities
- Complexity metrics acceptable

<!--
PRESENTER NOTES:
- Сьогодні ми імплементуємо всі три типи! / Today we'll implement all three types!
- Build - Phase 3
- Tests - Phase 4  
- Code Quality - Phase 5
- Кожен додає новий рівень захисту / Each adds a new layer of protection

SAY: "Three types of quality gates. First - Build gates. Can your code even compile? Basic, but critical. You'd be surprised how often code that 'works on my machine' fails to build in CI."

SAY: "Second - Test gates. Do your tests pass? What's your code coverage? This is where you catch logic bugs. 100% of unit tests must pass - no exceptions."

SAY: "Third - Code Quality gates. Static analysis, linting, security scans. These catch issues that tests miss - code smells, potential bugs, security vulnerabilities."

CONNECT: "Today we'll build all three. Phase 3 adds build gate. Phase 4 adds test gate. Phase 5 adds code quality gate. Layer by layer, we're building confidence."
-->

---

## Quality Gates in Action

### Example: Our Workshop Pipeline

```
Phase 1: Git Checkout         → Basic gate: code exists
Phase 2: Go Environment       → Basic gate: tools ready
Phase 3: Build                → Gate: code compiles
Phase 4: Tests                → Gate: tests pass + coverage
Phase 5: Static Analysis      → Gate: code quality OK
Phase 6: Artifacts            → Gate: ready for deployment
```

### Real-World Impact
- **Failed build** = syntax error caught immediately
- **Failed tests** = logic bug caught before merge
- **Failed linting** = maintainability issues prevented
- **Passed all gates** = high confidence in quality

<!--
PRESENTER NOTES:
- Кожна phase додає quality gate / Each phase adds a quality gate
- Ми будуємо їх поступово - progressive approach / We build them progressively
- В реальних проектах може бути 10+ gates / Real projects can have 10+ gates
- Security scans, dependency checks, performance tests, etc.

SAY: "Here's how it maps to our workshop. Each phase adds a new quality gate. Phase 1-2 are setup. Phase 3 - build gate, does it compile? Phase 4 - test gate, do tests pass? Phase 5 - code quality gate, is it maintainable? Phase 6 - we create deployable artifacts."

REAL-WORLD: "In production systems, you might have 10+ gates: security scanning, dependency vulnerability checks, performance tests, integration tests, API contract validation, database migration tests... Each one catching a different class of problems."

IMPACT: "Look at the real-world impact. Failed build? Syntax error caught in 2 minutes, not discovered in production. Failed tests? Logic bug caught before code review. Failed linting? Maintainability issues prevented. Pass all gates? High confidence this code is production-ready."

EMPHASIZE: "Progressive approach. We don't build everything at once. We add gates incrementally, understand each one, see the value. By Phase 6, we have a production-ready pipeline."
-->

---

## Static Code Analysis - Deep Dive

### What It Checks

#### **Code Smells**
- Overly complex functions
- Duplicated code
- Unused variables
- Dead code

#### **Bugs**
- Potential null pointer dereferences
- Resource leaks
- Logic errors
- Race conditions

#### **Security Issues**
- SQL injection vulnerabilities
- Hardcoded secrets
- Insecure cryptography
- Path traversal risks

<!--
PRESENTER NOTES:
- Static analysis = аналіз коду без його виконання
- Знаходить проблеми, які можуть не проявитись в тестах
- В нашому workshop: golangci-lint (40+ linters!)
- Це як spell checker, але для коду
-->

---

## Code Coverage as Quality Metric

### What is Code Coverage?
Percentage of code executed by tests

### Types
- **Line Coverage**: % of lines executed
- **Branch Coverage**: % of decision branches tested
- **Function Coverage**: % of functions called

### Typical Thresholds
- 🔴 <50% - Poor, lots of untested code
- 🟡 50-70% - Acceptable for some projects
- 🟢 70-85% - Good, solid coverage
- 🔵 85%+ - Excellent, thorough testing

### Reality Check
**Coverage ≠ Quality, but low coverage = definite risk**

<!--
PRESENTER NOTES:
- Coverage показує ЩО виконується, не ЩО правильно / Coverage shows WHAT runs, not WHAT is correct
- 100% coverage не означає bug-free code / 100% coverage doesn't mean bug-free
- Але низький coverage = багато непротестованого коду / But low coverage = lots of untested code
- В нашому проекті: 41.2% - є куди рости! / Our project: 41.2% - room for improvement!
- Quality gate: можна встановити мінімум, наприклад 70% / Can set minimum threshold, e.g. 70%

SAY: "Important distinction: coverage tells you WHAT code runs during tests, not WHETHER it's correct. You can have 100% coverage and still have bugs if your tests don't check the right things."

ANALOGY: "It's like checking every room in a house, but not actually looking for problems. You visited every room - 100% coverage - but missed the leak in the bathroom."

REALITY: "That said, low coverage is definitely a problem. Our workshop project? 41.2% coverage. That means 58.8% of code has never been executed by tests. Lots of potential hiding places for bugs."

PRACTICAL: "Good teams aim for 70-85% coverage. You can set this as a quality gate - if coverage drops below threshold, build fails. Forces people to write tests."
-->

---

<!-- _class: lead -->

# Best Practices
## Do This, Not That

<!--
PRESENTER NOTES:
- Зараз розглянемо proven practices / Now let's look at proven practices
- Це не теорія, а real-world досвід / This isn't theory - it's real-world experience
- Дотримання цих practices = успішний CI/CD / Following these = successful CI/CD

SAY: "Now for best practices. These aren't academic theories - these are battle-tested approaches from real production systems. Companies doing CI/CD well follow these patterns. Companies struggling? Usually violating several of these."
-->

---

## CI/CD Best Practices - Pipeline Design

### ✅ DO

**Keep Builds Fast**
- Target: <10 minutes for full pipeline
- Use caching for dependencies
- Parallelize independent tasks
- Fail fast - run quick tests first

**Make Builds Reproducible**
- Pin dependency versions
- Use containers for consistency
- Version everything (including tools)
- Same result every time

**Keep Pipelines Simple**
- One responsibility per stage
- Easy to understand and debug
- Clear stage names
- Good error messages

<!--
PRESENTER NOTES:
- Швидкість КРИТИЧНА - developers чекають на feedback / Speed is CRITICAL - devs wait for feedback
- Якщо build 30+ хвилин - developers не чекають / If build takes 30+ min - devs don't wait
- Reproducible = run сьогодні = run завтра (same result)
- Simplicity = maintainability

SAY: "Three principles for pipeline design. First - speed. Target under 10 minutes for full pipeline. Why? Developer attention span. If your build takes 30 minutes, developers context-switch to other work. They don't see the failure immediately. The code isn't fresh in their mind anymore."

TIPS: "How to speed up? Cache dependencies. Don't download the internet every time. Parallelize independent tasks. Fail fast - run quick unit tests before slow integration tests."

SAY: "Second - reproducibility. Same code, same build, same result. Today, tomorrow, next year. Pin dependency versions. Use containers. If it worked yesterday and fails today, something changed - you need to know what."

SAY: "Third - simplicity. One stage, one job. Clear names. Good error messages. Complex pipelines are hard to debug. When something breaks at 3am, you'll thank yourself for keeping it simple."

EMPHASIZE: "Our workshop pipeline follows all of these. Takes about 2 minutes. Reproducible - same result every time. Simple - easy to understand what each stage does."
-->

---

## CI/CD Best Practices - Code Quality

### ✅ DO

**Automate Quality Checks**
- Run linters on every commit
- Enforce formatting (no manual reviews for style)
- Use static analysis tools
- Automated security scans

**Test Early, Test Often**
- Unit tests run first (fast feedback)
- Integration tests after build
- Smoke tests before full test suite
- Test in production-like environment

**Version Everything**
- Code (Git)
- Dependencies (lock files)
- Infrastructure (IaC)
- Configuration
- Even CI pipeline itself!

<!--
PRESENTER NOTES:
- Quality checks мають бути automatic, не manual / Quality checks must be automatic
- Не витрачайте час на code reviews про formatting / Don't waste code review time on formatting
- "Test early" = shift-left approach
- Version control = time machine для вашого проекту / Version control is a time machine

SAY: "Code quality practices. First - automate quality checks. Run linters on every commit. Enforce code formatting automatically. Why? Don't waste precious code review time debating spaces vs tabs. Let tools handle style, humans focus on logic."

EXAMPLE: "I've seen code reviews with 50 comments about indentation. What a waste! Configure prettier or gofmt, make it automatic, never discuss formatting again."

SAY: "Test early, test often. Unit tests run first - they're fast, give quick feedback. Integration tests come later - they're slower. Don't run your 30-minute integration suite before your 30-second unit tests. Fail fast."

SAY: "Version everything. Not just code - dependencies too. Use lock files (package-lock.json, go.sum). Version your infrastructure (Terraform). Version your configuration. Even version your CI pipeline itself! Everything in Git. Git is truth."

WHY: "So you can go back in time. 'This worked last month, what changed?' Check Git. Dependencies? Check lock file. Infrastructure? Check Terraform. No mysteries."
-->

---

## CI/CD Best Practices - Security & Secrets

### ✅ DO

**Secrets Management**
- NEVER commit secrets to Git
- Use secret management tools (Vault, AWS Secrets Manager)
- Rotate secrets regularly
- Audit secret access

**Security Scanning**
- Scan dependencies for vulnerabilities
- Static Application Security Testing (SAST)
- Container image scanning
- Regular security audits

**Access Control**
- Principle of least privilege
- Use service accounts
- Audit logs for all operations

<!--
PRESENTER NOTES:
- НІКОЛИ не комітьте паролі, API keys, tokens / NEVER commit passwords, API keys, tokens
- Історія: GitHub scans commits, revokes leaked AWS keys automatically
- Security - не afterthought, а built-in / Security isn't an afterthought, it's built-in
- "Shift-left security" - перевіряємо безпеку рано / Check security early

SAY: "Security and secrets - critical topic. Rule number one: NEVER commit secrets to Git. Not API keys, not passwords, not tokens. Git history is forever. Even if you delete it later, it's still there in history."

FACT: "GitHub actively scans commits for AWS credentials and automatically revokes them. That's how big this problem is."

SOLUTION: "Use secret management tools - HashiCorp Vault, AWS Secrets Manager, Azure Key Vault. Or at minimum, environment variables loaded at runtime. Never hardcode secrets."

SAY: "Security scanning. Scan your dependencies - libraries you use might have known vulnerabilities. Use tools like Snyk, Dependabot. Scan your container images. Run SAST tools to find security issues in your code."

PRINCIPLE: "Shift-left security. Don't wait until production to think about security. Check for vulnerabilities in CI. Catch them early when they're cheap to fix."
-->
- Regular security audits

**Access Control**
- Principle of least privilege
- Use service accounts
- Audit logs for all operations
- MFA for critical operations

<!--
PRESENTER NOTES:
- НІКОЛИ не комітьте паролі, API keys, tokens
- Історія: GitHub scans commits, revokes leaked AWS keys automatically
- Security - не afterthought, а built-in
- "Shift-left security" - перевіряємо безпеку рано
-->

---

## CI/CD Best Practices - Artifacts & Deployments

### ✅ DO

**Artifact Management**
- Build once, deploy many times
- Immutable artifacts (never modify)
- Include version metadata
- Retention policy (don't keep forever)

**Deployment Strategy**
- Automated rollback capability
- Blue-green or canary deployments
- Smoke tests after deployment
- Monitor post-deployment

**Documentation**
- Pipeline as Code (Jenkinsfile in Git)
- README for setup instructions
- Runbook for troubleshooting

<!--
PRESENTER NOTES:
- Build once = consistency, efficiency
- Той самий artifact в dev, staging, production / Same artifact in dev, staging, production
- Immutable = якщо щось не так, знаємо що саме deployed / Immutable means we know exactly what's deployed
- Rollback - ОБОВ'ЯЗКОВО мати план B / Rollback - MUST have a plan B
- Documentation = future you буде вдячний / Documentation - future you will thank yourself

SAY: "Artifacts and deployments. Golden rule: build once, deploy many times. Build your artifact in CI, then deploy that SAME artifact to dev, staging, and production. Don't rebuild for each environment - you might get different results."

EXPLAIN: "Immutable artifacts. Once created, never modified. You deploy version 1.2.3, you know exactly what's running. If something breaks, you know exactly which version to blame. You can download that artifact from six months ago and it's identical to what ran in production."

PRACTICAL: "Include metadata in artifacts. Version number, git commit SHA, build date, who triggered it. When production breaks, you need to know exactly what's deployed."

CRITICAL: "Always have rollback capability. Deployments fail. Bugs slip through. You need a plan B. Blue-green deployments, canary releases - these give you safe rollback options."

DOCUMENTATION: "Document everything. Pipeline as code in Git. README explains setup. Runbook explains troubleshooting. When you're debugging at 3am, you'll thank yourself for writing it down."

TODAY: "In Phase 6, we create artifacts with full metadata. Version, commit SHA, build date - everything you need to track what's deployed."
-->

---

<!-- _class: lead -->

# Common Mistakes
## Learn from Others' Pain

<!--
PRESENTER NOTES:
- Зараз розглянемо типові помилки / Now let's look at common mistakes
- Всі ці помилки реальні / All these mistakes are real-world examples
- Краще вчитись на чужих помилках 😊 / Better to learn from others' mistakes
- Якщо ви робите щось з цього списку - не соромтесь, ми всі через це пройшли / If you're doing any of these - don't be embarrassed, we've all been there

SAY: "Now for the fun part - mistakes! These aren't theoretical - these are real problems I've seen in production. Expensive problems. If you're currently doing any of these, don't worry - every single person in this room has made at least one of these mistakes. Including me. Many times. That's how we learn."
-->

---

## ❌ Mistake #1: Manual Steps

### The Problem
```bash
echo "Build done. Now manually:"
echo "  1. SSH to server, run deploy.sh"
echo "  2. Update config.yaml"
echo "  3. Notify team"
```

### Why It Fails
Human error • Bottleneck • Inconsistent

### ✅ Solution
Automate everything in the pipeline

<!--
PRESENTER NOTES:
- Класична помилка: "автоматизація" з manual steps / Classic: "automation" with manual steps
- "Semi-automatic" = не automatic / "Semi-automatic" is NOT automatic
- Якщо щось можна автоматизувати - автоматизуйте / If it can be automated - automate it
- Manual steps = weak links в ланцюгу / Manual steps are the weak links

SAY: "This is mistake number one for a reason - it's everywhere. Someone builds an 'automated' pipeline, but then: 'Now manually SSH to the server and run this script'. That's not automation! That's automation theater. The human becomes the bottleneck."

STORY: "I've seen this: Friday 6pm, deploy ready, but Sarah who knows the manual steps already left. Weekend delayed. All because of three manual steps that could've been automated."

ASK: "Honest show of hands - who has manual steps in their pipeline right now?" [Wait] "What's stopping you from automating them?"

EMPHASIZE: "If a step needs to happen, the pipeline does it. Period. No humans required."
-->

---

## ❌ Mistake #2: Ignoring Failed Tests

### The Problem
- "Flaky test, just rerun" 🔄
- "Always fails, ignore it" 🙈
- "Fix later" (never) ⏰
- Disable instead of fix

### Why It Fails
Destroys trust • Masks bugs • Growing debt

### ✅ Solution
- Fix immediately or delete
- Make tests stable
- Red build = event, not norm

<!--
PRESENTER NOTES:
- "Flaky tests" = tests that randomly fail
- Якщо tests always fail - це не test, це broken code / If tests always fail - that's not a test, that's broken code
- Broken window theory: один ignored test → більше ignored tests / One ignored test leads to more
- Red build має бути EVENT, не norm / Red build should be an EVENT, not the norm
- Zero tolerance для ignored failures / Zero tolerance for ignored failures

SAY: "This is a slippery slope. One test starts failing intermittently. 'It's flaky, just rerun'. Then another. 'That one always fails, ignore it'. Soon you have five red tests and nobody cares. This is the broken window theory - one broken window leads to more."

STORY: "I joined a team where CI was always red. I asked 'is it supposed to be red?' They said 'yeah, ignore those three tests'. That's when you've lost. Your safety net has holes."

EMPHASIZE: "Two choices: fix immediately, or delete the test. A test you ignore is worse than no test - it trains you to ignore failures. When a real bug appears, nobody notices because red is normal."
-->

---

## ❌ Mistake #3: No Infrastructure Version Control

### The Problem
```bash
# Manual Jenkins setup
# Credentials in UI
# "Bob knows how it works"
```

### Why It Fails
Can't reproduce • No audit • "Bus factor" 🚌

### ✅ Solution
Infrastructure as Code (IaC) • Jenkinsfile in Git

<!--
PRESENTER NOTES:
- "Infrastructure as Code" = ваша інфраструктура в Git / Your infrastructure in Git
- "What if Bob wins lottery?" - Bus factor / The "hit by a bus" problem
- Manual configuration = tribal knowledge
- Сьогодні: ми використовуємо Jenkinsfile (pipeline as code!) / Today: we use Jenkinsfile!
- Vagrant/Docker = reproducible environments

SAY: \"Here's a scary scenario. Bob manually configured Jenkins two years ago. Clicked through menus, stored credentials in the UI, installed plugins. Documented nothing. Bob wins the lottery, moves to Bahamas. Congratulations, Bob! Your Jenkins crashes. Can you recreate it? No. This is the 'bus factor' - if Bob gets hit by a bus, you're in trouble.\"\n\nSOLUTION: \"Infrastructure as Code. Everything in Git. Jenkinsfile defines your pipeline. Docker or Vagrant defines your environment. Anyone can recreate the entire setup from scratch in minutes. Notice in our workshop - Jenkinsfile is in Git? That's intentional. That's best practice.\"\n\nEMPHASIZE: \"If it's not in version control, it doesn't exist. Documentation becomes outdated, people leave, memories fade. Git is the single source of truth.\"\n-->

---

## ❌ Mistake #4: Committing Secrets to Git

### What People Do Wrong
```go
// config.go
const APIKey = "sk-1234567890abcdef"  // ❌ NEVER!
const DBPassword = "MyPassword123"    // ❌ NEVER!
```

### Why It's Bad
- **Forever in Git history** (even if deleted later)
- Security breach
- Compliance violations
- Credential rotation nightmare

### ✅ Do This Instead
```go
// config.go
APIKey := os.Getenv("API_KEY")        // ✅ From environment
DBPassword := getSecret("db-password") // ✅ From secret manager
```

<!--
PRESENTER NOTES:
- Git history = permanent record
- Навіть якщо видалите - воно в history / Even if you delete it - it's in history
- GitHub автоматично scans для AWS keys та revokes / GitHub auto-scans for AWS keys and revokes them
- Real incident: Uber breach через leaked key в Git / Real incident: Uber breach from leaked key
- Environment variables or secret managers ONLY
- .gitignore для .env files / Always .gitignore your .env files

SAY: "This is a career-ending mistake. Someone commits an API key to Git. 'Oops, let me delete that commit'. Too late. Git history is forever. That key is permanently in your repository's history. GitHub actually scans for AWS keys and automatically revokes them now - that's how common this problem is."

STORY: "Real example: Uber had a major data breach traced to AWS credentials committed to a private GitHub repo. Cost them $148 million. All because someone committed a password."

EMPHASIZE: "Never, ever commit secrets. Use environment variables or secret managers. Always. No exceptions. Set up .gitignore for .env files before you even write code."
-->

---

## ❌ Mistake #5: Skipping Static Analysis

### The Problem
- "We'll lint before release" (never) 🤥
- "Linters are annoying" (they save time!) ⏱️
- "Takes too long" (seconds actually) ⚡

### Why It Fails
Debt grows • Bugs slip • Code rots

### ✅ Solution
Lint on every commit • Required gate

<!--
PRESENTER NOTES:
- Static analysis = cheap bug detection
- Знаходить помилки БЕЗ running code / Finds bugs WITHOUT running code
- "Annoying" linters save hours of debugging
- В нашому workshop: Phase 5 = static analysis / In our workshop: Phase 5
- golangci-lint знаходить ~40 типів проблем / golangci-lint finds ~40 types of issues

SAY: "People skip linting because 'linters are annoying'. You know what's annoying? Debugging a null pointer exception at 3am in production. Static analysis finds bugs without running code. It's like spell-check for code."

STORY: "I've seen linters catch: using a closed database connection, goroutine leaks, SQL injection vulnerabilities. All without running a single test. Takes 10 seconds. How long does debugging in production take?"

EMPHASIZE: "In Phase 5 today, we add golangci-lint. It runs 40+ linters. We make it a required gate - if linting fails, build fails. No merge until code is clean."

ASK: "How many times has a linter saved you from a bug?" [Wait for responses]
-->

---

## ❌ Mistake #6: No Build Cleanup

### The Problem
```groovy
stage('Build') {
    // No cleanup! 🗑️
    sh 'go build'  // ❌ Old artifacts!
}
```

### Why It's Bad
- Old artifacts contaminate build
- "Works in CI, fails locally" (or vice versa)
- False positives/negatives
- Debugging nightmare

### ✅ Do This Instead
```groovy
pipeline {
    stages {
        stage('Cleanup') {
            steps {
                deleteDir()  // ✅ Clean slate!
            }
        }
        stage('Checkout') { ... }
    }
}
```

<!--
PRESENTER NOTES:
- Clean workspace = reproducible builds
- Старі artifacts можуть mask проблеми
- "It worked last time" - може through old files
- Завжди починайте з чистого стану
- В нашому workshop: Phase 3 додає cleanup
- Disk space management також важливий
-->

---

## ❌ Mistake #7: Slow Feedback Loops

### What People Do Wrong
- Pipeline takes 45+ minutes
- Developers don't wait for results
- Running all tests always (no smart ordering)
- Sequential execution of independent tasks

### Why It's Bad
- Developers context-switch
- Multiple commits before feedback
- Bugs compound
- Productivity loss

### ✅ Do This Instead
- Optimize for speed (<10 min ideal)
- Fail fast (unit tests first)
- Parallelize independent stages
- Cache dependencies
- Incremental testing

<!--
PRESENTER NOTES:
- Швидкість = КЛЮЧ до ефективності CI/CD
- Developer context: coding → switch to другий task → 45 min → "що я робив?"
- Fast feedback = immediate fix, minimal impact
- Slow pipelines = ignored pipelines
- Target: coffee break length (5-10 min)
- Питання: скільки у вас займає build?
-->

---

## ❌ Mistake #8: Not Testing the Build Process Locally

### What People Do Wrong
- Develop → Commit → Wait for CI
- "Let's see if it passes CI"
- No local testing before push

### Why It's Bad
- Wastes CI resources
- Slow feedback
- Frustrates team (broken builds)
- Red pipeline normalized

### ✅ Do This Instead
```bash
# Before committing
./scripts/build.sh        # ✅ Test build locally
go test ./...             # ✅ Run tests locally
golangci-lint run         # ✅ Lint locally
git commit && git push    # ✅ Now push
```

<!--
PRESENTER NOTES:
- CI - не для debugging your code
- Local testing = instant feedback
- CI має бути confirmation, не discovery
- "If it passes locally, it passes CI"
- Pre-commit hooks можуть автоматизувати
- В нашому workshop: scripts/ для local testing!
-->

---

## ❌ Mistake #9: No Artifact Versioning

### What People Do Wrong
```bash
# Always overwrites same file
cp app /deploy/app        # ❌ Which version is this?
```

### Why It's Bad
- Can't track what's deployed
- Rollback impossible
- No audit trail
- Debugging production issues hard

### ✅ Do This Instead
```bash
# Version-tagged artifacts
VERSION=$(git describe --tags --always)
tar -czf app-${VERSION}.tar.gz app
# app-v1.2.3-abc123.tar.gz ✅
```

<!--
PRESENTER NOTES:
- Versioning = traceability
- Production issue? Який version deployed?
- Need to rollback? До якої version?
- В нашому workshop: Phase 3 додає version injection
- Metadata: version, commit SHA, build date
- Це як номерний знак для вашого build
-->

---

<!-- _class: lead -->

# Theory Summary
## Key Takeaways

<!--
PRESENTER NOTES:
- Підведемо підсумки теоретичної частини
- Це фундамент для практики
- Будь-які питання перед перервою?
-->

---

## CI/CD Theory - Key Points

### Core Principles
✅ **Automate Everything** - Manual = error-prone
✅ **Fast Feedback** - Developers need quick results
✅ **Quality Gates** - Automated checks at every stage
✅ **Version Control** - Everything in Git
✅ **Reproducible** - Same result every time

### What NOT to Do
❌ Manual steps in automation
❌ Ignoring failed tests
❌ Committing secrets
❌ Skipping static analysis
❌ No cleanup between builds

<!--
PRESENTER NOTES:
- Ці principles застосовуються до будь-якого CI/CD tool
- Jenkins, GitLab, GitHub Actions - principles однакові
- Наступна частина: практика!
- Перерва 10 хвилин, потім hands-on
-->

---

<!-- _class: lead -->

# ☕ Break Time
## 10 Minutes

**Coming up next:**
- Project structure
- Environment setup
- Building the pipeline (Phase 1-6)

<!--
PRESENTER NOTES:
- Перерва 10 хвилин
- Після перерви - практична частина
- Переконайтесь що всі мають доступ до репозиторію
- Vagrant/Docker ready
-->

---

<!-- _class: lead -->

# Part 2: Practice
## Let's Build a Pipeline!

<!--
PRESENTER NOTES:
- Welcome back!
- Тепер застосуємо теорію на практиці
- Будемо будувати реальний CI/CD pipeline
- Поступово, phase by phase
-->

---

<!-- _class: lead -->

# 📁 Project Structure

<!--
PRESENTER NOTES:
- Спочатку розглянемо що ми будемо будувати
- Простий Go web application
- Realistic project structure
-->

---

## Project Components

```
workshop-cicd/
├── cmd/webapp/              # Go web application
│   ├── main.go              # HTTP server (port 8090)
│   └── main_test.go         # Unit tests
├── jenkins/phases/          # 🎓 6 Progressive phases
│   ├── phase1-basic-checkout.jenkinsfile
│   ├── phase2-add-go-environment.jenkinsfile
│   ├── phase3-add-build.jenkinsfile
│   ├── phase4-add-tests.jenkinsfile
│   ├── phase5-add-static-analysis.jenkinsfile
│   └── phase6-add-artifacts.jenkinsfile
├── docker/                  # Jenkins in Docker
├── scripts/                 # Automation scripts
│   ├── build.sh            # Local build script
│   └── install-jenkins.sh  # Jenkins setup
└── Vagrantfile             # VM setup (Ubuntu 24.04)
```

<!--
PRESENTER NOTES:
- Realistic project structure
- cmd/webapp = наш application
- jenkins/phases = 6 етапів воркшопу
- scripts/ = для local testing (best practice!)
- 2 варіанти setup: Vagrant або Docker
-->

---

## Go Web Application

### Features
- Simple HTTP server on port **8090**
- RESTful endpoints:
  - `GET /` - Web UI with application info
  - `GET /health` - Health check (JSON)
  - `GET /version` - Version information (JSON)
  
### Quality Metrics
- Unit tests with **41.2% coverage**
- Security: Configured timeouts (Read, Write, Idle)
- Version injection via build flags
- Linter-compliant code

<!--
PRESENTER NOTES:
- Простий але realistic application
- Health endpoint = standard practice
- Version endpoint = для debugging production
- 41.2% coverage = є куди рости (good example!)
- Security timeouts = захист від slow clients
-->

---

## 🎯 Workshop Learning Objectives

By the end of this workshop, you will:

✅ Understand CI/CD principles and quality gates
✅ Connect Git repositories to Jenkins
✅ Configure automated build triggers
✅ Set up Go environment automatically
✅ Build applications with version injection
✅ Implement automated testing with coverage
✅ Add static code analysis (linting)
✅ Create and archive build artifacts
✅ Apply CI/CD best practices

<!--
PRESENTER NOTES:
- Це не просто "зробити pipeline"
- Ви зрозумієте WHY за кожним кроком
- Progressive approach = learning-friendly
- Кожна phase builds на попередній
-->

---

<!-- _class: lead -->

# 🔧 Environment Setup

<!--
PRESENTER NOTES:
- Перед тим як почати - треба setup environment
- 2 варіанти: Vagrant (recommended) або Docker
- Якщо вже маєте - супер, перевіримо
-->

---

## Two Setup Options

### **Option 1: Vagrant VM** _(Recommended for Workshop)_
- Ubuntu 24.04 LTS
- Pre-configured Jenkins
- Port: **8080**
- Initial admin password: `8e6b171e8fd147bf99bdd3507d7bf861`

```bash
cd workshop-cicd
vagrant up
# Wait 5-10 minutes for setup
# Access: http://localhost:8080
```

### **Option 2: Docker**
- Jenkins in container
- Port: **8081**

```bash
cd docker
docker-compose up -d
# Access: http://localhost:8081
```

<!--
PRESENTER NOTES:
- Vagrant = ізольована VM, closest to real server
- Docker = легший, швидший, але потребує Docker Desktop
- Оберіть те що вам зручніше
- Якщо Vagrant - запустіть ЗАРАЗ (takes time)
- Password вже надано = no waiting
-->

---

## Prerequisites Check

### Required
✓ **Vagrant** + VirtualBox (for Vagrant option)
✓ **Docker** Desktop (for Docker option)
✓ **Git** installed
✓ **4GB+ RAM** available
✓ Repository cloned

### Verify Installation
```bash
# Check Git
git --version

# Check Vagrant (if using)
vagrant --version

# Check Docker (if using)
docker --version

# Clone repo
git clone https://github.com/epam-msdp/CICD-workshop-day1.git
cd CICD-workshop-day1
```

<!--
PRESENTER NOTES:
- Перевірте що всі мають prerequisites
- Якщо хтось не готовий - можна працювати в парах
- Git ОБОВ'ЯЗКОВО
- Ask: хто вже має все готове?
- Якщо багато не готові - допоможіть зараз
-->

---

<!-- _class: lead -->

# 🎓 The 6 Phases
## Progressive Pipeline Building

<!--
PRESENTER NOTES:
- Зараз розглянемо структуру воркшопу
- 6 phases = 6 етапів
- Кожна phase = новий capability
- Progressive = build knowledge step by step
-->

---

## Progressive Learning Approach

### How It Works
Each phase builds on the previous one:

1. **Phase 1**: Basic Git Checkout
2. **Phase 2**: Go Environment + Triggers
3. **Phase 3**: Build + Cleanup
4. **Phase 4**: Tests + Coverage
5. **Phase 5**: Static Analysis (Quality Gate!)
6. **Phase 6**: Artifacts + Archive (Production-Ready!)

### Why Progressive?
- ✅ Understand each component
- ✅ See incremental value
- ✅ Easier to debug
- ✅ Build confidence gradually

<!--
PRESENTER NOTES:
- НЕ "ось готовий pipeline, розбирайтесь"
- Ми будуємо крок за кроком
- Кожна phase = ви бачите що додалось
- Як Lego: спочатку база, потім більше features
- Після Phase 6 = production-ready pipeline!
-->

---

## Phase 1: Basic Git Checkout

### What You'll Learn
- Connect Jenkins to Git repository
- Basic Jenkinsfile structure
- Declarative pipeline syntax
- SCM checkout process

### Pipeline Content
```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
    }
}
```

**Goal**: Successfully clone the repository in Jenkins

<!--
PRESENTER NOTES:
- Найпростіша phase - just checkout
- Але важлива: connection Git → Jenkins
- "agent any" = run на будь-якому доступному agent
- Declarative syntax = читабельний, рекомендований
- Після цієї phase: Jenkins може читати наш код
-->

---

## Phase 2: Go Environment + Triggers

### What You'll Learn
- Automated build triggers (SCM polling)
- Installing tools in pipeline
- Architecture detection (amd64/arm64)
- Environment variables

### New Features Added
```groovy
triggers {
    pollSCM('* * * * *')  // Poll every minute
}
// + Go 1.21.5 installation
// + Dynamic PATH configuration
```

**Goal**: Auto-build on code changes + Go ready

<!--
PRESENTER NOTES:
- Triggers = автоматизація!
- Poll SCM = Jenkins перевіряє Git кожну хвилину
- Є зміни? → Запускає build
- Альтернатива: webhooks (instant, але потребує setup)
- Go installation = pipeline сам встановлює що потрібно
- Architecture detection = працює на Mac (arm64) і Linux (amd64)
-->

---

## Phase 3: Build + Cleanup

### What You'll Learn
- Workspace cleanup (clean slate!)
- Compiling Go applications
- Version injection via build flags
- Build artifacts organization

### Key Steps Added
```groovy
stage('Cleanup') {
    steps {
        deleteDir()  // ✅ Remember: avoid old artifacts!
    }
}
stage('Build') {
    steps {
        sh '''
            VERSION=$(git describe --tags --always)
            go build -ldflags "-X main.Version=${VERSION}" \
                     -o bin/app cmd/webapp/main.go
        '''
    }
}
```

**Goal**: Compiled binary with version info

<!--
PRESENTER NOTES:
- deleteDir() = застосовуємо best practice!
- Чистий workspace = reproducible builds
- Version injection = build-time metadata
- git describe дає нам readable version
- Після build: bin/app готовий до запуску
- ЦЕ вже mini CI/CD pipeline!
-->

---

## Phase 4: Tests + Coverage

### What You'll Learn
- Running unit tests in pipeline
- Code coverage reporting
- JUnit XML integration
- Test result visualization

### Testing Pipeline Added
```groovy
stage('Test') {
    steps {
        sh '''
            go test -v -coverprofile=coverage.out ./...
            go tool cover -func=coverage.out
        '''
    }
}
post {
    always {
        junit '**/test-results.xml'  // Test results in UI
    }
}
```

**Goal**: Automated testing with visibility

<!--
PRESENTER NOTES:
- Тести = КРИТИЧНА частина CI/CD
- Without tests = немає confidence
- Coverage report показує untested code
- JUnit format = Jenkins розуміє, показує graphs
- post/always = навіть якщо tests fail, ми бачимо results
- Quality gate: можна додати "мінімум 70% coverage"
-->

---

## Phase 5: Static Analysis

### What You'll Learn
- Code quality enforcement
- Multiple linting tools
- Quality gate implementation
- Fast failure strategy

### Tools Added
- **golangci-lint** - Meta-linter (runs 40+ linters!)
- **go vet** - Official Go static analyzer
- **gofmt** - Code formatting checker

```groovy
stage('Static Analysis') {
    steps {
        sh 'golangci-lint run --timeout=5m'
        sh 'go vet ./...'
        sh 'test -z "$(gofmt -l .)"'  // No formatting issues
    }
}
```

**Goal**: Enforce code quality standards

<!--
PRESENTER NOTES:
- ЦЕ quality gate в дії!
- Якщо linter знаходить проблеми → pipeline fails
- golangci-lint = powerful, 40+ checks
- go vet = finds suspicious code
- gofmt = formatting consistency
- Застосовуємо best practice: automate quality checks
- Питання: який linter ви використовуєте?
-->

---

## Phase 6: Artifacts + Archive

### What You'll Learn
- Creating deployment artifacts
- Tarball packaging
- Metadata generation
- Artifact archival in Jenkins
- Build retention policies

### Artifact Contents
```
artifacts/
├── app              # Compiled binary
├── version.txt      # Build metadata
│   ├── VERSION=v1.0.0-abc123
│   ├── COMMIT_SHA=abc123def456
│   ├── BUILD_DATE=2024-12-18T10:30:00Z
│   └── GO_VERSION=1.21.5
└── run.sh           # Startup script
```

```groovy
stage('Archive') {
    steps {
        archiveArtifacts artifacts: 'artifacts/*.tar.gz'
    }
}
```

**Goal**: Production-ready, deployable artifacts

<!--
PRESENTER NOTES:
- FINAL PHASE = повноцінний CI/CD!
- Artifact = все що треба для deployment
- Binary + metadata + startup script
- Tarball = easy to transfer and extract
- archiveArtifacts = зберігається в Jenkins
- Можна download і deploy на сервер
- Retention = не зберігаємо все forever
- ЦЕ вже production-ready pipeline!
-->

---

<!-- _class: lead -->

# 🔄 Complete Pipeline Flow
## Putting It All Together

<!--
PRESENTER NOTES:
- Подивимось на complete flow
- Від commit до deployable artifact
- Це те що ми збудували!
-->

---

## Final Pipeline Architecture (Phase 6)

```
1. Trigger: Code Commit → Git Push
         ↓
2. Trigger: SCM Poll detects change (every minute)
         ↓
3. Cleanup: deleteDir() - Clean workspace
         ↓
4. Checkout: Clone Git repository
         ↓
5. Environment: Install Go 1.21.5, set PATH
         ↓
6. Dependencies: go mod download
         ↓
7. Static Analysis: golangci-lint + go vet + gofmt
         ↓ (Quality Gate #1)
8. Build: Compile with version injection
         ↓ (Quality Gate #2)
9. Test: Run unit tests + coverage
         ↓ (Quality Gate #3)
10. Artifacts: Create tarball + metadata
         ↓
11. Archive: Store in Jenkins
         ↓
12. ✅ Success: Ready for deployment!
```

<!--
PRESENTER NOTES:
- Ось complete flow що ми збудували
- 3 quality gates: static analysis, build, tests
- Якщо любий fails → pipeline stops
- Success = high confidence in quality
- Від commit до artifact = повна автоматизація
- Це можна deploy на production!
-->

---

<!-- _class: lead -->

# 🚀 Let's Get Started!
## Hands-On Lab

<!--
PRESENTER NOTES:
- Час для практики!
- Зараз разом пройдемо setup
- Потім ви самі будете проходити phases
- Я допомагаю якщо є проблеми
-->

---

## Step-by-Step Setup

### 1. Clone Repository
```bash
git clone https://github.com/epam-msdp/CICD-workshop-day1.git
cd CICD-workshop-day1
```

### 2. Start Environment
```bash
# Option A: Vagrant (Recommended)
vagrant up
# Access Jenkins: http://localhost:8080
# Password: 8e6b171e8fd147bf99bdd3507d7bf861

# Option B: Docker
cd docker && docker-compose up -d
# Access Jenkins: http://localhost:8081
```

<!--
PRESENTER NOTES:
- Робимо це РАЗОМ, крок за кроком
- Vagrant up займає 5-10 хвилин
- Docker швидше - 2-3 хвилини
- Поки Jenkins запускається - можна подивитись код
- Питання: всі успішно клонували?
-->

---

## Create Pipeline & Run Phases

### Jenkins Setup (5 min)
1. Open Jenkins → **New Item**
2. Name: `workshop-pipeline`, Type: **Pipeline**
3. Pipeline from SCM → Git
4. URL: `https://github.com/epam-msdp/CICD-workshop-day1.git`
5. Script Path: `jenkins/phases/phase1-basic-checkout.jenkinsfile`

### Progress Through Phases (60-90 min)
- Phase 1 → Build Now → Verify
- Update Script Path to Phase 2 → Build
- Repeat for Phases 3-6
- Each phase adds new capabilities
- Final result: Production-ready pipeline!

<!--
PRESENTER NOTES:
- Проведу через весь процес step-by-step
- Після кожної phase обговоримо результати
- Темп гнучкий - підлаштуємось під аудиторію
- Мета: розуміння, не просто execution
- Phase 6 = повний успіх!
-->

---

<!-- _class: lead -->

# Thank You!
## Happy Building! 🎉

### Resources
- 📖 Repository: https://github.com/epam-msdp/CICD-workshop-day1
- 📋 Complete Guide: `jenkins/phases/README.md`
- 💬 Questions: Ask anytime!

**Remember:**
*"Automate everything you can,*
*test everything you build,*
*and ship with confidence!"*

<!--
PRESENTER NOTES:
- Заключні слова - дякуємо за участь!
- Resources доступні для self-study
- Encourage застосування в real projects
- Stay available для питань після workshop
- Good luck building your pipelines! 🚀
-->
