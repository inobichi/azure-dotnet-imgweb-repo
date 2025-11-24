# เอกสารภาพรวมโปรเจค (ภาษาไทย)

เอกสารนี้อธิบายภาพรวมและรายละเอียดของโปรเจคใน repository นี้เป็นภาษาไทย พร้อมคำสั่งสำหรับ build, test และรันทดสอบแอพ

**สรุปสั้นๆ:**
- **ชื่อโปรเจค:** ตัวอย่างแอพ .NET (Web)
- **เทคโนโลยีหลัก:** .NET 8 (target framework `net8.0`), Razor pages
- **โครงสร้างหลัก:** มีไฟล์ `Web.sln`, โปรเจค `Web.csproj` และโฟลเดอร์ `tests/Calculator.Tests`

**โครงสร้างไฟล์ที่สำคัญ:**
- `Web.sln` : Solution file ของโปรเจค
- `Web.csproj` : โปรเจคเว็บหลัก (Razor pages)
- `Pages/Index.cshtml` และ `Pages/Index.cshtml.cs` : หน้าเริ่มต้นของเว็บ (อย่าแปลคำว่า `cshtml`)
- `Calculator.cs`, `Options.cs`, `Program.cs`, `Appsettings.json` : โค้ดและการตั้งค่าระบบ
- `tests/Calculator.Tests/CalculatorTests.cs` : ชุดทดสอบ unit tests
- โฟลเดอร์ `bin/` และ `obj/` : ไบนารีและไฟล์ build artifacts (ไม่ควรแก้ไขโดยตรง)

**การติดตั้งและความต้องการเบื้องต้น:**
- ติดตั้ง .NET SDK (แนะนำ .NET 8) บนเครื่อง
- ตรวจสอบเวอร์ชันด้วย:

```
dotnet --version
dotnet --list-sdks
```

ถ้าต้องการใช้ container หรือ CI ให้แน่ใจว่า image มี .NET 8 SDK

**คำสั่งทั่วไปสำหรับการพัฒนา**

- สร้าง (build) ทั้ง solution:

```
dotnet build Web.sln -c Debug
```

- สร้างแบบ release:

```
dotnet build Web.sln -c Release
```

- รันทดสอบ (run unit tests):

```
dotnet test
```

หรือรันทดสอบเฉพาะโปรเจคทดสอบ:

```
dotnet test tests/Calculator.Tests/Calculator.Tests.csproj
```

- รันเว็บแอพในโหมดพัฒนา (จาก repo root):

```
dotnet run --project ./Web.csproj
```

หลังคำสั่งนี้ แอพจะเริ่มฟังที่ `http://localhost:5000` หรือพอร์ตที่ระบบกำหนดขึ้นอยู่กับ `launchSettings`/`ASPNETCORE_URLS` และ environment

- สร้าง publish artifacts สำหรับ deployment:

```
dotnet publish Web.sln -c Release -o ./publish

# รันไฟล์ที่ publish แล้ว (ตัวอย่าง):
dotnet ./publish/Web.dll
```

**การรันทดสอบแอพ (manual checks):**
- รันคำสั่ง `dotnet run --project ./Web.csproj` แล้วเปิดเบราว์เซอร์ไปที่ `http://localhost:5000` หรือ URL ที่แสดงในเทอร์มินัล
- ตรวจสอบหน้า `Pages/Index.cshtml` เพื่อดูข้อความหรือฟังก์ชันตัวอย่าง

**คำแนะนำสำหรับการพัฒนาเพิ่มเติม:**
- ถ้าจะแยกโค้ดเพิ่ม ควรสร้างโฟลเดอร์ `src/` (ถ้าต้องการ) แล้วย้ายโปรเจคย่อยเข้ามา แต่ในสถานะปัจจุบันโปรเจคอยู่ที่ root
- หลีกเลี่ยงการ commit ไฟล์ใน `bin/` หรือ `obj/` — ให้ใส่ไว้ใน `.gitignore`

**การรันในสภาพแวดล้อม CI/CD (ตัวอย่าง):**
- ขั้นตอนทั่วไปใน pipeline:
  - ติดตั้ง .NET SDK (8.x)
  - `dotnet restore`
  - `dotnet build -c Release`
  - `dotnet test`
  - `dotnet publish -c Release -o publish`

**ปัญหาที่อาจพบและการแก้ไขเบื้องต้น:**
- ถ้า `dotnet test` ล้มเหลว: ตรวจสอบข้อความ error ในเทอร์มินัลและแก้โค้ด/fixture ตามที่ระบุ
- ถ้า `dotnet run` แจ้งพอร์ตถูกใช้งาน: ตั้งตัวแปร `ASPNETCORE_URLS` หรือระบุ `--urls` เวลา `dotnet run`

```
# ตัวอย่าง: รันที่พอร์ต 5001
ASPNETCORE_URLS="http://localhost:5001" dotnet run --project ./Web.csproj
# หรือ
dotnet run --project ./Web.csproj --urls "http://localhost:5001"
```

**ไฟล์ทดสอบ (ตำแหน่ง):**
- `tests/Calculator.Tests/Calculator.Tests.csproj` : โปรเจคทดสอบ
- `tests/Calculator.Tests/CalculatorTests.cs` : โค้ดทดสอบ

---
ถ้าต้องการ ผมสามารถ: commit ไฟล์นี้ให้, รัน `dotnet test` บนเครื่อง dev container นี้ (ถ้าต้องการ), หรือแปลงเป็นภาษาอังกฤษ/เพิ่มตัวอย่างโค้ดได้ แจ้งมาครับว่าต้องการขั้นตอนถัดไปแบบไหน

---
<div align="left">
Copyright © inobichi
</div>
