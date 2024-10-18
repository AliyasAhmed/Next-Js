# **Creating API in Next.js**
As we Know we can use nextjs as `Backend` as well so we can easily Create api in it.

we are receiving data from the route.JavaScript to page.JavaScript, after which the data is displayed on the console and terminal
```js
app/page.js

"use client"
export default function Home() {
  const handleclick = async () => {
    let data = {
      name:"aliyas",
      role:"coder"
    }
    let a = await fetch("/api/add", { method: "POST", headers: { "Content-type": "application/json" },
      body:JSON.stringify(data),
    })
    
    let res = await a.json()
    console.log(res)
  }
  return (
    <div>
      <h1>Next.js api routes demo</h1>
      <button onClick={handleclick}>Click me</button>
    </div>
  );
}
```

```js
api/add/route.js

import { NextResponse } from "next/server"
export const POST = async (request) =>{
    let data = await request.json()
    console.log(data)
    return NextResponse.json({success:true, data:data})

}
```

Let's break down **every line** in detail so you can understand exactly what's going on:

### **Client-Side (React Component)**

```js
"use client" 
```
- This tells Next.js that this part of the code should run in the **browser**. Next.js has both server-side and client-side code, and this ensures it runs in the browser.

---

```js
export default function Home() {
```
- This is the definition of a **React component** called `Home`. It represents a web page or UI component in your app.

---

```js
const handleclick = async () => {
```
- This defines a function called `handleclick`. It's **asynchronous** (uses `async`) because it will handle a task that takes time (sending and receiving data from the server). This function runs when you click the button (explained later).

---

```js
let data = {
    name: "aliyas",
    role: "coder"
}
```
- Here, you're creating an object called `data` with two properties: `name` and `role`. This data is what will be sent to the server when you click the button.

---

```js
let a = await fetch("/api/add", {
    method: "POST",
    headers: { "Content-type": "application/json" },
    body: JSON.stringify(data)
})
```
- `fetch` is used to send an HTTP request to the server. Here:
  1. You are sending the request to the `/api/add` endpoint (this is defined on the server side).
  2. The method is `POST`, which means you're sending data.
  3. The `headers` tell the server that you're sending **JSON** data.
  4. `body: JSON.stringify(data)` sends the `data` object to the server, but it converts it to a JSON string first (since the server expects data in this format).
  5. `await` is used because you're waiting for the server to respond before moving to the next step.

---

```js
let res = await a.json()
```
- After the `fetch` request completes, you get the response (`a`) from the server. Here, you're using `await` again because you need to wait for the response to be converted into JSON format. The `res` variable will hold the JSON data sent back by the server.

---

```js
console.log(res)
```
- This logs the server's response (`res`) to the **browser console**. You can see it by opening your browser's developer tools (press F12 or right-click and select "Inspect").

---

```js
<button onClick={handleclick}>Click me</button>
```
- This creates a **button** on your webpage. When you click it, the `handleclick` function runs, which triggers the process of sending data to the server and receiving a response.

---

```js
return (
    <div>
        <h1>Next.js api routes demo</h1>
        <button onClick={handleclick}>Click me</button>
    </div>
);
```
- This is the **JSX** that defines the UI. It returns an `<h1>` heading and the button. The `onClick={handleclick}` ensures that the `handleclick` function runs when the button is clicked.

---

# **Server-Side (API Route)**

```js
import { NextResponse } from "next/server"
```
- This imports the `NextResponse` object from Next.js, which is used to create structured responses (JSON or other formats) from the server back to the browser.

---

```js
export const POST = async (request) => {
```
- This defines the **API route** that handles a POST request. When the browser sends a POST request to `/api/add`, this function runs. 

---

```js
let data = await request.json()
```
- This will read data from the client side which we already defined in object form `{name:"aliyas", role:"coder"}` 

---

```js
console.log(data)
```
- This logs the received data to the **server console** (wherever your Next.js app is running). You can see it in the terminal or environment where you're running the app.

---

```js
return NextResponse.json({ success: true, data: data })
```
- This sends a JSON response back to the client. Its main function is to send the json formatted object so it could be readable.
  1. `success: true` indicates that the operation was successful.
  2. `data: data` sends back the exact same data that the server received, so the client can confirm that the data was received correctly.

---

### **Summary of How It Works:**

1. **Client-side (Browser)**:
   - You click a button.
   - The client (your webpage) prepares some data (`name` and `role`) and sends it to the server using the `fetch` function.
   - After sending the data, the server responds, and the client logs the server's response.

2. **Server-side (API Route)**:
   - The server receives the data sent from the client.
   - It logs the data in the server console.
   - It sends back a response to the client, confirming that everything worked (`success: true`) and including the data that was sent.

### Here how it works

Exactly! Here's how it works step by step:

1. **page.js (Client-side)**:
   - The client (page.js) makes a request to **route.js** when you click the button.
   - It sends data using a **POST** request.

2. **route.js (Server-side)**:
   - The server (route.js) receives that request.
   - It processes the data (for example, logs it to the terminal).
   - It then **responds** back with some JSON data (in this case, `{ success: true, data: data }`).

3. **Back to page.js**:
   - The client (page.js) receives the response from **route.js**.
   - It logs the response (the JSON data) to the console, which you can see in the browser console.

So, yes, you're **getting data from route.js** to page.js, and it's displayed in both the **browser console** and **server terminal**.


