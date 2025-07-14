# ⚖️ Load Balancing using Nginx and Flask

This project demonstrates **Load Balancing** using:

- `Nginx` as the load balancer
- `Gunicorn` to serve a simple Flask application
- Two backend servers on different ports

## 🧱 Architecture Overview

```
Client -> Nginx Load Balancer (192.168.0.10)
        -> Backend1 (192.168.0.11:8001)
        -> Backend2 (192.168.0.12:8002)
```

---

## 🛠️ Backend Server Setup

### 1. Install Flask and Gunicorn
```bash
pip install flask gunicorn
```

### 2. Create app.py

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello_World!"

if __name__ == '__main__':
    app.run(debug=False, host='0.0.0.0')
```

### 3. Run on Different Ports

- Backend 1:
```bash
gunicorn --bind 0.0.0.0:8001 app:app
```

- Backend 2:
```bash
gunicorn --bind 0.0.0.0:8002 app:app
```

---

## ⚙️ Nginx Load Balancer Setup

### 1. Install Nginx
```bash
sudo apt install nginx
```

### 2. Edit Configuration

```nginx
upstream backend_servers {
    server 192.168.0.11:8001;
    server 192.168.0.12:8002;
}

server {
    listen 80;

    location / {
        proxy_pass http://backend_servers;
    }
}
```

### 3. Restart Nginx
```bash
sudo systemctl restart nginx
```

---

## 🧪 Testing

Open a browser or run:
```bash
curl http://192.168.0.10
```
Results will alternate between the two backend servers.
