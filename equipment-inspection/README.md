# Equipment Pre-Use Inspection Form

Static Tejas SOP 1.12 equipment inspection form hosted under Tejas Reporting.

## Live path

- `/equipment-inspection/`
- Portal card added on `tejasreporting.com`
- Backoffice Field Ops link points to `https://tejasreporting.com/equipment-inspection/`

## Delivery setup

This form now uses the same backend pattern as the Incident Report form:

1. Operator completes the inspection.
2. Browser generates the SOP-style PDF with jsPDF.
3. PDF is uploaded through the Arena Worker/GHL proxy.
4. Each recipient gets an email with the inspection details and PDF download link.
5. A local PDF copy downloads to the operator's device as a fail-safe.

No EmailJS setup is required.

## Recipients

Every submission is emailed to:

- jbaustert@tejasenvironmental.com
- rguerrero@tejasenvironmental.com
- rseymour@tejasenvironmental.com

To change recipients, edit the `RECIPIENTS` array in `index.html`.

## Backend dependencies

- Worker: `https://arena-api.jean-475.workers.dev`
- GHL proxy endpoints:
  - `/api/ghl/contacts/upsert`
  - `/api/upload-to-contact`
  - `/api/ghl/conversations/messages`

## Notes

- PDF generation is client-side.
- PDF upload/email delivery requires network access to the Worker.
- If upload/email fails, the operator still receives a downloaded PDF copy.
