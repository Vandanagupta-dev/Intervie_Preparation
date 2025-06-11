यहाँ पर ASP.NET **Web Forms** से संबंधित कुछ **महत्वपूर्ण Interview Questions with Answers** दिए गए हैं, जो Fresher और Experience दोनों लेवल पर पूछे जा सकते हैं:

---

### ✅ **Basic Level Questions**

#### **1. What is ASP.NET Web Forms?**

**Answer:**
ASP.NET Web Forms is a part of the ASP.NET framework that allows developers to build dynamic websites using a drag-and-drop, event-driven model. It provides rich server-side controls and maintains state over HTTP through ViewState.

---

#### **2. What is ViewState in ASP.NET Web Forms?**

**Answer:**
ViewState is a method to preserve the state of server-side controls between postbacks. It stores data in a hidden field on the page and is automatically handled by ASP.NET.

```csharp
ViewState["Username"] = txtUsername.Text;
```

---

#### **3. What is the difference between Web Forms and MVC?**

**Answer:**

| Web Forms               | MVC                    |
| ----------------------- | ---------------------- |
| Event-driven model      | Request-response based |
| Uses ViewState          | Stateless              |
| Tightly coupled with UI | Separation of concerns |
| Less control over HTML  | Full control over HTML |

---

#### **4. What is a PostBack in Web Forms?**

**Answer:**
PostBack is the process by which a page sends data back to the same page on the server. It happens when any event (like button click) triggers a server-side method.

---

#### **5. What is the Page Life Cycle in Web Forms?**

**Answer:**
The major stages are:

1. **Page Request**
2. **Start**
3. **Initialization**
4. **Load**
5. **Postback Event Handling**
6. **Rendering**
7. **Unload**

---

#### **6. What is the difference between Server.Transfer and Response.Redirect?**

**Answer:**

| Server.Transfer                                | Response.Redirect                   |
| ---------------------------------------------- | ----------------------------------- |
| Transfers to another page without a round-trip | Performs a round-trip to the client |
| Faster                                         | Slower                              |
| URL in browser does not change                 | URL changes                         |
| Only works within the same application         | Can navigate to external sites      |

---

### ✅ **Intermediate Level Questions**

#### **7. What are the different types of state management in ASP.NET Web Forms?**

**Answer:**

* **Client-side:**

  * ViewState
  * Cookies
  * Hidden Fields
  * Query Strings
* **Server-side:**

  * Session State
  * Application State
  * Cache

---

#### **8. What is the use of Global.asax file?**

**Answer:**
The `Global.asax` file is used for application-level events such as:

* Application\_Start
* Session\_Start
* Application\_Error
* Session\_End

---

#### **9. What is a Master Page?**

**Answer:**
A Master Page defines a common layout (like header, footer, menu) that can be shared across multiple pages. Content pages use the layout of the master page.

```html
<%@ Master Language="C#" %>
```

---

#### **10. What is the difference between Repeater, DataList, and GridView?**

**Answer:**

| Control      | Features                                               |
| ------------ | ------------------------------------------------------ |
| **Repeater** | Lightweight, customizable, no built-in paging/sorting  |
| **DataList** | Allows multi-column layout                             |
| **GridView** | Richest control with built-in sorting, paging, editing |

---

### ✅ **Advanced Level Questions**

#### **11. How does ViewState work internally?**

**Answer:**
ViewState is serialized into a base64-encoded string and stored in a hidden field named `__VIEWSTATE` on the page. During postback, ASP.NET deserializes this string to restore control states.

---

#### **12. What are User Controls and Custom Controls?**

**Answer:**

* **User Control (.ascx):** Reusable page fragments; created easily but limited to one application.
* **Custom Control:** Compiled into DLL; can be used across multiple applications.

---

#### **13. What is the role of the `IsPostBack` property?**

**Answer:**
`IsPostBack` determines whether the page is loaded for the first time or as a result of a postback. Useful to prevent overwriting data on every request.

```csharp
if (!IsPostBack)
{
   // Load dropdown data only on initial load
}
```

---

#### **14. How can you improve performance in ASP.NET Web Forms?**

**Answer:**

* Disable unnecessary ViewState
* Use caching (Output, Data)
* Use Server.Transfer instead of Response.Redirect
* Avoid large controls when not needed
* Use efficient data binding

---

#### **15. Can we have multiple forms on a single ASP.NET Web Form page?**

**Answer:**
Yes, but only **one form** can have `runat="server"`. Other forms must use JavaScript or HTML form elements and cannot post to server directly.

---

### ✅ Bonus: Mini Coding Question

**Q: How do you bind a GridView with data from SQL Server?**
**Answer:**

```csharp
protected void Page_Load(object sender, EventArgs e)
{
    if (!IsPostBack)
    {
        SqlConnection con = new SqlConnection("YourConnectionString");
        SqlDataAdapter da = new SqlDataAdapter("SELECT * FROM Users", con);
        DataTable dt = new DataTable();
        da.Fill(dt);
        GridView1.DataSource = dt;
        GridView1.DataBind();
    }
}
```

---

अगर आप चाहें तो मैं PDF या प्रैक्टिस सेट भी तैयार कर सकता हूँ। क्या आप चाहेंगे कि इन सवालों का **हिंदी-English मिक्स** या **pure हिंदी** वर्जन भी तैयार किया जाए?

