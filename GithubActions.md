<aside>
💡

https://www.youtube.com/watch?v=Xwpi0ITkL3U

</aside>

## Github On Events:

!image.png

### HOW TO TRIGGER THEM:

!image.png

# *Github Actions other core/basic stuff:*

tell me in super short what is pull request

A **Pull Request (PR)** is a **request to merge your code changes into another branch** (usually `main`).

**Example:**

1. Create a feature branch.
2. Make changes and push them.
3. Open a **Pull Request**.
4. Others review your code.
5. If approved, it's merged into `main`.

**Super short:** **PR = "Please review and merge my code."**

Today 9:39 PM 

Teach me this Github Actions feature, I am not being able to understand about it,

This is one of the most confusing GitHub Actions topics initially because there are **3 different ways to pass data**, and people mix them up.

The trick is to first understand the hierarchy:

```
Workflow
│
├── Job 1
│   ├── Step 1
│   ├── Step 2
│   └── Step 3
│
└── Job 2
    ├── Step 1
    └── Step 2
```

- A **job** contains **steps**.
- Steps inside the **same job** can share some things.
- Different jobs **cannot** directly share variables.

---

# Method 1: Passing data between steps (using `$GITHUB_OUTPUT`)

Imagine Step 1 calculates a version number.

```
steps:
  - name: Generate version
    id: version
    run:|
      echo "version=1.0.5" >> $GITHUB_OUTPUT

  - name: Use version
    run:|
      echo "Version is ${{ steps.version.outputs.version }}"
```

## What's happening?

### Step 1

```
echo"version=1.0.5" >>$GITHUB_OUTPUT
```

means

> "Save an output called `version`."
> 

It stores:

```
version = 1.0.5
```

Notice the step has an **id**:

```
id: version
```

Without an `id`, GitHub doesn't know which step's output you're referring to.

---

### Step 2

Reads it:

```
${{ steps.version.outputs.version }}
```

Break it down:

```
steps
   ↓
version      (step id)
   ↓
outputs
   ↓
version      (output name)
```

Result:

```
1.0.5
```

---

# Think of it like a box

```
Step 1

version = 1.0.5
        ↓
$GITHUB_OUTPUT
```

Then Step 2 opens the box.

```
steps.version.outputs.version
```

---

# Method 2: Passing Data Between steps using Environment variables (`$GITHUB_ENV`)

This one is easier.

Instead of creating an **output**, you're creating an **environment variable**.

Step 1

```
steps:
  - run:|
      echo "NAME=Taha" >> $GITHUB_ENV
```

Now every following step can simply do

```
- run:|
    echo $NAME
```

Output

```
Taha
```

Notice the difference.

With OUTPUT:

```
${{ steps.step-id.outputs.foo }}
```

With ENV:

```
$NAME
```

Much shorter.

---

# Think of it like your terminal

When you write

```
exportNAME=Taha
```

every command afterwards can use

```
echo$NAME
```

`$GITHUB_ENV` works similarly, except GitHub manages it for you.

---

# Method 3: Pass data from one job to another

This is where beginners usually get lost.

Suppose you have

```
Job A
```

and

```
Job B
```

Jobs are isolated.

Job B **cannot** see Job A's environment variables.

```
Job A
NAME=Taha
```

Job B

```
NAME ????
```

Nothing exists.

---

Instead, Job A must expose an **output**.

Job A

```
jobs:
  producer:

    outputs:
      username: ${{ steps.create.outputs.name }}
```

This says

> "Take the output from this step and expose it as a job output."
> 

---

The step:

```
steps:
  - id: create
    run:|
      echo "name=Taha" >> $GITHUB_OUTPUT
```

Now the job output becomes

```
username = Taha
```

---

Job B

Must depend on Job A.

```
needs: producer
```

Meaning

```
Wait for producer first.
```

Now access it:

```
${{ needs.producer.outputs.username }}
```

Break it apart.

```
needs
    ↓
producer      (job)
    ↓
outputs
    ↓
username
```

Result

```
Taha
```

---

# Why not use `$GITHUB_ENV`?

Because `$GITHUB_ENV` only works **inside one job**.

```
Job A

Step1
↓

Step2
↓

Step3
```

Yes ✔

But

```
Job A
↓

Job B
```

No ❌

---

# Summary

## Inside one job

### Use `$GITHUB_ENV`

```
Step 1

echo "NAME=Taha" >> $GITHUB_ENV
```

Later

```
echo$NAME
```

---

## Inside one job (step output)

```
Step 1

echo "age=25" >> $GITHUB_OUTPUT
```

Later

```
${{ steps.stepid.outputs.age }}
```

---

## Between jobs

Job A

```
Step output
      ↓
Job output
```

Job B

```
needs.producer.outputs.age
```

---

# A memory trick

```
$GITHUB_ENV
```

Think:

> **ENV = Environment variable**
> 
> 
> Works only inside the current job.
> 

---

```
$GITHUB_OUTPUT
```

Think:

> **OUTPUT = I want another step or another job to read this value.**
> 

---

## Cheat Sheet

| Need | Use | Read with |
| --- | --- | --- |
| Share variable with later steps in the **same job** | `$GITHUB_ENV` | `$VAR_NAME` |
| Share a **step's output** with another step | `$GITHUB_OUTPUT` | `${{ steps.<id>.outputs.<name> }}` |
| Share data from **Job A → Job B** | `$GITHUB_OUTPUT` + `jobs.<job>.outputs` + `needs` | `${{ needs.<job>.outputs.<name> }}` |

Once you remember **where the data needs to go** (same step → same job → another job), choosing between `$GITHUB_ENV` and `$GITHUB_OUTPUT` becomes much more intuitive.

Today 11:16 PM

whats this input and output in github actions and the $GITHUB_ENV & $GITHUB_OUTPUT and also this id thing

This is the core concept. Once you understand **input**, **output**, **`id`**, **`$GITHUB_ENV`**, and **`$GITHUB_OUTPUT`**, GitHub Actions becomes much easier.

---

# First, what is an Input and Output?

Think about a function in Python.

```
defadd(a,b):# Inputsreturna+b# Output
```

- **Inputs** = data you give to something.
- **Outputs** = data it returns.

GitHub Actions works the same way.

For example, imagine a step generates a Docker image tag.

```
Step:
Generate Docker Tag

Input:
Current commit

↓

Output:
v1.2.3
```

That output can be used by another step.

---

# What is `id`?

Suppose you have two steps.

```
steps:
  - name: Build
    run: echo "Building..."

  - name: Test
    run: echo "Testing..."
```

Can another step say

> "Give me the output of Build"?
> 

No.

Because **Build** is just a display name.

GitHub needs a unique identifier.

So we add:

```
steps:
  - name: Build
    id: build
    run: ...
```

Now GitHub knows

```
Step

name = Build
id   = build
```

The `id` is like a **variable name**.

Just like in Python:

```
x=10
```

`x` is the identifier.

Similarly:

```
id: build
```

`build` is the identifier.

---

# What is `$GITHUB_OUTPUT`?

Imagine Step 1 discovers a version.

```
Step 1

Version = 1.0.5
```

How can Step 2 know it?

Step 1 writes it into a special file.

```
echo"version=1.0.5" >>$GITHUB_OUTPUT
```

Think of `$GITHUB_OUTPUT` as a **mailbox**.

```
Step 1

version=1.0.5

↓

📬 $GITHUB_OUTPUT
```

Now GitHub remembers:

```
This step produced

version = 1.0.5
```

Another step can read it.

```
${{ steps.build.outputs.version }}
```

Break it apart:

```
steps
   ↓
build       (step id)
   ↓
outputs
   ↓
version
```

Result:

```
1.0.5
```

---

# Why is `id` required?

Imagine:

```
steps:

- id: build
- id: test
- id: deploy
```

Each step might produce outputs.

GitHub needs to know:

```
Whose output?

build?
test?
deploy?
```

That's why we write

```
steps.build.outputs.version
```

instead of just

```
outputs.version
```

---

# What is `$GITHUB_ENV`?

This is completely different.

Instead of creating an **output**, you're creating an **environment variable**.

Example:

```
echo"NAME=Taha" >>$GITHUB_ENV
```

Think:

```
Step 1

↓

Environment

NAME=Taha
```

Every step after this can simply do

```
echo$NAME
```

No `steps`.

No `outputs`.

Just

```
$NAME
```

---

# Difference between ENV and OUTPUT

### ENV

```
Step 1

↓

NAME=Taha

↓

Environment
```

Later:

```
echo$NAME
```

---

### OUTPUT

```
Step 1

↓

version=1.0.5

↓

Step Output
```

Later:

```
${{ steps.build.outputs.version }}
```

---

# Real Example

### Step 1

```
- name: Generate Version
  id: version
  run:|
    echo "tag=v1.0.0" >> $GITHUB_OUTPUT
    echo "AUTHOR=Taha" >> $GITHUB_ENV
```

Now GitHub stores

```
Output

tag = v1.0.0
```

Environment

```
AUTHOR = Taha
```

---

### Step 2

```
- run:|
    echo "Author: $AUTHOR"
    echo "Tag: ${{ steps.version.outputs.tag }}"
```

Output:

```
Author: Taha
Tag: v1.0.0
```

---

# Why do we even have outputs?

Imagine this step:

```
Create Docker Image

↓

Generated image name

↓

myapp:v1.2.3
```

The next step needs that value.

Instead of generating it again, it just reads the **output**.

```
Step 1
↓

Output
↓

Step 2
```

---

# Easy Analogy

Imagine an office.

### `$GITHUB_ENV`

Everyone in the office can see a whiteboard.

```
Whiteboard

NAME=Taha
```

Every employee (later step) can read it.

---

### `$GITHUB_OUTPUT`

One employee finishes some work.

He puts the result into an envelope.

```
Employee

↓

Envelope

version=1.2.3
```

Another employee asks:

> "Give me the output from employee **build**."
> 

GitHub finds the envelope using the step's `id` and gives you the value.

---

## One-line memory trick

- **`id`** → The step's unique name (so GitHub can find it).
- **`$GITHUB_OUTPUT`** → Save a value as a **step output** for other steps (or jobs).
- **`$GITHUB_ENV`** → Create an **environment variable** that later steps in the **same job** can use.
- **Input** → Data you give to a step, action, or workflow.
- **Output** → Data that step, action, or workflow produces for others to use.

what are github enviorments

This is another GitHub Actions feature, but **GitHub Environments are NOT the same as environment variables (`$GITHUB_ENV`)**. The names are similar, which confuses many beginners.

---

# What is a GitHub Environment?

A **GitHub Environment** is a **named deployment target** where you deploy your application.

Examples:

```
Development
Testing
Staging
Production
```

Imagine your application has three places it can be deployed:

```
Developer
     │
     ▼
Development
     │
     ▼
Staging
     │
     ▼
Production
```

Each of these can be a GitHub Environment.

---

# Why do we need them?

Different environments need different settings.

For example:

| Environment | Database |
| --- | --- |
| Development | dev-db |
| Staging | staging-db |
| Production | prod-db |

Instead of hardcoding values, GitHub lets each environment have its own:

- Secrets
- Variables
- Protection rules

---

# Example

Suppose your workflow deploys your app.

```
jobs:
  deploy:
    runs-on: ubuntu-latest

    environment: production
```

Here:

```
environment: production
```

means

> "This job is deploying to the **Production** environment."
> 

---

# Environment Secrets

Suppose you need an API key.

Development

```
API_KEY = dev123
```

Production

```
API_KEY = prod987
```

You store them separately.

When deploying to Development:

```
environment: development
```

GitHub automatically provides

```
API_KEY = dev123
```

When deploying to Production:

```
environment: production
```

GitHub provides

```
API_KEY = prod987
```

Same workflow.

Different secrets.

---

# Protection Rules

This is one of the biggest reasons to use GitHub Environments.

Imagine someone pushes code.

Without protection:

```
Push
 ↓
Deploy to Production 😨
```

With a Production environment:

```
Push
 ↓
Approval Required
 ↓
Manager clicks Approve
 ↓
Deploy
```

So Production deployments can require:

- Manual approval
- Specific reviewers
- Wait timers
- Branch restrictions

---

# Environment Variables vs GitHub Environments

These are **completely different**.

### `$GITHUB_ENV`

Inside a workflow:

```
echo"NAME=Taha" >>$GITHUB_ENV
```

Creates a temporary variable.

Only exists while that job runs.

---

### GitHub Environment

Created in:

```
Repository
    ↓
Settings
    ↓
Environments
```

Example:

```
Development
Staging
Production
```

These are permanent configurations for deployments.

---

# Real-world Example

Suppose your company has:

```
Development Server
Staging Server
Production Server
```

Development

```
URL = dev.myapp.com
DB = dev database
API_KEY = dev key
```

Production

```
URL = myapp.com
DB = production database
API_KEY = production key
```

Your workflow can deploy to either one just by changing:

```
environment: development
```

or

```
environment: production
```

GitHub automatically uses the correct secrets and variables for that environment.

---

# Quick Comparison

| Feature | Purpose |
| --- | --- |
| `$GITHUB_ENV` | Create a temporary environment variable for later **steps in the same job**. |
| `$GITHUB_OUTPUT` | Pass values between steps or jobs. |
| **GitHub Environment** | Represents a deployment target (Development, Staging, Production) with its own secrets, variables, and deployment protection rules. |

### Memory trick

- **`$GITHUB_ENV`** → *Environment variable* (temporary, inside one job).
- **GitHub Environment** → *Deployment environment* (Development, Staging, Production).

## GitHub Runners:

!image.png

## GitHub Artifacts:

!image.png

## GitHub Caches

!image.png

## GitHub Token & permissions

!image.png

## GitHub External Authentication & OIDC Auth

!image.png

Open ID Connect practical is shown in video

## GitHub Popular Actions:

!image.png

!image.png

!image.png

## GitHub Re-usable Actions

!image.png

2:

!image.png

3:

!image.png

## Github Actions Workflow fail fix Approach:

!image.png

## Best Practices

!image.png

!image.png

!image.png
