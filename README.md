import requests,json,time,os,threading 
from flask import Flask, jsonify, request
app = Flask(__name__)
PORT = int(os.environ.get("PORT", 5000))
login_data = {
    "username": "Kyris", # đổi thành username acc mcserver vào đây
    "email": "", # hoặc dùng mail để mail thì thôi username 
    "password": {
        "value": "Kyris123", # password điền vào 
        "repeat": ""
    }
}
sub_url = "https://www.mcserverhost.com/api/servers/4653c8b6/subscription" # dán vào đây 
def run_automation():
    session = requests.Session()
    while True:
        response = session.post("https://www.mcserverhost.com/api/login", headers={'Content-Type': 'application/json'}, data=json.dumps(login_data))
        if response.status_code == 200:
            session.post(sub_url, headers={'Content-Type': 'application/json'}, data=json.dumps({}))
        elif response.status_code == 406:
            print("Sai tài khoản hoặc mật khẩu rồi!")
        elif response.status_code == 403:
            print("IP của bạn có thể đã bị chặn bởi website")
        else:
            print(f"Lỗi {response.status_code} - {response.text}")
        time.sleep(3000)
@app.route('/')
def home():
    return "Renew đang chạy."
if __name__ == '__main__':
    au= threading.Thread(target=run_automation)
    au.daemon = True
    au.start()
    app.run(host='0.0.0.0', port=PORT)
