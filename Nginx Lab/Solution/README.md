# Nginx Lab Solution

## 1. Install Nginx

```bash
sudo apt update
sudo apt install nginx
```

---

## 2. Change Default File

I replaced the default index file with my custom file:

```bash
sudo mv /var/www/html/index.html /var/www/html/Eslam.html
```

Updated Nginx configuration:
/etc/nginx/sites-enabled/default

```bash
server {
    listen 80;
    server_name localhost;

    root /var/www/html;
    index Eslam.html;
}
```

---

## 3. Multiple HTML Files on Different Ports (My Solution)

Instead of creating only 2 files, I implemented a more advanced solution by creating 3 more applications running on different ports.
```bash
server {
    listen 81;
    server_name localhost;

    root /var/www/html;
    index DevOps81.html;
}
server {
    listen 82;
    server_name localhost;

    root /var/www/html;
    index DevOps82.html;
}
server {
    listen 83;
    server_name localhost;

    root /var/www/html;
    index DevOps83.html;
}
```
### Architecture

```bash
server 1 (main VM) with nginx [ Eslam.html, DevOps81, DevOps82, DevOps83 ]
                                 ↓             ↓          ↓        ↓
                                 80            81         82       83
```

---

### HTML Pages

```html
<h1>DevOps 80</h1>
```

```html
<h1>DevOps 81</h1>
```

```html
<h1>DevOps 82</h1>
```

```html
<h1>DevOps 83</h1>
```

---

### Nginx Configuration

```bash
server {
    listen 80;
    server_name localhost;

    root /var/www/html;
    index Eslam.html;
}
server {
    listen 81;
    server_name localhost;

    root /var/www/html;
    index DevOps81.html;
}
server {
    listen 82;
    server_name localhost;

    root /var/www/html;
    index DevOps82.html;
}
server {
    listen 83;
    server_name localhost;

    root /var/www/html;
    index DevOps83.html;
}
```
---

### Why I give each different port?

Using different ports allows multiple applications to run on the same server without conflict. Each port acts as a separate entry point, so Nginx can route traffic correctly to each application.

---

### Important Note

#### Same Server

Ports must be different:

```bash
server 1 (main VM) with nginx [ app1, app2, app3, app4 ]
                                ↓      ↓     ↓    ↓
                                80     81    82   83
```

#### Different Servers

Ports can be the same:

```bash
server 1 with nginx [ app1 ] → 80
server 2 with nginx [ app2 ] → 80
server 3 with nginx [ app3 ] → 80
server 4 with nginx [ app4 ] → 80
```

---

## 4. Load Balancer Between 2 Servers

```bash
upstream backend {
    server 127.0.0.1:81;
    server 127.0.0.1:82;
}

server {
    listen 80;

    location / {
        proxy_pass http://backend;
    }
}
```

---

## 5. Differences Between Apache and Nginx

| Feature      | Apache            | Nginx                        |
| ------------ | ----------------- | ---------------------------- |
| Architecture | Process-based     | Event-driven                 |
| Performance  | Slower under load | High performance             |
| Static Files | Good              | Very fast                    |
| Concurrency  | Limited           | Handles many connections     |
| Use Case     | Dynamic content   | Reverse proxy, load balancer |
