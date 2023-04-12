```
sudo apt install python3
```
```
sudo apt install python3-venv
```
```
mkdir flaskapp
```
```
cd flaskapp
```
```
python3 -m venv flaskappenv
```
```
source flaskappenv/bin/activate
```
```
pip install wheel
```
```
pip install flask gunicorn
```

# flaskapp.py

```
nano myflaskapp.py
```
```
from flask import Flask
app = Flask(__name__)

@app.route("/")
def myflask():
    return "Here is!"

if __name__ == "__main__":
    app.run(host='0.0.0.0')
```

```
sudo ufw allow 5000 
```

# wsgi.py
```
nano wsgi.py
```
```
from myflaskapp import app

if __name__ == "__main__":
    app.run()
```

```
gunicorn myflaskapp:app 
```
```
gunicorn --bind 0.0.0.0:5000 wsgi:app
```
```
deactivate
```

# system file
```
sudo nano /etc/systemd/system/flaskapp.service
```
```
[Unit]
Description=Gunicorn instance to serve flaskapp
After=network.target

[Service]
User=root
Group=www-data
WorkingDirectory=/root/flaskapp
Environment="PATH=/root/flaskapp/flaskappenv/bin"
ExecStart=/root/flaskapp/flaskappenv/bin/gunicorn --workers 3 --bind unix:/root/flaskapp/flaskapp.sock -m 007 wsgi:app

[Install]
WantedBy=multi-user.target
```
```
sudo chown root:www-data -R /root/flaskapp
```
```
ls -ld /root/flaskapp/
```
```
systemctl start flaskapp
systemctl enable flaskapp 
systemctl status flaskapp 
```
```
screen -S app
```
```
flask run
```
