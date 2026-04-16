# Apache HTTP Server Lab - My Answers

## 1. Install Apache HTTP Server

On Ubuntu, I install Apache using:

```bash
sudo apt update
sudo apt install apache2
```

On RHEL/CentOS, I use:

```bash
sudo yum install httpd
```

2. Create HTML Pages

I create two simple HTML files inside the web root directory /var/www/html.

```bash
page1.html
<h1>This is Page 1</h1>
page2.html
<h1>This is Page 2</h1>
```

3. Redirect Page1 → Page2

I redirect Page1 to Page2 using Apache Redirect rule:

```bash
Redirect /page1.html /page2.html
```

I place this inside the Apache configuration file or virtual host.

4. Enable Authentication (Username & Password)

First, I install required tools:

```bash
sudo apt install apache2-utils
```

Then I create a password file:

```bash
htpasswd -c /etc/apache2/.htpasswd eslam
```

After that, I configure Apache like this:

```bash
<Directory "/var/www/html/secure">
    AuthType Basic
    AuthName "Restricted Content"
    AuthUserFile /etc/apache2/.htpasswd
    Require valid-user
</Directory>
```

5. Allow Access to Specific User Only

```bash
I allow only a specific user by writing:
<Directory "/var/www/html/secure">
    AuthType Basic
    AuthName "Restricted Content"
    AuthUserFile /etc/apache2/.htpasswd
    Require user eslam #instead of valid-user
</Directory>
```

6. Disable Directory Listing

I disable directory listing using:

```bash
Options -Indexes
```

7. Change Default Index Page

I change the default index page by setting:

```bash
DirectoryIndex default.html
```
