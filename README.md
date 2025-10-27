Got it 👍 — here’s your guide **fully reformatted for GitHub**, with professional markdown structure, bold section titles, proper syntax highlighting for code, and clean spacing for readability.

---

# 🚀 **KRNL Attestor + Real Estate DApp Full Setup Guide**

This guide walks you through wallet setup, API creation, smart contract deployment, Docker configuration, Attestor setup, and frontend launch for the **KRNL Real Estate Investment DApp**.

---

## 🪙 **Step 1: Create and Fund Wallet**

1. Create a new **Ethereum wallet** (MetaMask recommended).
2. Fund it with **0.15 Sepolia ETH** using any of these faucets:

   * [Google Cloud Faucet](https://cloud.google.com/application/web3/faucet/ethereum/sepolia)
   * <img width="1144" height="516" alt="image" src="https://github.com/user-attachments/assets/9a6d28fb-0a60-45fa-a36e-2469c6ca7b40" />

   * [Sepolia Faucet PK910](https://sepolia-faucet.pk910.de/)
   * <img width="451" height="757" alt="image" src="https://github.com/user-attachments/assets/9939910e-f2e9-4090-aa02-292fbdbb578a" />

3. Keep your **private key safe** — you’ll need it later.

---

## 🐳 **Step 2: Create Docker Account and Repository**

1. Sign up on [Docker Hub](https://hub.docker.com/).
2. <img width="682" height="729" alt="image" src="https://github.com/user-attachments/assets/8f57eaa8-d854-4b76-8d2d-63bf5674d41a" />

3. Create a new repository: [Create Repository](https://hub.docker.com/repositories)
<img width="1568" height="654" alt="image" src="https://github.com/user-attachments/assets/af6f40ff-b4bc-4fa3-9b62-8dbd22c66a51" />

   * **Repository Name:** `attestor-realestate`
   * **Visibility:** Public or Private
   * Click **Create**.

---

## 🔍 **Step 3: Etherscan API Setup**

1. Register on [Etherscan](https://etherscan.io/register).
2. <img width="674" height="584" alt="image" src="https://github.com/user-attachments/assets/ab3c3010-2e2f-44b7-8ee6-16af79696d86" />

3. Go to your [API Dashboard](https://etherscan.io/apidashboard).
4. <img width="664" height="359" alt="image" src="https://github.com/user-attachments/assets/349af442-4d81-4269-baee-628d38cb0996" />

5. Click **+ADD → Name your project → Create New API Key**.
6. Copy and save your **API Key Token**.

---

## 🔐 **Step 4: Privy Setup**

1. Go to [Privy Dashboard](https://dashboard.privy.io/).
2. <img width="628" height="588" alt="image" src="https://github.com/user-attachments/assets/4b41d0d9-44b8-4725-ba10-b39791bb9776" />

3. Under **Applications**, click **New App**.
4. Fill in:

   * App Name: any name
   * Platform: **Web and Mobile**
5. Click **Create App** and save your **App ID** and **App Secret**.

---

## ⚙️ **Step 5: Pimlico API Key**

1. Go to [Pimlico Dashboard](https://dashboard.pimlico.io/sign-up).
2. <img width="559" height="826" alt="image" src="https://github.com/user-attachments/assets/a9ebb818-63e3-4ed5-93c7-3d2d4e1b0856" />

3. Navigate to [API Keys](https://dashboard.pimlico.io/apikeys).
4. <img width="679" height="360" alt="image" src="https://github.com/user-attachments/assets/282ce9de-ace0-4c4a-8c92-a76f66236ae6" />

5. Click **Generate New API Key**, name it, and copy your key.

---

## 🌐 **Step 6: Get Sepolia RPC URL**

1. Go to [MetaMask Developer Portal](https://developer.metamask.io/).
2. <img width="948" height="803" alt="image" src="https://github.com/user-attachments/assets/acc1c838-05cd-42aa-a33f-5d9cb5eae4e2" />

3. Create a new **API Key**.
4. Under **Active Endpoints**, copy the **Ethereum Sepolia RPC URL**.

---

## 💻 **Step 7: VPS Setup**

```bash
# Connect to your VPS and create a working directory
mkdir krnl
cd krnl

# Clone the Real Estate DApp repository
git clone https://github.com/KRNL-Labs/poc-dapp-realestateinvestment-7702.git hello-krnl
cd hello-krnl

# Install screen and start a new session
sudo apt install screen -y
screen -S krnl
```

---

## 🧾 **Step 8: Environment Setup**

```bash
cd contracts
cp .env.example .env
nano .env
```

**Edit and fill the following fields:**

```
PRIVATE_KEY=<your_wallet_private_key> (include 0x)
MOCK_USDC_ADDRESS=0xF2Ea67F83b58225edF11F3Af4A5733B3E0844509
DELEGATED_ACCOUNT_ADDRESS=0x9969827E2CB0582e08787B23F641b49Ca82bc774
ETHERSCAN_API_KEY=<your_etherscan_api_key>
SEPOLIA_RPC_URL=<your_personal_rpc_url>
```

Save and exit:

```
CTRL + X → Y → Enter
```

Activate environment:

```bash
source .env
```

---

## 🧩 **Step 9: Install Forge Dependencies**

```bash
forge install OpenZeppelin/openzeppelin-contracts
forge install eth-infinitism/account-abstraction@v0.7.0
forge install foundry-rs/forge-std
```

---

## 🏗️ **Step 10: Deploy the Contract**

```bash
forge script script/Deploy.s.sol --rpc-url sepolia --broadcast
```

After deployment, note your:

```
REAL_ESTATE_ADDRESS=<YOUR_REAL_ESTATE_ADDRESS>
```

---

## 🐋 **Step 11: Docker Login & PAT Setup**

```bash
sudo docker login -u <your_docker_username>
```

Use your **Personal Access Token (PAT)** as the password.
Generate it from [Docker Account Settings](https://app.docker.com/accounts) → **Personal Access Tokens → Generate New Token** (grant Read/Write/Delete).

---

## ⚡ **Step 12: Attestor Keygen Setup**

```bash
docker pull ghcr.io/krnl-labs/attestor-keygen:latest
sudo docker images
```

Find your `IMAGE ID` and tag it:

```bash
sudo docker tag <IMAGE_ID> <docker_username>/attestor-realestate:latest
sudo docker push <docker_username>/attestor-realestate:latest
```

---

## 🔧 **Step 13: Setup the Attestor**

```bash
curl https://public.mypinata.cloud/ipfs/bafkreifvezdhwvmi6psqqk6vxalazp56ovx3fmgqkmfu5ih5xyxsdbfixi -o create-attestor-standalone.sh
chmod +x create-attestor-standalone.sh
sudo ./create-attestor-standalone.sh
```

**Fill in when prompted:**

```
Project Name: realestate
Docker Registry: docker.io
Docker Username: <your_docker_username>
Private Key: <without 0x>
```

**Add secrets:**

```
rpcSepoliaURL=<your_rpc>
pimlico-apikey=<your_pimlico_api_key>
OPENAI_API_KEY=mock-api
done
```

Detach and reattach the screen if needed:

```bash
CTRL + A + D
screen -r krnl
```

Successful output:

```
════════════════════════════════════════════════════════
     Attestor Created Successfully! 🎉
════════════════════════════════════════════════════════
Project: realestate
Build ID: aa3283
Docker Image: docker.io/<username>/attestor-realestate:latest
```

---

## 🖥️ **Step 14: Frontend Setup**

```bash
cd ../frontend
cp .env.example .env
nano .env
```

**Fill the following:**

```
VITE_PRIVY_APP_ID=<your_privy_app_id>
VITE_PRIVY_APP_SECRET=<your_privy_app_secret>
VITE_CHAIN_ID=11155111
VITE_DELEGATED_ACCOUNT_ADDRESS=0x9969827E2CB0582e08787B23F641b49Ca82bc774
VITE_TARGET_CONTRACT_OWNER=<YOUR_WALLET_ADDRESS>
VITE_REAL_ESTATE_INVESTMENT_ADDRESS=<YOUR_REAL_ESTATE_ADDRESS>
VITE_MOCK_USDC_ADDRESS=0xF2Ea67F83b58225edF11F3Af4A5733B3E0844509
VITE_ATTESTOR_IMAGE=docker.io/<username>/attestor-realestate:latest
VITE_RPC_URL=<your_rpc_url>
```

Save and run:

```bash
source .env
npm install
npm run dev
```

Expected output:

```
VITE v7.1.7  ready in 763 ms
➜  Local:   http://localhost:5173/
➜  Network: use --host to expose
```

---

## 🌍 **Step 15: Access the App**

```bash
# Allow port and reload firewall
sudo ufw allow 5173/tcp
sudo ufw reload

# Verify if port is active
lsof -i :5173
```

If not accessible, use SSH tunneling:

```bash
ssh -L 5173:localhost:5173 root@<your_vps_ip>
```

Then open [http://localhost:5173](http://localhost:5173) in your browser.
<img width="1701" height="653" alt="image" src="https://github.com/user-attachments/assets/367f0b58-0694-4f11-92a9-84b22a87b2fe" />

---

## 🧠 **Step 16: Using the Interface**

1. Connect your **MetaMask wallet** (with 0.15 Sepolia ETH).
2. Fund the **Embedded Wallet** with ~0.13 Sepolia ETH.
3. Click **Mint 1M USDC**.
4. Go to **Smart Account Authorization → Authorize**.
5. <img width="1438" height="680" alt="image" src="https://github.com/user-attachments/assets/a77bb36a-18b8-4fa0-8607-cb76ada99ba1" />

6. Under **Workflow Execution**:

   * Scenario A → **Run Property Analysis**
   * <img width="588" height="768" alt="image" src="https://github.com/user-attachments/assets/9f656c84-ff6f-4fa2-b3e1-d2c23132c6ae" />

   * Scenario B → **Purchase Property** → sign message
   * <img width="590" height="774" alt="image" src="https://github.com/user-attachments/assets/df314651-54bd-4d12-889b-ac5a37641984" />

✅ Your **KRNL Real Estate Attestor** is now fully operational!

---

