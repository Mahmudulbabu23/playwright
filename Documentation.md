# PlayWright

## Installing Playwright Pytest

* Install the Pytest plugin

```command
pip install pytest-playwright
```

* Install the required browser

```command
playwright install

playwright install webkit

playwright install --help

playwright install-deps

playwright install-deps chromium

playwright install --with-deps chromium
```

* Upgrade pip

```command
pip install --upgrade pip
```

## Running Test

```command
pytest

pytest filename -s

pytest -rP

pytest --browser webkit

pytest --browser webkit --browser firefox

pytest test_login.py --browser webkit --browser firefox

pytest test_login.py --browser-channel msedge

# only running tests headlessly
playwright install --with-deps --only-shell
```

## Parallelism: Running Multiple Tests at Once

```command
pip install pytest-xdist

pytest --numprocesses auto
```

## Updating Playwright

```command
pip install pytest-playwright playwright -U
```

## Sitting up GitHub Actions

```yml
name: Playwright Tests
on:
  push:
    branches: [ main, master ]
  pull_request:
    branches: [ main, master ]
jobs:
  test:
    timeout-minutes: 60
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v5
    - name: Set up Python
      uses: actions/setup-python@v6
      with:
        python-version: '3.13'
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
    - name: Ensure browsers are installed
      run: python -m playwright install --with-deps
    - name: Run your tests
      run: pytest --tracing=retain-on-failure
    - uses: actions/upload-artifact@v4
      if: ${{ !cancelled() }}
      with:
        name: playwright-traces
        path: test-results/
```

## Navigation

```text
page.goto(https://google.com)
```

## Timeout Command

```command
page.wait_for_timeout(5000)
```

## Basic actions

| Action | Description |
| -------- | ------------- |
| locator.check() | Check the input checkbox |
| locator.click() | Click the element |
| locator.uncheck() | Uncheck the input checkbox |
| locator.hover() | Hover mouse over the element |
| locator.fill() | Fill the form field, input text |
| locator.focus() | Focus the element |
| locator.press() | Press single key |
| locator.set_input_files() | Pick files to upload |
| locator.select_option() | Select option in the drop down |

## Assertion

| Assertion | Description |
| ----------- | ------------- |
| expect(locator).to_be_checked() | Checkbox is checked |
| expect(locator).to_be_enabled() | Control is enabled |
| expect(locator).to_be_visible() | Element is visible |
| expect(locator).to_contain_text() | Element contains text |
| expect(locator).to_have_attribute() | Element has attribute |
| expect(locator).to_have_count() | List of elements has given length |
| expect(locator).to_have_text() | Element matches text |
| expect(locator).to_have_value() | Input element has value |
| expect(page).to_have_title() | Page has title |
| expect(page).to_have_url() | Page has URL |
| expect(locator).to_be_attached() | Element is attached |
| expect(locator).to_be_checked() | Checkbox is checked |
| expect(locator).to_be_disabled() | Element is disabled |
| expect(locator).to_be_editable() | Element is editable |
| expect(locator).to_be_empty() | Container is empty |
| expect(locator).to_be_enabled() | Element is enabled |
| expect(locator).to_be_focused() | Element is focused |
| expect(locator).to_be_hidden() | Element is not visible |
| expect(locator).to_be_in_viewport() | Element intersects viewport |
| expect(locator).to_be_visible() | Element is visible |
| expect(locator).to_contain_text() | Element contains text |
| expect(locator).to_have_attribute() | Element has a DOM attribute |
| expect(locator).to_have_class() | Element has a class property |
| expect(locator).to_have_count() | List has exact number of children |
| expect(locator).to_have_css() | Element has CSS property |
| expect(locator).to_have_id() | Element has an ID |
| expect(locator).to_have_js_property() | Element has a JavaScript property |
| expect(locator).to_have_text() | Element matches text |
| expect(locator).to_have_value() | Input has a value |
| expect(locator).to_have_values() | Select has options selected |
| expect(page).to_have_title() | Page has a title |
| expect(page).to_have_url() | Page has a URL |
| expect(response).to_be_ok() | Response has an OK status |

## Code Gen Command (Automatic code generate-locator)

```command
playwright codegen https://google.com
```

## Actions

```python
# Text input
page.get_by_role("textbox").fill("Peter")

# Date input
page.get_by_label("Birth date").fill("2020-02-02")

# Time input
page.get_by_label("Appointment time").fill("13:15")

# Local datetime input
page.get_by_label("Local time").fill("2020-03-02T05:15")
```

## Checkboxes and radio buttons

```python
# Check the checkbox
page.get_by_label('I agree to the terms above').check()

# Assert the checked state
expect(page.get_by_label('Subscribe to newsletter')).to_be_checked()

# Select the radio button
page.get_by_label('XL').check()
```

## Select Options

```python
# Single selection matching the value or label
page.get_by_label('Choose a color').select_option('blue')

# Single selection matching the label
page.get_by_label('Choose a color').select_option(label='Blue')

# Multiple selected items
page.get_by_label('Choose multiple colors').select_option(['red', 'green', 'blue'])
```

## Mouse click

```python
# Generic click
page.get_by_role("button").click()

# Double click
page.get_by_text("Item").dblclick()

# Right click
page.get_by_text("Item").click(button="right")

# Shift + click
page.get_by_text("Item").click(modifiers=["Shift"])

# Hover over element
page.get_by_text("Item").hover()

# Click the top left corner
page.get_by_text("Item").click(position={ "x": 0, "y": 0})

# Force Click
page.get_by_role("button").click(force=True)
```

## Type characters

```python
# Press keys one by one
page.locator('#area').press_sequentially('Hello World!')
```

## Upload Files

```python
# Select one file
page.get_by_label("Upload file").set_input_files('myfile.pdf')

# Select multiple files
page.get_by_label("Upload files").set_input_files(['file1.txt', 'file2.txt'])

# Select a directory
page.get_by_label("Upload directory").set_input_files('mydir')

# Remove all the selected files
page.get_by_label("Upload file").set_input_files([])

# Upload buffer from memory
page.get_by_label("Upload file").set_input_files(
    files=[
        {"name": "test.txt", "mimeType": "text/plain", "buffer": b"this is a test"}
    ],
)


with page.expect_file_chooser() as fc_info:
    page.get_by_label("Upload file").click()
file_chooser = fc_info.value
file_chooser.set_files("myfile.pdf")
```

## Focus element

```python
page.get_by_label('password').focus()
```

## Drag and Drop

```python
page.locator("#item-to-be-dragged").drag_to(page.locator("#item-to-drop-at"))
```

## Scrolling

```python
# Scroll the footer into view, forcing an "infinite list" to load more content
page.get_by_text("Footer text").scroll_into_view_if_needed()

# Position the mouse and scroll with the mouse wheel
page.get_by_test_id("scrolling-container").hover()
page.mouse.wheel(0, 10)

# Alternatively, programmatically scroll a specific element
page.get_by_test_id("scrolling-container").evaluate("e => e.scrollTop += 100")
```
