
# 🛡️ Deploying PQ-WireGuard on Azure

This guide walks you through setting up **PQ-WireGuard** on an **Azure VM** and connecting from a local client using a custom WireGuard module with **post-quantum cryptography**.

---

## 🧰 Requirements

- Azure account
- Two Linux systems (e.g., one Azure VM + one local Linux)
- AVX2-compatible CPU (Intel ≥ Haswell, AMD ≥ Excavator)
- Root access on both systems
- `PQ-WireGuard` source code

---

## ☁️ Step 1: Create Azure VM

1. Go to [Azure Portal](https://portal.azure.com)
2. **Create VM**:
   - **Image**: Ubuntu 22.04 LTS
   - **Size**: Standard D2s_v3 (or any AVX2-capable)
   - **Authentication**: SSH Key
   - **Inbound Ports**: Open **UDP 12345**
3. SSH into the VM:
   ```bash
   ssh azureuser@<azure-vm-ip>
   ```

---

## ⚙️ Step 2: Install Dependencies

On the **Azure VM**:

```bash
sudo apt update && sudo apt install -y \
    build-essential \
    linux-headers-$(uname -r) \
    git \
    curl \
    wget \
    iproute2 \
    iptables \
    wireguard-tools
```

---

## 🔧 Step 3: Build PQ-WireGuard

```bash
# Clone PQ-WireGuard source (adjust URL if needed)
git clone [https://your.repo.url/PQ-WireGuard.git](https://github.com/MallocSecurity/PQ-Wireguard)
cd PQ-WireGuard/WireGuard

# Build and install
make
sudo make install

# Load kernel module
sudo modprobe wireguard
```

---

## 📁 Step 4: Upload Configuration

From your local machine (after running `setup_vpn_config.sh`):

```bash
scp -r server/ azureuser@<azure-vm-ip>:/wg_config
```

On the Azure VM:

```bash
cd /wg_config
chmod +x run.sh
sudo ./run.sh
```

---

## 💻 Step 5: Set Up the Client

On your local Linux machine:

```bash
sudo cp -r client-1/ /wg_config
cd /wg_config
chmod +x run.sh
sudo ./run.sh
```

Update `client-1/wg0.conf`:

```ini
Endpoint = <azure-vm-ip>:12345
```

---

## ✅ Step 6: Test the VPN

On the **client**:

```bash
ping 10.0.0.1
```

On the **server (Azure VM)**:

```bash
ping 10.0.0.2
```

---

## 🔒 Security Tips

- Limit UDP 12345 access to your client IP via Azure NSG
- Disable SSH password login
- Monitor the VPN with logs or `tcpdump`

---

## 📌 Notes

- PQ-WireGuard is not compatible with regular WireGuard clients
- Both client and server must use AVX2-supported hardware and the custom kernel module
