one session = one task
name your session immediately
commit frequently within a session
use /btw for quick question
export a session before a big refactor

## 1
Add two links to the footer in @template/base.html:
    - "Terms and Conditions"
    - "Privacy Policy"
Both should be plain text links, no special styling needed for now.

Point both to "#" as placeholder hrefs since the pages don't exist yet.

Do not modift anything else on the page.

git commit -m "landing:add terms and privacy links to footer"

## 2
Create a "Terms and Conditons" page for spendly.
1.Add a new route in [@app.py](http://app.py/): GET /terms ->renders templates/terms.html
2. Create  template/terms/html with generic terms and conditions ontent appropriate for a personal expense tracking app.
Include sections like:Acceptance of Terms, User Responsibilities, Data Usage, Limitation of Liability, and Contact Information.
Extend base.html if it exits, otherwise match the style of landing.html for consistency.
3.In @templates/landing.html, update the "Terms and Conditions" link in the footer to point to "/terms" instead of "#".
4 - Make sure the appearance matches the website's theme and style for a cohesive user experience.

git commit -m "landing:add terms and conditions page and route"

## 3
Do the same as you did for the "Terms and Conditions" page, but for a "Privacy Policy" page.
1. Add a new route in [@app.py][https://app.py]:
 GET /privacy -> renders templates/privacy.html.
2. Create templates/privacy.html with generic privacy policy content suitable for a personal expense tracking app.
include sections like: Data we collect, How we use data, Data sharing and third parties, User rights, Data security, and Contact information.
Extend base.html if it exists, otherwise match the style of landing.html for consistency.
3. In @templates/landing.html, update the "Privacy Policy" link in the footer to point to "/privacy" instead of "#".
4. Ensure the design is consistent with the rest of the site for a seamless user experience.

## 04
I've attacted a ascreenshot of the updated Spendly hero section design.

Modify only the hero section in @templatses/landing.html and @static/css/landing.css to match exactly, Do not change anything else on the page.

git commit -m "landing:redesign hero section to match mockup"

## 05
Add a modal to @templates/landing.html that appearrs when the user clicks "See how it work".
Requirements:
- Clicking "See how it works" opens a modal overlay.
- Modal contains an embedded youtube video (use any placeholder Youtube URL
for now, I will replace it later).
- Video should be playable within the modal.
- Clicking the close button OR click the modal close the modal.
- When the modal closes, the video must stop playing ( not continue in background).
- No page libraries of dependencies - vanilla JS only, since we are not
using any JS framework in this project
Do no modify anything else on the page.

git commit -m "landing:add youtube  modal for 'See how it click' "

## After all these commit we push origin main main
git push origin main


## 06
