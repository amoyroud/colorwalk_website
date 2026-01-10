# Setup Guide for ColorWalk Invite Links

This guide explains the configuration for universal links (iOS) for ColorWalk.

## ✅ Already Configured

- ✅ Invite landing page (`invite.html`) with query param parsing
- ✅ Deep linking logic (`colorwalk://invite?ref=XXX&from=Name`)
- ✅ Fallback to App Store
- ✅ iOS App Store ID: `6756232994`
- ✅ Open Graph meta tags for rich previews
- ✅ Personalization with referrer name
- ✅ `.well-known/apple-app-site-association` file configured
- ✅ Vercel hosting configuration

## ⚠️ Android Support

Android App Links are **not currently configured**. The `assetlinks.json` file exists but is not active. When Android support is needed in the future, configure the SHA256 certificate fingerprint in `.well-known/assetlinks.json`.

### 3. Domain Configuration (if different from colorwalk.club)
**Location**: `invite.html` (lines 11, 14, 17, 20)

**Action**: If your domain is different from `colorwalk.club`, update the Open Graph meta tags:

- `og:url`: Update to your domain
- `og:image`: Update image URL to your domain
- `twitter:url`: Update to your domain
- `twitter:image`: Update image URL to your domain

## Hosting Setup

### Vercel (Configured)
The `vercel.json` file is already configured. Just deploy and it will work:
- Sets `Content-Type: application/json` for `.well-known` files
- Rewrites `/invite` to `/invite.html`

**Just deploy to Vercel and universal links will work!**

## Testing

### Test Universal Links (iOS)

1. **Verify file is accessible over HTTPS**:
   ```
   curl -I https://yourdomain.com/.well-known/apple-app-site-association
   ```
   Should return: `Content-Type: application/json`

2. **Test on iOS device**:
   - Share link: `https://yourdomain.com/invite?ref=TEST123&from=TestUser`
   - If app is installed, it should open directly
   - If app is not installed, should redirect to App Store

### Android App Links (Not Configured)

Android App Links are not currently configured. When needed in the future:
1. Get SHA256 fingerprint from your Android signing certificate
2. Update `.well-known/assetlinks.json` with the fingerprint
3. Verify with Google's tool: https://digitalassetlinks.googleapis.com/v1/statements:list

## Troubleshooting

### Universal Links not working on iOS

1. **Check Content-Type**:
   ```bash
   curl -I https://yourdomain.com/.well-known/apple-app-site-association
   ```
   Must return: `Content-Type: application/json`

2. **Check file is valid JSON**:
   ```bash
   curl https://yourdomain.com/.well-known/apple-app-site-association | python3 -m json.tool
   ```

3. **Verify App ID**: Ensure App ID in `apple-app-site-association` matches your app's Team ID + Bundle ID

4. **Clear iOS cache**: On iOS device, try sharing link again (iOS caches the association file)

### Android App Links Troubleshooting

Android App Links are not currently configured. If implementing in the future:
1. Ensure SHA256 fingerprint matches exactly
2. Verify package name matches (case-sensitive)
3. Use Google's verification tool to test

## URL Structure

Invite links follow this structure:
```
https://yourdomain.com/invite?ref=REFERRAL_CODE&from=REFERRER_NAME
```

**Example**:
```
https://colorwalk.club/invite?ref=ABC123&from=John%20Doe
```

The deep link sent to the app will be:
```
colorwalk://invite?ref=ABC123&from=John%20Doe
```

## Notes

- Both `.well-known` files MUST be served over HTTPS (required by both iOS and Android)
- Both files MUST return HTTP 200 (no redirects)
- Content-Type MUST be `application/json` (not `text/html` or `text/plain`)
- iOS caches the association file, so changes may take time to propagate
- Android requires the SHA256 fingerprint to match exactly
