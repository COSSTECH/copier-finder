# Copier & Printer Recommendation Tool

A self-contained recommendation engine that guides Central Office Supply customers through a quick questionnaire to find the right copier or printer for their office needs. The tool is available online and integrates with EmailJS to send personalized recommendations directly to customers.

## Overview

This is a **single-file HTML/CSS/JavaScript application** (no external dependencies except Google Fonts and EmailJS) that provides two distinct product recommendation paths:

1. **Sharp MFPs (Multi-Function Printers)** — Full-function office copiers with high-volume capabilities
2. **Brother Desktop Printers** — Compact, reliable workhorse printers for everyday use

Users select their product category, answer a quick questionnaire, receive a personalized recommendation, and get it via email with full specifications and product links.

## Features

- **Two Product Paths**: Users choose between Sharp or Brother early in the flow
- **Smart Recommendations**: Volume-based recommendation logic that selects the best model
- **Current Machine Detection**: If replacing, the tool attempts to detect the current machine's speed and may suggest a speed bump
- **Email Integration**: Sends branded HTML emails with recommendation, specs, and links
- **Responsive Design**: Navy sidebar + centered content area, works on desktop
- **No Build Required**: Just open `index.html` in a browser

## File Structure

```
Copier Remmoenation Tool/
├── index.html          (Complete app — HTML, CSS, JS, models, logic)
├── README.md           (This file)
```

## Sharp MFP Path (4 Steps)

**Form Flow:**
1. Print Needs (color preference, current machine, replacing?)
2. Print Volume (monthly page estimate)
3. Features (finishing options like stapling, hole punch, etc.)
4. Contact Info (name, business, email, phone, notes)

**Recommendation Logic:**
- Maps color preference + monthly volume to a Sharp model
- If replacing and current machine is known, detects current speed and may recommend a faster model
- Sharp models included: MX-3070, MX-4070, MX-5070, MX-6070, MX-7070 (various colors/capabilities)

**Email**: Sharp-branded email with navy header (#003d5c), full specs, PDF link, and contact next-steps

## Brother Desktop Printer Path (3 Steps)

**Form Flow:**
1. Color Preference (color vs. B&W, replacing?)
2. Print Volume (monthly page estimate with Brother-specific tiers)
3. Contact Info (name, business, email, phone, notes)

**Recommendation Logic:**
- Maps color + volume to Brother model
- Color options: MFC-L3780CDW (under 500), MFC-L8970CDW (500–1k), MFC-L9670CDW (over 1k)
- B&W options: MFC-L2820DW (under 500), MFC-L5915DW (500–1k), MFC-L6915DW (over 1k)

**Email**: Brother-branded email with navy/teal header (#004B87), full specs, brochure PDF link, contact next-steps

## Technical Stack

- **Language**: HTML5 + Vanilla JavaScript (ES6)
- **Styling**: CSS Custom Properties for theming (easy color/font swaps)
- **Fonts**: Google Fonts (Syne + DM Sans)
- **Email Service**: EmailJS (client-side email via external service)
- **No Build Step**: Runs as-is in any modern browser

## Installation & Deployment

### Local Testing
1. Open `index.html` in a web browser
2. No server required — all logic is client-side

### Deployment
1. Host `index.html` on any web server
2. No database or backend needed
3. Configure EmailJS API keys (see below) in the `<script>` section

### EmailJS Setup

EmailJS credentials are embedded in the HTML (public key only — safe to expose):

```javascript
const EMAILJS_PUBLIC_KEY  = '..._your_public_key_...';
const EMAILJS_SERVICE_ID  = '..._your_service_...';
const EMAILJS_TEMPLATE_ID = '..._your_template_...';
const BCC_EMAIL           = 'sales@centralofficesupply.com';
```

**Steps to configure:**
1. Sign up at [emailjs.com](https://emailjs.com)
2. Create an email service (Gmail, Outlook, or custom SMTP)
3. Create email template for Sharp path and Brother path
4. Copy Service ID, Template ID, and Public API Key
5. Update constants in `index.html` (search for `EMAILJS_PUBLIC_KEY`)

## Models

### Sharp MFPs

Included models with full specs:
- **MX-3070**: Entry-level, 30 ppm, color optional
- **MX-4070**: Mid-range, 40 ppm, color available
- **MX-5070**: High-volume, 50 ppm, color + finishing
- **MX-6070**: Enterprise, 60 ppm, advanced finishing
- **MX-7070**: Top-tier, 70 ppm, maximum capabilities

Each has: model name, series, speed, color type, max paper size, reason, badges, spec PDF URL, and full specifications list.

### Brother Desktop Printers

Included models:
- **Color**: MFC-L3780CDW, MFC-L8970CDW, MFC-L9670CDW
- **Monochrome**: MFC-L2820DW, MFC-L2980DW, MFC-L5915DW, MFC-L6915DW

Each includes product page link and official Brother brochure PDF.

## Volume Tiers

### Sharp
- Under 2k pages/month
- 2k–5k pages/month
- 5k–10k pages/month
- Over 10k pages/month

### Brother
- Under 500 pages/month
- 500–750 pages/month
- 750–1,000 pages/month
- Over 1,000 pages/month

## Customization

### Update Branding
- **Company Name**: Search for "Central Office Supply" and replace
- **Phone**: 785-632-2177 (update throughout)
- **Email**: sales@centralofficesupply.com (update throughout)
- **Sidebar Logo**: Replace the `<img src="...logo.png...">` in the sidebar

### Update Colors
CSS custom properties at the top of the `<style>` section:
```css
--navy:          #0a1931;
--blue:          #0066cc;
--blue-glow:     #f0f5ff;
--text:          #1a2540;
--text-light:    #667085;
--surface:       #ffffff;
--border:        #d0d8e8;
```

### Update Product Models
Edit the `MODELS` object and `MODEL_LOOKUP` array in the JavaScript section. Each model needs:
- Key (unique ID like `'sharp-mx-3070-c'`)
- model, series, speed, colorType, maxPaper
- reason, badges, specPdfUrl, productUrl
- fullSpecs array

## Testing Checklist

- [ ] Category selection (Sharp vs. Brother) works
- [ ] Sharp path: All 4 form steps display correctly
- [ ] Brother path: All 3 form steps display correctly
- [ ] Validation: Required fields show error messages
- [ ] Volume thresholds map to correct models (test each tier)
- [ ] Current machine detection works (try entering a known model)
- [ ] Email sends and displays correctly (check for HTML formatting)
- [ ] Results page shows correct model, hero color, and specs
- [ ] Restart button returns to category selection
- [ ] Mobile behavior (if needed)

## Recommendation Logic Details

### Sharp Recommendation
```
colorPref + volume → model
```
Example: Color + 5k–10k pages → MX-5070C

### Brother Recommendation
```
colorPref + volume → model
```
Example: Mono + 500–750 pages → MFC-L5915DW

### Speed Bump (Sharp Only)
If replacing a known model and current speed < recommended speed, customer gets an upsell note: "You're currently on a 40ppm machine — we've recommended a 50ppm model that gives you more headroom."

## Support & Maintenance

### Future Updates
- Add new Sharp or Brother models by updating `MODELS` and `MODEL_LOOKUP`
- Adjust volume thresholds in `getSharpRecommendation()` and `getBrotherRecommendation()`
- Update email templates in `submitForm()` and `submitBrotherForm()` functions

### Contact
- **Business**: Central Office Supply
- **Sales**: 785-632-2177 | sales@centralofficesupply.com
- **Website**: centralofficesupply.com

## License & Notes

- Built for Central Office Supply
- Single-file deployment (no dependencies beyond Google Fonts + EmailJS)
- Specs sourced from Sharp USA and Brother USA product pages
- Subject to change without notice

---

**Last Updated**: June 2026  
**Version**: 2.0 (Sharp + Brother paths)
