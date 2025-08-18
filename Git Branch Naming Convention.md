
# Git Branch Naming Convention

To keep our workflow clean, consistent, and easy to understand, all branches should follow these naming rules.

---

## 📌 General Rules
- Use **lowercase letters** and **hyphens** (`-`) to separate words.  
- Always prefix the branch name with its **type** (feature, bugfix, hotfix, release, chore, etc.).  
- When applicable, include the **issue/ticket ID** (e.g., from Jira, GitHub Issues).  
- Keep branch names **short but descriptive**.  

---

## 🚀 Branch Types

### 1. Features
For new features and enhancements.
```

feature/<short-description>
feature/<ticket-id>-<short-description>

```
**Examples**
```

feature/user-authentication
feature/PROJ-123-payment-gateway

```

---

### 2. Bug Fixes
For non-urgent bug fixes.
```

bugfix/<short-description>
bugfix/<ticket-id>-<short-description>

```
**Examples**
```

bugfix/login-error
bugfix/4567-cart-total-miscalculation

```

---

### 3. Hotfixes
For urgent production issues.
```

hotfix/<short-description>

```
**Examples**
```

hotfix/security-patch
hotfix/checkout-crash

```

---

### 4. Releases
For preparing production releases.
```

release/<version>

```
**Examples**
```

release/1.2.0
release/2025.08

```

---

### 5. Chores
For maintenance, refactoring, CI/CD, or dependency updates.
```

chore/<short-description>

```
**Examples**
```

chore/dependency-updates
chore/ci-config

```

---

### 6. Experiments
For research and experimental work (may never be merged).
```

experiment/<short-description>

```
**Examples**
```

experiment/new-search-algorithm

```

---

## ✅ Summary
| Type      | Prefix      | Example                          |
|-----------|-------------|----------------------------------|
| Feature   | `feature/`  | `feature/user-authentication`    |
| Bug Fix   | `bugfix/`   | `bugfix/1234-login-error`        |
| Hotfix    | `hotfix/`   | `hotfix/security-patch`          |
| Release   | `release/`  | `release/1.2.0`                  |
| Chore     | `chore/`    | `chore/dependency-updates`       |
| Experiment| `experiment/`| `experiment/new-search-algorithm` |

---
