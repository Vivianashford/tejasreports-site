# Equipment Pre-Use Inspection Form

Static Tejas SOP 1.12 equipment inspection form hosted under Tejas Reporting.

## Live path

- `/equipment-inspection/`
- Portal card added on `tejasreporting.com`
- Backoffice Field Ops link points to `https://tejasreporting.com/equipment-inspection/`

## Delivery setup

Pure-HTML email handoff. No third-party email service or CRM.

1. Operator completes the inspection.
2. Browser generates the SOP-style PDF with jsPDF.
3. PDF is archived to the Arena Worker (Cloudflare R2) for audit + permanent download link.
4. The operator's email client (or native mobile share sheet) opens with:
   - **To:** all three recipients (plus the operator if they entered an email)
   - **Subject:** equipment + date + safe-to-operate state
   - **Body:** full inspection details + PDF download link
   - **Attachment:** the actual PDF file (mobile only, via Web Share API)
5. Operator hits **Send** in their email client.
6. A local PDF copy is also downloaded as a fail-safe.

## Recipients

Every submission is emailed to:

- jbaustert@tejasenvironmental.com  (Jessica Baustert)
- rguerrero@tejasenvironmental.com  (Bobby Guerrero)
- rseymour@tejasenvironmental.com  (Reneé Seymour)
- The operator/inspector email entered on the form (added as a 4th recipient if filled)

To change recipients, edit the `EQUIPMENT_INSPECTION_RECIPIENTS` array in `arena-portal/worker.js` and redeploy with `npx wrangler deploy`.

## Backend dependencies

- Worker: `https://arena-api.jean-475.workers.dev`
  - `POST /api/equipment-inspections` — archives PDF + photos to R2, returns a permanent download URL.

No GHL, no CRM, no email service tokens.

## Notes

- PDF generation is client-side.
- The operator must click **Send** in their email client — the page cannot auto-send.
- On mobile, the PDF is attached as a real file. On desktop, the PDF appears as a download link in the email body.
- If the Worker archive call fails, the operator still gets a local PDF download and the email composer still opens.
