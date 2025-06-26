# Manual WebDriver Control with and without Postman – A Guide for SDETs

## 🎯 Objective

By the end of this guide, you will be able to:

* Understand how Selenium communicates with browsers under the hood
* Use both Postman and curl to manually control browser actions
* Start ChromeDriver sessions and send WebDriver commands like navigate, click, type, and screenshot
* Perform browser automation without writing Selenium code
* Mimic Selenium actions step-by-step to strengthen your debugging and architecture knowledge

---

## 🔥 Overview

This project demonstrates how to control Google Chrome via the WebDriver JSON Wire Protocol using **Postman**, without writing a single line of Selenium code.

You’ll learn how to:

* Launch Chrome via ChromeDriver
* Send WebDriver commands via Postman
* Navigate to URLs, click elements, input text, and more
* Perform Google Search using manual commands

---

## 📦 What's Included

**Postman Collections:**

* `WebDriver_Raw_Control_with_Variables.postman_collection.json`
* `Simple_Google_Search.postman_collection.json`

**Environment File:**

* `WebDriver_Environment.postman_environment.json`

### ✅ Operations Covered

| Step | Description                 |
| ---- | --------------------------- |
| 1.   | Start ChromeDriver Session  |
| 2.   | Navigate to URL             |
| 3.   | Find Element by Selector    |
| 4.   | Click Element               |
| 5.   | Send Keys to Input/Textarea |
| 6.   | Perform Google Search       |
| 7.   | Take Screenshot (Base64)    |
| 8.   | Quit Browser Session        |

---

## 🛠️ How to Use (Step-by-Step)

### 🧰 Without Postman: Using curl

🔄 **Note:** By default, ChromeDriver starts on `http://localhost:9515`. If your system starts it on a different port (e.g., 9516 or 4444), make sure to update the port in all curl commands accordingly.
You can also mimic all WebDriver commands using `curl` (command-line HTTP tool).

#### 🔸 Start Session

```bash
curl -X POST http://localhost:9515/session \
  -H "Content-Type: application/json" \
  -d '{
    "capabilities": { "firstMatch": [{}] }
  }'
```

#### 🔸 Navigate to URL

```bash
curl -X POST http://localhost:9515/session/<sessionId>/url \
  -H "Content-Type: application/json" \
  -d '{ "url": "https://the-internet.herokuapp.com/login" }'
```

#### 🔸 Find Element (CSS)

```bash
curl -X POST http://localhost:9515/session/<sessionId>/element \
  -H "Content-Type: application/json" \
  -d '{ "using": "css selector", "value": "button.radius" }'
```

#### 🔸 Click Element

```bash
curl -X POST http://localhost:9515/session/<sessionId>/element/<elementId>/click \
  -H "Content-Type: application/json" \
  -d '{}'
```

#### 🔸 Send Keys

```bash
curl -X POST http://localhost:9515/session/<sessionId>/element/<elementId>/value \
  -H "Content-Type: application/json" \
  -d '{ "text": "your_username" }'
```

#### 🔸 Press Enter

```bash
curl -X POST http://localhost:9515/session/<sessionId>/element/<elementId>/value \
  -H "Content-Type: application/json" \
  -d '{ "text": "" }'
```

#### 🔸 Take Screenshot

```bash
curl http://localhost:9515/session/<sessionId>/screenshot
```

#### 🔸 Quit Session

```bash
curl -X DELETE http://localhost:9515/session/<sessionId>
```

Replace `<sessionId>` and `<elementId>` with values received from earlier curl responses.

### 1. 🚀 Start ChromeDriver Manually

Download [ChromeDriver](https://chromedriver.chromium.org/downloads) matching your Chrome version.

Open Command Prompt:

```bash
chromedriver.exe
```

ChromeDriver will start on a port like `http://localhost:9515` (default), but it might vary depending on system config. If the port is different, update it in both curl and Postman requests accordingly.

---

### 2. 🧪 Use Postman to Send WebDriver Commands

#### ⏱️ Start Session

* Method: POST
* URL: `{{host}}/session`
* Body:

```json
{
  "capabilities": {
    "firstMatch": [{}]
  }
}
```

👉 Copy the `sessionId` from the response and paste into Postman environment.

#### 🌐 Navigate to URL

* Method: POST
* URL: `{{host}}/session/{{sessionId}}/url`
* Body:

```json
{
  "url": "https://the-internet.herokuapp.com/login"
}
```

#### 🔍 Find Button Element

* Method: POST
* URL: `{{host}}/session/{{sessionId}}/element`
* Body:

```json
{
  "using": "css selector",
  "value": "button.radius"
}
```

👉 Copy `elementId` from response.

#### 👆 Click Element

* Method: POST
* URL: `{{host}}/session/{{sessionId}}/element/{{elementId}}/click`
* Body:

```json
{}
```

#### ⌨️ Send Keys to Input

* Method: POST
* URL: `{{host}}/session/{{sessionId}}/element/{{elementId}}/value`
* Body:

```json
{
  "text": "your_username"
}
```

#### 📝 Send Keys to Textarea (using unique elementId)

* Method: POST
* URL: `{{host}}/session/{{sessionId}}/element/{{elementId}}/value`
* Body:

```json
{
  "text": "This is a message typed into textarea"
}
```

#### 📸 Screenshot

* Method: GET
* URL: `{{host}}/session/{{sessionId}}/screenshot`

#### ❌ Quit Session

* Method: DELETE
* URL: `{{host}}/session/{{sessionId}}`

---

## 🌐 Bonus: Perform Google Search (Demo Ready)

### 🤖 What Does "Mimic" Mean?

When we say "mimic" in this project, we mean **simulating what Selenium does internally** — without writing any Selenium code.

For example, instead of:

```csharp
driver.FindElement(By.Name("q")).SendKeys("Selenium WebDriver");
```

You use this WebDriver command directly:

```json
POST /session/{{sessionId}}/element/{{elementId}}/value
{
  "text": "Selenium WebDriver"
}
```

You're mimicking the same action manually, step-by-step. This helps you understand and control what happens behind the scenes.

Use `Simple_Google_Search.postman_collection.json` for demo.

### ✅ Steps

1. Start Session
2. Navigate to `https://www.google.com`
3. Find Input Field (by name="q")
4. Send Keys: `Selenium WebDriver`
5. Press Enter (`\uE007`)

### 🔍 Example for Enter Key:

```json
{
  "text": "\uE007"
}
```

---

## 🎯 Why SDETs Should Learn This

### 🧠 Master Selenium Internals

Know how your framework actually communicates with browsers.

### 🔍 Debug Like a Pro

Manually replay commands to investigate failures.

### 🧰 Build Custom Tools

Integrate WebDriver commands into CLI tools, services, or low-code workflows.

### 🔄 Learn Once, Use Everywhere

Raw WebDriver knowledge applies to Selenium, Playwright, Puppeteer, and DevTools.

---

## 🤝 Contribute

PRs are welcome if you'd like to add more WebDriver operations!

## 📢 Author

Johil Angelo &#x20;
SDET | Automation Enthusiast

Let’s connect on [LinkedIn](https://www.linkedin.com/in/johil-angelo-aa66b91b5/)!
