# CourseHub – Implemented Features

This version adds the requested role-based learning workflow.

## Admin

- Manage courses.
- Assign an active teacher to a course.
- Published courses require an assigned teacher.
- Upload course images (JPG/JPEG/PNG/WEBP, max 10 MB).
- Course images are shown on homepage, browse courses and learning cards.
- View enrollments and payment records.
- Receive enrollment notifications after verified payment.
- View and clear notification read state by opening the notification page.

## Teacher

- My Courses only shows courses assigned to the logged-in teacher.
- My Students lists enrolled students grouped by course.
- Edit assigned course details.
- Create weekly modules from Week 1 through the course duration, with a hard maximum of Week 5.
- Upload PDF, MP4 and WebM course materials.
- Maximum material upload size is 250 MB; multipart request parsing is configured for this limit.
- Delete modules and materials.
- Receive notifications when a new student's payment is verified.

## Student

- My Learning contains active paid/enabled courses.
- Each course displays Week 1 through Week 5 as applicable to the course duration.
- Reading PDFs and class videos are served through an authorized controller action.
- Course files are stored outside `wwwroot` so direct public URLs cannot bypass enrollment checks.
- My Payments uses the SSLCommerz gateway abstraction already present in CourseHub.
- Development mode includes a local payment simulation for testing the complete enrollment workflow without a public callback URL.
- Student notifications remain available.

## SSLCommerz

- Hosted checkout initiation.
- Success/failure/cancel callback URLs.
- IPN endpoint with signature verification.
- Server-side validation by validation ID or transaction ID.
- Amount, currency and transaction ID matching.
- Risk level handling.
- Duplicate completion protection.
- Student, teacher and admin notifications after a verified completed payment.

## Database compatibility

The project uses `DatabaseSchemaInitializer` at startup so the new course module/material tables and `Course.InstructorId` are added to an existing CourseHub MariaDB database without requiring a manually created migration.

The initializer also assigns existing teacher-created courses to their teacher and limits legacy course durations above five weeks to five weeks.

## Local run

1. Start Apache and MySQL in XAMPP.
2. Confirm the connection string in `appsettings.json`.
3. Open the `CourseHub` folder in Visual Studio.
4. Build the solution.
5. Run with the `http://localhost:5000` profile.
6. Use the seeded accounts from `DatabaseSeeder.cs`.

Real SSLCommerz callbacks require a public HTTPS `SslCommerz:CallbackBaseUrl`. Use the development Local test button to verify the full enrollment/notification/learning flow before configuring a public sandbox callback.

## Upload reliability update

- Course image uploads are persisted under `wwwroot/uploads/courses` and are immediately available to static course cards after save.
- Teacher PDF/video uploads are persisted under `App_Data/course-materials` and saved to `CourseMaterials` in the database before redirecting.
- Failed material database saves remove the newly written file so the filesystem and database do not drift.
- Student course cards open the weekly learning page, which reloads modules and materials from MariaDB.

## Admin teacher management

- Admins can open `Manage Users` and choose `Add Teacher`.
- Teacher creation collects full name, unique email, password, confirmation and optional phone.
- Teacher passwords are stored as BCrypt hashes; the plaintext password is never stored.
- Public registration remains Student-only, so visitors cannot self-promote to Teacher.
- A newly created teacher is active immediately and can sign in without a database edit.
- The teacher automatically appears in the Admin course-assignment dropdown.
- The teacher dashboard exposes My Courses, My Students, Manage Content, Week 1–5 modules, PDF/video uploads and Notifications.
- A welcome notification is created for the new teacher.


## Revenue Split & Completion Certificate Update

- Successful verified course payments are allocated 20% to CourseHub and 80% to the assigned teacher.
- `Payment` stores `TeacherId`, `PlatformRevenue`, and `TeacherRevenue` so historical allocations are stable.
- Admin has `Platform Revenue` with total, platform and teacher revenue.
- Teacher has `My Revenue` with total course revenue, 80% teacher revenue and 20% platform revenue.
- Existing payment records are backfilled during startup.
- Students can mark PDF/video learning materials complete.
- My Learning and course learning pages show a green progress bar.
- At 100% material completion, the enrollment becomes Completed and a unique CourseHub certificate is generated.
- Students can download the certificate as a PDF from the completed course.
