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
