# Bypass CAPTCHAs With Puppeteer

A quick guide to bypass CAPTCHAs by mimicking human behavior with Puppeteer. Skip the guide by signing up to Bright Data and opting-in for the [Web Unlocker API](https://brightdata.com/products/web-unlocker). 

## Step 1: Project Setup

```bash
mkdir bypass_captcha_puppeteer
cd bypass_captcha_puppeteer
npm init -y
npm install puppeteer
```

Structure:

```bash
bypass_captcha_puppeteer/
├── index.js
└── package.json
```

Include ```"type"```: ```"module"``` in ```package.json```:

```json
{
  "name": "bypass_captcha_puppeteer",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "type": "module",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "puppeteer": "^23.10.4"
  }
}
```

## Step 2: Test Puppeteer (No Stealth)

```javascript
import puppeteer from 'puppeteer';
const visitBotAnalyzerPage = async () => {
  try {
    const browser = await puppeteer.launch();
    const page = await browser.newPage();
    const url = 'https://bot.sannysoft.com/';
    console.log(`Navigating to ${url}...`);
    await page.goto(url, { waitUntil: 'networkidle2' });
    console.log('Taking full-page screenshot...');
    await page.screenshot({ path: 'anti-bot-analysis.png', fullPage: true });
    console.log('Screenshot taken');
    await browser.close();
    console.log('Browser closed');
  } catch (error) {
    console.error('An error occurred:', error);
  }
};
// run the script
visitBotAnalyzerPage();
```

Run:

```bash
node index.js
```

You may fail some bot checks, prompting CAPTCHAs.

## Step 3: Install Stealth Plugin

```bash
npm install puppeteer-extra puppeteer-extra-plugin-stealth
```

Replace ```puppeteer``` import with ```puppeteer-extra``` and add the plugin:

```javascript
import puppeteer from 'puppeteer-extra';
import StealthPlugin from 'puppeteer-extra-plugin-stealth';

// Add the stealth plugin
puppeteer.use(StealthPlugin());

const visitBotAnalyzerPage = async () => {
  try {
    const browser = await puppeteer.launch();
    console.log('Launching browser in stealth mode...');
    const page = await browser.newPage();
    const url = 'https://bot.sannysoft.com/';
    console.log(`Navigating to ${url}...`);
    await page.goto(url, { waitUntil: 'networkidle2' });
    console.log('Taking full-page screenshot...');
    await page.screenshot({ path: 'anti-bot-analysis.png', fullPage: true });
    console.log(`Screenshot taken`);
    await browser.close();
    console.log('Browser closed. Script completed successfully');
  } catch (error) {
    console.error('Error occurred:', error);
  }
};
// run the script
visitBotAnalyzerPage();
```

Run again:

```bash
node index.js
```

Stealth reduces bot detection and CAPTCHAs.

## If Stealth Isn’t Enough

For advanced WAFs and bot detection, try extra plugins (e.g., ```puppeteer-extra-plugin-anonymize-ua```) or dedicated tools. Tools like [Bright Data’s Web Unlocker API](https://brightdata.com/products/web-unlocker) handle reCAPTCHA, hCaptcha, and a lot more.
