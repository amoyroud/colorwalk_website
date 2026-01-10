# ColorWalk Landing Page - Application Structure

## Overview
This is a static HTML landing page for ColorWalk, a mobile app that turns everyday walks into playful color hunts. The site provides information about the app, handles early access signups, and supports universal links for mobile app invite sharing.

## Project Structure

```
/
├── index.html                 # Main landing page
├── earlyaccess.html           # Early access signup page
├── invite.html                # Invite landing page with deep linking support
├── assets/                    # Static assets (images)
│   ├── colorwalk-1.png
│   ├── colorwalk-2.png
│   └── home-background.png
├── .well-known/              # Universal Links / App Links configuration
│   ├── apple-app-site-association  # iOS Universal Links (no extension)
│   ├── assetlinks.json            # Android App Links
│   └── _headers                    # Netlify headers (alternative)
└── vercel.json               # Vercel hosting configuration

```

## Features

### 1. Main Landing Page (`index.html`)
- Hero section with app introduction
- Features showcase
- Gallery section with Instagram links
- Call-to-action to early access signup

### 2. Early Access Page (`earlyaccess.html`)
- Embedded Google Form for waitlist signup
- Responsive design matching main site

### 3. Invite Landing Page (`invite.html`)
- **Query Parameters**: Accepts `?ref=XXX&from=Name` (referral code and referrer name)
- **Deep Linking**: Attempts to open `colorwalk://invite?ref=XXX&from=Name`
- **Fallback**: Redirects to App Store (iOS) or Play Store (Android) if app not installed
- **Personalization**: Displays referrer name in content
- **Rich Previews**: Open Graph and Twitter Card meta tags for social sharing
- **User Experience**: Button-based deep link attempt with automatic fallback detection

### 4. Universal Links / App Links

#### iOS Universal Links
- **File**: `/.well-known/apple-app-site-association`
- **App ID**: `4P4Y5J4S64.com.antoinemoyroud.colorwalk`
- **Paths**: `/invite*`, `/join*`
- **Content-Type**: Must be `application/json` (configured in hosting files)

#### Android App Links
- **File**: `/.well-known/assetlinks.json`
- **Status**: Not currently configured (Android support not needed for now)
- **Package**: `com.antoinemoyroud.colorwalk`
- **SHA256 Fingerprint**: Not configured yet
- **Note**: File exists but Android App Links are not active

## Hosting Configuration

### Vercel (`vercel.json`)
- Sets `Content-Type: application/json` for `.well-known` files
- Rewrites `/invite` to `/invite.html`
- **Primary hosting platform** (no other platforms configured)

## Design System

### Colors
- **Background Deep**: `#0C0A0F`
- **Surface**: `#1E1A22`
- **Surface Glass**: `rgba(30, 26, 34, 0.85)`
- **Text Primary**: `#FAF7F2`
- **Text Secondary**: `rgba(250, 247, 242, 0.6)`
- **Accent Colors**:
  - Coral: `#FF6B5B`
  - Amber: `#FFAA33`
  - Sage: `#5BD3A8`
  - Lilac: `#B8A9FF`

### Typography
- **Display Font**: 'Sora', sans-serif (weights: 500, 600, 700)
- **Body Font**: 'DM Sans', sans-serif (weights: 400, 500, 600)

### Spacing
- xs: 8px
- sm: 16px
- md: 24px
- lg: 40px
- xl: 64px

### Border Radius
- Card: 24px
- Pill: 999px

## Critical Requirements for Universal Links

1. **Content-Type Headers**: Both `.well-known` files MUST return `Content-Type: application/json`
2. **HTTP Status**: Both files MUST return HTTP 200 (no redirects)
3. **HTTPS**: All files MUST be served over HTTPS
4. **File Extensions**: 
   - `apple-app-site-association` has NO file extension in URL (but file can be named `.json` on server)
   - `assetlinks.json` has `.json` extension
5. **Accessibility**: Files must be accessible without authentication

## Configuration Needed

### iOS Universal Links
- ✅ App ID configured: `4P4Y5J4S64.com.antoinemoyroud.colorwalk`
- ✅ App Store ID configured: `6756232994`

### Android App Links
- ⏸️ Not configured (Android support not needed for now)
- File exists at `.well-known/assetlinks.json` but is not active

## Testing Universal Links

### iOS Testing
1. Ensure site is served over HTTPS
2. Verify `/.well-known/apple-app-site-association` returns `Content-Type: application/json`
3. Test on device: `https://yourdomain.com/invite?ref=TEST123&from=TestUser`
4. Should open app directly if installed

### Android Testing
1. Ensure site is served over HTTPS
2. Verify `/.well-known/assetlinks.json` returns `Content-Type: application/json`
3. Test on device: `https://yourdomain.com/invite?ref=TEST123&from=TestUser`
4. Should open app directly if installed and verified

## Notes
- The invite page attempts deep linking with a 2.5 second timeout before showing fallback
- Open Graph meta tags are dynamically updated based on `from` query parameter
- All hosting configurations ensure proper Content-Type headers for `.well-known` files
