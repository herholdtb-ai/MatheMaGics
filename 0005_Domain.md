**✅ No Problem! Using www.hsgrabouw.co.za is actually simpler**

When you use the **main domain**, you have **two excellent options** that are often **easier** than using a subdomain:

---

## **Option 1: Host API on Same Domain (Recommended)**

**Strategy:** Deploy your Socratic Math Tutor API to a **subdirectory** of your main domain:
- `https://www.hsgrabouw.co.za/api/tutor` (instead of `https://tutor.hsgrabouw.co.za`)

**Benefits:**
- ✅ **No CORS issues** (same origin)
- ✅ **Simpler configuration**
- ✅ **More secure** (cookies/sessions work seamless)
- ✅ **Better for WIX** (no cross-domain restrictions)

**How to set up:**
1. **Deploy API to a server** you control (VPS, shared hosting, etc.) that serves `www.hsgrabouw.co.za`
2. **Configure your web server** (Apache/Nginx) to route `/api/tutor/*` to your Flask app
3. **Update WIX widget** to call `https://www.hsgrabouw.co.za/api/tutor/analyze`

**Updated WIX Widget Code:**
```html
<script>
  async function handleSubmit() {
    const file = document.getElementById('mathProblem').files[0];
    if (!file) { alert('Please upload an image!'); return; }

    const formData = new FormData();
    formData.append('file', file);

    try {
      const response = await fetch('https://www.hsgrabouw.co.za/api/tutor/analyze', {
        method: 'POST',
        body: formData
      });
      const data = await response.json();
      document.getElementById('tutorResponse').innerHTML = `
        <h4>Problem Type:</h4><p>${data.problem_type}</p>
        <h4>Socratic Question:</h4><p>${data.socratic_question}</p>
        <h4>Similar Practice Problem:</h4><p>${data.practice_problem}</p>
      `;
    } catch (error) {
      document.getElementById('tutorResponse').innerHTML = 
        `<p style="color: red;">Error: ${error.message}</p>`;
    }
  }
</script>
```

---

## **Option 2: Keep API on Different Domain + CORS**

If you **must** host the API separately (Vercel/Netlify/Render):

**Changes needed:**
1. **CORS must allow** `https://www.hsgrabouw.co.za` (not a subdomain)
2. **Update widget URL** to point to your external API
3. **Configure CORS headers** in your Flask app:

```python
from flask_cors import CORS

app = Flask(__name__)
CORS(app, origins=["https://www.hsgrabouw.co.za"])  # Allow your main domain
```

4. **Update Vercel/Netlify headers** (if using):
```json
{
  "headers": [
    {
      "source": "/api/(.*)",
      "headers": [
        {"key": "Access-Control-Allow-Origin", "value": "https://www.hsgrabouw.co.za"}
      ]
    }
  ]
}
```

---

## **⚠️ Important Considerations for Main Domain**

1. **HTTPS Required**: Both WIX and modern browsers require HTTPS for security.
2. **Mixed Content**: Ensure your API URL uses `https://` (not `http://`).
3. **File Uploads**: Max file size may be limited by hosting plan (often 10-25MB).
4. **Rate Limiting**: Implement to prevent abuse if API is public.
5. **API Key Security**: Keep your OpenRouter API key on the server side (never expose to frontend).

---

## **📋 Which Approach Should You Choose?**

| Feature | Same Domain (Option 1) | Different Domain (Option 2) |
|---------|------------------------|----------------------------|
| CORS Issues | ❌ None | ✅ Need proper CORS headers |
| Hosting Flexibility | ⚠️ Needs server control | ✅ Use Vercel/Netlify free |
| Security | ✅ Highest (no secrets exposed) | ⚠️ Moderate (CORS must be right) |
| Setup Complexity | ⚠️ Requires server config | ✅ Easier deployment |
| Cost | ⚠️ May need hosting | ✅ Free tier available |

**Recommendation:** If you have any server that can host Python (even cheap shared hosting), use **Option 1**. Otherwise, use **Option 2** with Vercel/Netlify and configure CORS correctly.

---

**Question for you:**
- Do you have **existing web hosting** for `www.hsgrabouw.co.za` that can run a Python/Flask app?
- Or would you prefer to use **free hosting** (Vercel/Netlify) with CORS?

Let me know and I'll provide the exact configuration code for your choice! 🚀
