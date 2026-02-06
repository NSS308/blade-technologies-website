# Blade Technologies Website

## File Structure

```
website/
├── index.html          # Main page
├── css/
│   └── styles.css      # All styling
├── js/
│   └── main.js         # Mobile menu + form handling
├── images/
│   └── logo.png        # Transparent logo
└── README.md           # This file
```

---

## Deploying to cPanel

### Step 1: Prepare Files

1. Open the `website` folder on your computer
2. Select ALL files inside (index.html, css folder, js folder, images folder)
3. Right-click and compress/zip them into a single `.zip` file
   - Name it something like `blade-website.zip`

**Important:** Zip the *contents* of the website folder, not the folder itself. When you open the zip, you should see index.html at the root level.

### Step 2: Log into cPanel

1. Go to your hosting provider's cPanel login (usually `yourdomain.com/cpanel` or `yourdomain.com:2083`)
2. Enter your username and password

### Step 3: Open File Manager

1. In cPanel, find and click **File Manager**
2. Navigate to `public_html` (this is your website's root directory)
   - If using a subdomain or addon domain, navigate to that domain's folder instead

### Step 4: Upload and Extract

1. Click **Upload** in the top toolbar
2. Drag and drop your `blade-website.zip` file or click to select it
3. Wait for upload to complete
4. Go back to File Manager (click the back link)
5. Find your uploaded `.zip` file
6. Right-click it and select **Extract**
7. Make sure it extracts to the current directory (`public_html`)
8. Click **Extract Files**

### Step 5: Verify

1. After extraction, you should see:
   ```
   public_html/
   ├── index.html
   ├── css/
   ├── js/
   └── images/
   ```
2. Delete the `.zip` file (right-click → Delete)
3. Visit your domain in a browser - your site should be live!

### Troubleshooting

**Site not showing?**
- Make sure `index.html` is directly inside `public_html`, not in a subfolder
- Clear your browser cache (Ctrl+Shift+R)

**Images not loading?**
- Check that the `images` folder was extracted properly
- File paths are case-sensitive on some servers

**Old site still showing?**
- Clear browser cache
- Wait a few minutes for DNS/server cache to update

---

## Making the Contact Form Work

The contact form currently shows a success message but doesn't actually send emails. Here are your options:

### Option A: Formspree (Easiest - Free tier available)

1. Go to [formspree.io](https://formspree.io) and create a free account
2. Create a new form and copy your form endpoint (looks like `https://formspree.io/f/xxxxxxxx`)
3. Edit `index.html` and find this line:
   ```html
   <form class="contact-form" id="contactForm">
   ```
4. Change it to:
   ```html
   <form class="contact-form" id="contactForm" action="https://formspree.io/f/YOUR_ID" method="POST">
   ```
5. Open `js/main.js` and delete or comment out the entire contact form handler section (lines 45-70 approximately)
6. Re-upload the changed files to cPanel

### Option B: PHP Mail (If your hosting supports PHP)

Create a file called `send-mail.php` in `public_html`:

```php
<?php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $name = strip_tags(trim($_POST["name"]));
    $email = filter_var(trim($_POST["email"]), FILTER_SANITIZE_EMAIL);
    $company = strip_tags(trim($_POST["company"]));
    $message = strip_tags(trim($_POST["message"]));

    $to = "neikostrange@bladetechnologies.dev";
    $subject = "New Contact Form Submission from $name";

    $body = "Name: $name\n";
    $body .= "Email: $email\n";
    $body .= "Company: $company\n\n";
    $body .= "Message:\n$message";

    $headers = "From: $email\r\nReply-To: $email";

    if (mail($to, $subject, $body, $headers)) {
        header("Location: index.html?success=1");
    } else {
        header("Location: index.html?error=1");
    }
    exit;
}
?>
```

Then update the form in `index.html`:
```html
<form class="contact-form" action="send-mail.php" method="POST">
```

And remove the JavaScript form handler in `main.js`.

---

## Customization Quick Reference

### Change Colors

Edit `css/styles.css` - all colors are at the top:

```css
:root {
    --color-primary: #7b4cac;        /* Main purple */
    --color-primary-dark: #5f3a87;   /* Darker purple for hovers */
    --color-secondary: #1a1a1a;      /* Black text */
    /* ... */
}
```

### Change Logo

1. Replace `images/logo.png` with your new logo
2. Make sure background is transparent (PNG format)
3. Recommended size: at least 500px wide for crisp display

### Change Content

All content is in `index.html`. Search for the text you want to change and edit it directly.

---

## Local Preview

To preview changes before uploading:

**Windows (Python):**
```bash
cd website
python -m http.server 8000
```
Then open: http://localhost:8000

**VS Code:**
Install "Live Server" extension, right-click `index.html` → "Open with Live Server"

---

## Support

For questions about the website, contact the developer.

For hosting/cPanel issues, contact your hosting provider's support.
