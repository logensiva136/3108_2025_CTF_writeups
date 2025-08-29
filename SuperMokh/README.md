# SuperMokh
---
## Recon
got base64 encoded login detail on html
![alt text](SuperMokh_base64_login_html.png)

Once logged in has `View Flag` Button to view.
![alt text](SuperMokh_show_flag_access_denied.png)

But access denied to view.
On continous recon, got another hint on code.
```html
document.getElementById('viewFlagBtn').addEventListener('click', function() {
    // Hide any previous error messages
    document.getElementById('accessDeniedSection').style.display = 'none';
    
    // Check authentication and redirect to flag page
    fetch('dashboard.php', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify({action: 'check_access'})
    })
    .then(response => response.json())
    .then(data => {
        if (data.success) {
            // Access granted - redirect to flag page
            window.location.href = 'flag.php';
        } else {
            // Access denied - show error section instead of alert
            document.getElementById('accessDeniedSection').style.display = 'block';
            // Scroll to the error message smoothly
            document.getElementById('accessDeniedSection').scrollIntoView({ 
                behavior: 'smooth' 
            });
        }
    })
    .catch(error => {
        console.error('Error:', error);
        // Show error section for network errors too
        document.getElementById('accessDeniedSection').style.display = 'block';
        document.getElementById('accessDeniedSection').scrollIntoView({ 
            behavior: 'smooth' 
        });
    });
});
```

base on the code, only user `SuperMokh` can access to flag page. So we look for ATO or PrivEsc on session tokens.

in this case has JWT.

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6Imd1ZXN0Iiwicm9sZSI6InZpc2l0b3IiLCJpYXQiOjE3NTY0ODAyOTYsImV4cCI6MTc1NjQ4Mzg5Nn0.4n50QRAkkiKcynnpYv1bdv98qoSIuVAgx-AMYySml8w
```

read JWT Token using bash:
```
echo "<your_jwt>" | cut -d "." -f2 | base64 -d | jq
```
![alt text](SuperMokh_decode_JWT.png)


edit on JWT.io

![alt text](SuperMokh_edit_jwt_token.png)

once replace session token JWT, successfully got the flag!

![alt text](SuperMokh_success.png)

Success:
```
http://ctf.bahterasiber.my/share/solve?user_id=492&challenge_id=6&mac=4160445d9a0e7490a2b0a68230ac4754514e568e
```