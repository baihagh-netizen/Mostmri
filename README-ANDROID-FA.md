# محاسبه‌گر مستمری — نسخه بازنویسی‌شده بر اساس mostam.xlsx

## ویژگی‌ها
- رابط فارسی و راست‌چین؛ ورودی تاریخ با قالب شمسی `YYYY/MM/DD`.
- محاسبه آفلاین و ذخیره محلی پرونده‌ها، بازیابی آخرین پرونده، ویرایش/حذف پرونده‌ها.
- چاپ گزارش و ذخیره PDF از پنجره چاپ اندروید.
- خروجی Excel-compatible `.xls` (SpreadsheetML) و پشتیبان/بازیابی JSON.
- تست خودکار تطبیق خروجی نمونه اکسل قبل از build.

## نکته حیاتی درباره تطبیق
فایل اکسل مرجع بعضی ورودی‌ها را به‌صورت عدد ثابت نگه می‌دارد نه فرمول تاریخ‌محور: B3=845 روز، D4=36.3 سال، B6=25، B7=3، B8=25.3، B16=14.3 و B23=13.32. به همین علت این مقادیر به‌عنوان فیلدهای ورودی مستقل حفظ شده‌اند؛ تغییر تاریخ به‌تنهایی این مقادیر را تغییر نمی‌دهد، چون چنین رابطه‌ای در خود اکسل وجود ندارد. درصد افزایش سال آینده در C4 برابر 20+12.5 است اما در فرمول C5/C6 استفاده نشده؛ برای تطبیق عین اکسل در محاسبه حقوق نیز وارد نشده است.

فرمول‌های پیاده‌شده:
- PromotionPercent = ElapsedDays × PromotionRate ÷ RequiredDays
- SalaryPriorMonths = Base1405 × (MonthsLastYear − 1) × 1.325
- SalaryLastMonth = (Base1405 + Base1405 × PromotionPercent/100) × 1.325
- Total24 = LastYearTotal + Base1405 × 12 + Base1404 × 9
- Average24 = Total24 / 24
- FinalAverage = Average24 × (CompanyYears + PurchasedYears) / 30
- Attraction = AttractionYears × (8 × 1.3) / 30
- Position = PositionYears × (SelectedPositionRate × 1.3) / 30
- Degree = DegreeYears × (SelectedDegreeRate × 1.3) / 30
- TotalPercentage = Special + Workshop + Attraction + Position + Degree
- Pension = FinalAverage × (1 + TotalPercentage/100)

## مقادیر اعتبارسنجی از اکسل
- ترفیع: 3.858447488584475
- میانگین ۲۴ ماه: 40254268.78805175
- میانگین نهایی: 51391283.15274606
- جمع درصدها: 101.9762
- مستمری: 103798160.8431567

## ساخت APK با GitHub Actions
1. یک repository جدید در GitHub بسازید.
2. تمام فایل‌های داخل این پوشه را در ریشه repository آپلود کنید؛ پوشه مخفی `.github/workflows` باید حفظ شود.
3. از تب Actions، workflow «Build Android APK» را باز و Run workflow را اجرا کنید.
4. پس از سبز شدن workflow، پایین صفحه اجرای workflow در بخش Artifacts، فایل `pension-calculator-debug-apk` را دانلود کنید.
5. ZIP را باز کنید و `app-debug.apk` را روی گوشی نصب کنید. در صورت درخواست Android، نصب از این منبع را مجاز کنید.

این build از نوع debug است و برای نصب شخصی است؛ برای انتشار عمومی باید نسخه release امضا شود.
