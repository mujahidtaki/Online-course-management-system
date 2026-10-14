# CourseHub manual test checklist

Run these in order after `dotnet build` succeeds.

## 1. Admin

Login:
- Email: `admin@coursehub.com`
- Password: `admin123`

Check:
1. Admin dashboard opens.
2. Courses page lists courses and assigned teachers.
3. Create a draft course with no teacher.
4. Try publishing it without a teacher — it must be rejected.
5. Assign a teacher.
6. Upload a JPG/PNG/WEBP course image.
7. Save and verify the image appears in course cards.
8. Open Enrollments.
9. Open Payments.
10. Open Notifications.

## 2. Teacher

Login:
- Email: `sarah@coursehub.com`
- Password: `teacher123`

Check:
1. My Courses only contains assigned courses.
2. My Students opens.
3. Edit Course opens only for assigned courses.
4. Manage content opens.
5. Create Week 1.
6. Create Weeks 2–5.
7. Attempt Week 6 — it must be rejected.
8. Upload a PDF.
9. Upload an MP4/WebM.
10. Delete a material.
11. Delete a module.
12. Open Notifications.

## 3. Student

Login:
- Email: `alice@student.com`
- Password: `student123`

Check:
1. Browse Courses opens.
2. Course images appear.
3. Enroll in a course.
4. My Payments shows a pending payment.
5. In Development mode, use Local test.
6. Payment becomes Completed.
7. Enrollment becomes Active.
8. My Learning now contains the course.
9. Open Continue Learning.
10. Week 1–5 appear according to course duration.
11. Open the PDF.
12. Watch the video.
13. Open Notifications.

## 4. Enrollment notifications

After a new enrollment:
- Admin receives `New enrollment pending`.
- Assigned teacher receives `New enrollment pending`.

After verified payment:
- Admin receives `New student enrolled`.
- Assigned teacher receives `New student enrolled`.

## 5. Security

Try changing URLs/IDs manually:
- Teacher must not edit another teacher's course.
- Teacher must not manage another teacher's modules.
- Student must not open another course's material ID.
- Unpaid/pending students must not access My Learning.
- Direct `/App_Data/course-materials/...` files must not be publicly accessible.

## 6. SSLCommerz sandbox

Do this only after the local workflow passes.

Set:

```json
"SslCommerz": {
  "StoreId": "YOUR_SANDBOX_STORE_ID",
  "StorePassword": "YOUR_SANDBOX_STORE_PASSWORD",
  "UseSandbox": true,
  "CallbackBaseUrl": "https://YOUR-PUBLIC-HTTPS-HOST"
}
```

Do not put production credentials into source control.

A public HTTPS callback is required for SSLCommerz callbacks/IPN.


## Revenue Split / Progress / Certificate

- [ ] Complete a BDT 1,000 payment: Platform Revenue = BDT 200.00.
- [ ] Complete a BDT 1,000 payment: Teacher Revenue = BDT 800.00.
- [ ] Admin Platform Revenue includes only completed payments.
- [ ] Teacher My Revenue contains only that teacher's completed payments.
- [ ] A different teacher cannot see another teacher's revenue.
- [ ] Student My Learning starts at 0% when no material is completed.
- [ ] Marking a material complete updates the green progress bar.
- [ ] Progress survives logout/login.
- [ ] At 100%, enrollment becomes Completed.
- [ ] Certificate is generated once with a unique CourseHub certificate ID.
- [ ] Certificate PDF downloads successfully.
- [ ] Reopening the completed course does not create a second certificate.
