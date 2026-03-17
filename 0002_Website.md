## 🌐 **Integrating Socratic Math Tutor with WIX Website**

### ⚠️ Important Limitations
WIX does **not** support direct Python execution or complex backend services. To use our Socratic Math Tutor on **www.hsgrabouw.co.za**, we need a web-compatible integration approach.

### 🔧 Practical Integration Solutions

#### 1️⃣ **Web API + Embedded Widget (Recommended)**
- **Create a lightweight JSON API** from our Socratic Math Tutor
- **Build a simple JavaScript widget** that:
  - Handles image uploads:
    ```html
    <input type="file" id="problemImage" accept=".(png|jpg|jpeg)"_mem_placeholder="Image Upload" _style="display:none"
      _value="Upload a math problem image" _callback="handleImageUpload" _type="textinput" _disabled="false"
    />
    ```
  - Sends image to API endpoint
  - Displays Socratic questions and receives answers
  - Renders generated practice problems

- **Host API on a service like Vercel, Netlify, or Render** (free tier available)
- **Embed in WIX** using:
  1. **HTML iframe**: `</iframe><script src="https://api.yourservice.com/tutor-widget.js"></script>`
  2. **WIX HTML embedded code**: Add `
    <div id="tutor-container"></div>
    <script>
      /* Load the widget */
      const script = document.createElement('script');
      script.src = 'https://api.yourservice.com/tutor-widget.js';
      document.body.appendChild(script);
    </script>
    `

#### 2️⃣ **WIX Code + Velo Integration**
- Use **Velo by WIX** (JavaScript-based development platform)
- Create a **custom element** that:
  1. Captures image uploads via WIX Form + Button
  2. Sends image to external API via `fetch()`
  3. Renders Socratic responses directly in the page

- Example Velo snippet:
  ```javascript
  import { save } from 'wix-storage';

  export function tutorImageUpload(event) {
      wixMedia.getImageUrlFromInput('#problemImageInput').then(url => {
          fetch('https://api.yourservice.com/socratic', {
              method: 'POST',
              headers: {'Content-Type': 'application/json'},
              body: JSON.stringify({imageUrl: url}),
          })
          .then(res => res.json())
          .then(data => displaySocraticResponse(data))
          .catch(err => console.error('Error:', err));
      });
  }

  export function displaySocraticResponse(data) {
      // Render questions, practice problems, etc.
      #socraticResponse.htmlContent = data.response;
  }
  ```

#### 3️⃣ **Direct Web Interface (Separate Domain)**
- Create a **standalone web app** at a subdomain like `tutor.hsgrabouw.co.za`
- Use modern web framework (React/Vue) with the Socratic logic
- Connect to the API backend
- Embed the entire app in WIX via **HTML iframe**

### 🚀 **Step-by-Step Implementation Plan**

1. **Set Up API Backend**
   - Choose hosting platform (Vercel/Netlify/Render)
   - Deploy Socratic Math Tutor as REST API:
     - `POST /api/analyze` - Accepts image, returns Socratic questions
     - `GET /api/practice` - Returns practice problems
     - `POST /api/explain` - Returns concept explanations

2. **Create Frontend Widget**
   - Basic HTML/CSS/JS interface for image upload and response display
   - Communication with API via `fetch()`
   - Handle CORS if needed

3. **Configure WIX Site**
   - Add HTML element to page
   - Insert widget script or iframe
   - Style to match site branding

4. **Connect Student Authentication**
   - For session management, use:
     - JWT tokens (short-lived)
     - Or WIX Member Login integration

### 📦 **What You'll Need To Provide**

- **Free API credentials** for the Socratic Math Tutor backend
- **Hosting account** for the API (Vercel/Netlify free tier sufficient)
- **WIX access** to add custom HTML/JS elements
- **Domain setup** (optional for custom subdomain)

### 💡 **Key Educational Benefits**

- Students get **guided discovery** learning without direct answers
- Automatic generation of **similar practice problems**
- Clear **concept explanations** when requested
- Works on any device with a browser
- Fully integratable with existing WIX pages

### ⚙️ **Technical Notes**

- **CORS Configuration**: Ensure API allows requests from `hsgrabouw.co.za`
- **Rate Limiting**: Implement basic rate limiting to prevent abuse
- **API Key Protection**: Use environment variables, not exposed in frontend code
- **Privacy**: No student data is stored permanently

### 📎 **Next Steps**

1. **Would you like me to:** 
   - Generate the full API endpoint code?
   - Create the frontend widget HTML/JS?
   - Provide specific WIX embedding instructions?

2. **Authentication requirements**: Our tutor requires an OpenRouter API key for LLM access. Do you already have one?

The integration is completely feasible and will allow your students to use the Socratic Math Tutor directly from your WIX site!

---

**Would you like me to proceed with creating the API endpoint code and frontend widget implementation?**
