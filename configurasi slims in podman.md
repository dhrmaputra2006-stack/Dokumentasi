# Setup Podman

## 1. Konfigurasi SubUID dan SubGID

```bash
sudo nvim /etc/subuid
sudo nvim /etc/subgid
```

## 2. Aktifkan Podman

```bash
sudo systemctl enable --global podman
```

## 3. Buat Direktori Konfigurasi Container

```bash
mkdir -p ~/.config/containers
```

Edit file konfigurasi storage:

```bash
nvim ~/.config/containers/storage.conf
```

Isi file:

```ini
[storage]

driver = "overlay"

[storage.options.overlay]

mount_program = ""

mountopt = "userxattr"
```

## 4. Konfigurasi Registry Container

```bash
sudo nvim /etc/containers/registries.conf
```

Cari dan sesuaikan:

```ini
# unqualified-search-registries = ["docker.io"]
```

---

# Install SLiMS Container

## 1. Masuk ke Direktori Container

```bash
cd ~/.config/containers
```

## 2. Install Podman Compose

```bash
sudo pacman -S podman-compose
```

## 3. Download Source SLiMS

```bash
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```

## 4. Ekstrak File ZIP

```bash
sudo pacman -S unzip
unzip master.zip
```

## 5. Verifikasi File

```bash
ls
```

## 6. Konfigurasi Port Unprivileged

```bash
sudo nvim /etc/sysctl.d/99-custom.conf
```

Isi file:

```ini
net.ipv4.ip_unprivileged_port_start=80
```

Terapkan konfigurasi:

```bash
sudo sysctl --system
```

## 7. Masuk ke Direktori Compose

```bash
cd compose
ls
```

## 8. Backup File Compose

```bash
mv docker-compose.yaml docker-compose.yaml.bck
ls
```

## 9. Edit File Compose

```bash
nvim docker-compose.yaml
```
