SWARDLINE TURF MANAGEMENT WEBSITE: SETUP GUIDE
==============================================

A. WHAT IS IN THIS FOLDER
  index.html, about.html, contact.html ...  the pages
  services/                                 one page per service
  images/<service>/                         photos, one folder per service
  data/gallery.json                         the list of photos shown on the site
  css/, js/                                 design and behaviour (no need to edit)
  functions/api/quote.js                    the quote-form backend (emails you the enquiry)

B. SET UP THE QUOTE FORM (free, about 10 minutes). Do this BEFORE going live.
 1. Email delivery: create a free account at https://resend.com using Swardlineturf@gmail.com.
    Go to API Keys > Create API Key. Copy it. (Free accounts can send only to the
    email they signed up with, which is exactly what you need.)
 2. Spam protection: in the Cloudflare dashboard open Turnstile > Add widget.
    Add your site's domain (you can add the .pages.dev address too). Choose "Managed".
    Copy the SITE KEY and the SECRET KEY.
 3. Open contact.html in a text editor and replace YOUR_TURNSTILE_SITE_KEY with your site key.
 4. Deploy (section C), then in Cloudflare: Workers & Pages > your project >
    Settings > Variables and Secrets > add (type "Secret"):
        RESEND_API_KEY        = your Resend key
        TURNSTILE_SECRET_KEY  = your Turnstile secret key
        TO_EMAIL              = Swardlineturf@gmail.com
    Then redeploy (Deployments > Retry/redeploy) so the variables take effect.
 5. Test: send a quote request from the Contact page. It should arrive in your inbox
    (check Spam the first time). Replying to it replies to the customer.

C. UPLOAD TO CLOUDFLARE PAGES
  The form backend needs a deploy that includes the "functions" folder.
  Easiest route: GitHub.
   1. Create a free GitHub account and a new repository; upload everything in this folder.
   2. In Cloudflare: Workers & Pages > Create > Pages > Connect to Git > pick the repository.
   3. Build command: leave empty. Build output directory: / (or leave blank). Save and Deploy.
   4. Every time you change a file on GitHub, the site updates automatically.
  Direct upload (drag and drop) also works but only picks up the functions folder if you
  deploy with Wrangler (npx wrangler pages deploy .). GitHub is simpler.
  HTTPS is automatic. Add your own domain under Custom domains.
  After you have a domain: replace YOURDOMAIN in sitemap.xml and robots.txt.

D. CHANGE PHOTOGRAPHS (no code editing)
  1. Put your new photo in the right folder, e.g. images/scarifying/my-lawn-after.jpg.
     Use JPG or WebP, about 1600 pixels wide, ideally under 400 KB.
  2. Open data/gallery.json and add a line in that service's list:
        { "src": "/images/scarifying/my-lawn-after.jpg", "alt": "Short description of the photo" }
     (mind the commas between lines).
  3. The FIRST photo in each list is the main photo of that service page and its card
     on the home page. The rest appear in the page's gallery.
  4. To remove a photo, delete its line (and the file). To replace one, change its "src".
  Lists: hero (home banner), work (home gallery), about, cutting, scarifying, weed-control,
  overseeding, renovation, irrigation, leaf-removal, consultancy, pest-disease, artificial-grass.
  The grey/green "placeholder" images are labelled and can be deleted once replaced.

E. CHANGE TEXT
  Open the page (e.g. services/scarifying.html) in any text editor and edit the words
  between the tags. Telephone, email and WhatsApp details appear in every page's footer
  and in contact.html; use find-and-replace across the folder to change them.

F. NOTES
  - Facebook link is a search link for "Swardline Turf". Replace it (find
    facebook.com/search/top?q=Swardline%20Turf) with your page's direct address.
  - No reviews, guarantees or qualifications beyond those you supplied have been added.


G. NEW: FAQ PAGE, SOCIAL MEDIA AND SEO
  - FAQ page: faq.html. To edit a question or answer, open the file in a text editor and change the words
    inside <summary>...</summary> (question) and <p>...</p> (answer). Keep the matching text in the
    <script type="application/ld+json"> block near the top of the file in step, as Google uses it.
  - Your website address: pages contain the placeholder https://YOURDOMAIN. This is replaced automatically
    with your live address by functions/_middleware.js, so you do not need to edit anything.
    Once you buy a custom domain, add a variable in Cloudflare: SITE_URL = https://www.yourdomain.co.uk
  - Social preview image: images/social/swardline-share.jpg (1200 x 630). It shows when a page is shared on
    Facebook, WhatsApp and similar. Replace the file (same name) to change it.
  - After going live, test a link in Facebook's Sharing Debugger (developers.facebook.com/tools/debug)
    and click "Scrape Again" if you change the image.
  - Free extras worth doing: create a Google Business Profile, and in Cloudflare turn on Web Analytics
    (Workers & Pages > your project > Metrics) to see visitor numbers.
  - Photos: JPEG photos were resized to a maximum of 1800 pixels and compressed (same file names).
    iPhone .HEIC files cannot be shown on websites: convert them to JPG first.
