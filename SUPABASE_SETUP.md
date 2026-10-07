# TuneUp service setup

TuneUp is a static page. Account recovery and playlist export need the following provider configuration before they can be used.

## Supabase accounts and password recovery

1. Create a Supabase project and enable email/password sign-in under **Authentication → Providers → Email**.
2. Configure an SMTP provider under **Project Settings → Authentication → SMTP Settings**. Supabase's built-in email service is intended for testing and may only send to authorized addresses.
3. Set **Authentication → URL Configuration → Site URL** to `https://jacquelineg04.github.io/TuneUp/` and add that exact URL to the redirect URL allow list. Auth emails return to the deployed site even when signup was started from localhost.
4. The project URL and public publishable key are configured in `index.html`. Keep them pointed at the project root URL (not its `/rest/v1/` REST endpoint). Never put a Supabase `service_role` key in this page.
5. Set the minimum password length to 8 in Supabase Auth settings. TuneUp's account form accepts passwords from 8 through 128 characters and usernames from 3 through 24 letters, numbers, dots, dashes, or underscores.
6. Keep the confirmation and password-recovery templates using Supabase's confirmation URL (for example, `{{ .ConfirmationURL }}`); the app supplies the deployed page as the redirect destination.

Login uses email and password. New accounts also collect a username. Visitors can choose **Continue as guest** to use TuneUp without signing in; guest mode does not create an account. On the create-account form, users can request another confirmation email. Supabase rate-limits email requests; wait for its cooldown before retrying, and configure custom SMTP for reliable delivery. The previous browser-only demo accounts are not migrated to Supabase.

## Spotify playlist export

Create a Spotify developer app, enter its Client ID in TuneUp, and register the callback URL shown in the connection dialog. Connect Spotify and approve the playlist modification permissions. Users with an older read-only connection should use **Reconnect Spotify** to approve the new permissions.

TuneUp searches Spotify for each track by title and artist, creates a private playlist, and adds the tracks it can match. Unmatched tracks are reported and skipped.

## Apple Music playlist export

An Apple Developer account, MusicKit configuration, and a server that issues short-lived MusicKit developer tokens are required. Paste a valid developer token in the Apple Music connection dialog and authorize TuneUp with the MusicKit prompt. Do not put the Apple private signing key in `index.html`.

TuneUp searches Apple Music for each track by title and artist, creates a library playlist, and includes the tracks it can match. Unmatched tracks are reported and skipped.
