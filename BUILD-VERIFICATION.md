# CourseHub verification status

The project was statically reviewed after the feature implementation.

## Implemented in source

- Admin course management.
- Admin teacher assignment.
- Admin course image upload.
- Admin enrollment/payment pages.
- Admin enrollment notifications.
- Teacher assigned-course security.
- Teacher My Students.
- Teacher weekly modules, maximum five weeks.
- Teacher PDF/video upload and deletion.
- Protected student PDF/video delivery.
- Student My Learning.
- Student weekly learning outline.
- Student notifications.
- SSLCommerz initiation, validation, IPN and development simulation.
- Enrollment notifications for admin and assigned teacher.
- Homepage cleanup and transition animations.

## Local verification required

This environment does not contain the .NET SDK, so a real `dotnet build` cannot be executed here.

Run in Visual Studio's terminal:

```powershell
dotnet clean
dotnet build
dotnet run
```

Then test the role-by-role checklist in `IMPLEMENTED-FEATURES.md`.

The runtime database initializer is intentionally included because this version adds new tables/columns to an existing CourseHub MariaDB database.
