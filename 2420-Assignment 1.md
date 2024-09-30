
# **Creating and Configuring a DigitalOcean Droplet with Arch Linux**

## **Introduction**
In this tutorial, I’ll walk you through setting up a DigitalOcean droplet running Arch Linux, using SSH keys for secure access, and automating basic tasks with cloud-init. By the end of this guide, you'll have a functional remote server that you can connect to securely without using passwords.

---

## **Step 1: Generate SSH Keys**

SSH keys are used for secure access to your server. First, we need to generate a pair of SSH keys on your local machine.

### **Generate SSH Key Pair**

Open your terminal or PowerShell and run the following command:

```bash
ssh-keygen -t rsa -b 4096
```

This command creates two files: a private key (`id_rsa`) and a public key (`id_rsa.pub`). When asked where to save the key, just press **Enter** to accept the default location.

If you want extra security, you can set a passphrase, but it’s optional.

### **Copy Your Public Key**

To connect securely, you need to copy your public key and add it to your DigitalOcean account. To see your public key, run:

```bash
cat ~/.ssh/id_rsa.pub
```

Copy the key that starts with `ssh-rsa` and save it somewhere for the next step.

---

## **Step 2: Add Your SSH Key to DigitalOcean**

1. Log into **[DigitalOcean](https://www.digitalocean.com/)**.
2. Go to **Settings > Security > SSH Keys**.
3. Click **Add SSH Key**, then paste the public key you copied earlier.
4. Give it a name you’ll recognize (e.g., "My Laptop Key").

---

## **Step 3: Create a Custom Droplet Running Arch Linux**

Now that your SSH key is added, you can create a droplet.

1. **Go to the Droplets page** and click on **Create Droplet**.
2. In the **Choose an Image** section, select **Arch Linux** under the **Custom Images** tab.
3. Choose the plan that works for you (the $5/month plan is fine for this project).
4. In the **Authentication** section, select **SSH Key** and choose the key you added earlier.
5. Scroll down to the **Advanced Options** section.

---

## **Step 4: Automate Setup with Cloud-Init**

Cloud-init is a great way to automatically set up your droplet with basic configurations (like creating a user, installing packages, etc.).

### **Add Cloud-Init Script**

Paste the following cloud-init configuration into the **User Data** field in the Advanced Options:

```yaml
#cloud-config
users:
  - name: newuser
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh_authorized_keys:
      - ssh-rsa your-public-key-here
package_update: true
packages:
  - sudo
  - git
ssh_pwauth: false
```

This script:
- Creates a new user (`newuser`) with passwordless sudo access.
- Installs basic packages like `sudo` and `git`.
- Disables root login over SSH.

Replace `your-public-key-here` with your actual public SSH key.

---

## **Step 5: Connect to Your Droplet**

Once the droplet is ready, you’ll get its IP address from DigitalOcean.

To connect to your server using SSH, run:

```bash
ssh newuser@your-droplet-ip
```

Replace `your-droplet-ip` with the actual IP address of your droplet. If everything is set up correctly, you’ll be logged into your new server without needing to enter a password.

---


## **Conclusion**

That’s it! You’ve set up a secure DigitalOcean droplet running Arch Linux, added an SSH key, and automated the initial setup using cloud-init. Now you can use your server for whatever projects or tasks you need.

