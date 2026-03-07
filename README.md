# ==========================================
# File: task14-steps.txt
# Goal: Create a Free-Tier Ubuntu Linux VM
# OS: Ubuntu 24.04 LTS (or 22.04 LTS)
# Access: Key-based SSH
# ==========================================

## STEP 1: Generate an SSH Key Pair (Local Machine)
# Open your local terminal and generate a secure ED25519 key pair.
# Leave the passphrase blank for automatic login, or add one for extra security.

ssh-keygen -t ed25519 -f ~/.ssh/ubuntu_free_vm -C "ubuntu-vm-key"

# This creates two files:
# 1. ~/.ssh/ubuntu_free_vm (Private Key - keep this safe)
# 2. ~/.ssh/ubuntu_free_vm.pub (Public Key - this goes to the cloud provider)

# View and copy your public key:
cat ~/.ssh/ubuntu_free_vm.pub


## STEP 2: Launch the VM (AWS EC2 Free Tier Example)
# If using Google Cloud (e2-micro) or Oracle Cloud (Always Free ARM), 
# the UI steps are similar, but the free-tier instance names differ.

1. Log in to the AWS Management Console and go to "EC2".
2. Click "Launch instance".
3. Name: "Ubuntu-Free-VM-Task14"
4. OS Images (AMI): Select "Ubuntu" -> "Ubuntu Server 24.04 LTS" (or 22.04). 
   * Ensure it is marked "Free tier eligible".
5. Instance type: Select "t2.micro" (or "t3.micro" depending on region).
   * Ensure it is marked "Free tier eligible".
6. Key pair (login): 
   * Click "Create new key pair" OR "Import key pair".
   * If importing, paste the contents of `~/.ssh/ubuntu_free_vm.pub` that you copied earlier.
7. Network settings -> Security Firewall:
   * Create a new security group.
   * Check "Allow SSH traffic from" -> select "My IP" (recommended for security) or "Anywhere" (0.0.0.0/0).
8. Configure storage:
   * Leave at 8GB gp3, or increase up to 30GB (the maximum allowed under the AWS Free Tier).
9. Click "Launch instance".


## STEP 3: Connect to the VM via SSH
# Wait a minute for the VM to boot up. 
# Find the "Public IPv4 address" in your AWS EC2 Instance summary.

# Restrict permissions on your private key (required by SSH):
chmod 400 ~/.ssh/ubuntu_free_vm

# Connect to the server using the default 'ubuntu' user:
# Replace <PUBLIC_IP> with your actual instance IP address.
ssh -i ~/.ssh/ubuntu_free_vm ubuntu@<PUBLIC_IP>

# If prompted with "Are you sure you want to continue connecting (yes/no/[fingerprint])?", type:
yes


## STEP 4: Initial Server Setup (Minimal Maintenance)
# Once logged in, it is best practice to update the package lists and upgrade the system.

sudo apt update && sudo apt upgrade -y

# (Optional) Remove unused packages to keep the minimal footprint:
sudo apt autoremove -y

# Done! The VM is now running, secured with key-based SSH, and ready for use.
