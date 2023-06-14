# Puppeteer-Test-Script
const puppeteer = require('puppeteer');

async function run() {

  const browser = await puppeteer.launch();

  const page = await browser.newPage();

  const source = 'Delhi';

  const destination = 'Jaipur';

  const date = '15 April 2023';

  // Navigate to Google Flights

  await page.goto('https://www.google.com/flights');

  // Type source, destination, and date values

  await page.waitForSelector('input[aria-label="Leaving from"]');

  await page.type('input[aria-label="Leaving from"]', source);

  await page.waitForSelector('input[aria-label="Going to"]');

  await page.type('input[aria-label="Going to"]', destination);

  await page.waitForSelector('input[aria-label="Departure date"]');

  await page.type('input[aria-label="Departure date"]', date);

  // Submit the form

  await page.keyboard.press('Enter');

  

  // Wait for the flight results to load

  await page.waitForSelector('.gws-flights-results__result-list');

  // Extract flight prices

  const prices = await page.evaluate(() => {

    const airlines = document.querySelectorAll('.gws-flights-results__carriers-airline');

    const prices = document.querySelectorAll('.gws-flights-results__price');

    const result = {};

    for (let i = 0; i < airlines.length; i++) {

      const airline = airlines[i].innerText.trim();

      const price = prices[i].innerText.trim();

      result[airline] = price;

    }

    return result;

  });

  console.log(prices);

  await browser.close();

}

run().catch(console.error);
