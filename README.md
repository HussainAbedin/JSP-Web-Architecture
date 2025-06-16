# JSP-Web-Architecture
# JSP Lecture 1 – JSP Lecture 1: Introduction, Lifecycle, and Directives

---

## Servlet vs JSP

- **Servlet** is meant for **processing logic**  
  e.g., user verification, checking balance, database communication

- **JSP** is meant for **presentation logic**  
  e.g., displaying login, inbox, error page

### Key Points:
1. Don’t use `println()` in Servlets for presentation.
2. Don’t write business logic inside JSP.
3. JSP uses only **tags** (HTML/XML style).
4. JSPs are **auto-compiled and auto-loaded**.
5. **Servlets** run in **Catalina** container, **JSPs** run in **Jasper** container.
6. JSP is **built on top of Servlet** technology.

---

## JSP API

- Interfaces:
  - `JspPage`: has `jspInit()`, `jspDestroy()`
  - `HttpJspPage`: has `_jspService(HttpServletRequest, HttpServletResponse)`

- Internal Base Class:
```java
public abstract class HttpJspBase {
  public final void init(ServletConfig config) throws ServletException {
    jspInit();
  }
  public final void destroy() {
    jspDestroy();
  }
  public final void service(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException {
    _jspService(request, response); // user-written JSP code placed here
  }
}
```

**Note:**
- Cannot override `init()`, `destroy()`, or `_jspService()` in JSP.
- `_jspService()` is auto-generated; prefix `_` means system-generated.

---

## JSP Life Cycle

1. Translation (.jsp → .java)
2. Compilation (.java → .class)
3. Servlet loading
4. Instantiation
5. Initialization (calls `jspInit()`)
6. Request Processing (calls `_jspService()`)
7. De-instantiation (calls `jspDestroy()`)

- On **first request**: all 7 steps
- On **subsequent requests**: only step 6

### Precompilation to Avoid Delay:
```url
http://localhost:9999/FirstApp/index.jsp?jsp_precompile=true
```

---

## JSP Scripting Elements

### 1. Template Text
Plain HTML. Gets converted to `out.write()`.

```jsp
<h1>The server time is <%= new java.util.Date() %></h1>
```

Translates to:
```java
out.write("<h1>The server time is ");
out.print(new java.util.Date());
out.write("</h1>");
```

**Note:**
- `write()` → character only
- `print()` → any data type

---

## JSP Directives

Directives are **translation-time instructions** to JSP Engine.

### 3 Types:
1. `page`
2. `include`
3. `taglib`

---

### `page` Directive

**Syntax:**
```jsp
<%@ page attribute="value" %>
```

#### Common Attributes:

| Attribute      | Description                          |
|----------------|--------------------------------------|
| `import`       | Import Java classes                  |
| `session`      | Enable/disable session object        |
| `contentType`  | MIME type for response               |
| `language`     | e.g., "java"                         |
| `isErrorPage`  | Marks this page to handle exceptions |
| `errorPage`    | Redirects on error                   |
| `pageEncoding` | Encoding (e.g., UTF-8)               |
| `isThreadSafe` | Marks thread safety                  |
| `isElIgnored`  | Disables Expression Language (EL)    |

---

### Valid Examples:

```jsp
<%@ page import="java.util.Date, java.util.ArrayList" %>
<%@ page session="false" %>
<%@ page language="java" %>
```

### Invalid Examples:

- `<%@ page session="TRUE" %>` ❌
- `<%@ page session="divya" %>` ❌

---

### Special Rules:

- You can use `import` multiple times with **different values**.
- Default imports:
```java
import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.jsp.*;
import java.lang.*;
```

---

### Sample Output:

```html
<h1>The server time is :: Tue Feb 14 22:37:24 IST 2023</h1>
<h2>Array List object creation is :: 0</h2>
```

---

📌 **End of JSP Lecture 1 Notes**

Author: Hussain Abedin  
GitHub: [@HussainAbedin](https://github.com/HussainAbedin)
