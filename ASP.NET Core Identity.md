### ASP.NET Core Identity Setup (हिंदी-इंग्लिश मिश्रित)

#### 1. **क्या है ASP.NET Core Identity? (What is it?)**
- एक **authentication और authorization system** जो ASP.NET Core apps में यूजर मैनेजमेंट के लिए बनाया गया है।
- फीचर्स: यूजर रजिस्ट्रेशन, लॉगिन, पासवर्ड रीसेट, रोल्स, क्लेम्स, और थर्ड-पार्टी ऑथेंटिकेशन (Google/Facebook)।

---

#### 2. **बेसिक सेटअप स्टेप्स (Step-by-Step Setup)**
**Step 1: प्रोजेक्ट क्रिएशन (Project Creation)**
```bash
dotnet new webapp -n MyApp -au Individual
```
- `-au Individual`: Identity के साथ प्रोजेक्ट बनाता है।

**Step 2: डेटाबेस कॉन्फ़िगरेशन (Database Setup)**
- `appsettings.json` में कनेक्शन स्ट्रिंग ऐड करें:
```json
"ConnectionStrings": {
  "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=MyAppDB;Trusted_Connection=True;"
}
```

**Step 3: माइग्रेशन अप्लाई करें (Apply Migrations)**
```bash
dotnet ef database update
```
- यह कमांड `AspNetUsers`, `AspNetRoles` जैसे टेबल्स डेटाबेस में बनाती है।

---

#### 3. **आइडेंटिटी कॉन्फ़िगरेशन (Configuration in Program.cs)**
```csharp
builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(connectionString));

builder.Services.AddDefaultIdentity<IdentityUser>(options => {
    options.Password.RequireDigit = true;
    options.Password.RequiredLength = 8;
    options.SignIn.RequireConfirmedAccount = true;
})
.AddEntityFrameworkStores<ApplicationDbContext>();
```

---

#### 4. **यूजर रजिस्ट्रेशन/लॉगिन (User Registration/Login)**
- Views ऑटो-जेनरेट होती हैं `Areas/Identity/Pages/Account` में।
- कस्टमाइज़ करने के लिए:
```bash
dotnet aspnet-codegenerator identity -dc ApplicationDbContext --files "Account.Register"
```

---

#### 5. **रोल्स मैनेजमेंट (Roles Management)**
**रोल्स ऐड करें (Add Roles):**
```csharp
builder.Services.AddIdentity<IdentityUser, IdentityRole>()
    .AddEntityFrameworkStores<ApplicationDbContext>()
    .AddDefaultTokenProviders();
```

**रोल असाइन करें (Assign Role):**
```csharp
var user = await _userManager.FindByEmailAsync("user@example.com");
await _userManager.AddToRoleAsync(user, "Admin");
```

---

#### 6. **ऑथेंटिकेशन एट्रिब्यूट्स (Authentication Attributes)**
- **कंट्रोलर/एक्शन पर लागू करें:**
```csharp
[Authorize] // सभी एक्शन्स के लिए
[Authorize(Roles = "Admin")] // सिर्फ एडमिन के लिए
```

---

#### 7. **क्लेम्स-बेस्ड ऑथराइज़ेशन (Claims-Based Auth)**
**क्लेम ऐड करें:**
```csharp
var claims = new List<Claim> {
    new Claim("Department", "Finance")
};
await _userManager.AddClaimsAsync(user, claims);
```

**चेक करें:**
```csharp
[Authorize(Policy = "FinanceOnly")]
public IActionResult FinanceReport() { ... }
```

**पॉलिसी डेफाइन करें:**
```csharp
builder.Services.AddAuthorization(options => {
    options.AddPolicy("FinanceOnly", policy => 
        policy.RequireClaim("Department", "Finance"));
});
```

---

#### 8. **एक्सटर्नल लॉगिन (External Logins)**
**Google से लॉगिन ऐड करें:**
```csharp
builder.Services.AddAuthentication()
    .AddGoogle(options => {
        options.ClientId = "YOUR_CLIENT_ID";
        options.ClientSecret = "YOUR_SECRET";
    });
```

---

#### 9. **पासवर्ड रीसेट (Password Reset)**
- **Email सेटअप ज़रूरी है:** `appsettings.json` में:
```json
"EmailSettings": {
  "SmtpServer": "smtp.gmail.com",
  "Port": 587,
  "Username": "you@gmail.com",
  "Password": "your-password"
}
```

---

#### 10. **कॉमन एरर फिक्सेस (Common Issues & Fixes)**
- **माइग्रेशन एरर:** `dotnet ef migrations add InitialCreate --context ApplicationDbContext`
- **कनेक्शन स्ट्रिंग:** सुनिश्चित करें डेटाबेस नेम सही है।
- **रोल्स नहीं दिख रहे:** `AddRoles` सर्विस में ऐड करें।

---

#### 11. **बेस्ट प्रैक्टिसेज़ (Best Practices)**
1. पासवर्ड स्ट्रेंथ कस्टमाइज़ करें।
2. सेंसिटिव डेटा के लिए HTTPS यूज़ करें।
3. `[ValidateAntiForgeryToken]` हमेशा ऐड करें।
4. सिक्योरिटी अपडेट्स रेगुलर चेक करें।

---

#### 12. **टेस्टिंग आइडेंटिटी (Testing Tips)**
- `UserManager` और `SignInManager` को मॉक करें।
- टेस्ट केस: इनवैलिड क्रेडेंशियल्स, लॉकआउट स्केनरियो।

---

#### रिसोर्सेज़ (Resources):
1. [ऑफिशियल डॉक्स](https://learn.microsoft.com/en-us/aspnet/core/security/authentication/identity?view=aspnetcore-7.0)
2. GitHub पर सैंपल कोड: `dotnet/aspnetcore` रेपो।

> **नोट:** आइडेंटिटी को EF Core के साथ यूज़ करना कॉमन है, पर आप Dapper या अन्य ORM भी इस्तेमाल कर सकते हैं। कस्टम स्टोर्स के लिए `IUserStore` इंटरफ़ेस इम्प्लीमेंट करें।
-----
### Step 2: Install Required NuGet Packages for ASP.NET Core Identity (हिंदी-इंग्लिश मिश्रित)

ASP.NET Core Identity को काम करने के लिए निम्न चार NuGet पैकेज्स इंस्टॉल करने ज़रूरी हैं:

#### 1. **Microsoft.AspNetCore.Identity.EntityFrameworkCore**
- **क्या करता है?**  
यह पैकेज ASP.NET Core Identity और Entity Framework Core के बीच ब्रिज बनाता है।  
- **ज़रूरत क्यों?**  
इसके बिना आप यूजर्स, रोल्स और क्लेम्स जैसी आइडेंटिटी डेटा को डेटाबेस में स्टोर नहीं कर सकते।  
- **कैसे इंस्टॉल करें?**
  ```bash
  dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
  ```

#### 2. **Microsoft.AspNetCore.Identity.UI**
- **क्या करता है?**  
प्री-बिल्ट UI (Razor Pages) देता है जिसमें लॉगिन, रजिस्ट्रेशन जैसे सभी कॉमन फीचर्स शामिल होते हैं।  
- **ज़रूरत क्यों?**  
बिना इसके आपको सारा UI खुद बनाना पड़ेगा। ये पैकेज तुरंत काम करने वाला UI देता है जिसे बाद में कस्टमाइज़ किया जा सकता है।  
- **कैसे इंस्टॉल करें?**
  ```bash
  dotnet add package Microsoft.AspNetCore.Identity.UI
  ```

#### 3. **Microsoft.EntityFrameworkCore.SqlServer**
- **क्या करता है?**  
SQL Server के साथ कम्यूनिकेट करने के लिए EF Core प्रोवाइडर देता है।  
- **ज़रूरत क्यों?**  
अगर आप SQL Server डेटाबेस यूज़ कर रहे हैं, तो ये पैकेज ज़रूरी है क्योंकि ये EF Core को SQL क्वेरीज़ जनरेट करने में मदद करता है।  
- **कैसे इंस्टॉल करें?**
  ```bash
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

#### 4. **Microsoft.EntityFrameworkCore.Tools**
- **क्या करता है?**  
डेवलपमेंट टूल्स देता है जैसे Database Migrations और Scaffolding।  
- **ज़रूरत क्यों?**  
बिना इसके आप EF Core Migrations (जैसे `dotnet ef migrations add`) नहीं चला सकते। ये डेटाबेस स्कीमा अपडेट करने के लिए ज़रूरी है।  
- **कैसे इंस्टॉल करें?**
  ```bash
  dotnet add package Microsoft.EntityFrameworkCore.Tools
  ```

---

### **इंस्टॉलेशन के बाद क्या करें? (Next Steps after Installation):**
1. **कनेक्शन स्ट्रिंग ऐड करें** `appsettings.json` में:
```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=.;Database=MyAppDB;Trusted_Connection=True;"
  }
}
```

2. **Program.cs में कॉन्फ़िगर करें**:
```csharp
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);

// डेटाबेस कॉन्टेक्स्ट ऐड करें
builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(
        builder.Configuration.GetConnectionString("DefaultConnection")));

// आइडेंटिटी सर्विसेज रजिस्टर करें
builder.Services.AddDefaultIdentity<IdentityUser>(options => {
    options.Password.RequireDigit = true;
    options.Password.RequiredLength = 8;
})
.AddEntityFrameworkStores<ApplicationDbContext>();
```

---

### **कॉमन इश्यूज़ और सॉल्यूशन्स (Common Issues & Fixes):**
1. **पैकेज वर्ज़न कॉन्फ्लिक्ट**:
   - सभी पैकेज्स का वर्ज़न एक ही .NET Core वर्ज़न के लिए होना चाहिए
   - फिक्स: `dotnet restore` चलाएं और वर्ज़न अलाइन करें

2. **माइग्रेशन कमांड नहीं मिल रही**:
   - कारण: `Microsoft.EntityFrameworkCore.Tools` इंस्टॉल नहीं हुआ
   - फिक्स: पैकेज को फिर से इंस्टॉल करें

3. **SQL Server कनेक्टिविटी इश्यू**:
   - चेक करें: SQL Server इंस्टेंस चल रहा है, कनेक्शन स्ट्रिंग सही है

---

### **बेस्ट प्रैक्टिसेज़ (Best Practices):**
1. **पैकेज वर्ज़न अलाइनमेंट**:
   ```xml
   <!-- .csproj फाइल में -->
   <PackageReference Include="Microsoft.AspNetCore.Identity.EntityFrameworkCore" Version="7.0.0" />
   <PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer" Version="7.0.0" />
   ```
   सभी पैकेज्स का वर्ज़न समान रखें

2. **डेवलपमेंट vs प्रोडक्शन**:
   - डेवलपमेंट में `LocalDB` यूज़ करें
   - प्रोडक्शन में `SQL Server` या `Azure SQL`

3. **पासवर्ड पॉलिसी**:
   ```csharp
   services.AddIdentity<IdentityUser, IdentityRole>(options => {
       options.Password.RequireUppercase = true;
       options.Password.RequireNonAlphanumeric = true;
   })
   ```
   अपने ऐप्लिकेशन के हिसाब से पासवर्ड रूल्स सेट करें

4. **UI कस्टमाइज़ेशन**:
   ```bash
   dotnet aspnet-codegenerator identity -dc ApplicationDbContext --files "Account.Login"
   ```
   स्पेसिफिक पेजेस को जनरेट करके उन्हें मॉडिफ़ाई करें

> **नोट:** ये चारों पैकेज ASP.NET Core Identity के लिए कोर फाउंडेशन हैं। इनके बिना आप प्रॉपर ऑथेंटिकेशन/ऑथराइज़ेशन सिस्टम नहीं बना सकते।

---
### Step 3: डेटाबेस कॉन्टेक्स्ट क्लास डिफाइन करना (Define Database Context Class)

#### 1. **IdentityDbContext क्या है?**
- ASP.NET Core Identity का **कोर कम्पोनेंट** जो EF Core के साथ इंटीग्रेट करता है
- `DbContext` क्लास से इनहेरिट होता है
- यूजर, रोल्स, क्लेम्स जैसी आइडेंटिटी एंटिटीज को मैनेज करता है

#### 2. **क्यों जरूरी है?**
- **ऑटोमैटिक टेबल क्रिएशन**: IdentityDbContext के अंदर पहले से डिफाइंड हैं:
  ```csharp
  DbSet<IdentityUser> Users { get; set; }
  DbSet<IdentityRole> Roles { get; set; }
  DbSet<IdentityUserClaim> UserClaims { get; set; }
  // और भी एंटिटीज...
  ```
- **रिलेशनशिप मैनेजमेंट**: यूजर-रोल्स के बीच के रिलेशन्स को हैंडल करता है
- **कस्टमाइजेशन**: अपनी एप्लीकेशन के हिसाब से एक्सटेंड किया जा सकता है

#### 3. **इम्प्लीमेंटेशन स्टेप्स**
**Models फोल्डर में ApplicationDbContext.cs फाइल बनाएं:**
```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;

namespace YourProjectName.Models
{
    // IdentityDbContext को इनहेरिट करें
    public class ApplicationDbContext : IdentityDbContext
    {
        // कंस्ट्रक्टर: ऑप्शन्स को बेस क्लास में पास करें
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }

        // ऑप्शनल: कस्टम DbSet ऐड करें
        // public DbSet<YourCustomModel> CustomModels { get; set; }

        // ऑप्शनल: मॉडल कॉन्फिगरेशन ओवरराइड करें
        protected override void OnModelCreating(ModelBuilder builder)
        {
            base.OnModelCreating(builder);
            
            // कस्टम कॉन्फिगरेशन यहाँ ऐड करें
            // builder.Entity<YourCustomModel>().Property(...)
        }
    }
}
```

#### 4. **IdentityDbContext के अंदर क्या है?**
ये टेबल्स ऑटोमैटिक बनेंगी:
| टेबल नाम               | डिस्क्रिप्शन                          |
|-------------------------|----------------------------------------|
| `AspNetUsers`           | यूजर्स की बेसिक इन्फो                 |
| `AspNetRoles`           | रोल्स (Admin, User आदि)              |
| `AspNetUserRoles`       | यूजर और रोल्स के बीच मैपिंग          |
| `AspNetUserClaims`      | यूजर के क्लेम्स (Key-Value पेयर्स)  |
| `AspNetUserLogins`      | एक्सटर्नल लॉगिन प्रोवाइडर्स          |
| `AspNetUserTokens`      | ऑथेंटिकेशन टोकन्स                    |

#### 5. **कस्टम यूजर क्लास के साथ यूज़ करना**
अगर डिफॉल्ट `IdentityUser` से एक्सट्रा फील्ड्स चाहिए:
```csharp
public class ApplicationUser : IdentityUser
{
    public string FullName { get; set; }
    public DateTime DateOfBirth { get; set; }
    // अपनी कस्टम प्रॉपर्टीज ऐड करें
}

// कॉन्टेक्स्ट को अपडेट करें
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    // बाकी कोड समान रहेगा
}
```

#### 6. **Program.cs में रजिस्टर करें**
```csharp
using Microsoft.EntityFrameworkCore;
using YourProjectName.Models;

var builder = WebApplication.CreateBuilder(args);

// डेटाबेस कॉन्फिगरेशन
builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(
        builder.Configuration.GetConnectionString("DefaultConnection")));

// Identity कॉन्फिगरेशन
builder.Services.AddDefaultIdentity<IdentityUser>(options => 
    options.SignIn.RequireConfirmedAccount = true)
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

#### 7. **कॉमन एरर और सॉल्यूशन**
1. **"No database provider has been configured"**:
   - सॉल्यूशन: `AddDbContext` में `UseSqlServer()` कॉल करें

2. **माइग्रेशन एप्लाई नहीं हो रहा**:
   ```bash
   dotnet ef migrations add InitialIdentitySetup
   dotnet ef database update
   ```

3. **कस्टम यूजर क्लास नहीं दिख रही**:
   - सुनिश्चित करें कि `AddDefaultIdentity<ApplicationUser>` यूज़ कर रहे हैं

#### 8. **बेस्ट प्रैक्टिसेज़**
1. **सेपरेट प्रोजेक्ट**: कॉन्टेक्स्ट क्लास को `Data` नाम के अलग फोल्डर में रखें
2. **ऑनमॉडलक्रिएटिंग**: कॉन्फिगरेशन को साफ़ रखने के लिए
   ```csharp
   protected override void OnModelCreating(ModelBuilder modelBuilder)
   {
       modelBuilder.Entity<IdentityUser>().Property(u => u.Email).IsRequired();
       base.OnModelCreating(modelBuilder);
   }
   ```
3. **SQLite फॉर डेवलपमेंट**: टेस्टिंग के लिए हल्का डेटाबेस
   ```csharp
   options.UseSqlite("Data Source=mydb.db");
   ```

> **नोट:** `IdentityDbContext` ASP.NET Core Identity का **हार्ट** है। ये सभी इन्फ्रास्ट्रक्चर प्रोवाइड करता है जिससे आपको यूजर मैनेजमेंट के लिए मैन्युअल कोड नहीं लिखना पड़ता।

---
### Differences Between `AddIdentity` and `AddIdentityCore` in ASP.NET Core Identity  

ASP.NET Core Identity provides two primary methods for registering identity services: **`AddIdentity`** and **`AddIdentityCore`**. While both integrate identity features into the dependency injection container, they serve distinct purposes and cater to different application architectures. Below is a structured comparison:  

---

#### **`AddIdentity`**  
- **Purpose**:  
  Adds the **full identity system**, including UI support, cookie-based authentication, and user/role management.  
- **Services Registered**:  
  - `UserManager<TUser>`  
  - `RoleManager<TRole>`  
  - `SignInManager<TUser>` (handles authentication cookies)  
  - `IUserStore<TUser>`, `IRoleStore<TRole>`  
  - Cookie authentication (e.g., `AddAuthentication().AddCookie()`)  
  - Identity-related services for views (e.g., email sender, token generators).  
- **Use Cases**:  
  - Traditional MVC applications with Razor Pages/views for login, registration, and user management.  
  - Apps requiring built-in UI, cookie authentication, and ready-to-use identity workflows.  
- **Typical Setup**:  
  ```csharp
  services.AddIdentity<IdentityUser, IdentityRole>()
          .AddEntityFrameworkStores<ApplicationDbContext>()
          .AddDefaultTokenProviders();
  ```

---

#### **`AddIdentityCore`**  
- **Purpose**:  
  Adds **minimal core services** for user management only, **excluding** authentication, cookies, roles, and UI.  
- **Services Registered**:  
  - `UserManager<TUser>` (core user operations)  
  - `IUserStore<TUser>`  
  - Password validators, hashers, and basic user management.  
  - ⚠️ **Excludes**: `SignInManager`, `RoleManager`, cookie auth, and UI-related services.  
- **Use Cases**:  
  - APIs (e.g., Web API projects using token-based auth like JWT).  
  - Custom authentication flows (e.g., OAuth, OpenID Connect).  
  - Apps where roles, cookies, or built-in UI are unnecessary.  
- **Typical Setup**:  
  ```csharp
  services.AddIdentityCore<IdentityUser>()
          .AddEntityFrameworkStores<ApplicationDbContext>()
          .AddUserManager<UserManager<IdentityUser>>();
  // Requires manual auth setup (e.g., JWT):
  services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)...;
  ```

---

### Key Comparison  
| Feature                | `AddIdentity`                          | `AddIdentityCore`               |  
|------------------------|----------------------------------------|---------------------------------|  
| **User Management**    | ✅ Includes `UserManager`              | ✅ Includes `UserManager`       |  
| **Role Management**    | ✅ Includes `RoleManager`              | ❌ Excluded (add via `AddRoles`) |  
| **Authentication**     | ✅ Cookie-based (with `SignInManager`) | ❌ Requires manual setup         |  
| **Built-in UI Support**| ✅ Yes (e.g., Razor Pages)             | ❌ No                           |  
| **Ideal For**          | MVC/Razor Pages apps                   | APIs/custom auth flows          |  

---

### When to Use Which?  
- Use **`AddIdentity`** for:  
  Traditional web apps (MVC/Razor Pages) needing **out-of-the-box login/registration UI** and cookie auth.  
- Use **`AddIdentityCore`** for:  
  **API-centric apps** (SPAs, mobile backends) or **custom auth implementations** (e.g., JWT, OAuth) where you manage authentication independently.  

> 💡 **Note**: With `AddIdentityCore`, you can optionally add roles using `.AddRoles<TRole>()` and manually configure authentication (e.g., JWT).  

For deeper implementation details (e.g., configuring identity tables), refer to the [ASP.NET Core Identity documentation](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/identity).

---
ज़रूर, नीचे मैंने **`AddIdentity`** और **`AddIdentityCore`** के बीच के अंतर को **हिंदी में सरल तरीके से** समझाया है:

---

### 🔹 `AddIdentity` क्या है?

`AddIdentity` का उपयोग तब किया जाता है जब आपको **पूरी ASP.NET Core Identity सिस्टम** चाहिए — जिसमें **User Management**, **Role Management**, **Sign-In**, और UI सपोर्ट (जैसे लॉगिन, रजिस्ट्रेशन के लिए पेजेस) शामिल होते हैं।

#### 👉 विशेषताएँ:

* यह **UserManager**, **RoleManager**, **SignInManager** जैसी सारी सर्विसेस को रजिस्टर करता है।
* इसमें **UI components** का सपोर्ट भी होता है — जैसे लॉगिन और रजिस्ट्रेशन के पेज।
* यह अधिकतर **ASP.NET Core MVC ऐप्लिकेशन** में इस्तेमाल होता है जहाँ **views और controllers** होते हैं।

#### ✅ कब इस्तेमाल करें?

जब आपको एक पूरा यूज़र लॉगिन-सिस्टम चाहिए जिसमें UI, cookies, roles, authentication सब कुछ शामिल हो।

---

### 🔹 `AddIdentityCore` क्या है?

`AddIdentityCore` केवल Identity का **core हिस्सा** रजिस्टर करता है। इसका मतलब यह है कि इसमें केवल यूज़र और रोल मैनेजमेंट की सर्विसेस होती हैं — जैसे **UserManager** और **RoleManager**, लेकिन इसमें SignInManager या cookies की सुविधाएँ नहीं होतीं।

#### 👉 विशेषताएँ:

* यह हल्का (lightweight) है और केवल बेसिक सर्विसेस देता है।
* इसमें **UI सपोर्ट** या **cookie-based authentication** शामिल नहीं होता।
* ये Web API जैसी applications के लिए उपयुक्त है, जहाँ आप **token-based authentication** जैसे JWT का इस्तेमाल करते हैं।

#### ✅ कब इस्तेमाल करें?

जब आप **ASP.NET Core Web API** बना रहे हैं और खुद से authentication system (जैसे JWT token) हैंडल करना चाहते हैं — बिना built-in लॉगिन UI के।

---

### 🔚 निष्कर्ष:

| Feature                 | AddIdentity                    | AddIdentityCore                 |
| ----------------------- | ------------------------------ | ------------------------------- |
| Scope                   | Full Identity System           | Basic/Core Identity Only        |
| UserManager/RoleManager | ✅ शामिल                        | ✅ शामिल                         |
| SignInManager           | ✅ शामिल                        | ❌ शामिल नहीं                    |
| UI Components           | ✅ लॉगिन/रजिस्ट्रेशन पेज आदि    | ❌ कोई UI नहीं                   |
| Cookie Auth             | ✅ शामिल                        | ❌ शामिल नहीं                    |
| Use Case                | MVC Apps with full auth system | API Apps (JWT/Token-based auth) |

---

अगर आप MVC Application बना रहे हैं, तो `AddIdentity` यूज़ करें।
अगर आप Web API बना रहे हैं और सिर्फ core functionality चाहिए, तो `AddIdentityCore` यूज़ करें।
---
आपके द्वारा साझा की गई इमेज में दिख रहे हैं कि **`IdentityCoreDB`** नामक एक SQL Server डेटाबेस के अंदर **ASP.NET Core Identity** से संबंधित कई टेबल्स ऑटोमैटिकली बन गए हैं जैसे:

* `AspNetUsers`
* `AspNetRoles`
* `AspNetUserRoles`
* `AspNetUserClaims`
* `AspNetRoleClaims`
* `AspNetUserLogins`
* `AspNetUserTokens`
* और एक `__EFMigrationsHistory` टेबल भी।

अब हम समझते हैं कि ये टेबल्स **कैसे ऑटोमैटिकली बनती हैं** और **क्यों बनती हैं**।

---

## 🔧 आपका कोड:

```csharp
var connectionString = builder.Configuration.GetConnectionString("SQLServerIdentityConnection") 
    ?? throw new InvalidOperationException("Connection string 'SQLServerIdentityConnection' not found.");

builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(connectionString));

// Configure Identity Services
builder.Services.AddIdentity<IdentityUser, IdentityRole>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

---

## ✅ इस कोड से क्या होता है?

### 1. **DbContext रजिस्ट्रेशन (`ApplicationDbContext`)**:

आपने `ApplicationDbContext` को `UseSqlServer` के साथ रजिस्टर किया है — इसका मतलब EF Core अब इस DB (IdentityCoreDB) को उपयोग करेगा।

### 2. **Identity Configuration (`AddIdentity`)**:

आपने `AddIdentity<IdentityUser, IdentityRole>()` का उपयोग किया है, जो:

* ASP.NET Core Identity के सभी आवश्यक सर्विसेस को रजिस्टर करता है।
* EF Core के माध्यम से `ApplicationDbContext` को स्टोर के रूप में उपयोग करता है।

### 3. **Migration चलाना**:

आपने इस config के बाद Visual Studio या CLI से EF Core Migration कमांड्स चलाए होंगे जैसे:

```bash
dotnet ef migrations add InitialCreate
dotnet ef database update
```

इससे EF Core:

* Identity से जुड़े सारे entity classes (`IdentityUser`, `IdentityRole` आदि) को देखता है।
* उनके लिए SQL स्कीमा जनरेट करता है।
* और आपकी SQL Server DB में सारे **Identity tables** बना देता है।

---

## 📋 Identity Tables का मतलब

| Table Name              | Purpose                                                      |
| ----------------------- | ------------------------------------------------------------ |
| `AspNetUsers`           | यूज़र्स की मुख्य जानकारी (Email, PasswordHash, UserName आदि) |
| `AspNetRoles`           | Roles (जैसे: Admin, User) की लिस्ट                           |
| `AspNetUserRoles`       | कौन-सा यूज़र कौन-से रोल में है                               |
| `AspNetUserClaims`      | यूज़र के लिए कस्टम क्लेम्स (जैसे DOB, Country आदि)           |
| `AspNetRoleClaims`      | रोल-आधारित क्लेम्स                                           |
| `AspNetUserLogins`      | External login providers (जैसे Google, Facebook) के लिए      |
| `AspNetUserTokens`      | Tokens (जैसे reset password, auth tokens आदि)                |
| `__EFMigrationsHistory` | किस माईग्रेशन को रन किया गया है, इसका ट्रैक रखता है          |

---

## 🔚 निष्कर्ष:

आपके द्वारा लिखी गई 3 लाइनों की config और एक बार EF Migration चलाने से ही पूरा Identity सिस्टम का database structure तैयार हो जाता है।
इसका फायदा यह है कि आपको manually कोई टेबल बनाने की ज़रूरत नहीं होती।

अगर चाहें तो मैं आपके लिए `ApplicationDbContext` और माईग्रेशन कमांड्स का step-by-step प्रोसेस भी तैयार कर सकता हूँ।


