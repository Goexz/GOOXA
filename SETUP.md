# ใส่แดชบอร์ด Metrics ในหน้าโปรไฟล์ GitHub

ชุดนี้ใช้ GitHub Metrics ของ `lowlighter/metrics` แบบเดียวกับระบบในภาพอ้างอิง ตั้งค่าไว้สำหรับบัญชี **Goexz**

เมื่อ workflow ทำงาน จะดึงข้อมูลจริงจาก GitHub มาสร้างสถิติ repository, contribution, ปฏิทินสามมิติ, ภาษาที่ใช้ และกราฟกิจกรรมตามวันกับชั่วโมง ตั้งเวลาอัปเดตทุกวันเวลาเที่ยงคืนไทย โดย GitHub อาจเริ่ม scheduled run ช้ากว่าเวลาที่ตั้งไว้

## 1. ใส่ไฟล์ใน repository โปรไฟล์

ใช้ public repository **`Goexz/Goexz`** เพื่อให้ README แสดงบนหน้าโปรไฟล์

อัปโหลดไฟล์จาก ZIP โดยคงตำแหน่งดังนี้:

| ไฟล์ | ตำแหน่ง |
| --- | --- |
| `README.md` | root ของ repository |
| `github-metrics.svg` | root ของ repository |
| `SETUP.md` | root ของ repository |
| `assets/languages.svg` | ภายในโฟลเดอร์ `assets` |
| `.github/workflows/metrics.yml` | ภายใน `.github/workflows` |

ถ้ามี README เดิม ให้ใช้ README ของชุดนี้แทน ส่วนไฟล์อื่นของคุณเก็บไว้ได้

อย่าลืมโฟลเดอร์ **`.github`** เพราะเป็นที่เก็บ workflow หากอัปโหลดผ่านหน้าเว็บแล้วหาโฟลเดอร์นี้ไม่พบ ให้ใช้ **Add file → Create new file** และตั้งชื่อไฟล์เป็น `.github/workflows/metrics.yml` แล้วคัดลอกเนื้อหา YAML ไปใส่

## 2. เพิ่ม token ครั้งเดียว

GitHub Metrics ใช้ Personal Access Token อ่านข้อมูลบัญชี ส่วนการ commit ภาพกลับเข้า repository ใช้ `GITHUB_TOKEN` ที่ GitHub Actions สร้างให้อัตโนมัติ

1. ในบัญชี GitHub เปิด **Settings → Developer settings → Personal access tokens → Tokens (classic)**
2. สร้าง token สำหรับ Metrics และตั้งอายุการใช้งานตามที่ต้องการ ชุดนี้ใช้ข้อมูลสาธารณะ จึงเริ่มได้โดยไม่เลือก scope เพิ่มตามเอกสาร Metrics
3. ใน repository `Goexz/Goexz` เปิด **Settings → Secrets and variables → Actions → New repository secret**
4. ตั้งชื่อ secret เป็น **`METRICS_TOKEN`** แล้วใส่ token ในช่อง Secret

เก็บ token ในช่อง secret ของ GitHub ส่วน YAML ใช้ `${{ secrets.METRICS_TOKEN }}` ตามที่เตรียมไว้แล้ว

## 3. สร้างสถิติครั้งแรก

1. เปิดแท็บ **Actions** ของ repository
2. เลือก workflow **Update GitHub Metrics**
3. กด **Run workflow** บน default branch แล้วรอจนเสร็จ
4. Workflow จะ commit ภาพ `github-metrics.svg` ใหม่ให้ เปิดหน้าโปรไฟล์เพื่อดูผล

ก่อน run ครั้งแรก `github-metrics.svg` จะแสดงการ์ดแนะนำการตั้งค่า เมื่อ run สำเร็จจะเปลี่ยนเป็นแดชบอร์ดจากข้อมูลจริงของ Goexz ตัวเลขและกราฟขึ้นอยู่กับข้อมูลที่มีอยู่ในบัญชี

## ปรับแต่ง

| ต้องการเปลี่ยน | แก้ตรงไหน |
| --- | --- |
| ข้อความ ลิงก์ และผลงาน | `README.md` |
| สีหัวข้อ Metrics | `extras_css` ใน workflow |
| เวลาอัปเดต | `schedule.cron` ใช้เวลา UTC |
| ปฏิทินครึ่งปี | เปลี่ยน `plugin_isocalendar_duration` เป็น `half-year` |
| จำนวนภาษาที่แสดง | `plugin_languages_limit` สูงสุด 8 |
| ช่วงกิจกรรมที่นำมาวิเคราะห์ | `plugin_habits_days` |

ถ้า workflow แจ้งว่าไม่มี `METRICS_TOKEN` ให้เพิ่ม secret แล้วกด run ใหม่ ถ้า token หมดอายุให้เปลี่ยนค่า secret จากนั้น run อีกครั้ง ดูรายละเอียดข้อผิดพลาดได้ใน Actions

เอกสารอ้างอิง:

- [GitHub — Profile README](https://docs.github.com/en/account-and-profile/how-tos/profile-customization/managing-your-profile-readme)
- [GitHub Metrics — Action setup](https://github.com/lowlighter/metrics/blob/master/.github/readme/partials/documentation/setup/action.md)
- [Metrics — Core configuration](https://github.com/lowlighter/metrics/blob/master/source/plugins/core/README.md)
- [Metrics — Isometric calendar](https://github.com/lowlighter/metrics/blob/master/source/plugins/isocalendar/README.md)
- [Metrics — Languages](https://github.com/lowlighter/metrics/blob/master/source/plugins/languages/README.md)
- [Metrics — Coding habits](https://github.com/lowlighter/metrics/blob/master/source/plugins/habits/README.md)
