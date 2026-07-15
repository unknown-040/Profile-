  `~/.gitconfig`
  
    # ============================================================
    # Identity
    # ============================================================
    [user]
        name = KARTIK RAO
        email = kartikrao501@gmail.com
        # signingkey = YOUR_GPG_KEY_ID     # uncomment if using GPG
    
    # ============================================================
    # Editor & pager
    # ============================================================
    [core]
        editor = YOUR_EDITOR
        autocrlf = input                   # macOS/Linux; use 'true' on Windows
        excludesfile = ~/.gitignore_global
        whitespace = trailing-space,-space-before-tab
        # attributesfile = ~/.gitattributes_global
    
    [diff]
        tool = vscode
        algorithm = histogram
    
    [difftool "vscode"]
        cmd = code --wait --diff $LOCAL $REMOTE
    
    [merge]
        tool = vscode
        conflictstyle = zdiff3
        keepBackup = false
    
    [mergetool "vscode"]
        cmd = code --wait --merge $LOCAL $BASE $REMOTE $MERGED
    
    [pager]
        diff = delta
        log = delta
        reflog = delta
        show = delta
        branches = delta
        grep = delta
    
    # ============================================================
    # Behavior
    # ============================================================
    [init]
        defaultBranch = main
    
    [pull]
        rebase = true                     # keep a linear history
    
    [fetch]
        prune = true                      # auto-remove deleted remote branches
    
    [rebase]
        autoStash = true
        updateRefs = true
    
    [push]
        autoSetupRemote = true            # so `git push` just works on new branches
        followTags = true
    
    [commit]
        gpgsign = false                   # set to true after configuring GPG
        template = ~/.gitmessage.txt
    
    [tag]
        sort = version:refname
        gpgSign = false
    
    [branch]
        sort = committerdate
    
    [rerere]
        enabled = true                    # remember conflict resolutions
    
    # ============================================================
    # Colors
    # ============================================================
    [color]
        ui = auto
        diff = auto
        status = auto
        branch = auto
        interactive = auto
    
    [color "diff"]
        meta = yellow bold
        frag = magenta bold
        old = red bold
        new = green bold
    
    [color "status"]
        added = green
        changed = yellow
        untracked = cyan
    
    # ============================================================
    # Aliases — short and memorable
    # ============================================================
    [alias]
        # one-letter shortcuts
        s = status -sb
        l = log --oneline --graph --decorate -20
        ll = log --oneline --graph --decorate --all
        a = add -A
        c = commit
        p = push
        pl = pull
        co = checkout
        br = branch
        rb = rebase
        st = stash
    
        # staging
        aa = add -A
        au = add -u
        ap = add -p
    
        # commit shortcuts
        cm = commit -m
        ca = commit --amend
        cna = commit --amend --no-edit
        cfix = commit --fixup
        cmsg = commit --template ~/.gitmessage.txt
    
        # logs
        lg = log --oneline --graph --decorate --all
        lh = log --oneline --graph --decorate -10 HEAD
        lgg = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --all
        hist = log --follow -p --stat
        contributors = shortlog --summary --numbered --no-merges
    
        # diff
        d = diff
        ds = diff --staged
        dw = diff --word-diff
        dt = difftool
        dts = difftool --staged
    
        # branches & remotes
        branches = branch -a
        remotes = remote -v
        tags = tag -l
        cleanup = "!git branch --merged | grep -v '\\*\\|main\\|master\\|develop' | xargs -n 1 git branch -d"
    
        # inspection
        aliases = config --get-regexp ^alias\\.
        unstage = reset HEAD --
        uncommit = reset --soft HEAD~1
        discard = checkout --
        standup = log --since yesterday --author $(git config user.email) --oneline
    
        # searching
        grep = grep -Ii
        find = log --all --grep
        files = ls-files | grep
    
        # maintenance
        fixup = "!git commit --fixup=$1"          # usage: git fixup <sha>
        squash = "!git rebase -i --autosquash $1" # usage: git squash <sha>
        wip = "!git add -A && git commit -m 'wip'"
        undo = reset --soft HEAD~1
        nuke = "!git clean -fdx && git reset --hard"
    
    # ============================================================
    # URL rewrites (uncomment & edit if you have multiple GitHub accounts)
    # ============================================================
    # [url "git@github.com-YOUR_WORK_USER:"]
    #     insteadOf = "git@github.com:"
    
    # ============================================================
    # Conditional includes for work/personal split
    # ============================================================
    # [includeIf "gitdir:~/work/"]
    #     path = ~/.gitconfig-work
    # [includeIf "gitdir:~/personal/"]
    #     path = ~/.gitconfig-personal
  
  Companion file #1 — `~/.gitignore_global`
  
    # OS
    .DS_Store
    Thumbs.db
    desktop.ini
    
    # Editors
    .vscode/
    .idea/
    *.swp
    *.swo
    *~
    .fleet/
    .cursor/
    
    # Logs / temp
    *.log
    *.tmp
    *.bak
    *.swp
    .cache/
    
    # Env
    .env
    .env.*
    !.env.example
    
    # Node
    node_modules/
    npm-debug.log*
    yarn-debug.log*
    yarn-error.log*
    .pnpm-debug.log
    
    # Python
    __pycache__/
    *.py[cod]
    *.egg-info/
    .venv/
    venv/
    .pytest_cache/
    .mypy_cache/
    .ruff_cache/
    
    # Build output
    dist/
    build/
    out/
    target/
    *.min.js
    *.min.css
  
  Companion file #2 — `~/.gitmessage.txt` (commit template)
  
    # <type>(<scope>): <subject>            (50 chars max, imperative)
    #
    #   types: feat | fix | docs | style | refactor | perf | test | chore
    #
    # Why is this change needed?
    #   - bullet 1
    #   - bullet 2
    #
    # Anything reviewers should know?
    #   - links, screenshots, tradeoffs
    #
    # Breaking changes?
    #   -
    
    # ---------- Do not write below this line (filled by Git) ----------
  
  Quick install steps
  
    # 1. back up any existing config
    cp ~/.gitconfig ~/.gitconfig.bak
    
    # 2. paste the file
    nano ~/.gitconfig          # or code, vim, etc.
    
    # 3. install the companions
    curl -o ~/.gitignore_global https://example.com/your-raw-gist   # or paste manually
    nano ~/.gitmessage.txt
    
    # 4. verify
    git config --list --show-origin
    git aliases
  
  Optional: enable `delta` for nicer diffs
  
    brew install git-delta          # macOS
    # or: sudo apt install git-delta
  
  ---
  
