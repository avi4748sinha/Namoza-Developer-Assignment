# Task 3 - Integration Design

## Architecture

When a user submits the consultation form, I would send the data (name, phone number and clinic) to a backend service using a REST API. I would build this backend with FastAPI - it does not exist yet, this is the architecture I'm proposing for this integration.

I would use a direct backend API instead of Zapier or Make because it gives better control over validation, phone deduplication and third-party integrations.

The backend first searches HubSpot using the phone number because this form does not collect email. If the phone number already exists, the contact is updated. If it does not exist, a new contact is created.

After that, the backend sends a confirmation message through the Karix WhatsApp API.

The frontend also fires the `consultation_form_submitted` event using `dataLayer.push()`. Google Tag Manager captures this event and sends it to GA4. The same conversion can be imported into Google Ads for campaign optimisation.

## Biggest Failure Point

The biggest challenge is phone number deduplication. In many cases, family members may share the same phone number. So instead of creating duplicate contacts, the backend first checks the phone number in HubSpot.

If the phone number already exists, I would update only the required details like clinic preference, source and lead status. I would not overwrite the existing name because the same phone number may belong to another family member. Instead, I would add a note with the new enquiry details so the team can review it manually if needed.

## WhatsApp SLA

The confirmation message should be sent within two minutes after the form is submitted.

The main reason for delay could be a slow or unavailable Karix API. To handle this, the backend should retry the request automatically if the first attempt fails.

Failed requests should be logged with timestamps. I would also set up an alert on Slack or email if the gap between form submission and WhatsApp delivery crosses 90 seconds, so the team gets a warning before the 2-minute SLA is actually breached. This gives time to check the issue manually before it affects too many patients.
