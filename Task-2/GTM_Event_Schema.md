# Task 1 - GTM Event Schema

## Event Tracking Table

| Event Name | Trigger Type | Key Parameters | GA4 Report / Audience |
|------------|--------------|----------------|-----------------------|
| booking_step_complete | Custom Event | step_number, clinic_location, specialty | Funnel Exploration |
| consultation_form_submitted | Form Submit | patient_name, phone_number, clinic_location | Conversions |
| call_now_clicked | Click Trigger | page_name, clinic_location, phone_number | Engagement |
| whatsapp_clicked | Click Trigger | page_name, clinic_location, source | Engagement |
| patient_guide_downloaded | Form Submit + Link Click | guide_name, patient_name, phone_number | Lead Generation |
| clinic_page_view | Page View | clinic_name, city, page_url | Pages & Screens |
| blog_scroll_50 | Scroll Depth (50%) | article_name, scroll_percent, category | Engagement |
| blog_scroll_90 | Scroll Depth (90%) | article_name, scroll_percent, category | Engaged Users |

---

# Booking Funnel

The booking form has three steps:

1. Select Clinic Location & Specialty
2. Enter Name, Phone and Preferred Date
3. Confirm Booking

After each completed step, the frontend sends a `dataLayer.push()` event. GTM captures the event and sends it to GA4. This helps track where users leave the booking process.

---

## Step 1 JSON

```json
{
  "event": "booking_step_complete",
  "step_number": 1,
  "step_name": "location_specialty_selected",
  "clinic_location": "Indiranagar",
  "specialty": "Orthopaedics"
}
```

## Step 2 JSON

```json
{
  "event": "booking_step_complete",
  "step_number": 2,
  "step_name": "patient_details_entered",
  "patient_name": "Rahul",
  "phone_number": "9876543210",
  "preferred_date": "2026-07-10"
}
```

## Step 3 JSON

```json
{
  "event": "booking_step_complete",
  "step_number": 3,
  "step_name": "booking_confirmed",
  "booking_status": "success",
  "clinic_location": "Indiranagar"
}
```

---

# Funnel Tracking in GA4

A Funnel Exploration can be created using the `booking_step_complete` event with the `step_number` parameter.

- Step 1 → `step_number = 1`
- Step 2 → `step_number = 2`
- Step 3 → `step_number = 3`

This makes it easy to identify where users drop off during the booking process.

---

# Google Ads Conversion

**Selected Conversion:** `consultation_form_submitted`

### Reason

I selected this event because it represents a completed consultation request. It is the main goal of the landing page and provides the best signal for Google Ads to optimize campaigns towards real enquiries instead of intermediate actions like button clicks or page views.