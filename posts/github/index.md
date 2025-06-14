# GitHub Workflow


> "Behind every well-organized repo lies a story of purpose, experimentation, and constant iteration."

After years of hands-on experience in cybersecurity and development, I’ve learned that code is just one part of the battle. The **structure** we build around our work — our process, our repository, our mindset — is what allows our projects to scale, evolve, and remain resilient.

This GitHub Page is more than just a collection of code — it's a reflection of my workflow, my evolution, and the systems I’ve put in place to stay adaptable in a fast-changing world.

## Architecture

To give you a better idea of how I separate concerns and automate deployments, here's a simplified view of the repository workflow

Each directory, file, and branch serves a purpose. Here’s a high-level overview of how my main repository is structured:

```mermaid
flowchart LR
    L[Local Files]
    L2[Local Files]

    GH_Private[(GitHub Private Repo)]
    GH_Public[(GitHub Public Repo)]

    Actions[GitHub Actions]

    GH_Pages[GitHub Pages]
    Vercel[Vercel]
    More[More...]

    L -->|Git Push| GH_Private
    L2 -->|Git Push| GH_Public

    GH_Private -->|Git Submodule| GH_Public
    GH_Public --> Actions

    Actions -->|Deploy| GH_Pages
    Actions -->|Deploy| Vercel
    Actions -->|Deploy| More

```

## Branch

I follow a Git Flow-inspired model because it enforces discipline in collaborative work.
It allows features, hotfixes, and releases to evolve in parallel, with clear boundaries and responsibilities

Maintaining a clean and sustainable workflow is key in collaborative and long-term projects. Here’s the branching model I follow:

- **`main`** – The **production** branch. Only stable, reviewed, and tested code lives here.
- **`develop`** – The **integration** branch for ongoing development. Features are merged here after review and testing.
- **`feature/*`** – Used for individual feature development (e.g., `feature/log-capture`, `feature/api-hardening`).
- **`bugfix/*`** – Quick patches or specific bug resolutions.
- **`release/*`** – For preparing stable releases, including documentation and final testing.

This structure allows me to stay agile while ensuring quality and traceability in the work I deliver.

```mermaid
gitGraph
   commit id: "Initial commit"
   branch develop
   checkout develop
   commit id: "Init project structure"
   commit id: "Setup CI/CD"
   
   branch feature/log-capture
   checkout feature/log-capture
   commit id: "Add log capture module"
   checkout develop
   merge feature/log-capture id: "Merge log-capture"

   branch feature/api-hardening
   checkout feature/api-hardening
   commit id: "Improve API input validation"
   commit id: "Add rate limiting"
   checkout develop
   merge feature/api-hardening id: "Merge api-hardening"

   branch bugfix/token-expiry
   checkout bugfix/token-expiry
   commit id: "Fix token expiry bug"
   checkout develop
   merge bugfix/token-expiry id: "Merge token fix"

   branch release/v1.0
   checkout release/v1.0
   commit id: "Prepare release v1.0"
   checkout main
   merge release/v1.0 id: "Release v1.0 to production"
   commit id: "Hotfix README"

   checkout develop
   branch feature/ui-redesign
   commit id: "Start UI redesign"
```

{{< music auto="https://music.163.com/#/album?id=268399691" fixed=true >}}

<!--
![Снег падает без конца, покрывая души, как невидимый саван. Каждый снежинка — это забытое воспоминание, утерянный звук в безбрежной тишине. Здесь, в самом сердце зимы, когда ветер воет как зверь, человек идет, не зная точно, куда](images/coffee.jpg 'Человек продолжает идти, без цели, без вопросов, ведомый странной уверенностью, что всё уже написано, но всё равно он должен искать, снова и снова')
-->

---

> Author: [ProxyGeek](https://github.com/Pr0xyG33k)  
> URL: https://Pr0xyG33k.github.io/posts/github/  

