# LMS e-Commerce and video authorisation: a blueprint

## 1. System overview

Panopto does not include a native payment gateway or e-commerce engine. As such, all monetisation and transactional workflows must be managed externally. In this architecture, Moodle operates as the storefront (or primary integration point) and access gateway, while Panopto functions strictly as the secure video delivery tier. 

Access to premium video content is governed entirely by Moodle course enrolments passed to Panopto via Learning Tools Interoperability (LTI).

## 2. Sequence schematic

<img width="1221" height="595" alt="image" src="https://github.com/user-attachments/assets/ce9b7621-2fad-4c7c-b828-e0b461e21c09" />


## 3. Authorisation data flow

The integration relies on standard LTI standards to handle user provisioning and access control. The step-by-step technical flow is as follows:

1. **Transaction Phase:** The user completes a transaction via a standard payment gateway.
2. **Enrolment Trigger:** Upon payment validation, Moodle updates the user's database record to 'Enrolled' for the specified course. 
3. **Authentication (SSO):** The user logs in to Moodle. When they navigate to the course and attempt to view a video, Moodle initiates an LTI launch to Panopto.
4. **Role Synchronisation:** Panopto intercepts the LTI launch, authenticates the user, and automatically inherits the user's Moodle course context. 
5. **Access Granted:** Panopto applies Role-Based Access Control (RBAC), granting the user 'Viewer' permissions exclusively for the Panopto folder mapped to that specific Moodle course. 

## 4. Supported deployment models

The e-commerce front-end can be architected in two ways, depending on the client's catalogue and marketing requirements.

| Deployment Model | Architecture | Technical Considerations |
| :--- | :--- | :--- |
| **Native Moodle Enrolment** | Moodle + Payment Plugin (e.g., Stripe) → Moodle LTI → Panopto | Quickest deployment. Retains the entire transactional stack within Moodle. Moodle's native catalogue UI is functional but lacks advanced e-commerce marketing features. |
| **External E-Commerce Bridge** | WooCommerce / Shopify → Middleware API → Moodle LTI → Panopto | Recommended for enterprise commercial requirements. Requires maintaining API middleware (e.g., Edwiser Bridge) to synchronise the external storefront purchases with Moodle enrolments. |
