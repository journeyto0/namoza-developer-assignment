## Task 1 – GTM Event Schema

OrthoNow currently tracks only page views in Google Analytics 4, which makes it difficult to understand how users interact with important actions on the website. To improve tracking and support future marketing campaigns, I would implement the following GTM event tracking setup.

## GTM Event Schema

| Event Name | Trigger Type | Key Parameters | GA4 Report / Audience |
|------------|--------------|----------------|-----------------------|
| booking_step_complete | Custom Event | step_number, step_name, clinic_location, specialty | Funnel Exploration |
| booking_completed | Custom Event | booking_status, clinic_location, specialty | Conversions |
| call_now_click | Link Click | phone_number, page_name, clinic_location | Engagement Report |
| whatsapp_click | Link Click | page_name, clinic_location, destination_url | Engagement Report |
| patient_guide_download | Form Submission + File Download | patient_name, phone_number, guide_name | Lead Generation Audience |
| clinic_page_view | Page View | clinic_name, city, page_url | Pages & Screens |
| blog_scroll | Scroll Depth | article_title, scroll_percentage, category | Engagement Report |


## Tracking the 3-Step Booking Funnel

The appointment booking form has three steps. Since Google Tag Manager cannot automatically detect movement between the steps of a multi-step form, the front-end developer will implement a `dataLayer.push()` after each completed step. GTM will listen for these custom events and send them to Google Analytics 4. This setup allows the marketing team to measure how many users complete each step of the booking process and identify where users leave the funnel before completing a consultation booking.

### Step 1 – Select Clinic Location & Specialty

**User Action:**  
The user selects a clinic location and the required specialty.

**Trigger Type:** Custom Event

**Trigger Name:** `booking_step_complete`

**Condition:** `step_number = 1`

**Purpose:**  
Tracks users who successfully complete the first step of the booking form.

**dataLayer Push**

```json
{
  "event": "booking_step_complete",
  "step_number": 1,
  "step_name": "location_specialty_selected",
  "clinic_location": "{{clinic name}}",
  "specialty": "{{specialty selected}}"
}
```

### Step 2 – Enter Patient Details

**User Action:**  
The user enters their name, phone number and preferred consultation date.

**Trigger Type:** Custom Event

**Trigger Name:** `booking_step_complete`

**Condition:** `step_number = 2`

**Purpose:**  
Tracks users who provide their contact details and move to the final step of the booking process.

**dataLayer Push**

```json
{
  "event": "booking_step_complete",
  "step_number": 2,
  "step_name": "patient_details_entered",
  "patient_name": "{{patient name}}",
  "phone_number": "{{phone number}}",
  "preferred_date": "{{preferred date}}"
}
```

### Step 3 – Confirm Booking

**User Action:**  
The user reviews the details and confirms the consultation booking.

**Trigger Type:** Custom Event

**Trigger Name:** `booking_completed`

**Condition:** `step_number = 3`

**Purpose:**  
Tracks successful appointment bookings. This event will also be used as the primary conversion event in Google Ads.

**dataLayer Push**

```json
{
  "event": "booking_completed",
  "step_number": 3,
  "step_name": "booking_confirmed",
  "clinic_location": "{{clinic name}}",
  "specialty": "{{specialty selected}}"
}
```

## Funnel Drop-off in GA4

I would create a Funnel Exploration report in GA4 using the following steps:

1. **Location & Specialty Selected**
2. **Patient Details Entered**
3. **Booking Confirmed**

Using these three events, GA4 Funnel Exploration will show how many users complete each booking step and where users leave the booking process. This helps identify the step with the highest drop-off and provides clear insights for improving the consultation booking experience.

## Google Ads Conversion

**Selected Conversion Event:** `booking_completed`

**Reason**

I would import the `booking_completed` event into Google Ads because it represents a successfully completed consultation booking. This is the primary objective of the campaign and provides a more meaningful optimisation signal than intermediate actions such as clicks or partial form completion.
