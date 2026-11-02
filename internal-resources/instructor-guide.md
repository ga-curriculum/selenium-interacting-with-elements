<h1>
  <span class="headline">Selenium: Interacting with Elements</span>
  <span class="subhead">Instructor Guide</span>
</h1>

## Using Basic Locators

### General delivery guidance

- Encourage learners to actively use DevTools and run scripts with immediate feedback.
- Model the difference between robust and brittle selectors; show how to recover from mismatches or errors.
- Highlight when using one locator (such as a class name) might catch more than expected, reinforcing the importance of inspecting what your selector actually returns.
- For remote or hybrid classrooms: Adapt pair programming to virtual breakout rooms, and share sample scripts in a collaborative coding space.

### Discussion prompt solution

- CSS selectors offer the most control but can be more complex to construct. ID selectors are straightforward if unique IDs exist.
- Elements with shared styling or purposes (like navigation links) are naturally suited for class or CSS selector targeting.
- The choice often depends on the structure of the page and the availability of unique attributes.

---

## Building Stable CSS Selectors

### Delivery notes

- Emphasize the role of DevTools as an immediate way to test and troubleshoot CSS selectors before moving to code.
- Encourage learners to explain _why_ each selector works and what makes it robust. Challenge them to tweak selectors and see failure scenarios (e.g., broader matches, accidental selections).
- For remote delivery, learners can post screenshots of selector matches or share their script outputs via chat.
- Support learners in identifying stable selector features—point out good candidate attributes and caution against over-relying on visible text or presentation details.

---

### Activity solution (suggested answers)

Here are suggested CSS selectors that match the elements described in the revised partner exercise:

| Task                                                                                | Suggested CSS Selector              |
| ----------------------------------------------------------------------------------- | ----------------------------------- |
| Select the form element used to search for flights                                  | `#search-form`                      |
| Target the return date input field using its `name` attribute                       | `input[name="return-date"]`         |
| Select the section that contains the flight results                                 | `.results-section`                  |
| Match all flight cards using a class-based selector                                 | `.flight-card`                      |
| Select the header element containing the FalconAir title                            | `header`                            |
| Target the login form’s submit button, but only if it’s nested inside `#login-form` | `#login-form button[type="submit"]` |

> 💬 Encourage discussion around why using attributes like `name`, structural relationships like descendants, and form IDs creates more stable and readable selectors.

---

## Interacting with Web Elements

### Delivery guidance

- Encourage learners to narrate their thought process as they decide which interaction method to use in each part of the form automation activity. Emphasize connecting each method to common real-world scenarios (e.g., “users type into this field, so use `.send_keys()` here”).
- Demonstrate step-by-step how to verify selectors in Chrome DevTools before translating them to Selenium code; have participants practice this live.
- Scaffold the debugging process: model how to use print statements and the browser's live inspection to investigate errors or invisible elements.
- For remote or in-person delivery, encourage learners to paste their code or screenshots, along with notes on any issues encountered, for collaborative troubleshooting and feedback.

---

### Knowledge check answers

1. Correct answer: C) `.text`
2. Correct answer: B) The URL the link points to

---

### Activity solution (suggested output)

This is an example solution for automating a flight search using the FalconAir HTML page.

**`test_flight_page.py`**

```python

# 1. Fill out the search form
driver.find_element(By.ID, "departure").send_keys("Bahrain")
driver.find_element(By.ID, "destination").send_keys("London")
driver.find_element(By.ID, "departure-date").send_keys("2025-06-01")
driver.find_element(By.ID, "return-date").send_keys("2025-06-15")

# 2. Submit the form
driver.find_element(By.CSS_SELECTOR, "button.secondary").click()

# 3. Extract and print flight info
flight_cards = driver.find_elements(By.CLASS_NAME, "flight-card")

for card in flight_cards:
    flight_title = card.find_element(By.TAG_NAME, "h3").text
    flight_id = card.get_attribute("data-flight-id")
    book_text = card.find_element(By.CLASS_NAME, "book-btn").text
    print(f"{flight_title} | ID: {flight_id} | Button: {book_text}")
```

---

🏗️ **Under Construction**

We are constantly working to improve our resources for instructors and students.

**Have something to contribute to this Instructor Guide?** [Let us know](https://ga-curriculum.github.io/universal-resources-internal/module-feedback.html).
