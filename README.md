# 🌐 Lab 14: Networking in Containers (Podman)

## 🎯 Objectives

By the end of this lab, you will:

- Understand container networking fundamentals in Podman
- Learn how to inspect network configurations
- Practice running containers with port publishing and custom networks

---

## 📋 Prerequisites

- Linux system (Fedora, RHEL, CentOS recommended)
- Podman installed  
  - `sudo dnf install podman` (RHEL-based)
  - `sudo apt install podman` (Debian-based)
- Basic terminal usage

---

## 🧪 Task 1: List Podman Networks

### 🔹 Subtask 1.1: Check Available Networks

```bash
podman network ls
```

✅ *Expected Output*:

```
NETWORK ID    NAME     DRIVER
xxxxxxxxxxxx  podman   bridge
```

📌 *Explanation*: Podman comes with a default `bridge` network for basic container communication.

---

### 🔹 Subtask 1.2: Inspect Default Network

```bash
podman network inspect podman
```

🔍 *Key Concepts*:
- Displays subnet ranges, gateway, DNS settings, etc.
- Useful for connectivity troubleshooting.

---

## 🧾 Task 2: Inspect Network Settings

### 🔹 Subtask 2.1: Create a Custom Network

```bash
podman network create lab-network
```

✅ *Verify Creation*:

```bash
podman network ls | grep lab-network
```

---

### 🔹 Subtask 2.2: Inspect Custom Network

```bash
podman network inspect lab-network
```

📌 *Expected*: JSON output with subnet, gateway, and IPAM details.

💡 *Troubleshooting*:  
If the network fails to create, check rootless permissions:

```bash
podman info | grep rootless
```

---

## 🚀 Task 3: Run Containers with Port Publishing

### 🔹 Subtask 3.1: Run Container with Port Mapping

```bash
podman run -d --name webapp -p 8080:80 docker.io/library/nginx
```

📌 *Explanation*:
- `-p 8080:80`: Maps host port `8080` → container port `80`
- `-d`: Runs in detached/background mode

---

### 🔹 Subtask 3.2: Verify Port Accessibility

```bash
curl http://localhost:8080
```

✅ *Expected*: HTML output from Nginx welcome page

💡 *Troubleshooting*:
- Check container status:

  ```bash
  podman ps
  ```

- Check firewall ports (Fedora/CentOS/RHEL):

  ```bash
  sudo firewall-cmd --list-ports
  ```

---

### 🔹 Subtask 3.3: Attach Container to Custom Network

Stop and re-run the container:

```bash
podman stop webapp

podman run -d --name webapp -p 8080:80 \
  --network lab-network \
  docker.io/library/nginx
```

✅ *Confirm Network Assignment*:

```bash
podman inspect webapp | grep -A 5 "Networks"
```

---

## ✅ Summary

| Task                  | Command / Tool                         | Purpose                                     |
|-----------------------|----------------------------------------|---------------------------------------------|
| View networks         | `podman network ls`                    | List available Podman networks              |
| Inspect network       | `podman network inspect <name>`        | View network configuration                  |
| Create network        | `podman network create lab-network`    | Create isolated container network           |
| Run container         | `podman run -p 8080:80`                | Expose container services on host ports     |
| Attach to network     | `--network lab-network`                | Assign container to a specific network      |
| Debug connectivity    | `curl`, `firewall-cmd`, `podman logs` | Ensure service is accessible                |

---

## 👨‍💻 Author

**Abdullha Saleem** – DevOps | Linux | Docker | Podman | Kubernetes | AIOps  
📫 *Find more labs on my GitHub or connect with me on LinkedIn.*
