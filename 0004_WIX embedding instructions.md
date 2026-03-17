**🚀 WIX Embedding Instructions for Socratic Math Tutor**

### **Step 1: Deploy the API**
1. **Choose a hosting platform**:
   - **Vercel** (recommended for simplicity)
   - **Netlify** (also supports Python)
   - **Render** (if you need more control)

2. **Upload your API files**:
   - `api.py` (main Flask app)
   - `requirements.txt` (dependencies)
   - `settings.yaml.default` (configuration)

3. **Set environment variables**:
   - `OPENROUTER_API_KEY=your_openrouter_key`
   - `PORT=3000` (default for Vercel/Netlify)

4. **Enable CORS**:
   - In your API settings, add: `Access-Control-Allow-Origin: https://www.hsgrabouw.co.za`

### **Step 2: Create the WIX Widget**
1. **Add a Custom HTML Element** to your WIX page:
   - Go to **Manager > Pages > Edit Page** > **+** > **Custom HTML**
   - Paste the following code (replace `https://your-api-url.com/api/analyze` with your actual API URL):

   ```html
   <div id="socratic-math-tutor" style="max-width: 600px; margin: 20px auto; padding: 20px; border: 1px solid #ccc; border-radius: 8px;">
     <h3 style="text-align: center;">Math Tutor</h3>
     <input type="file" id="mathProblem" accept=".png,.jpg,.jpeg" placeholder="Upload math problem" style="margin: 10px 0; padding: 8px; border-radius: 4px;">
     <button id="solveBtn" style="background: #28a745; color: white; border: none; padding: 10px 15px; border-radius: 5px; margin: 10px 0;">Get Guided Help</button>
     <div id="tutorResponse" style="margin-top: 20px; padding: 15px; background: #f9f9f9; border-radius: 5px;"></div>

     <script>
       async function handleSubmit() {
         const file = document.getElementById('mathProblem').files[0];
         if (!file) { alert('Please upload an image!'); return; }

         const formData = new FormData();
         formData.append('file', file);

         try {
           const response = await fetch('https://your-api-url.com/api/analyze', {
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

       document.getElementById('solveBtn').addEventListener('click', handleSubmit);
     </script>
   </div>
   ```

2. **Style the Widget** (optional):
   - Add CSS to match your WIX theme:
     ```css
     #socratic-math-tutor {
       background: white;
       border-radius: 10px;
       box-shadow: 0 2px 10px rgba(0,0,0,0.1);
       padding: 20px;
       font-family: Arial, sans-serif;
     }
     #tutorResponse {
       margin-top: 20px;
       padding: 15px;
       background: #f9f9f9;
       border-radius: 5px;
     }
     ```

### **Step 3: Test the Integration**
1. **Visit your WIX page** and upload a math problem image.
2. **Click Get Guided Help** and verify:
   - The Socratic question appears.
   - Similar practice problems are generated.
   - No errors in the browser console (check via **F12 > Console**).

### **Troubleshooting**
- **API Not Responding**: Check if your API is deployed and CORS is configured.
- **Image Upload Fails**: Ensure the file is a valid image (PNG/JPEG).
- **404 Errors**: Verify your API URL in the widget code matches the deployed endpoint.

### **Next Steps**
Would you like me to:
1. Generate a **deployment script** for Vercel/Netlify?
2. Add **responsive styling** for mobile users?
3. Implement **session management** (e.g., saving user progress)?

Let me know! 😊
