
# 📘 **Sensus API – Updated Documentation (2025)**

FastAPI + SQLite backend for the **Sensus** project.  
Includes 4 functional data splits:

- **data_peek** – personal info  
- **note_peek** – notes with instant updates  
- **screen_peek** – screenshot, URL, contact  
- **commands** – remote command storage  

Supports:

- Multi-user (each user_id is its own profile)  
- Merge-safe updates  
- File-based screenshot uploads  
- Apple Shortcuts integration  
- Push subscription storage  
- Full clear/reset endpoints  
- PWA-ready frontend integration  

---

# 🗂 **Project Structure**

```
/main.py
/push.py
/render.yaml
/requirements.txt
/uploads/
```

Database:  
```
sensus.db (SQLite)
```

Uploads directory stores screenshots.

---

# 🔐 **User Model Summary**

Each user has:

### **data_peek**
- first_name  
- last_name  
- job_title  
- phone_number  
- birthday  
- address  

### **note_peek**
- note_name  
- note_body  

### **screen_peek**
- contact  
- url  
- screenshot_path  

### **commands**
- command  

### **Other**
- created_at  
- updated_at  
- *_updated_at timestamps  
- push subscription entries  

---

# 🚀 **Endpoints Overview**

## 🟦 Root Check
```
GET /
```

## 🟩 User Snapshot
```
GET /user/{user_id}
```

## 🟥 Delete User
```
DELETE /user/{user_id}
```

---

# 📬 **Push Subscription**
```
POST /push/subscribe/{user_id}
```
Body:
```json
{
  "subscription": { ... }
}
```

---

# 🟨 **SPLIT: data_peek**

### GET
```
GET /data_peek/{user_id}
```

### UPDATE
```
POST /data_peek/{user_id}
```

Example:
```json
{
  "first_name": "Jordan",
  "job_title": "Magician"
}
```

### CLEAR
```
POST /data_peek/{user_id}/clear
```

---

# 🟪 **SPLIT: note_peek**

### GET
```
GET /note_peek/{user_id}
```

### UPDATE
```
POST /note_peek/{user_id}
```

Example:
```json
{
  "note_name": "Shopping List",
  "note_body": "Eggs, Milk, Apples"
}
```

### CLEAR
```
POST /note_peek/{user_id}/clear
```

---

# 🟩 **SPLIT: screen_peek**

Screen Peek accepts **file uploads only** (multipart/form-data).

### GET
```
GET /screen_peek/{user_id}
```

### GET Screenshot File
```
GET /screen_peek/{user_id}/screenshot
```

### UPDATE (Unified File Endpoint)
```
POST /screen_peek/{user_id}
```

Fields:
- screenshot (File, optional)
- contact (Text, optional)
- url (Text, optional)

### CLEAR
```
POST /screen_peek/{user_id}/clear
```

Deletes screenshot file + resets data.

---

# 🟧 **SPLIT: commands**

### GET
```
GET /commands/{user_id}
```

### UPDATE
```
POST /commands/{user_id}
```

Example:
```json
{ "command": "refresh" }
```

### CLEAR
```
POST /commands/{user_id}/clear
```

---

# 🧨 **CLEAR ALL**
Clears all splits + deletes screenshot file.

```
POST /clear_all/{user_id}
```

---

# 📱 **Using Sensus API With Apple Shortcuts**

Below are exact examples for sending/receiving data.

---

# 🟦 **1. Update Data Peek (JSON)**

Use:
```
POST https://YOUR-API-DOMAIN/data_peek/Jordan
```

Body:
```json
{
  "first_name": "Jordan",
  "last_name": "Meller"
}
```

---

# 🟪 **2. Update Note Peek (JSON)**

```
POST https://YOUR-API-DOMAIN/note_peek/Jordan
```

Body:
```json
{
  "note_name": "Reminder",
  "note_body": "Feed the cat"
}
```

---

# 🟩 **3. Send Screenshot (FILE Upload)**

Use:
```
POST https://YOUR-API-DOMAIN/screen_peek/Jordan
```

**Request Body → Form**

Fields:
```
screenshot → File (Latest Screenshot)
contact → Text
url → Text
```

---

# 🟧 **4. Send Command (JSON)**

```
POST https://YOUR-API-DOMAIN/commands/Jordan
```

Body:
```json
{ "command": "refresh" }
```

---

# 🟫 **5. Clear All**
```
POST https://YOUR-API-DOMAIN/clear_all/Jordan
```

No body required.

---

# 📥 **Receiving Data in Shortcuts**

### Get screen_peek
```
GET https://YOUR-API-DOMAIN/screen_peek/Jordan
```

### Get screenshot file
```
GET https://YOUR-API-DOMAIN/screen_peek/Jordan/screenshot
```

---

# 🔐 **Multi-User**
Each user_id is isolated:

```
/data_peek/Jordan
/data_peek/Sarah
/data_peek/TestUser
```

---

# 🛠 **Local Development**

### Install packages
```
pip install -r requirements.txt
```

### Run server
```
uvicorn main:app --reload
```

### Open docs
```
http://127.0.0.1:8000/docs
```

---

# 🚀 **Deployment (Render)**

Render uses:
```
buildCommand: pip install -r requirements.txt
startCommand: uvicorn main:app --host 0.0.0.0 --port $PORT
```

Ensure repo root is selected.

---

# ⭐ Final Notes

This API is now:

- Clean  
- Unified  
- Easy to extend  
- Shortcut-friendly  
- React-ready  
- Multi-user  
- Professional  
- Screenshot-safe  

You're all set!
