<h1>
  <span class="headline">Finding Elements Using Selenium</span>
  <span class="subhead">Setup</span>
</h1>

### 1: Create Your Project Folder

In this lesson, you’ll start fresh with a new project just for testing element location and interaction.

Open a terminal and run:

```bash
mkdir finding-elements-using-selenium
cd finding-elements-using-selenium
```

### 2: Set Up a Virtual Environment

Create and activate a virtual environment inside this new folder:

```bash
python -m venv venv
```

Activate it:

**macOS/Linux:**

```bash
source venv/bin/activate
```

**Windows:**

```bash
venv\Scripts\activate
```

You should now see (venv) at the start of your terminal prompt.

### 3: Install Selenium

With the virtual environment active, install Selenium using pip:

```bash
pip install selenium
```

Check that it installed correctly:

```bash
pip show selenium
```

> ✅ If Selenium installed correctly, you’ll see output with its version and location.

### 4: Create Your Project Files

Inside your `finding-elements-using-selenium` folder, create the following files:

- `test_flight_page.py`
- `index.html`
- `styles.css`

## 5. Add your starter code

For this lesson we will be using a local html file as a mock site to test our automations.

### Add the following starter code to `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>FalconAir – Book Your Flight</title>
    <link rel="stylesheet" href="styles.css" />
  </head>
  <body>
    <header>
      <h1>FalconAir</h1>
      <nav class="main-menu">
        <a href="#/home">Home</a>
        <a href="#/about">About</a>
        <a href="#/flights">Flights</a>
        <a href="#/contact">Contact</a>
      </nav>
    </header>

    <section class="auth-section">
      <h2>Login to Your Account</h2>
      <form id="login-form">
        <label for="username">Username:</label>
        <input id="username" class="input-field" type="text" name="username" />

        <label for="password">Password:</label>
        <input
          id="password"
          class="input-field"
          type="password"
          name="password"
        />

        <button class="btn primary" type="submit">Login</button>
      </form>
    </section>

    <section class="search-section">
      <h2>Search for Flights</h2>
      <form id="search-form">
        <label for="departure">Departure City:</label>
        <input
          id="departure"
          name="departure"
          type="text"
          placeholder="e.g. London"
        />

        <label for="destination">Destination City:</label>
        <input
          id="destination"
          name="destination"
          type="text"
          placeholder="e.g. Dubai"
        />

        <label for="departure-date">Departure Date:</label>
        <input id="departure-date" name="departure-date" type="date" />

        <label for="return-date">Return Date:</label>
        <input id="return-date" name="return-date" type="date" />

        <button class="btn secondary" type="submit">Search Flights</button>
      </form>
    </section>

    <section class="results-section">
      <h2>Available Flights</h2>

      <div class="flight-card" data-flight-id="FA101">
        <h3>Flight FA101</h3>
        <p><strong>From:</strong> Bahrain (BAH)</p>
        <p><strong>To:</strong> Dubai (DXB)</p>
        <p><strong>Time:</strong> 09:45 AM</p>
        <button class="btn book-btn" data-action="book">Book Now</button>
      </div>

      <div class="flight-card" data-flight-id="FA202">
        <h3>Flight FA202</h3>
        <p><strong>From:</strong> Bahrain (BAH)</p>
        <p><strong>To:</strong> Riyadh (RUH)</p>
        <p><strong>Time:</strong> 01:15 PM</p>
        <button class="btn book-btn" data-action="book">Book Now</button>
      </div>

      <div class="flight-card" data-flight-id="FA303">
        <h3>Flight FA303</h3>
        <p><strong>From:</strong> Bahrain (BAH)</p>
        <p><strong>To:</strong> London Heathrow (LHR)</p>
        <p><strong>Time:</strong> 11:30 PM</p>
        <button class="btn book-btn" data-action="book">Book Now</button>
      </div>
    </section>

    <footer>
      <p>&copy; 2025 FalconAir. Connecting the Gulf and the World.</p>
    </footer>
  </body>
</html>
```

### Add the following starter code to `styles.css`:

```css
/* Base styling */
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  background-color: #f8f9fa;
  color: #333;
}

header {
  background-color: #003366;
  color: white;
  padding: 1rem;
}

h1 {
  margin: 0;
  font-size: 1.75rem;
}

nav.main-menu {
  margin-top: 0.5rem;
}

nav.main-menu a {
  color: white;
  margin-right: 1rem;
  text-decoration: none;
  font-weight: bold;
}

section {
  padding: 2rem;
  border-bottom: 1px solid #ccc;
  background-color: white;
}

.auth-section,
.search-section,
.results-section {
  max-width: 800px;
  margin: auto;
}

label {
  display: block;
  margin-top: 1rem;
  font-weight: bold;
}

input {
  width: 100%;
  padding: 0.5rem;
  margin-top: 0.25rem;
  box-sizing: border-box;
}

.btn {
  margin-top: 1rem;
  padding: 0.6rem 1.2rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.btn.primary {
  background-color: #0055a5;
  color: white;
}

.btn.secondary {
  background-color: #17a2b8;
  color: white;
}

.btn.book-btn {
  background-color: #28a745;
  color: white;
  margin-top: 0.5rem;
}

.flight-card {
  border: 1px solid #ccc;
  border-radius: 6px;
  padding: 1rem;
  margin-bottom: 1rem;
  background-color: #f1f1f1;
}

footer {
  background-color: #003366;
  color: white;
  text-align: center;
  padding: 1rem;
  font-size: 0.875rem;
}
```

### Add the following starter code to `test_flight_page.py`:

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.service import Service

# Replace with your actual path to chromedriver
chromedriver_path = "/Users/username/Downloads/chromedriver"
service = Service(executable_path=chromedriver_path)
driver = webdriver.Chrome(service=service)

# Replace with the full path to your index.html file
# Example (macOS): "file:///Users/yourname/Desktop/finding-elements-using-selenium/index.html"
# Example (Windows): "file:///C:/Users/yourname/Desktop/finding-elements-using-selenium/index.html"
driver.get("file:///absolute/path/to/index.html")


# Automate browser actions here

driver.quit()
```

> 💡 Tip: If ChromeDriver is already in your system PATH, you can skip the `Service` setup and just write:
>
> ```python
> driver = webdriver.Chrome()
> ```

## Setup Complete!

Once everything is set up, you should be able to:

- Open index.html in Chrome
- Run test_flight_page.py
- See a browser open and then close with no errors

If you do, you're ready to move on to element targeting and interaction!
