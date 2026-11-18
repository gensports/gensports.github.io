# GEN SPORTS: dynamic all-sports platform

Public website + private admin dashboard on one Supabase database. No sports content is pre-filled. Free stack: HTML/CSS/JS, Supabase (free tier), GitHub Pages.

## Folder structure
```
index.html          public website
admin.html          private admin dashboard
style.css           shared design (dark/light)
config.js           your Supabase URL + anon key
assets/logo.png     GEN SPORTS logo (header crop, hero, footer, favicon)
supabase/schema.sql tables, indexes, RLS policies, legal page placeholders
```

## 1. Supabase setup
1. Create a free project at supabase.com.
2. Open **SQL Editor**, paste all of `supabase/schema.sql`, click **Run**.
3. **Project Settings > API**: copy the *Project URL* and the *anon public* key into `config.js`. Never use the `service_role` key.
4. **Authentication > Providers > Email**: keep Email on. Turn **off** "Allow new users to sign up" once your admin exists.

## 2. Admin account
1. **Authentication > Users > Add user**: enter your email and a strong password, tick "Auto confirm user".
2. In **SQL Editor** run (use your email):
```sql
insert into public.admins (user_id)
select id from auth.users where email = 'you@example.com';
```
Only users listed in `admins` can write data. Admins can be added only through the SQL Editor.

## 3. GitHub Pages
1. Create a GitHub repository and upload every file in this folder (keep `assets/` and `supabase/`).
2. **Settings > Pages**: Source = *Deploy from a branch*, branch `main`, folder `/ (root)`.
3. Site: `https://USERNAME.github.io/REPO/`  Admin: `.../admin.html`
4. **Supabase > Authentication > URL Configuration**: set Site URL to your Pages address.

## 4. How to use
1. **Sports & Pages**: add a sport (type *Sport*). Set a *Parent* to make a category under it. Publish.
2. **Sections**: pick the page, create a section, choose a display type, add custom fields (text, long text, number, date, dropdown, URL, status). Publish.
3. **Content**: pick the section, add records, publish.
4. **Navigation**: published sports appear in the menu automatically; add extra links here.
5. **Site Settings**: name, tagline, footer, ads on/off, optional AdSense publisher ID.
6. Every item supports Draft/Publish, Show/Hide, Reorder (↑ ↓), Duplicate, Delete, Preview.
7. A page with slug `home` shows its sections on the homepage (featured area).

## Testing checklist
- Open `index.html`: empty state shows, no sports content.
- Sign in at `admin.html`; a non-admin account must be refused.
- Create sport > section > fields > record. While draft, confirm it is **not** visible in a private/incognito window.
- Publish all three levels: it appears on the public site. Unpublish any level: it disappears.
- In incognito, try `sb.from('pages').insert(...)` from the console: must fail (RLS).
- Test search, theme toggle and mobile width.

## Notes
- Preview opens `index.html#/p/slug?preview=1`; drafts show only while you are signed in to admin in the same browser.
- Search reads up to 1000 published records in the browser (fine for the free tier; move to a Postgres full-text index if you outgrow it).
- Ads: set **Ad placements = On**; add your AdSense ID after Google approves your site. Approval is not guaranteed. Replace the legal page placeholders with your own policies.
- The logo is used as supplied. It contains a "Cricket Information Network" line; for a pure all-sports brand, replace `assets/logo.png` with a version without that line.
