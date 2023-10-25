# Use Windows PC as Intelligent Power Manager

<!-- TOC start -->

- [Step 1: Install OpenSSH on Windows:](#step-1-install-openssh-on-windows)
- [Step 2: Generate SSH Keys:](#step-2-generate-ssh-keys)
- [Step 3: Copy the Public Key to the Linux NAS:](#step-3-copy-the-public-key-to-the-linux-nas)
- [Step 4: Test SSH Connection:](#step-4-test-ssh-connection)
- [Step 5: Create a Batch File on Windows:](#step-5-create-a-batch-file-on-windows)
- [Step 6: Execute the Batch File on Shutdown:](#step-6-execute-the-batch-file-on-shutdown)
- [What is SSH](#what-is-ssh)

<!-- TOC end -->

<!-- TOC --><a name="use-windows-pc-as-intelligent-power-manager"></a>

To allow a Windows computer to send a command to a Linux NAS via SSH to shut down the NAS when the Windows PC is turned off, you need to create a script or a batch file. Here is a general method to achieve this:

## Step 1: Install OpenSSH on Windows:

Ensure that your Windows computer has the OpenSSH client installed. You can do this through Windows settings.

## Step 2: Generate SSH Keys:

Generate SSH keys on the Windows computer with the following PowerShell command:

```powershell
ssh-keygen
```

Follow the instructions to generate the keys. This will create a public key (id_rsa.pub) that you will use to connect to the Linux NAS without a password.

# Step 3: Copy the Public Key to the Linux NAS:

Copy the content of the generated `id_rsa.pub` file on the Windows computer and add it to the `~/.ssh/authorized_keys` file on the Linux NAS. You can do this via SSH or by manually copying and pasting the file.

```powershell
cat ~/.ssh/id_rsa.pub | ssh root@nas "mkdir ~/.ssh; cat >> ~/.ssh/authorized_keys"
```

# Step 4: Test SSH Connection:

Test if you can connect to the Linux NAS from the Windows computer without a password using the following command:

(Replace "root" with the actual user account on the Linux NAS, and "nas" with the NAS's IP address or hostname.)


```bash
ssh root@nas
```


# Step 5: Create a Batch File on Windows:

Create a batch file (e.g., `shutdown_nas.bat`) on the Windows computer with the following content:

(Replace "root" with the actual user account on the Linux NAS.)

<details>
<summary>Command "shutdown -h now"</summary>

```batch
@echo off
ssh root@nas "shutdown -h now"
```
</details>

<details>
<summary>Command "/sbin/poweroff"</summary>

```batch
@echo off
ssh root@nas "/sbin/poweroff"
```
</details>

# Step 6: Execute the Batch File on Shutdown:

To run this batch file when you shut down the Windows computer, you can set a group policy. Here's how:

- Press Win + R to open the Run window.
- Type "gpedit.msc" and press Enter.
- Go to "Computer Configuration" > "Windows Settings" > "Scripts (Startup/Shutdown)".
- Choose "Shutdown" and click "Add," then select the batch file you created.

Now, the batch file will run when you shut down the Windows computer, sending the SSH command to shut down the Linux NAS before the Windows PC is powered off.


# What is SSH
![What is SSH and how does it work?](https://surfshark.com/wp-content/uploads/2022/04/SSH_vs_VPN_2.svg)
[source](https://surfshark.com/blog/ssh-vs-vpn)

What is SSH and how does it work?

SSH stands for “Secure Shell.” It is a network protocol that allows you to safely access remote devices and transfer data or run commands. This technology encrypts and disguises your traffic, ensuring the security of your connection.

This means that you can access network resources from practically anywhere. It’s particularly useful when you want secure communication between your computer at work and home. 


