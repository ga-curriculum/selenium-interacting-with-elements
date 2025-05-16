<h1>
  <span class="headline">Selenium: Interacting with Elements</span>
  <span class="subhead">Interacting with Web Elements</span>
</h1>

**Learning Objective:** Apply interaction methods like `.click()`, `.send_keys()`, `.text`, and `.get_attribute()` on selected elements.

## Introduction to web element interaction in Selenium

_Locating_ elements is like knowing where the doors, switches, and text fields are in a digital building. _Interacting_ with them is how you actually open doors, press buttons, and fill out forms.

In this lesson, we will break down each main Selenium interaction method—how it works, when to use it, and how you can combine your Python skills with these tools to automate meaningful web page actions.

## Common Selenium interaction methods

After using `find_element()` or `find_elements()` to select a target on your page, Selenium offers a handful of dedicated methods to interact with that element. These are:

1. `.click()` — Simulates a mouse click.
2. `.send_keys()` — Types text into input fields or text areas.
3. `.text` — Retrieves the text displayed to the user inside an element.
4. `.get_attribute()` — Extracts the value of an element’s HTML attribute (such as `value`, `href`, or `placeholder`).

> Think of these as the basic _verbs_ in your Selenium language. They unlock the ability to automate almost any user interaction within the browser.

Each method has a specific purpose—and when combined, they enable you to replicate how users fill out forms, click on navigational links, verify page states, and access information that may not be visible to the user.

## Using `.click()` for buttons and links

Buttons and links drive most web interactions—submitting forms, starting a search, or navigating between pages. The `.click()` method is your automation script’s way of pressing these controls.

### Example:

```python
login_button = driver.find_element(By.CSS_SELECTOR, "button.primary")
login_button.click()
# This clicks the login button, just as a user would.
```

When `.click()` is called, Selenium triggers all behaviors connected to that element—whether that is submitting a form, opening a modal, or starting a transition.

#### Common use cases

- Submitting a completed form (such as logging in or registering)
- Following links to navigate to new pages or sections
- Triggering pop-ups or revealing additional content

> ⚠ Make sure the element is visible and enabled before clicking. Selenium will give an error if the item is hidden or disabled.

> 🧠 If your button or link is controlled by JavaScript (for example, it has an `onclick` handler), `.click()` will automatically execute that code in the browser.

## Demonstrating data input via `.send_keys()`

Many automated tasks require entering information. The `.send_keys()` method acts like a virtual keyboard, sending characters or even special keys to text fields, search bars, or text areas.

### Example:

```python
username_input = driver.find_element(By.ID, "username")
username_input.send_keys("my_awesome_username")

# This types "my_awesome_username" into the field.
```

You can continue sending more characters or even use special keys (like `Keys.RETURN`) for actions like submitting forms.

> Special keys (like tab or return) are provided via Selenium’s `Keys` module. Import as needed with `from selenium.webdriver.common.keys import Keys`.

#### Common use cases

- Typing names, emails, or search queries
- Filling out forms or comments
- Sending keyboard shortcuts

> 💡 `.send_keys()` appends to the field’s existing content. Use `.clear()` before entering new text if you want to overwrite.

```python
username_input.clear()
username_input.send_keys("new_user")
```

You can combine text and special keys in one call:

```python
from selenium.webdriver.common.keys import Keys

username_input.send_keys("user_name", Keys.TAB, "password")
```

## Retrieving text content with `.text`

Sometimes, you need to confirm whether your action succeeded. For example, after log in, you want to check for a welcome message. The `.text` property returns only the _visible_ text inside an element—ignoring any hidden HTML.

### Scenario

Suppose your app displays a “Welcome back!” banner after a successful login. Rather than relying on what you think should happen, you can confirm by extracting the actual banner text.

### Example:

```python
banner = driver.find_element(By.CLASS_NAME, "confirmation-banner")
message = banner.text
print(message) # Prints: Welcome back, Jane!
```

The output matches what users actually see on the page—ideal for confirming success or scraping information for tests.

#### Common use cases

- Verifying success or error messages
- Checking menu items, article text, or labels
- Asserting expected values in test automation

> 💡 Remember: `.text` is a property, not a method. Use it as `element.text` rather than `element.text()`.

> ⚠ If the element contains no visible text (like an empty `<div>`), `.text` returns an empty string.

## Extracting element attributes using `.get_attribute()`

The data you need is not always visible—often, it’s stored in attributes like `value`, `href`, `src`, or custom `data-*` fields within the HTML. The `.get_attribute()` method allows you to access these internal values.

### Examples:

```python
email_input = driver.find_element(By.CSS_SELECTOR, 'input[type="email"]')
current_value = email_input.get_attribute("value")
print(current_value) # Prints the current content of the input

### Example: Getting a navigation link’s URL
```

```python
profile_link = driver.find_element(By.CSS_SELECTOR, ".profile-link")
profile_url = profile_link.get_attribute("href")
print(profile_url) # Prints: https://example.com/profile/123

### Example: Accessing a custom data attribute
```

```python
user_card = driver.find_element(By.CSS_SELECTOR, "div.profile-card")
user_id = user_card.get_attribute("data-user-id")
print(user_id) # Prints: 123
```

#### Common use cases

- Reading what a user has entered into a form field
- Extracting where a link points
- Gathering values stored in custom attributes (`data-user-id`, `aria-label`, etc.)

> 💡 You must use the exact attribute name shown in HTML, such as `'href'`, `'value'`, or `'data-user-id'`.

> ⚠ If the attribute doesn’t exist on the element, `.get_attribute()` returns `None`.

<div class="activity guided-walkthrough">
  <h2 class="title">Automating User Interaction</h2>
  <span class="minutes">15 min</span>
</div>

Now that you’ve learned how to locate elements using different selectors, let’s practice simulating a real user flow on the FalconAir mock site.

### Scenario: Automate a flight search interaction

Using your `index.html` page from the FalconAir project, let's write a Python script that:

1. Fills in the **departure city** and **destination city** fields.
2. Sets the **departure date** and **return date**.
3. Clicks the **Search Flights** button.
4. After "submission," collects and prints out:
   - All **flight titles** (e.g., “Flight FA101”)
   - The `data-flight-id` for each flight card
   - The text of each **Book Now** button

Use the interaction methods we just learned:

- `.send_keys()`
- `.click()`
- `.text`
- `.get_attribute()`

### Solution Code

```python
# Step 1: Fill out the search form
driver.find_element(By.ID, "departure").send_keys("Bahrain")
driver.find_element(By.ID, "destination").send_keys("London")
driver.find_element(By.ID, "departure-date").send_keys("2025-06-01")
driver.find_element(By.ID, "return-date").send_keys("2025-06-15")

# Step 2: Submit the form
driver.find_element(By.CSS_SELECTOR, "button.secondary").click()

# Step 3: Extract and print flight information
flight_cards = driver.find_elements(By.CLASS_NAME, "flight-card")

for card in flight_cards:
    flight_title = card.find_element(By.TAG_NAME, "h3").text
    flight_id = card.get_attribute("data-flight-id")
    book_text = card.find_element(By.CLASS_NAME, "book-btn").text
    print(f"{flight_title} | ID: {flight_id} | Button: {book_text}")
```

## Knowledge checks

❓ 1. Which Selenium method would you use to extract the visible text from a welcome banner after a user logs in?

- A) `.click()`
- B) `.send_keys()`
- C) `.text`
- D) `.get_attribute('text')`

❓ 2. In Selenium, what does `.get_attribute('href')` return when called on a navigation link element?

- A) The visible label for the link
- B) The URL the link points to
- C) The HTML tag used
- D) The class of the link
