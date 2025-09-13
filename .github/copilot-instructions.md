# UtaKingdom (歌王国) Landing Page
UtaKingdom is a Japanese music community mobile app. This repository contains the Jekyll-based static landing page that handles deep linking to the mobile app and OAuth authentication callbacks. The site is hosted on GitHub Pages at utakingdom.ksth.dev.

Always reference these instructions first and fallback to search or bash commands only when you encounter unexpected information that does not match the info here.

## Working Effectively

### Initial Setup
- Install Ruby and Jekyll dependencies:
  ```bash
  sudo gem install jekyll bundler jekyll-redirect-from
  ```
- Build takes <1 second. NEVER CANCEL. Set timeout to 30+ seconds for safety.
- No complex dependencies or package managers required.

### Building the Site
- `jekyll build` -- builds static site to _site/ directory. Takes <1 second.
- `jekyll build --watch` -- builds and watches for changes during development.

### Running the Development Server
- `jekyll serve --host 0.0.0.0 --port 4000` -- starts local development server.
- Site will be available at http://localhost:4000
- Server starts in <5 seconds. NEVER CANCEL. Set timeout to 30+ seconds.
- Use Ctrl+C to stop the server.

### Production Deployment
- Site automatically deploys to GitHub Pages when changes are pushed to main branch.
- No manual deployment steps required.
- GitHub Pages uses Jekyll automatically based on _config.yml.

## Validation

### Manual Testing Scenarios
After making changes, ALWAYS test these scenarios:

1. **Landing Page Functionality**:
   - Load http://localhost:4000/ 
   - Verify page displays "歌王国アプリをダウンロードして始めよう！"
   - Verify Google Play download link is present and functional
   
2. **Deep Link Testing** (limited in development):
   - Load http://localhost:4000/some/path 
   - Verify page attempts to generate utakingdom://some/path deep link
   - Verify fallback to app store link after timeout

3. **OAuth Callback Flow**:
   - Load http://localhost:4000/auth/callback/?code=test&state=test
   - Verify page handles OAuth parameters correctly
   - Verify debug information is displayed
   - Verify deep link generation for app callback

4. **404 Handling**:
   - Load http://localhost:4000/nonexistent/path
   - Verify 404 page loads with deep linking functionality
   - Verify same behavior as landing page for unknown paths

### Build Validation
- Always run `jekyll build` before committing changes.
- Check _site/ directory is generated correctly.
- Verify no build errors or warnings.

## Important Files and Structure

### Key Files
- `index.html` -- Main landing page with deep linking logic
- `404.html` -- Custom 404 page with deep linking (Jekyll Front Matter included)  
- `auth-callback.html` -- OAuth callback handler (legacy, use auth/callback/ instead)
- `auth/callback/index.html` -- Primary OAuth callback handler with detailed debugging
- `_config.yml` -- Jekyll configuration with plugins and routing
- `.htaccess` -- Apache URL rewrite rules for OAuth routing

### Configuration Files
- `_config.yml`:
  ```yaml
  plugins:
    - jekyll-redirect-from
  permalink: pretty
  ```
- `.well-known/assetlinks.json` -- Android App Links verification for com.kusutohu.UtaKingdom
- `CNAME` -- Points to utakingdom.ksth.dev for GitHub Pages custom domain

### Build Artifacts (DO NOT COMMIT)
- `_site/` -- Generated static files (excluded in .gitignore)
- `.sass-cache/`, `.jekyll-cache/` -- Jekyll cache directories

## Common Development Tasks

### Making Content Changes
- Edit HTML files directly -- no templating system used
- Maintain consistent styling across all pages
- Preserve Japanese language content and UTF-8 encoding
- Test all deep linking functionality after changes

### Updating Deep Link Scheme  
- Search for `utakingdom://` in all HTML files
- Update scheme consistently across all files
- Test with actual mobile app if possible

### OAuth Flow Changes
- Primary file: `auth/callback/index.html` 
- Legacy file: `auth-callback.html` (consider deprecating)
- Test parameter parsing for `code`, `state`, `error` parameters
- Verify debug output shows correct values

### CSS Styling Updates
- All styles are inline in HTML files -- no external stylesheets
- Maintain consistent gradient background: `linear-gradient(135deg, #667eea 0%, #764ba2 100%)`
- Mobile-first responsive design approach

## Repository Context

### Purpose
This is a companion web presence for the UtaKingdom mobile app that:
- Provides app download links for users who don't have the app installed
- Handles deep linking from web to mobile app  
- Processes OAuth authentication callbacks
- Serves as a fallback landing page

### Mobile App Integration
- Android app package: `com.kusutohu.UtaKingdom`
- Custom URL scheme: `utakingdom://`
- Google Play Store: https://play.google.com/store/apps/details?id=com.kusutohu.UtaKingdom
- App Links verification configured in `.well-known/assetlinks.json`

### Hosting
- Platform: GitHub Pages  
- Domain: utakingdom.ksth.dev
- SSL: Automatic via GitHub Pages
- CDN: Automatic via GitHub Pages

## Debugging Common Issues

### Jekyll Build Fails
- Verify `jekyll-redirect-from` plugin is installed: `gem install jekyll-redirect-from`
- Check _config.yml syntax is valid YAML
- Ensure file permissions are correct

### Deep Links Not Working  
- Cannot be fully tested in development environment
- Verify URL scheme generation logic in JavaScript
- Test with actual mobile device and app for full validation
- Fallback behavior should always work (app store links)

### OAuth Callback Issues
- Check URL parameter parsing in browser developer tools
- Verify debug output in `auth/callback/index.html` 
- Test with actual OAuth provider callback URLs
- Ensure deep link generation includes all required parameters

### Site Not Loading
- Verify Jekyll server is running on correct port
- Check for JavaScript errors in browser console  
- Ensure all file paths are correct and case-sensitive

Remember: This is a simple static site with no complex build process. Focus on HTML, CSS, and JavaScript changes rather than infrastructure modifications.