# Use SSH Authentication with a Repo on GitHub

## Introduction

This guide outlines the steps to set up SSH authentication for a GitHub repository. Using SSH keys for authentication enhances security and removes the need to enter your username and password when pushing code to GitHub.

## Prerequisites

- A GitHub account
- Git installed on your local machine
- SSH configured on your local machine

## Steps to Set Up SSH Authentication

### 2. Generate SSH Keys

```sh
cd ~/.ssh
ssh-keygen -t rsa -b 4096 -C "your-email@example.com"
ls
cat id_rsa.pub  # View the generated public key
```

Copy the output and add it to your GitHub account under:
**GitHub -> Settings -> SSH and GPG keys -> New SSH Key**

![Git hub ](<../../../Pictures/Screenshots/Screenshot 2025-02-04 151455.png>)

### 3. Add the SSH Key to the SSH Agent

```sh
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

### 4. Test SSH Connection to GitHub

```sh
ssh -T git@github.com
```

If successful, you should see a message like:

```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

### 5. Configure GitHub Repository with SSH

1. Log in to GitHub.
2. Click on your profile picture (top-right corner) and select **Settings**.
3. Navigate to **SSH and GPG keys** in the left sidebar.
4. Click **New SSH Key**.
5. Provide a descriptive title (e.g., `my-laptop-github-key`).
6. Paste the copied **public key** (`id_rsa.pub`) into the key field.
7. Click **Add SSH Key**.
8. Verify the key is added successfully.

### Createing Repository in Github account

Steps to Create a Repository in GitHub

1. Create a New Repository on GitHub

Log in to your GitHub account.

Click on your profile picture (top-right corner) and select Your repositories.

Click New to create a new repository.

Enter a repository name (e.g., tech501-test-ssh).

Optionally, add a description.

Choose repository visibility: Public or Private.

(Optional) Check "Initialize this repository with a README".

Click Create repository.

# 2. Connect Local Repository to GitHub Using SSH

```
mkdir tech501-test-ssh
cd tech501-test-ssh/
git init
echo "# tech501-test-ssh" > README.md
git branch -M main
git remote add origin git@github.com:Nmudassar/tech501-test-ssh.git
git add .
git commit -m "added readme with one line"
git push -u origin main

```

## 7. Verify and Push Code

```cd ~/onedrive/Documents/github/tech501-test-ssh
git add .
git commit -m "test file one file"
git push -u origin main
```

8. Troubleshooting

Ensure the SSH key is correctly added to GitHub.

Use `ssh-add -l` to check if the key is loaded.

If you face permission issues, verify your SSH key's file permissions:

```chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub
```

If you receive an authentication failure:

```ssh -vT git@github.com

```

This will provide verbose debugging output.

### Conclusion

By setting up SSH authentication, you eliminate the need to enter your credentials manually while working with GitHub repositories. This method enhances security and streamlines the development workflow.

### 7. Troubleshooting

- Ensure the SSH key is correctly added to GitHub.
- Use `ssh-add -l` to check if the key is loaded.
- If you face permission issues, verify your SSH key's file permissions:
  ```sh
  chmod 600 ~/.ssh/id_rsa
  chmod 644 ~/.ssh/id_rsa.pub
  ```
- If you receive an authentication failure:
  ```sh
  ssh -vT git@github.com
  ```
  This will provide verbose debugging output.

## Conclusion

By setting up SSH authentication, you eliminate the need to enter your credentials manually while working with GitHub repositories. This method enhances security and streamlines the development workflow.
