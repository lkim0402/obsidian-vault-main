- venv 생성
	- [[Virtual environment using pip and venv]]
	- `pip install playwright`, `playwright install`
- waiting for a while
	- `await page.waitForTimeout(5000)`
	- but if u want to halt test to inspect the DOM or network, you can use `await page.pause()` instead -> this stops execution entirely and opens the Playwright Inspector, keeping browser open indefinitely until u manually resume it

```python
import asyncio
import re
from playwright.async_api import async_playwright


async def login_saramin(username: str, password: str) -> bool:
    async with async_playwright() as p:
        browser = await p.chromium.launch(headless=False)
        page = await browser.new_page()

        await page.goto("https://www.saramin.co.kr/zf_user/auth")

        await page.click("button.btn_tab.t_com")
        await page.fill("#id", username)
        await page.fill("#password", password)
        await page.click(".btn_login")

        await page.wait_for_load_state("networkidle")

        # is_logged_in = await page.locator(".btn_logout").is_visible()

        # if is_logged_in:
        #     print("Login successful")
        #     user_name = await page.locator(".user_name").inner_text()
        #     print(f"Logged in as: {user_name}")
        # else:
        #     print("Login failed")
        #     # await browser.close()
        #     return is_logged_in

        # popup remove
        popup_close_selectors = [
            ".btn_close",
            ".close_btn",
            ".popup_close",
            ".layer_close",
            "button[class*='close']",
            "a[class*='close']",
            "button:has-text('취소')"
        ]
        closed_count = 0
        while True:
            closed_any = False
            for selector in popup_close_selectors:
                close_btns = await page.locator(selector).all()
                for close_btn in close_btns:
                    if await close_btn.is_visible():
                        await close_btn.click()
                        await page.wait_for_timeout(500)
                        closed_count += 1
                        closed_any = True
                        break
                if closed_any:
                    break
            if not closed_any:
                break
        print(f"Closed {closed_count} popup(s)")

        # # 진행중 공고
        # await page.get_by_text("진행중 공고", exact=True).click()
        # await page.wait_for_load_state("networkidle")
        # print("Navigated to 진행중 공고")

        # # 미열람 buttons and extract their counts
        # unread_buttons = await page.locator("text=미열람").all()
        # print(f"Found {len(unread_buttons)} 미열람 elements")

        # # 3. Iterate and process only those with count >= 1
        # for btn in unread_buttons:
        #     text = await btn.inner_text()
        #     match = re.search(r"\d+", text)
        #     if not match:
        #         continue
        #     count = int(match.group())
        #     if count < 1:
        #         continue

        #     print(f"Processing 미열람 button with count {count}: '{text.strip()}'")

        #     # 4. Click to trigger authentication UI
        #     await btn.click()
        #     await page.wait_for_load_state("networkidle")
        #     print("Authentication UI triggered")

        # await browser.close()
        return True


if __name__ == "__main__":
    asyncio.run(login_saramin("findaforyou", "finda1005!"))

```

```
<button class="btn_close" aria-label="Close" onclick="closeMessage();">×</button>

<button type="button" class="BtnClose btnClose btn_layer_close"><span class="blind">닫기</span></button>
```