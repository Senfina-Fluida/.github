# FLUIDA 

# P2P Submarine Swaps Between Lightning Network and TON

This project enables **peer-to-peer (P2P) submarine swaps** between the **Bitcoin Lightning Network** and **TON blockchain**. It consists of three main components:
1. **Telegram Bot**: Facilitates communication between users and manages the swap lifecycle.
2. **MiniApp**: Provides a user-friendly interface for creating, selecting, and executing swaps.
3. **TON Smart Contract**: Handles the logic for locking, unlocking, and refunding tgBTC.

The system allows users to securely exchange Bitcoin (via Lightning) and tgBTC (on TON) in a **trustless, decentralized manner** without intermediaries.

---

## **How It Works**

### **1. Swap Creation**
- A user creates a swap by specifying the **amount** and **destination** (Lightning or TON) using the bot or MiniApp.
- The bot stores the swap details (e.g., amount, destination, and user's chat ID) and marks it as **pending**.

### **2. Swap Selection**
- Another user views the list of **pending swaps** using the bot or MiniApp.
- They select a swap, and the bot notifies the swap creator that their swap has been selected.

### **3. Swap Execution**
- **For tgBTC-to-Lightning Swaps**:
  - The swap creator locks their tgBTC in the TON smart contract via the MiniApp by providing:
    - A **hashlock** (derived from the Lightning invoice).
    - A **timelock** (set to the invoice's expiration time or expiration time plus a buffer).
  - The counterparty pays the Lightning invoice and reveals the **preimage** to claim the tgBTC.
- **For Lightning-to-tgBTC Swaps**:
  - The counterparty locks their Bitcoin on the Lightning Network.
  - The swap creator pays the Lightning invoice and reveals the preimage to claim the tgBTC.

### **4. Refund Mechanism**
- If the swap is not completed before the **timelock** expires, the user can refund their locked tgBTC.

---

## **Components**

### **1. Telegram Bot**
- **Commands**:
  - `/start`: Opens the MiniApp to get started.
  - `/create [destination] [amount]`: Creates a new swap (e.g., `/create Lightning 10`).
  - `/list`: Lists all available swaps.
  - `/mypending`: Lists swaps created by the user.
  - `/myselected`: Lists swaps selected by the user.
  - `/select [swapId]`: Selects a specific swap to participate in.
  - `/finish [swapId]`: Completes a selected swap.
  - `/help`: Displays all available commands.
- **Functionality**:
  - Manages swap creation, selection, and completion.
  - Notifies users when their swap is selected or completed.

### **2. MiniApp**
- **Features**:
  - **Swap Creation**: Users can create swaps by specifying the amount and destination.
  - **Swap Listing**: Users can view and select available swaps.
  - **Swap Execution**:
    - For tgBTC-to-Lightning swaps, users lock tgBTC in the TON smart contract.
    - For Lightning-to-tgBTC swaps, users pay the Lightning invoice and claim tgBTC.
  - **Refund Mechanism**: Users can refund their tgBTC if the swap isn’t completed before the timelock expires.
- **Integration**:
  - Communicates with the bot using `WebApp.sendData`.
  - Interacts with the TON blockchain using the `tonconnect` library and the TONX RPC provider.

### **3. TON Smart Contract**
- **Functionality**:
  - Stores swap details (e.g., hashlock, timelock, and participant addresses).
  - Verifies preimages to ensure atomicity.
  - Enforces timelocks to allow refunds if the swap isn’t completed on time.
- **Key Operations**:
  - **Lock tgBTC**: Users lock tgBTC in the contract by providing a hashlock and timelock.
  - **Claim tgBTC**: Users claim tgBTC by providing the correct preimage.
  - **Refund tgBTC**: Users refund their tgBTC if the timelock expires.

---

## **Why Use This Project?**
- **Trustless**: No need for intermediaries; swaps are executed securely using cryptographic protocols.
- **User-Friendly**: The bot and MiniApp make it easy to manage swaps, even for non-technical users.
- **Cross-Chain**: Enables seamless exchanges between Bitcoin (Lightning) and TON (tgBTC).
- **Decentralized**: Built on blockchain technology, ensuring transparency and security.

---

## **License**
This project is licensed under the [MIT License](LICENSE).
