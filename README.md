```js

const { chromium } = require('playwright');

(async () => {
  const browser = await chromium.launch();
  const context = await browser.newContext();

  // Construct Basic Auth header: "Basic base64(username:password)"
  // Replace with your actual username and password:
  const username = '**************************************';    // API Key
  const password = '';
  const basicAuth = 'Basic ' + Buffer.from(`${username}:${password}`).toString('base64');

  // Use the APIRequest object from the context
  const apiRequest = await context.request;

  // Make the POST request with Basic Auth header
  const response = await apiRequest.post('https://api.ashbyhq.com/job.list', {
    headers: {
      'Authorization': basicAuth
    }
  });

  console.log(`Status: ${response.status()}`);

  if (response.ok()) {
    const json = await response.json();
    console.log('Response JSON:', json);
  } else {
    console.error('Failed to get jobs:', await response.text());
  }

  await browser.close();
})();
```
