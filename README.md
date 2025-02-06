# Nginx Installation on Ubuntu

This guide will walk you through installing the latest Nginx on an Ubuntu machine. It includes all the necessary steps, from installing prerequisites to verifying the installation.

---

## Prerequisites

Ensure that your system is up to date and that the required packages are installed.

```bash
sudo apt update
sudo apt install curl gnupg2 ca-certificates lsb-release ubuntu-keyring
```

---

## Step 1: Import the Official Nginx Signing Key

To ensure the authenticity of the Nginx packages, you need to import the official Nginx signing key.

```bash
curl https://nginx.org/keys/nginx_signing.key | gpg --dearmor \
    | sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null
```

---

## Step 2: Verify the Key

Verify that the downloaded file contains the proper key by running:

```bash
gpg --dry-run --quiet --no-keyring --import --import-options import-show /usr/share/keyrings/nginx-archive-keyring.gpg
```

### Expected Output:

```
pub   rsa2048 2011-08-19 [SC] [expires: 2027-05-24]
      573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62
uid                      nginx signing key <signing-key@nginx.com>
```

---

## Step 3: Add Nginx Repository

### Stable Version:

To set up the apt repository for stable Nginx packages, run:

```bash
echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] \
http://nginx.org/packages/ubuntu `lsb_release -cs` nginx" \
    | sudo tee /etc/apt/sources.list.d/nginx.list
```

### Mainline Version:

If you would prefer the mainline version, run:

```bash
echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] \
http://nginx.org/packages/mainline/ubuntu `lsb_release -cs` nginx" \
    | sudo tee /etc/apt/sources.list.d/nginx.list
```

---

## Step 4: Set Up Repository Pinning

To prioritize Nginx's official repository over the distribution-provided packages, add the following pinning configuration:

```bash
echo -e "Package: *\nPin: origin nginx.org\nPin: release o=nginx\nPin-Priority: 900\n" \
    | sudo tee /etc/apt/preferences.d/99nginx
```

---

## Step 5: Install Nginx

Now that the repository is set up, you can install Nginx by running the following commands:

```bash
sudo apt update
sudo apt install nginx
```

---

## Step 6: Start and Enable Nginx Service

Once Nginx is installed, ensure that it starts automatically and is running:

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

You can verify the status of Nginx by running:

```bash
sudo systemctl status nginx
```

---

## Step 7: Configure Your Firewall (Optional)

If you're using UFW (Uncomplicated Firewall), allow traffic for Nginx:

```bash
sudo ufw allow 'Nginx Full'
```

Check UFW status:

```bash
sudo ufw status
```

---

## Step 8: Verify Nginx Installation

After installation, verify that Nginx is working by visiting your server’s IP address in a web browser or by running:

```bash
curl http://localhost
```

You should see the default Nginx welcome page.

---

## Troubleshooting

If you encounter issues, try restarting Nginx:

```bash
sudo systemctl restart nginx
```

Or check the Nginx logs for any errors:

```bash
sudo tail -f /var/log/nginx/error.log
```

---

## Conclusion

You have successfully installed Nginx on Ubuntu and are now ready to configure and serve your web applications.

---

