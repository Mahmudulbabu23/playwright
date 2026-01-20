# Playwright

## Install Python
```
https://www.python.org/downloads/
```
## Upgrade PIP
```
pip install --upgrade pip
```

## Playwrite Install
```
pip install playwrite

python -m playwright install

playwright install

pip show playwright
```

## Pytest Install
```
pip install pytest
or
pip install pytest-playwright
```

## Pytest HTML Report
```
pip install pytest-html

pytest --html=Playwright_Report.html
```

## Open Browser
```
from playwright.sync_api import sync_playwright
import asyncio

with sync_playwright() as p:
    browser = p.chromium.launch(headless=False)
    page = browser.new_page()
    page.goto("http://playwright.dev")
    print(page.title())
    page.wait_for_timeout(5000)
```

```
# async def main():
# Playwright supports two variations of the API: synchronous and asynchronous. If your modern project uses asyncio, you should use async API:

async def main():
    async with async_playwright() as p:
        browser = await p.chromium.launch(headless=False)
        page = await browser.new_page()
        await page.goto("http://playwright.dev")
        print(await page.title())
        await browser.close()

asyncio.run(main())
```

## Screenshot
```
from playwright.sync_api import sync_playwright
import asyncio

with sync_playwright() as p:
    browser = p.chromium.launch(headless=False)
    page = browser.new_page()
    page.goto("http://playwright.dev")
    page.screenshot(path="screen.png")
    browser.close()
```

## Locator css Selector
```
from playwright.sync_api import sync_playwright
import asyncio

with sync_playwright() as p:
    browser = p.chromium.launch(headless=False)
    page = browser.new_page()
    page.goto("https://demo.automationtesting.in/Index.html")
    # page.wait_for_timeout(2000)
    # page.screenshot(path="screenn.png")
    # browser.close()

    # css Selector - id, class, tagname-name-username, placeholder, etc(tagName[attribute='value])

    # using ID
    emailTextBox = page.wait_for_selector("#email")
    emailTextBox.click()
    emailTextBox.type("test@gmail.com")
    page.wait_for_timeout(5000)

    # using id
    clickButton = page.wait_for_selector("#enterimg")
    clickButton.click()
    page.wait_for_timeout(5000)


    page.goto("https://opensource-demo.orangehrmlive.com/web/index.php/auth/login")
    # Using tagName
    usernameField = page.wait_for_selector("input[name='username']")
    usernameField.type("Admin")
    page.wait_for_timeout(5000)
    passwordField = page.wait_for_selector("input[name='password']")
    passwordField.type("admin123")
    page.wait_for_timeout(5000)
    loginButton = page.wait_for_selector("button[type='submit']")
    loginButton.click()
    page.wait_for_timeout(5000)
```
## Locator Xpath
```
from playwright.sync_api import sync_playwright
import asyncio

with sync_playwright() as p:
    browser = p.chromium.launch(headless=False)
    page = browser.new_page()
    page.goto("https://opensource-demo.orangehrmlive.com/web/index.php/auth/login")

    #Xpath - Relative xpath
    # //tagName[@attribute='value']
    # //input[@name='username']
    usernameField = page.wait_for_selector("//input[@name='username']")
    usernameField.type("Admin")
    page.wait_for_timeout(5000)

    passwordField = page.wait_for_selector("//input[@placeholder='Password']")
    passwordField.type("admin123")

    loginButton = page.wait_for_selector("//button[@type='submit']")
    loginButton.click()
    page.wait_for_timeout(5000)


    # Text() - //tagName[text()='value']
    # //p[text()='Forgot your password?']
    forgotPassword = page.wait_for_selector("//p[text()='Forgot your password? ']")
    forgotPassword.click()

    # Contains() - //tagName[contains(@attribute,'value')]
    # //a[contains(@href,'forgotPassword')]

```


## Select Drop Down
```
from playwright.sync_api import sync_playwright
import asyncio

with sync_playwright() as p:
    browser = p.chromium.launch(headless=False)
    page = browser.new_page()
    page.goto("https://demo.automationtesting.in/Register.html")

    # css selector
    # Using ID
    # skillsDropDown = page.wait_for_selector("#Skills")

    # Using xpath
    skillsDropDown = page.wait_for_selector("//select[@id='Skills']")

    skillsDropDown.click()
    page.wait_for_timeout(2000)
    skillsDropDown.select_option("Certifications")
    page.wait_for_timeout(2000)
```

## Radio and Check Box
```
from playwright.sync_api import sync_playwright
import asyncio

with sync_playwright() as p:
    browser = p.chromium.launch(headless=False)
    page = browser.new_page()
    page.goto("https://demo.automationtesting.in/Register.html")


    # Radio Button
    radio_button = page.wait_for_selector("input[value='Male']")
    radio_button.click()
    # radio_button.check()
    page.wait_for_timeout(5000)

    # Checkbox
    checkbox = page.wait_for_selector("//input[@value='Cricket']")
    checkbox.check()
    page.wait_for_timeout(5000)
    if checkbox.is_checked():
        print("Cricket Checkbox is checked-Passed")
    else:
        print("Cricket Checkbox is not checked-Failed")
```

## Alert Dialog Box Handling
```
from playwright.sync_api import sync_playwright
import asyncio

with sync_playwright() as p:
    browser = p.chromium.launch(headless=False)
    page = browser.new_page()
    page.goto("https://demo.automationtesting.in/Alerts.html")

    # Alert
    # Alert Box

    clickOption = page.wait_for_selector("//a[text()='Alert with OK & Cancel ']")
    clickOption.click()
    page.wait_for_timeout(5000)

    # Control to alert box
    page.on("dialog", lambda dialog : dialog.accept())
    # page.on("dialog", lambda dialog : dialog.dismiss ())
    # page.on("dialog", lambda dialog : print(dialog.message()))

    clickAlertButton = page.wait_for_selector("//button[text()='click the button to display a confirm box ']")
    clickAlertButton.click()
    page.wait_for_timeout(5000)
```

## Window Handling
```
from playwright.sync_api import sync_playwright
import asyncio

with sync_playwright() as p:
    browser = p.chromium.launch(headless=False)
    context = browser.new_context() # new_page handle only one tab and new_context handle multiple tabs
    page = context.new_page()
    page.goto("https://demo.automationtesting.in/Windows.html")


    # Window Handle
    openNewTabbed = page.wait_for_selector("//button[contains(text(), '    click   ')]")
    openNewTabbed.click()
    page.wait_for_timeout(5000)

    # how to find the total pages
    total_pages = context.pages
    print(len(total_pages))
    for i in total_pages:
        print(i)

    # how to switch to the new tab
    new_page = total_pages[1]
    new_page.bring_to_front()
    print(new_page.title())
    page.wait_for_timeout(5000)
    new_page.close()
```

## Mouse and Keyboard Actions
```
from playwright.sync_api import sync_playwright
import asyncio

with sync_playwright() as p:
    browser = p.chromium.launch(headless=False)
    page = browser.new_page()
    page.goto("https://demo.automationtesting.in/Alerts.html")

    # Mouse Hover Action
    hoverMouse = page.wait_for_selector("//a[text()='SwitchTo']")
    hoverMouse.hover()
    page.wait_for_timeout(5000)

    # click on element
    hoverMouse.click()

    # Double Click
    hoverMouse.dblclick()

    # Right Click
    hoverMouse.click(button="right")

    # Keyboard Action
    #shift click
    hoverMouse.click(modifiers=["Shift"])

    # keyboard
    # A-Z, 0-9, F1-F12, all special characters, arrow keys, Enter, Escape, Backspace, Tab, Shift, Control, Alt, Meta, CapsLock, PageUp, PageDown, End, Home, Insert, Delete, and Pause.
    page.keyboard.press("a")
    page.keyboard.press("F1")
    page.keyboard.press("ArrowDown")
    page.keyboard.press("Enter")
    page.keyboard.press("Escape")
```

## Web Table Elements
```
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch(headless=False)
    context = browser.new_context()
    page = context.new_page()
    # Open URL
    page.goto("https://www.techlistic.com/2017/02/automate-demo-web-table-with-selenium.html")

    table = page.wait_for_selector("//table[@id='customers']")
    tr = table.query_selector_all("tr")
    print(len(tr))

    td = table.query_selector_all("td")
    print(len(td))

    for row in tr:
        cells = row.query_selector_all("td")
        for cell in cells:
            print(cell.text_content())
    page.wait_for_timeout(2000)
```

## File Upload Handling
```
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch(headless=False)
    context = browser.new_context()
    page = context.new_page()
    page.goto("https://demo.automationtesting.in/FileUpload.html")

    file_upload = "./fileuploadFile.txt"
    upload_locator = page.query_selector("//input[@id='input-4']")
    upload_locator.set_input_files(file_upload)
    page.wait_for_timeout(5000)
```

## Screenshot and Video Recording
```
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch(headless=False)
    context = browser.new_context(record_video_dir='./videos')
    page = context.new_page()
    page.goto("https://opensource-demo.orangehrmlive.com/web/index.php/auth/login")

    page.wait_for_selector("//input[@name='username']").fill("Admin")
    page.wait_for_selector("//input[@name='password']").fill("admin123")
    page.wait_for_timeout(5000)
    page.screenshot(path="screensshot.png")
    page.wait_for_selector("//button[text()=' Login ']").click()
    page.screenshot(path="screensssshot.png")
    page.wait_for_timeout(5000)
    page.close()

```

## Scrolling
```
with sync_playwright() as p:
    browser = p.chromium.launch(headless=False)
    context = browser.new_context()
    page = context.new_page()
    page.goto("https://demo.automationtesting.in/DynamicData.html")

    # Scroll to the bottom of the page
    page.evaluate("window.scrollTo(0, document.body.scrollHeight)")
    page.wait_for_timeout(5000)
```

# Pytest and Playwright Framework

## Run Pytest
```
pytest
pytest /main.py
```

## Run Chrome Browser
```
import pytest
from playwright.sync_api import playwright

def test_login(playwright):
    browser = playwright.chromium.launch(headless=False)
    page = browser.new_page()
    page.goto("https://opensource-demo.orangehrmlive.com/web/index.php/auth/login")
    assert page.title() == "OrangeHRM"
```

## Pytest Fixures
```
1. Function
2. Class
3. Module
4. Package
5. Session
```

## Test Chrome browser
```
import pytest
from playwright.sync_api import playwright

def test_login(playwright):
    browser = playwright.chromium.launch(headless=False)
    page = browser.new_page()
    page.goto("https://opensource-demo.orangehrmlive.com/web/index.php/auth/login")
    assert page.title() == "OrangeHRM"
```

## Fixtures Demo
```
import pytest
from playwright.sync_api import sync_playwright

@pytest.fixture(scope="module") # scope="module" is used to open the browser once and close it after all the tests are executed
def browser():
    with sync_playwright() as p:
        browser = p.chromium.launch(headless=False)
        yield browser # yeild is used to return the browser object to the test function
        browser.close()

@pytest.fixture(scope="function") # scope="function" is used to open the browser before each test and close it after the test is executed
def page(browser):
    page = browser.new_page()
    yield page
    page.close()

def test_keyword_search(page):
    page.goto("https://www.google.com")
    page.fill('input[name="q"]', "Playwright")
    page.press('input[name="q"]', "Enter")
    assert page.title() == "Playwright - Google Search"
```
## Class and Function
```
import pytest
from playwright.sync_api import sync_playwright

@pytest.fixture(scope='class')
def browser():
    with sync_playwright() as p:
        browser = p.chromium.launch(headless=False)
        request.cls.browser = browser
        yield browser
        browser.close()

@pytest.fixture(scope='function')
def page(browser):
    page = browser.new_page()
    yield page
    page.close()

@pytest.mark.usefixtures("browser")
class test_title:
    def test_title(self, page):
        page.goto("https://www.google.com")
        assert page.title() == "Google"
```

## Test Data Validation
```
import pytest
from playwright.sync_api import sync_playwright

@pytest.fixture(scope='module')
def browser_handle():
    with sync_playwright() as p:
        browser = p.chromium.launch(headless=False)
        yield browser
        browser.close()

@pytest.fixture(scope='function')
def page_handle(browser):
    page = browser.new_page()
    yield page
    page.close()

@pytest.mark.parametrize("invalid_username, invalid_password", [("Adminn", "admin1233"), ("Admin", "admin1234")])
def test_invalid_login(page_handle, invalid_username, invalid_password):
    page_handle.goto("https://opensource-demo.orangehrmlive.com/web/index.php/auth/login")
    page_handle.wait_for_selector("//input[@name='username']").type(invalid_username)
    page_handle.wait_for_selector("//input[@name='password']").type(invalid_password)
    page_handle.wait_for_selector("//button[@type='submit']").click()
    page_handle.wait_for_timeout(2000)
    error_message = page_handle.wait_for_selector("//div[@role='alert']//p").text_content()
    assert "Invalid credentials" == error_message

```

## HTML Report Generation
```
pip install pytest-html
pytest --html=my_report.html
```

