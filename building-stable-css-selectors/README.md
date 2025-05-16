<h1>
  <span class="headline">Selenium: Interacting with Elements</span>
  <span class="subhead">Building Stable CSS Selectors</span>
</h1>

**Learning Objective:** Build stable CSS selectors using tag names, classes, attributes, and descendant/child relationships.

## Reviewing CSS selector syntax and structure

CSS selectors serve as address labels for elements on a web page—guiding Selenium or any browser automation tool to the right location within the _DOM_. Writing precise selectors is crucial for building automation scripts that are maintainable and resilient.

A CSS selector is a pattern that identifies elements by:

- _Tag name_: The type of element (like `input` or `button`)
- _Class_: A reusable identifier that might appear on multiple elements
- _ID_: A unique identifier that should appear only once per page

> 📚 The _DOM_ (Document Object Model) is the structured representation of the webpage’s content. Automation tools like Selenium interact with the DOM to perform actions and gather page data.

### Example: Basic selectors

```css
button           /* All <button> elements */
.input-field     /* All elements with class "input-field" */
#login-form      /* The element with id "login-form" */
```

> 🏆 Use your browser’s DevTools to test these selectors and observe which elements they match. This helps you verify your logic before integrating selectors into your automation script.

## Combining `tag` names and `classes` for precise element selection

Often, selecting by tag name or class is too broad. By combining these, you can create more accurate and less error-prone selectors.

- `button.primary` finds only `<button>` elements with class `primary`
- `.btn.primary` selects elements that have both `btn` and `primary` classes, regardless of tag

> 💡 Combining selectors is like narrowing a search in a database—ensuring you don’t accidentally interact with the wrong record (or in this case, element) if multiple buttons or fields are present.

### Example:

```css
input.input-field      /* All <input> elements with class "input-field" */
nav.main-menu a        /* All <a> links inside an element with class "main-menu" */
```

```python
login_button = driver.find_element(By.CSS_SELECTOR, "button.primary")
nav_links = driver.find_elements(By.CSS_SELECTOR, "nav.main-menu a")
```

## Incorporating `attribute` selectors to refine targeting

At times, tag and class information are not enough—especially when elements on a page look similar or their classes change with style updates. _Attribute selectors_ allow you to target elements based on particular characteristics, such as `type`, `placeholder`, or custom data attributes.

- `[attribute=value]` finds elements with exact attribute values
- `[attribute^=value]` matches if the attribute starts with a value
- `[attribute*=value]` matches if the attribute contains the value

> 📚 An _attribute selector_ helps you pinpoint elements by properties other than class or id. For example, HTML elements often have _data-_ attributes used specifically to support testing or automation.

### Example:

```css
input[type="password"]              /* All password input fields */
button[data-action="submit"]        /* Any button with a specific data-action */
a[href^="https://"]                 /* Links beginning with "https://" */
input[placeholder*="email"]         /* Inputs whose placeholder includes "email" */
```

```python
password_input = driver.find_element(By.CSS_SELECTOR, 'input[type="password"]')
submit_button = driver.find_element(By.CSS_SELECTOR, 'button[data-action="submit"]')
```

> 💡 Attribute selectors are especially valuable on dynamic pages or frameworks where styling classes change frequently but data attributes remain stable.

## Applying `descendant` and `child` selectors

Webpages are naturally nested—menus contain links, lists contain items, forms contain inputs. CSS provides two key patterns for matching elements within these structures:

- _Descendant selector_ (`A B`): Matches any `B` inside `A` at any nesting depth
- _Child selector_ (`A > B`): Matches only direct children—`B` immediately inside `A`

> 💡 Think of this like searching for files in directories:
>
> - Descendant: Any file named `report.txt` anywhere within the `documents` folder, regardless of subfolders
> - Child: Only files named `report.txt` directly in the `documents` folder, not in its subfolders

### Example:

```css
form#login-form input      /* All inputs inside the login form */
ul.menu > li               /* Only direct <li> children of a <ul> with class "menu" */
.main-menu a               /* All links inside .main-menu at any depth */
```

```python
menu_items = driver.find_elements(By.CSS_SELECTOR, "ul.menu > li")
login_inputs = driver.find_elements(By.CSS_SELECTOR, "form#login-form input")
```

## Creating unique and reliable selectors for automation

Writing a functional selector is just the start. In automation, the best selectors are:

- **Unique** – Only match the element(s) needed
- **Stable** – Stay valid even if page appearance or class names change
- **Readable** – Are easily understood by you and your teammates

> 🏆 Best practice: Always validate selectors by checking if they match _only_ the intended element, and nothing more.

### Example: Choosing a stable selector

Suppose you’re targeting a “Submit” button in a review form:

| Selector                                 | Comment                           |
| ---------------------------------------- | --------------------------------- |
| `button`                                 | Too broad; could match any button |
| `.submit`                                | May match several elements        |
| `form.review-form button[type="submit"]` | Specific and robust               |

```python
submit_btn = driver.find_element(By.CSS_SELECTOR, "form.review-form button[type='submit']")
```

> 💡 Anchoring selectors to both a specific parent _and_ a property like `type="submit"` is a great way to boost reliability.

<div class="activity partner-exercise">
  <h2 class="title">Crafting CSS selectors for various page elements</h2>
  <span class="minutes">15 min</span>
</div>

Practice is key. Use your `index.html` file from the FalconAir mock site, open it in your browser, and explore the page structure using DevTools.

With a partner, write a CSS selector for each of the following:

**Tasks:**

- Select the **form element** used to search for flights.
- Target the **return date input field** using its `name` attribute.
- Select the **section that contains the flight results**.
- Match all **flight cards** using a class-based selector.
- Select the **header element** containing the FalconAir title.
- Target the **login form's submit button**, but only if it's nested inside the `#login-form`.

💡 Use Chrome DevTools: press `Cmd+F` or `Ctrl+F` in the Elements panel to test your selector. It should highlight the exact element(s) you're targeting.

## Strategies to maintain selector stability amidst page updates

Webpages evolve; selectors can suddenly stop working if not chosen carefully. Here’s how to maximize stability:

- **Prefer IDs, `data-*`, or ARIA attributes** because designers and developers tend to keep these consistent.
- **Avoid long, brittle chains** (e.g., `body > div > ...`) which break with even small layout shifts.
- **Regularly review your selectors**. As you spot patterns—for instance, lots of selectors starting with the same class—consider ways to simplify.
- **Refactor when possible** to prevent repeated logic across tests or scripts. A maintainable automation suite always pays off over time.

### Example: Good and bad practices

Bad selector (fragile):

```css
body > div.app > main > section.content > div.profile-card > button.btn
```

Better selector (contextual, resilient):

```css
div.profile-card button.follow-btn[data-action="follow"]
```

> ⚠️ Avoid selectors tied to layout or presentation. Focus on semantic context and attributes intended for structure or automation.
