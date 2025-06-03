# 📢 LINE Notify Bot - Monthly Spotify Reminder

This project is a simple LINE Messaging API bot using Google Apps Script (GAS) to send a **monthly group reminder** with a QR payment image and a mention to all group members.

> 📅 Sends a message on the **31st of every month**, or **30th if 31st doesn't exist (like in February or April)**.

## 💡 Features

- Sends custom text message 
- Includes an image (e.g., QR code)
- Automatically runs every month using Google Apps Script Trigger

## 📦 Requirements

- LINE Messaging API (with your **channel access token** and **group ID**)
- [Google Apps Script](https://script.google.com/)
- Internet access

## 🛠️ Setup Instructions

### 1. Create a LINE Bot

- Go to [LINE Developers Console](https://developers.line.biz/)
- Create a new provider and messaging API channel
- Enable `Messaging API`
- Get your **Channel Access Token**
- Add your bot to a LINE group and **send at least one message** to retrieve the `groupId`

### 2. Google Apps Script Setup

1. Go to [Google Apps Script](https://script.google.com/)
2. Create a new project
3. Paste this code in `Code.gs`:

```javascript
function sendLineNotification() {
  const accessToken = 'YOUR_ACCESS_TOKEN'; // Replace this
  const groupId = 'YOUR_GROUP_ID'; // Replace this

  const messages = [
    {
      type: 'text',
      text: '@All จ่ายเงินค่า Spotify ด้วยนะะ 36.5 บาท truemoney-xxxxxxxx',
      mention: {
        mentionees: [{
          index: 0,
          length: 4,
          type: 'all'
        }]
      }
    },
    {
      type: 'image',
      originalContentUrl: 'https://YOUR-IMAGE-ADDRESSES.png',
      previewImageUrl: 'https://YOUR-IMAGE-ADDRESSES.png'
    }
  ];

  const payload = {
    to: groupId,
    messages: messages
  };

  const options = {
    method: 'post',
    contentType: 'application/json',
    headers: {
      Authorization: 'Bearer ' + accessToken
    },
    payload: JSON.stringify(payload)
  };

  UrlFetchApp.fetch('https://api.line.me/v2/bot/message/push', options);
}
```

### 3. Add Scheduler (Trigger)

1. Click the **clock icon** ⏰ in Apps Script to open "Triggers"
2. Click **+ Add Trigger**
3. Choose the following:

| Option              | Value                    |
|---------------------|--------------------------|
| Function to run     | `sendLineNotification`        |
| Deployment          | Head                     |
| Event source        | Time-driven              |
| Type of time-based trigger | Month timer         |
| Day of month        | 1st                     |
| Time of day         | Your choice (e.g., 9AM)  |


## 🖼️ Demo Output

> 📷 QR Payment image and message:
```
@All จ่ายเงินค่า Spotify ด้วยนะะ 36.5 บาท truemoney-xxxxxxxx
```

## 🖼️ Preview
![467830](https://github.com/user-attachments/assets/84bcc29e-c700-4968-a775-2b3f7b4193a9)

## 🔐 Security Notes

- Do not expose your access token in public repositories. Use [Script Properties](https://developers.google.com/apps-script/guides/properties) for secrets.
- This project is for educational and personal use only.

## 📄 License

[MIT License](LICENSE)

