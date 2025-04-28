[Why Use Containers?](#Containers101)

***On Node2***
# Container Build

### QUESTION #20:
Download containerfile from https://github.com/sachinyadav3496/Text-To-PDF/archive/refs/heads/master.zip
- Do not make any modification. 
- Build image with this container file.

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #20:
* First, you will need to install the Container Management Group packages:
```
[root@node2 ~]# yum install @”Container Management"
```
* Then, switch to the account of user that will be running the container (It is assumed that user named `andrew` exists on the system):

```
[root@node2 ~]# ssh andrew@localhost
[andrew@localhost ~]$ wget https://github.com/sachinyadav3496/Text-To-PDF/archive/refs/heads/master.zip
[andrew@localhost ~]$ unzip master.zip
[andrew@localhost ~]$ rm -rf master.zip
[andrew@localhost ~]$ cd Text-To-PDF-master
[andrew@localhost ~]$ podman build -t myapp .
```
* To check: Type ```podman image ls``` or ```podman images```:
```
[andrew@localhost ~]$ Text-To-PDF-master]$ podman image ls
REPOSITORY                          TAG       IMAGE ID       CREATED         SIZE
localhost/myapp                     Latest    9ba525b6e56a    17 seconds ago  479 MB
registry.fedoraproject.org/fedora   Latest    0e79e93fc530    2 days ago      196 MB
```

* SUCCESS!!


### Alternatively you may try this Containerfile:
```
git clone https://github.com/ucomesdag/container-rocky-systemd.git
```


---

<br><br><br><br>

# Containers101

# Rootless Containers with Podman: The Basics

As a developer, you have probably heard a lot about containers. A container is a unit of software that provides a packaging mechanism that abstracts the code and all of its dependencies to make application builds fast and reliable. An easy way to experiment with containers is with the Pod Manager tool (Podman), which is a daemonless, open source, Linux-native tool that provides a command-line interface (CLI) similar to the Docker container engine.

In this document, we will:

- Explain the benefits of using containers and Podman
- Introduce rootless containers and their importance
- Show how to use rootless containers with Podman through a practical example

---

## 1. Why Containers?

Containers isolate your applications from the underlying computing environment. They bind application logic and dependencies into a single unit, enabling developers to focus on code rather than environment discrepancies. Operations teams benefit by managing application deployment without worrying about software versions or configuration.

Containers virtualize at the operating system (OS) level, making them lightweight compared to virtual machines (VMs), which virtualize at the hardware level. Key advantages:

- **Low hardware footprint**
- **Environment isolation**
- **Quick deployment**
- **Multiple environment deployments**
- **Reusability**

---

## 2. Why Podman?

Podman simplifies finding, running, building, sharing, and deploying OCI‑compatible containers and images. Its main advantages include:

- **Daemonless**: No central service is required, unlike Docker.
- **Layer control**: Fine‑grained management of image layers.
- **Fork/exec model**: Uses a direct process model rather than client/server.
- **Rootless support**: Run containers without requiring root privileges on the host.

---

## 3. Why Rootless Containers?

Rootless containers allow unprivileged users to create, run, and manage containers without admin rights. Benefits:

- **Enhanced security**: Compromised container daemons or runtimes do not yield host root access.
- **Multi‑user support**: Multiple users can run containers on the same host safely.
- **Nested isolation**: Run containers within containers without privilege escalation.

From a security standpoint, running with fewer privileges reduces risk. Podman achieves this by spawning container processes as child processes of the user, with no central daemon.

---

## 4. Example: Using Rootless Containers with Podman

### 4.1. System Requirements

- **RHEL Enterprise Linux (RHEL) 7.7** or greater

### 4.2. Configuration

1. **Install dependencies:

   ```bash
   sudo yum install slirp4netns podman -y
   # Or install the Container Tools module:
   sudo yum install @container-tools -y
   ```
   - `slirp4netns` provides network connectivity in an unprivileged user namespace.

3. **Create a non-root user** (e.g., `rhel`):‑root user** (e.g., `rhel`):

   ```bash
   sudo useradd -c "Red Hat" rhel
   sudo passwd rhel
   ```

This new user is automatically configured for rootless Podman.

### 4.3. Connect as the User

Instead of `su -`, connect using a direct login method:

```bash
ssh rhel@localhost
```

This preserves the environment variables needed for rootless Podman.

### 4.4. Pull a RHEL Image

1. **Pull the Universal Base Image (UBI)**:

   ```bash
   podman pull registry.access.rhel.com/ubi7/ubi
   ```

2. **Inspect the image**:

   ```bash
   podman run registry.access.rhel.com/ubi7/ubi cat /etc/os-release
   ```

3. **List local images**:

   ```bash
   podman images
   ```

> Note: Creating and running containers from these images can be covered in a follow‑up guide.

### 4.5. Verify Rootless Configuration

Check UID/GID mappings inside the user namespace:

```bash
podman unshare cat /proc/self/uid_map
```

This confirms that Podman is using subordinate user IDs without root.

---

## 5. Conclusion and Tips

- **Storage location**: Rootless container data resides under the user’s home (e.g., `$HOME/.local/share/containers/storage`).
- **Privilege isolation**: Containers run with expanded UIDs/GIDs inside the namespace but have no extra host privileges.
- **Security best practice**: Fewer privileges on the host reduce the attack surface in multi‑user environments.

Enjoy experimenting with rootless containers using Podman!
