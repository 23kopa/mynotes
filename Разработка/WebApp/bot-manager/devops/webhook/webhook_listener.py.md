Путь: `/opt/webhook/github/webhook_listener.py`
```python
from flask import Flask, request
import subprocess
from datetime import datetime

app = Flask(__name__)

@app.route('/webhook', methods=['POST'])
def webhook():
    try:
        with open('/var/log/webhook_debug.log', 'a') as f:
            f.write(f'[{datetime.now()}] Webhook triggered\n')
        subprocess.Popen(['/bin/bash', '/var/www/botmanager/deploy.sh'])
        return 'OK', 200
    except Exception as e:
        with open('/var/log/webhook_debug.log', 'a') as f:
            f.write(f'[{datetime.now()}] Error: {e}\n')
        return 'Internal Server Error', 500

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5001)
```