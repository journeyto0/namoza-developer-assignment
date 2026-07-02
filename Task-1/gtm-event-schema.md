Task 1 – GTM Event Schema

OrthoNow currently tracks only page views in Google Analytics 4. Because of this, the marketing team cannot understand how users interact with important elements such as the booking form, Call Now buttons, WhatsApp widget or Patient Guide download.To solve this, I would implement the following GTM event tracking setup.

## GTM Event Schema

| Event Name | Trigger Type | Key Parameters | GA4 Report / Audience |
|------------|--------------|----------------|-----------------------|
| booking_step_complete | Custom Event | step_number, step_name, clinic_location, specialty | Funnel Exploration |
| booking_completed | Custom Event | booking_status, clinic_location, specialty | Conversions |
| call_now_click | Link Click | phone_number, page_name, clinic_location | Engagement |
| whatsapp_click | Link Click | page_name, clinic_location, destination_url | Engagement |
| patient_guide_download | Form Submission + File Download | patient_name, phone_number, guide_name | Lead Generation |
| clinic_page_view | Page View | clinic_name, city, page_url | Pages & Screens |
| blog_scroll | Scroll Depth | article_title, scroll_percentage, category | Engagement |

## Booking Funnel Tracking

The appointment booking form has three steps. Since GTM cannot automatically detect movement between form steps, the front-end developer will push a custom `dataLayer` event whenever a user completes a step. GTM will listen for these events and send them to GA4.

### Step 1 – Clinic Location & Specialty

Trigger: Custom Event

Event Name: `booking_step_complete`

Condition:

`step_number = 1`

```json
{
  "event": "booking_step_complete",
  "step_number": 1,
  "step_name": "location_specialty_selected",
  "clinic_location": "{{clinic name}}",
  "specialty": "{{specialty selected}}"
}
