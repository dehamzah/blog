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


### How Colima Works

Colima uses [Lima](https://github.com/lima-vm/lima) under the hood to run a lightweight Linux virtual machine on your Mac. Inside this VM, Colima sets up Docker (or containerd) so you can run containers just like you would with Docker Desktop.

When you use the Docker CLI (`docker run`, `docker compose`, etc.), your commands are sent to the Docker daemon running inside the Colima-managed VM. Colima handles networking, file sharing, and resource allocation between your Mac and the VM, making the experience seamless.

Key points:
- **Isolation:** Containers run inside a Linux VM, not directly on macOS.
- **Integration:** Docker CLI and Compose work out of the box.
- **Performance:** Colima is fast and uses fewer resources than Docker Desktop.
- **Customization:** You can easily adjust CPU, memory, and disk settings for the VM.

This architecture lets you use Docker on macOS without the overhead or licensing restrictions.


### Installation and usage


```
# Installation
brew install colima docker

# Start colima with default config
colima start

# Check the status
colima status
```

### Configure Colima VM Spec

Sometime you need to specify more VM Spec to Colima so that you can run more containers.

```
# Check Current Colima VM Spec
colima list

# Stop Colima First
colima stop

# Start with new Config
# You can see all the available config with `colima start --help`
colima start --cpu 10 --memory 16 --disk 120

# Check the new VM Spec
colima list
```

Next time you start with `colima start` it will remember the last configuration, you can verify it with command `colima list`.


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