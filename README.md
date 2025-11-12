# alltestE2E

This workspace contains Playwright end-to-end (E2E) test suites for multiple projects stored in separate subfolders, to make running and viewing results easier. This README summarizes the structure, installation instructions, and common commands.

## Project structure (summary)

- `fitrecipes-test-recipeDetail/` - A small E2E test project (contains `tests/basic.spec.ts`) that uses Playwright as a devDependency.
- `test_authNbrowse/` - The main project for testing authentication (auth) and the browse page (contains `tests/auth.e2e.spec.ts` and `tests/browse.e2e.spec.ts`). Its `package.json` includes scripts to run Playwright.
- `test-results/` - Folder to store test results (if present).

## Prerequisites

- Node.js (recommended LTS, e.g. 18.x or 20.x)
- npm or yarn
- If you want to run Playwright browsers locally, you may need to install browser binaries using the Playwright install command (see below)

Note: Some projects already include `@playwright/test` in `devDependencies`, but may not have locked versions or additional scripts.

## Example installation (per-project)

Each project has its own `package.json`. Install dependencies inside each project folder. Example (using zsh):

```bash
# Install dependencies for test_authNbrowse
cd test_authNbrowse
npm install

# Install Playwright browsers (if you want to run on local machine)
npx playwright install

# Return to the repository root or as needed
cd ..

# Install dependencies for fitrecipes-test-recipeDetail
cd fitrecipes-test-recipeDetail
npm install

# (Optional) Install Playwright browsers
npx playwright install
```

If you prefer `yarn`, replace `npm install` with `yarn`.

## Required environment variables

The `test_authNbrowse` project uses the following environment variables (set in a `.env` file or in your shell session):

- `TEST_EMAIL` - the test user's email
- `TEST_PASSWORD` - the test user's password
- (optional) `BASE_URL` - if you want to set the application's base URL; can also be set in a Playwright config or passed via env/CLI

Example `.env` file (place inside `test_authNbrowse/`):

```env
TEST_EMAIL=your-test-email@example.com
TEST_PASSWORD=your-test-password
# BASE_URL=http://localhost:3000/
```

Note: `auth.e2e.spec.ts` and `browse.e2e.spec.ts` expect `baseURL` to be configured in the project's `playwright.config.ts` or provided via Playwright environment setup.

## Important scripts

- In `test_authNbrowse/package.json` the following scripts are available:
  - `npm test` or `npm run test` — runs `playwright test`
  - `npm run test:headed` — runs `playwright test --headed` (shows the browser)
  - `npm run test:ui` — runs `playwright test --ui` (opens the Playwright Test Runner UI)
  - `npm run report` — shows the Playwright-generated report (`playwright show-report`)

- In `fitrecipes-test-recipeDetail/package.json` the `test` script is currently a placeholder (it errors). Consider changing it to run `playwright test`, similar to the other project.

Example runs (from `test_authNbrowse` folder):

```bash
cd test_authNbrowse
# Run all tests (headless)
npm test

# Run with visible browser
npm run test:headed

# Open Playwright Test Runner UI
npm run test:ui

# Show report after a run
npm run report
```

## Short description of tests in `test_authNbrowse/tests`

- `auth.e2e.spec.ts`
  - Verifies the auth (login) page loads
  - Tests navigation to the signup page
  - Checks for a Google login button (if present) without clicking it to avoid external OAuth popups
  - Tests invalid login behavior and successful login using `TEST_EMAIL`/`TEST_PASSWORD`
  - Tests logout behavior

- `browse.e2e.spec.ts`
  - Logs in using the helper `loginAndWait` and verifies the login response
  - Checks for a trending section (if present)
  - Verifies filter and sort controls (soft assertions — if missing, recorded as notes)
  - Opens the first recipe detail (if available) and checks title, ingredients, and instructions

Both test files are written to be tolerant of UI differences across staging/builds — they use soft assertions and `test.info().annotations` to record notes when certain elements are absent.

## Suggested minor improvements

- In `fitrecipes-test-recipeDetail/package.json`, change the `test` script to `playwright test` so tests can run:

```json
{
  "scripts": {
    "test": "playwright test"
  }
}
```

- Add a `README.md` to each project folder (optional) to describe environment variables, configuration, and example run commands
- Consider adding a `playwright.config.ts` in each project if you need a specific `baseURL`, timeouts, or other config

## Debugging and running single tests

- Run a single test file:

```bash
cd test_authNbrowse
npx playwright test tests/auth.e2e.spec.ts
```

- Run a single test in headed mode with devtools:

```bash
npx playwright test tests/auth.e2e.spec.ts --headed --debug
```

## Viewing test results

- Playwright will create a `playwright-report` folder (or the folder defined in config) after runs. Use `npm run report` to open the report UI.
- If you want to store results under `test-results/`, configure Playwright to output results to that folder.

## Summary

This workspace contains Playwright E2E test projects. `test_authNbrowse` can be run immediately after installing dependencies and requires `TEST_EMAIL`/`TEST_PASSWORD` to be set. `fitrecipes-test-recipeDetail` needs a small `package.json` update to run tests with `npm test`.

If you want, I can:

- Add example `playwright.config.ts` files for each project
- Update `fitrecipes-test-recipeDetail/package.json` to include a `test` script
- Create `.env.example` or per-project README files

Tell me which of these you'd like me to do next.
# alltestE2E

พื้นที่ทำงานนี้ประกอบด้วยชุดทดสอบ Playwright สำหรับหลายโปรเจกต์ (E2E tests) ที่เก็บไว้แยกกันในโฟลเดอร์ย่อย เพื่อให้ง่ายต่อการรันและดูผลลัพธ์ README นี้จะสรุปโครงสร้าง วิธีติดตั้ง และคำสั่งที่ใช้บ่อยเป็นภาษาไทย

## โครงสร้างโปรเจกต์ (สรุป)

- `fitrecipes-test-recipeDetail/` - โปรเจกต์ทดสอบ E2E ขนาดเล็ก (มี `tests/basic.spec.ts`) ใช้ Playwright เป็น devDependency
- `test_authNbrowse/` - โปรเจกต์หลักสำหรับทดสอบการล็อกอิน (auth) และหน้า browse (มี `tests/auth.e2e.spec.ts` และ `tests/browse.e2e.spec.ts`) และสคริปต์ใน `package.json` สำหรับรัน Playwright
- `test-results/` - โฟลเดอร์สำหรับเก็บผลลัพธ์ทดสอบ (ถ้ามี)

## สิ่งที่ต้องมี (Prerequisites)

- Node.js (แนะนำเวอร์ชัน LTS เช่น 18.x หรือ 20.x)
- npm หรือ yarn
- ถ้าต้องการรัน Playwright browsers, อาจต้องติดตั้ง browser binaries ผ่านคำสั่ง Playwright (รายละเอียดด้านล่าง)

หมายเหตุ: โปรเจกต์บางอันมี `devDependencies` ที่รวม `@playwright/test` อยู่แล้ว แต่ยังไม่มีการล็อกเวอร์ชันหรือสคริปต์เพิ่มเติมอื่น ๆ

## ตัวอย่างการติดตั้ง (แยกโฟลเดอร์)

แต่ละโปรเจกต์มี `package.json` ของตัวเอง ให้รันคำสั่งติดตั้งแยกกันภายในแต่ละโฟลเดอร์ ตัวอย่าง (ใช้ `zsh`):

```bash
# ติดตั้ง dependencies สำหรับ test_authNbrowse
cd test_authNbrowse
npm install

# ติดตั้ง browsers ของ Playwright (ถ้าต้องการรันจริงบนเครื่อง)
npx playwright install

# กลับไปยังโฟลเดอร์รากหรือตามต้องการ
cd ..

# ติดตั้ง dependencies สำหรับ fitrecipes-test-recipeDetail
cd fitrecipes-test-recipeDetail
npm install

# (ออปชัน) ติดตั้ง Playwright browsers
npx playwright install
```

ถ้าต้องการใช้ `yarn` ให้แทน `npm install` ด้วย `yarn` ตามที่สะดวก

## ตัวแปรสภาพแวดล้อมที่จำเป็น

โปรเจกต์ `test_authNbrowse` ใช้ตัวแปร env ต่อไปนี้ (ในไฟล์ `.env` หรือเซสชันเชลล์):

- `TEST_EMAIL` - อีเมลผู้ใช้ทดสอบ
- `TEST_PASSWORD` - รหัสผ่านของผู้ใช้ทดสอบ
- (ตัวเลือก) `BASE_URL` - ถ้าต้องการกำหนด baseURL ของแอป ให้ตั้งใน Playwright config หรือส่งเป็น `--config`/env ตามต้องการ

ตัวอย่างไฟล์ `.env` (วางใน `test_authNbrowse/`):

```env
TEST_EMAIL=your-test-email@example.com
TEST_PASSWORD=your-test-password
# BASE_URL=http://localhost:3000/
```

หมายเหตุ: ไฟล์ `auth.e2e.spec.ts` และ `browse.e2e.spec.ts` คาดหวังว่า `baseURL` จะตั้งไว้ใน `playwright.config.ts` ของโปรเจกต์หรือสภาพแวดล้อมของ Playwright

## สคริปต์ที่สำคัญ

- ใน `test_authNbrowse/package.json` มีสคริปต์ต่อไปนี้:
  - `npm test` หรือ `npm run test` — รัน `playwright test`
  - `npm run test:headed` — รัน `playwright test --headed` (เห็นหน้าเบราว์เซอร์)
  - `npm run test:ui` — รัน `playwright test --ui` (เปิด Playwright Test Runner UI)
  - `npm run report` — แสดงรายงานที่สร้างโดย Playwright (`playwright show-report`)

- ใน `fitrecipes-test-recipeDetail/package.json` ปัจจุบันมีเพียงสคริปต์ `test` ที่เป็น placeholder (ออก error) — คุณอาจปรับให้รัน `playwright test` เช่นเดียวกับอีกโปรเจกต์

ตัวอย่างการรัน (จากโฟลเดอร์ `test_authNbrowse`):

```bash
cd test_authNbrowse
# รันทุกเทส (HEADLESS)
npm test

# รันแบบมี UI/เห็นเบราว์เซอร์
npm run test:headed

# เปิด Playwright Test Runner UI
npm run test:ui

# แสดงรายงานหลังจากรันแล้ว
npm run report
```

## คำอธิบายสั้น ๆ ของเทสใน `test_authNbrowse/tests`

- `auth.e2e.spec.ts`
  - ตรวจสอบหน้า auth (login) ว่าโหลดได้
  - ตรวจสอบ navigation ไปหน้า signup
  - ตรวจสอบปุ่มล็อกอินด้วย Google (ถ้ามี) โดยไม่คลิกเพื่อหลีกเลี่ยง OAuth popup
  - ตรวจสอบกรณีล็อกอินไม่สำเร็จและกรณีล็อกอินสำเร็จ (ใช้ `TEST_EMAIL`/`TEST_PASSWORD`)
  - ตรวจสอบการ logout

- `browse.e2e.spec.ts`
  - ทำการล็อกอินผ่าน helper `loginAndWait` และตรวจสอบการตอบกลับ
  - ตรวจสอบการแสดงผลส่วน trending (ถ้ามี)
  - ตรวจสอบว่ามี controls สำหรับ filter และ sort (เป็น soft assertions — ถ้าไม่มีจะเป็น note)
  - เปิดหน้า recipe detail แรกที่มี (ถ้ามี) และตรวจสอบ title, ส่วน ingredients, และ instructions

ทั้งสองไฟล์ออกแบบให้ทนต่อความแตกต่างของ UI ใน staging/หลาย build — มีการใช้ soft assertions และ `test.info().annotations` เพื่อบันทึกข้อสังเกตเมื่อ element บางตัวหายไป

## การปรับปรุงที่แนะนำ (เล็กน้อย)

- ใน `fitrecipes-test-recipeDetail/package.json` ให้เปลี่ยนสคริปต์ `test` เป็น `playwright test` เพื่อรันเทสจริงได้:

```json
{
  "scripts": {
    "test": "playwright test"
  }
}
```

- เพิ่ม `README.md` ย่อยในแต่ละโฟลเดอร์โปรเจกต์ (optional) อธิบาย env, config, และตัวอย่างรัน
- พิจารณาเพิ่ม `playwright.config.ts` ในแต่ละโปรเจกต์ถ้าใช้ baseURL หรือ timeout เฉพาะ

## การดีบักและการรันแบบแยก

- รันเทสเฉพาะไฟล์:

```bash
cd test_authNbrowse
npx playwright test tests/auth.e2e.spec.ts
```

- รันเทสเดียวในโหมด headed และเปิด devtools:

```bash
npx playwright test tests/auth.e2e.spec.ts --headed --debug
```

## ดูผลลัพธ์ทดสอบ

- Playwright จะสร้างโฟลเดอร์ `playwright-report` หรือที่กำหนดใน config เมื่อรันสำเร็จ ใช้ `npm run report` เพื่อเปิด UI ของรายงาน
- ถ้าต้องการเก็บผลลัพธ์ใน `test-results/` ให้ปรับ Playwright config เพื่อ output ไปยังโฟลเดอร์นั้น

## สรุปสั้น ๆ

โฟลเดอร์ใน workspace นี้เป็นชุดทดสอบ E2E ที่ใช้ Playwright: โปรเจกต์ `test_authNbrowse` สามารถรันได้ทันทีหลังติดตั้ง dependencies และต้องตั้ง env `TEST_EMAIL`/`TEST_PASSWORD` ในขณะที่ `fitrecipes-test-recipeDetail` ยังต้องแก้ `package.json` เล็กน้อยหากต้องการรันด้วย `npm test` เหมือนกัน

ถ้าต้องการ ผมสามารถ:

- เพิ่ม `playwright.config.ts` ตัวอย่างสำหรับแต่ละโปรเจกต์
- ปรับ `package.json` ของ `fitrecipes-test-recipeDetail` ให้มีสคริปต์ `test`
- สร้าง `.env.example` หรือ README ย่อยในแต่ละโปรเจกต์

บอกผมว่าต้องการให้ผมทำข้อใดต่อได้เลย
# alltestE2E
