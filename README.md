# AUPS Vadakkumpuram Certificate Web App

This version keeps the existing certificate design and fixes:
- Student records load into the certificate register table.
- Submission supports the original `students` table schema (`class_division`) and the newer split-column schema.
- Student photo is saved through the `student-photos` Supabase Storage bucket when available; if storage upload is unavailable, a compressed photo is saved directly in `photo_path`.
- Certificate photo is clipped inside a fixed photo frame so it cannot spill outside the frame.
- Default photo frame is aligned to the supplied `certificate-sample.jpg`.
- Preview, individual PDF, bulk PDF, edit and delete are available.
- LP Arabic items and capital-letter validation remain enabled.

## Supabase setup
Open `config.js` and replace `PASTE_YOUR_PUBLISHABLE_KEY_HERE` with the **Supabase publishable key** from your project. Keep the existing project URL.

For the storage route, create a Storage bucket named `student-photos`. If the bucket policies do not allow uploads, the app will automatically fall back to storing a compressed image data URL in the existing `photo_path` text column.

The frontend uses the Supabase Data API and therefore requires the appropriate table/storage policies for the public/publishable client. Supabase documents that frontend Data API requests are controlled by Row Level Security policies.

### Print-quality certificate PDF update
- Bulk and individual certificate PDFs now capture the same 1024×683 certificate layout used by Preview.
- The canvas is rendered at 4× resolution before PDF creation for improved print clarity.
- PDFs use A4 landscape pages and preserve the certificate aspect ratio without stretching, so Preview alignment is retained.
- Certificate image is centered vertically on the A4 page with uniform scaling.
