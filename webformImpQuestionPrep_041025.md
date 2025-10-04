
## 🔹 Basic Level Questions

1. **ASP.NET Web Forms kya hai?**

   * Web Forms ek event-driven framework hai jo server controls, ViewState, aur postback model ka use karke rapid UI development allow karta hai.

2. **ASP.NET Web Forms aur MVC me kya difference hai?**

3. **Page Life Cycle ASP.NET Web Forms kaise work karta hai?**

   * Events: `Page_Init`, `Page_Load`, `Page_PreRender`, `Page_Unload`, etc.

4. **ViewState kya hai? Iska use aur limitation batao.**

5. **ASP.NET Web Forms me Server Controls aur HTML Controls me kya difference hai?**

6. **Postback kya hota hai?**

7. **State Management ke techniques batao (Client-side aur Server-side).**

8. **Web.config file ka use kya hai?**

9. **Master Page aur Content Page kya hota hai?**

10. **IsPostBack property ka use kya hai?**

---

## 🔹 Intermediate Level Questions

11. **Session vs Application State difference.**

12. **Server.Transfer aur Response.Redirect me kya fark hai?**

13. **ASP.NET Web Forms me Caching techniques kaise implement karte ho?**

14. **Global.asax file ka use kya hai?**

15. **Page.IsValid property ka use kab hota hai?**

16. **Cross-page posting kya hoti hai?**

17. **ASP.NET Web Forms me Validation Controls ke types kaun se hain?**

18. **User Control (.ascx) aur Custom Server Control me kya difference hai?**

19. **GridView aur Repeater control me kya difference hai?**

20. **UpdatePanel kya hai aur AJAX kaise implement hota hai Web Forms me?**

---

## 🔹 Advanced Level Questions

21. **ASP.NET Web Forms me Events kaise kaam karte hain (Event Bubbling)?**

22. **ASP.NET Web Forms me Security kaise implement karte ho (Authentication & Authorization)?**

23. **Session ke modes kaun se hain (InProc, StateServer, SQLServer, Custom)?**

24. **ViewState disable karne ka impact kya hoga?**

25. **Application Pool kya hai aur IIS me iska role kya hai?**

26. **ASP.NET Web Forms me Custom Error Handling kaise karte ho?**

27. **HttpHandlers aur HttpModules kya hote hain?**

28. **WebForms Performance optimization ke liye kya steps loge?**

29. **WebForms me strongly typed data binding kaise hoti hai?**

30. **ASP.NET Page Life Cycle me PreInit aur Init ke beech kya difference hai?**

---

⚡ **Tip for Interview:**
Interviewer mostly puchta hai:

* **Page Life Cycle** (sabse common).
* **State Management** (ViewState, Session, Cache).
* **Controls difference** (GridView vs Repeater, Server vs HTML).
* **Postback & IsPostBack**.
* **Web.config & Global.asax usage**.

---
-----------------------------------------------------------------------
Basic level
Perfect 👍
Main aapke diye **Basic Level ASP.NET Web Forms interview questions ke proper answers** likh kar deta hoon, short and crisp (interview friendly).

---

## 🔹 Basic Level Questions with Answers

### 1. **ASP.NET Web Forms kya hai?**

ASP.NET Web Forms ek **event-driven framework** hai jo server controls, ViewState aur postback model ka use karke UI banata hai.

* Ye drag-and-drop aur RAD (Rapid Application Development) ko support karta hai.
* Developer ko HTML aur JavaScript ke low-level details se free karta hai.

---

### 2. **ASP.NET Web Forms aur MVC me kya difference hai?**

| Feature          | Web Forms                          | MVC                           |
| ---------------- | ---------------------------------- | ----------------------------- |
| Architecture     | Event-driven, Page & Control based | Model-View-Controller pattern |
| State Management | ViewState, Session                 | Stateless (no ViewState)      |
| Testing          | Difficult (tightly coupled)        | Easy (Separation of concerns) |
| Control          | Server Controls (drag & drop)      | HTML Helpers, Razor syntax    |
| Performance      | Heavy (due to ViewState)           | Lightweight, faster           |

---

### 3. **Page Life Cycle ASP.NET Web Forms kaise work karta hai?**

Important Events order:

1. **Page_Init** → Page initialization (controls created).
2. **Page_Load** → Page data load hota hai (ViewState restore hota hai).
3. **Control Events** → Button click, dropdown change, etc.
4. **Page_PreRender** → Final changes before rendering.
5. **Page_Unload** → Page cleanup.

---

### 4. **ViewState kya hai? Iska use aur limitation batao.**

* **Definition:** ViewState ek hidden field (`__VIEWSTATE`) hai jo page aur control values ko **postback ke beech preserve** karta hai.
* **Use:** Automatically TextBox, DropDownList, CheckBox values wapas laata hai page reload ke baad.
* **Limitations:**

  * Page size badh jata hai.
  * Sirf current page ke liye valid hai.
  * Sensitive data ke liye secure nahi.

---

### 5. **ASP.NET Web Forms me Server Controls aur HTML Controls me kya difference hai?**

| Server Controls (`asp:`)                      | HTML Controls (`<input>`, `<select>`)       |
| --------------------------------------------- | ------------------------------------------- |
| Run on server (`runat="server"`)              | Run on client side only                     |
| Can handle server-side events (Click, Change) | Only client-side events (JavaScript)        |
| Integrated with ViewState                     | No ViewState support                        |
| Examples: GridView, DropDownList              | Examples: `<input type="text">`, `<select>` |

---

### 6. **Postback kya hota hai?**

* Jab ek ASP.NET Web Form me button click ya event trigger hota hai aur **page server ko request bhej kar reload hota hai**, ise **postback** kehte hain.
* Example: Button click → Page reload → Same page server par execute hota hai.

---

### 7. **State Management ke techniques batao (Client-side aur Server-side).**

**Client-Side:**

* ViewState
* Hidden Fields
* Cookies
* QueryString

**Server-Side:**

* Session
* Application
* Cache

---

### 8. **Web.config file ka use kya hai?**

* Application ke liye configuration settings rakhta hai.
* Example uses:

  * Database connection strings
  * Authentication & Authorization rules
  * Session management settings
  * Custom error handling

---

### 9. **Master Page aur Content Page kya hota hai?**

* **Master Page (.master):** Layout define karta hai (Header, Footer, Menu).
* **Content Page (.aspx):** Sirf specific content rakhta hai jo master ke layout me fit hota hai.
  👉 Isse **consistent look & feel** milta hai across website.

---

### 10. **IsPostBack property ka use kya hai?**

* `Page.IsPostBack` ek **Boolean property** hai jo batati hai ki page **first time load** ho raha hai ya **postback (reload after event)** ho raha hai.

**Example:**

```csharp
if (!IsPostBack)
{
    // Code runs only first time (not on button click reload)
    BindDropDownList();
}
```

---
Bahut accha 👍
Ab main aapke **Intermediate Level ASP.NET Web Forms interview questions** ke **short and crisp answers** likh raha hoon jo interview ke liye perfect rahenge.

---

## 🔹 Intermediate Level Questions with Answers

### 1. **Session vs Application State difference**

| Feature     | Session State (per user)       | Application State (global)     |
| ----------- | ------------------------------ | ------------------------------ |
| Scope       | Sirf ek user ke liye           | Sabhi users ke liye common     |
| Storage     | Server memory                  | Server memory                  |
| Lifetime    | User ke session tak            | Application ke start → end tak |
| Example Use | User login info, shopping cart | Global counters, config values |

---

### 2. **Server.Transfer aur Response.Redirect me kya fark hai?**

* **Server.Transfer**

  * Server-side redirect hota hai (control server ke andar hi ek page se dusre page pe transfer hota hai).
  * Faster (no round trip to browser).
  * URL change **nahi hota**.

* **Response.Redirect**

  * Client-side redirect hota hai (browser ko naya request bhejna padta hai).
  * Slower (extra round trip).
  * URL change **ho jata hai**.

---

### 3. **ASP.NET Web Forms me Caching techniques kaise implement karte ho?**

ASP.NET me 3 tarike ki caching hoti hai:

1. **Output Caching** → Pure page ya control cache karna.

   ```csharp
   <%@ OutputCache Duration="60" VaryByParam="None" %>
   ```
2. **Data Caching** → Objects ko cache me store karna.

   ```csharp
   Cache["Products"] = productsList;
   ```
3. **Fragment Caching** → UserControl ya page ke ek part ko cache karna.

---

### 4. **Global.asax file ka use kya hai?**

* Application-level events handle karne ke liye use hoti hai.
* Example:

  * `Application_Start` → App start hone par.
  * `Session_Start` → User session start par.
  * `Application_Error` → Global error handling.
* Isme app-level initialization code likha jata hai.

---

### 5. **Page.IsValid property ka use kab hota hai?**

* Jab **Validation Controls** use karte hain to unke validation result check karne ke liye.
* Agar sabhi validations pass ho gaye to `Page.IsValid == true`.

**Example:**

```csharp
if (Page.IsValid)
{
   // Process form
}
```

---

### 6. **Cross-page posting kya hoti hai?**

* Default me form postback same page par hota hai.
* **Cross-page posting** me ek page ki form dusre page par submit hoti hai.
* Implement by setting:

```aspx
<asp:Button ID="btnSubmit" runat="server" PostBackUrl="~/TargetPage.aspx" Text="Submit" />
```

* Target page me `PreviousPage` property se values access kar sakte ho.

---

### 7. **ASP.NET Web Forms me Validation Controls ke types kaun se hain?**

1. **RequiredFieldValidator** → Empty na ho.
2. **CompareValidator** → Value compare karna.
3. **RangeValidator** → Range ke andar value.
4. **RegularExpressionValidator** → Pattern match (email, phone, etc.).
5. **CustomValidator** → Apna custom logic.
6. **ValidationSummary** → Sab errors ek sath dikhata hai.

---

### 8. **User Control (.ascx) aur Custom Server Control me kya difference hai?**

| User Control (.ascx)             | Custom Server Control               |
| -------------------------------- | ----------------------------------- |
| Page-specific, drag & drop       | Reusable across projects            |
| Faster development               | More complex but powerful           |
| Compiled with page               | Compiled into separate DLL          |
| Example: Login form, Header.ascx | Custom Grid, special button control |

---

### 9. **GridView aur Repeater control me kya difference hai?**

| GridView                                 | Repeater                                       |
| ---------------------------------------- | ---------------------------------------------- |
| Rich features (sorting, paging, editing) | Lightweight, only data binding                 |
| Generates table-based structure          | Flexible (template-based, no table by default) |
| Slower (extra overhead)                  | Faster (no viewstate overhead)                 |
| Use when need auto features              | Use when need custom layout                    |

---

### 10. **UpdatePanel kya hai aur AJAX kaise implement hota hai Web Forms me?**

* **UpdatePanel** ek ASP.NET AJAX control hai jo page ke ek part ko partial postback ke through update karta hai bina full page reload ke.
* AJAX enable karne ke liye:

  1. Add `ScriptManager` on page.
  2. Wrap content inside `UpdatePanel`.

**Example:**

```aspx
<asp:ScriptManager runat="server" />
<asp:UpdatePanel runat="server">
   <ContentTemplate>
      <asp:Label ID="lblTime" runat="server" />
      <asp:Button ID="btnRefresh" runat="server" Text="Refresh" />
   </ContentTemplate>
</asp:UpdatePanel>
```

---

⚡ Ye saare answers **direct interview ke liye ready-made** hain — short, point-to-point, aur practical examples ke sath.

👉 Kya aap chahte ho mai **Advanced Level ASP.NET Web Forms interview questions ke bhi full answers** bana kar du?

