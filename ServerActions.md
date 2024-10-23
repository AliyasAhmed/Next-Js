# **Server Actions**

### **Notes on Server Actions in Next.js**

#### **What is a Server Action?**
Server Actions in Next.js allow us to handle form submissions directly on the server side without needing to create separate API routes. Everything runs on the server, which makes data fetching and processing more secure and efficient.

#### **Example with Server Action:**

```js

import fs from "fs/promises"
import { useRef } from "react"


export default function home () {
  const submitAction = async(e) =>{
    "use server" // this means when we run this function everything will run on the server side.
    
    console.log(e.get('name'),e.get('Address') )  //with this serveraction we dont have to usally make api to get the data we can do this
    let a = await fs.writeFile('aliyas.txt', `This is how its suppose to save or get data from the server side. My name is ${e.get('name')} and address is ${e.get("Address")} `) // Any thing we did in monodb we can do it here
    console.log(a)
  }
  let ref = useRef()
  return(
    <div className="flex flex-col  items-center justify-center h-screen">
      <form className="" action={(e)=>{submitAction(e); ref.current.reset() }}> {/*  this is when we want to clear the form after submitting*/}
      {/* <form className="" action={submitAction}> */} {/* This method will run everything normally unless we need to clear the from on every submit we cant use this that way */}
      <div className="flex flex-col border border-gray-500 p-7  rounded-lg">
      
        <label className="p-5" htmlFor="name">Name</label>
        <input className="p-2 px-10 rounded-md text-black" name="name" type="name" id ="name" />
        <label className="p-5" htmlFor="Address">Address</label>
        <input className="p-2 px-10 rounded-md text-black" name="Address" type="Address" id ="Address" />
        <button className="border border-gray my-6 p-3 rounded-md hover:bg-slate-700">Submit</button>
        </div>
        </form>
    </div>
  )
}
```

#### **Explanation:**
1. **`submitAction` function**:
   - Runs server-side because of the `"use server"` directive.
   - Grabs form data using `e.get("name")` and `e.get("Address")`.
   - Saves the data to a file using `fs.writeFile` (just like writing to a database).

2. **`useRef()`**:
   - Used to reset the form after the submission, making the form fields empty.

3. **Form submission:**
   - When the form is submitted, `submitAction` is called to handle the data and save it to the server.

---

#### **How This Differs from Traditional Methods:**

**Without Server Actions (Old Approach):**
1. We would create an API route in `api` folder (`pages/api/add.js`).
2. In the API route, we handle the form data and process it on the server.
3. From the client-side component (`page.js`), we'd make a `POST` request using `fetch` to send form data to that API route.

##### **Old Approach Example**:

1. **API Route (`pages/api/add.js`):**
   ```js
   export default async function handler(req, res) {
     if (req.method === "POST") {
       const { name, address } = req.body;
       // Handle data (e.g., save to database)
       res.status(200).json({ success: true, name, address });
     }
   }
   ```

2. **Client-Side Form Submission:**
   ```js
   const handleSubmit = async () => {
     let data = {
       name: "Aliyas",
       address: "XYZ Street",
     };

     let response = await fetch("/api/add", {
       method: "POST",
       headers: { "Content-Type": "application/json" },
       body: JSON.stringify(data),
     });

     let resData = await response.json();
     console.log(resData);
   };
   ```

##### **Key Differences:**
- **With Server Actions**: No need to create separate API routes; form submission logic directly in the server-side function.
- **Old Approach**: Requires a separate API route, and you'd need to handle both the client-side `fetch` and the server-side response.

#### **Benefits of Server Actions:**
- Simplifies code.
- No need for client-side `fetch` and API routes.
- Everything stays on the server, improving security for sensitive data like passwords.

# To Submit form and reset the form.

- First we need make a seperate file in which we write this code
```js
app/Actions/server.js

"use server" // this means when we run this function everything will run on the server side.
import fs from "fs/promises"
export const submitAction = async (e) => {

    console.log(e.get('name'), e.get('Address'))  //with this serveraction we dont have to usally make api to get the data we can do this
    let a = await fs.writeFile('aliyas.txt', `This is how its suppose to save or get data from the server side. My name is ${e.get('name')} and address is ${e.get("Address")} `) // Any thing we did in monodb we can do it here
    console.log(a)
}
```


- Then we would write this in another file With `Client server` as `use client` 
```js
"use client"
import { submitAction } from "./Actions/server"
import { useRef } from "react"


export default function home () {

  let ref = useRef()
  return(
    <div className="flex flex-col  items-center justify-center h-screen">
      <form ref={ref} className="" action={(e)=>{submitAction(e); ref.current.reset() }}> {/*  this is when we want to clear the form after submitting*/}
      {/* <form className="" action={submitAction}> */} {/* This method will run everything normally unless we need to clear the from on every submit we cant use this that way */}
      <div className="flex flex-col border border-gray-500 p-7  rounded-lg">
      
        <label className="p-5" htmlFor="name">Name</label>
        <input className="p-2 px-10 rounded-md text-black" name="name" type="name" id ="name" />
        <label className="p-5" htmlFor="Address">Address</label>
        <input className="p-2 px-10 rounded-md text-black" name="Address" type="Address" id ="Address" />
        <button className="border border-gray my-6 p-3 rounded-md hover:bg-slate-700">Submit</button>
        </div>
        </form>
    </div>
  )
}
```

This method is much easier then making an api to fetsh data or post data.
