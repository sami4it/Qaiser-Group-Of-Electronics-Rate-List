# QGE Portal — Email OTP Setup

This version adds the requested flow:

**Username + Password → Email OTP → Verify OTP → Open Portal**

## 1. Create a Supabase project

Create a project at https://supabase.com/.

In Authentication → Providers → Email, make sure Email authentication is enabled.

Create the QGE user in Supabase with the same email address that should receive the OTP. The user must already exist because this project uses `shouldCreateUser: false`.

## 2. Configure the OTP email template

In Supabase Authentication → Email Templates → Magic Link, change the email body so it contains the OTP token, for example:

```html
<h2>QGE Rate Portal verification code</h2>
<p>Your one-time login code is:</p>
<p style="font-size:28px;font-weight:700;letter-spacing:8px">{{ .Token }}</p>
<p>This code expires according to your Supabase Email OTP expiration setting.</p>
```

Supabase uses `{{ .Token }}` for the six-digit email OTP.

## 3. Configure the project

Copy `.env.example` to `.env` and fill in:

```text
VITE_SUPABASE_URL=https://YOUR-PROJECT.supabase.co
VITE_SUPABASE_ANON_KEY=YOUR_SUPABASE_PUBLISHABLE_OR_ANON_KEY
VITE_QGE_AUTH_EMAIL=your-authorized-email@example.com
```

Do not put a Supabase service-role/secret key in `.env` or in frontend code.

## 4. Configure the Site URL

In Supabase Authentication → URL Configuration, add your GitHub Pages URL:

```text
https://electronicsprice.github.io/Price/
```

## 5. Run locally

```bash
npm install
npm run dev
```

## 6. Deploy

Commit and push the project to GitHub. The existing GitHub Pages workflow will build and deploy it.

## Important security note

The current portal still publishes `rates.xlsx` through the GitHub Pages workflow. That means the spreadsheet itself is publicly downloadable even though the UI is locked behind login.

If the rate data is confidential, the next security step is to move the spreadsheet/data to private Supabase Storage or a protected database and fetch it only after authentication. Do not rely on hiding the portal with React as the only protection.

The existing username/password hashes are retained to preserve the current login behavior. For production-grade two-factor security, move the password verification to a server-side authentication system as well.
