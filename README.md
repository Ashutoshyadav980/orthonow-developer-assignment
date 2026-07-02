# OrthoNow Developer Assignment

## Candidate
Ashutosh Yadav

---

# Task 1 - GTM Event Schema

## Event Schema

| Event Name | Trigger Type | Key Parameters | GA4 Report / Audience |
|------------|-------------|---------------|-----------------------|
| booking_step_complete | Custom Event | step_number, clinic_location, specialty | Funnel Exploration |
| appointment_booking_completed | Custom Event | booking_id, clinic_location, specialty | Conversion Report |
| consultation_form_submitted | Custom Event | patient_name, patient_phone, lead_source | Conversion Tracking |
| call_now_click | Click Trigger | phone_number, page_location, clinic_location | Google Ads Attribution |
| whatsapp_click | Link Click Trigger | page_location, clinic_location, device_type | High Intent Audience |
| patient_guide_download | Form Submit + File Download | patient_name, patient_phone, guide_name | Lead Attribution |
| clinic_page_view | Page View Trigger | clinic_name, city, page_path | Clinic Performance |
| blog_scroll_depth | Scroll Trigger | article_title, scroll_percentage, category | Engagement Analysis |

---

## Booking Funnel Tracking

### Step 1 - Clinic Location and Specialty Selection

```json
{
  "event": "booking_step_complete",
  "step_number": 1,
  "step_name": "location_specialty_selected",
  "clinic_location": "{{clinic name}}",
  "specialty": "{{specialty selected}}"
}
```

### Step 2 - Patient Details Entry

```json
{
  "event": "booking_step_complete",
  "step_number": 2,
  "step_name": "patient_details_entered",
  "clinic_location": "{{clinic name}}",
  "preferred_date": "{{selected date}}"
}
```

### Step 3 - Booking Confirmation

```json
{
  "event": "appointment_booking_completed",
  "step_number": 3,
  "booking_id": "{{booking id}}",
  "clinic_location": "{{clinic name}}"
}
```

---

## Funnel Drop-Off Tracking

GA4 Funnel Exploration:

1. booking_step_complete where step_number = 1
2. booking_step_complete where step_number = 2
3. appointment_booking_completed

This setup allows analysis of drop-offs between booking stages and helps identify friction points in the appointment flow.

---

## Who Implements dataLayer Pushes?

The frontend developer implements the actual `window.dataLayer.push()` statements based on requirements provided by the analytics implementation team.

Google Tag Manager cannot automatically detect custom multi-step booking flows without explicit frontend implementation.

---

## Google Ads Conversion Recommendation

Recommended conversion import:

```text
consultation_form_submitted
```

Reason:

This event represents the highest intent lead action and is closest to actual revenue generation, making it the best optimization signal for Google Ads campaigns.

---

# Task 2 - Landing Page Build

Implemented in:

```text
Index.html
```

Features:

- Single self-contained HTML file
- HTML, CSS and Vanilla JavaScript only
- Mobile responsive design
- Trust signals and conversion-focused layout
- Name and Phone fields only
- GTM dataLayer push on successful submission
- Thank-you state without page reload
- Optimized for Core Web Vitals and PageSpeed

---

# Task 3 - Integration Architecture

## Architecture Flow

Landing Page  
↓  
Backend API Endpoint  
↓  
HubSpot Contact Search API  
↓  
Create Contact OR Update Existing Contact  
↓  
Karix WhatsApp Business API  
↓  
Google Ads Conversion API  

---

## Technology Choice

I would use the HubSpot Contacts API instead of embedded HubSpot forms because the landing page is custom built and requires custom phone-number deduplication logic before contact creation.

---

## Biggest Failure Point

HubSpot performs native deduplication using email addresses rather than phone numbers.

Since this landing page collects only phone numbers, duplicate patient records could be created if phone-based deduplication is not implemented.

Fallback approach:

- Search HubSpot contacts using phone number.
- If contact exists → update existing contact.
- If contact does not exist → create new contact.

---

## WhatsApp SLA Monitoring

The WhatsApp confirmation message must be delivered within two minutes.

Potential failure points:

- Karix API downtime
- Network failures
- Queue backlog
- Rate limiting

Monitoring approach:

- Store form submission timestamp.
- Store WhatsApp delivery timestamp.
- Trigger operational alert if delivery exceeds two minutes.

---

# Repository Contents

- Index.html
- README.md

---

# Submission

- GitHub Repository Link
- Loom Walkthrough Video
