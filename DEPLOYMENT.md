# Indoskyware Website Deployment Guide

## Website Files Location
```
/home/hermes/.hermes/kanban/workspaces/t_8afc31a2/
```

## Files Structure
```
├── index.html              # Homepage
├── about.html              # About page
├── business-units.html     # Business units overview
├── contact.html            # Contact form page
├── skytech.html            # SkyTech Solutions
├── skybusiness.html        # SkyBusiness Software
├── skydigital.html         # SkyDigital Agency
├── skycloud.html           # SkyCloud Services
├── server.js               # Local test server
├── package.json
├── assets/
│   └── logos/              # Logo SVG files
└── src/
    ├── styles/main.css     # Main stylesheet
    └── scripts/main.js     # Main JavaScript
```

## Deployment Options

### Option 1: Nginx (Recommended for SSL)
1. Upload files to server directory (e.g., `/var/www/indoskyware`)
2. Configure Nginx with SSL certificate

```nginx
server {
    listen 80;
    server_name www.indoskyware.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name www.indoskyware.com;
    
    ssl_certificate /path/to/ssl/certificate.crt;
    ssl_certificate_key /path/to/ssl/private.key;
    
    root /var/www/indoskyware;
    index index.html;
    
    location / {
        try_files $uri $uri/ /index.html;
    }
    
    # Cache static assets
    location ~* \.(css|js|jpg|png|svg)$ {
        expires 30d;
        add_header Cache-Control "public, immutable";
    }
}
```

### Option 2: Vercel (Easiest)
```bash
cd /home/hermes/.hermes/kanban/workspaces/t_8afc31a2
npm i -g vercel
vercel --prod
```

### Option 3: Netlify (Easiest - Drag & Drop)
1. Build the files folder
2. Go to netlify.com
3. Drag and drop the folder

### Option 4: Apache
```apache
<VirtualHost *:443>
    ServerName www.indoskyware.com
    DocumentRoot /var/www/indoskyware
    
    SSLEngine on
    SSLCertificateFile /path/to/cert.crt
    SSLCertificateKeyFile /path/to/key.key
    
    <Directory /var/www/indoskyware>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

## SSL Certificate (Let's Encrypt)
```bash
# Install certbot
sudo apt install certbot python3-certbot-nginx

# Obtain certificate
sudo certbot --nginx -d www.indoskyware.com -d indoskyware.com

# Auto-renewal
sudo certbot renew --dry-run
```

## Local Testing
```bash
cd /home/hermes/.hermes/kanban/workspaces/t_8afc31a2
node server.js
# Visit http://localhost:8080
```

## SEO Checklist
- [x] Meta descriptions on all pages
- [x] Semantic HTML structure
- [x] Proper heading hierarchy (h1 → h2 → h3)
- [x] Alt text for images (when added)
- [x] Open Graph tags (can be added)
- [x] XML sitemap (can be generated)

## Performance Notes
- CSS is ~23KB (minified would be smaller)
- JS is ~3.5KB 
- No build step required (static HTML)
- Consider image optimization before deployment