+++
title = "Skip Docker Desktop — Use Colima (and Lazydocker) Instead"
date = 2025-08-01
draft = false
[extra]
display_published = true 
author = "dehamzah"
+++


Docker Desktop is great—but if you're using it at work, you might be paying more than you need to.

Since Docker changed its licensing, companies with over 250 employees or $10M revenue must pay $9/user/month. For a 20-person dev team, that’s $2,160/year—just to run containers locally.

## Enter Colima

[Colima](https://github.com/abiosoft/colima) is a free, open-source alternative that runs Docker containers on macOS using Lima VMs.

## Why Colima?

- Drop-in replacement for Docker Desktop
- Supports Docker CLI, Compose, nerdctl
- Kubernetes support with --kubernetes
- Fast, lightweight, and easy to set up

Installation and usage in Mac OS:

```
brew install colima docker
colima start
```

## Need a UI? Use Lazydocker

Missing Docker Desktop’s dashboard? Try [Lazydocker](https://github.com/jesseduffield/lazydocker) — a terminal UI to manage containers, images, volumes, and logs.

Installation and usage in Mac OS:

```
brew install lazydocker
lazydocker
```

## Conclusion

If you're using Docker Desktop for work, switching to Colima + Lazydocker can save money without sacrificing functionality.

But if only for personal use, Docker Desktop is fine.