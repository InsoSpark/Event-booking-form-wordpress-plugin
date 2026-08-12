=== Event Booking Pro ===
Contributors: insospark
Tags: event booking, event registration, booking form, PayPal, Stripe, event registration form
Requires at least: 5.8
Tested up to: 6.8
Requires PHP: 7.4
Stable tag: 1.0.0
License: GPL-2.0-or-later
License URI: https://www.gnu.org/licenses/gpl-2.0.html

A configurable event registration and booking system for WordPress with attendee management, packages, add-ons, payment plans, PayPal, Stripe, and email notifications.

== Description ==

Event Booking Pro adds a complete multi-step event registration form to WordPress.

The plugin is designed for events such as family reunions, conferences, workshops, retreats, community events, and other paid registrations. Event details, packages, add-ons, payment options, email templates, and visual styling can be managed from the WordPress admin area.

Key features include:

* Four-step front-end registration wizard.
* Primary registrant information and household details.
* Multiple attendees with age group, shirt size, meal/allergy, access, and relationship information.
* Configurable registration packages and optional add-ons.
* Pay in Full, Deposit, and Installment payment plans.
* PayPal checkout.
* Stripe card payments using Payment Intents and Stripe Elements.
* Synchronous payment confirmation without requiring webhooks.
* Unique registration numbers.
* WordPress admin booking management.
* Booking status management: Pending, Paid, Refunded, and Cancelled.
* Customer confirmation emails.
* Payment confirmation emails.
* Admin notification emails.
* Editable email subjects and message templates with placeholders.
* Early-bird registration banner.
* Custom currency code and symbol.
* Custom colors, fonts, hotel button, and custom CSS.
* Search and filter bookings in the WordPress admin.
* Automatic server-side package and add-on pricing.
* WordPress nonce and capability checks for AJAX requests.
* Clean uninstall routine that removes plugin settings and booking records.

== Requirements ==

* WordPress 5.8 or later.
* PHP 7.4 or later.
* HTTPS is strongly recommended and should be used for production payment processing.
* A PayPal Business API application is required for PayPal payments.
* A Stripe account with API keys is required for Stripe payments.
* A working WordPress email configuration is recommended for reliable notification delivery.

== Installation ==

1. Upload the `event-booking-pro` folder to `/wp-content/plugins/`, or install the ZIP from **Plugins > Add New > Upload Plugin**.
2. Activate **Event Booking Pro** from **Plugins**.
3. Open **Event Booking > Settings** in the WordPress admin.
4. Configure the event information.
5. Configure packages, prices, and add-ons.
6. Configure payment plans.
7. Add PayPal and/or Stripe credentials if online payments are required.
8. Configure email notifications and templates.
9. Configure the appearance options.
10. Create or edit a WordPress page where the registration form should appear.
11. Add the shortcode:

`[event_booking_pro]`

The legacy-compatible shortcode below is also available:

`[event_booking]`

== Configuration ==

After activation, the plugin provides five settings areas.

=== General ===

Configure:

* Event name.
* Tagline.
* Event date.
* Venue name and address.
* Registration deadline.
* Registration number prefix.
* Early-bird deadline and savings text.
* Currency code and symbol.
* Refund policy.
* Confirmation message.

Registration numbers are generated using the configured prefix, for example:

`CC27-482910`

=== Packages & Add-Ons ===

Create the prices that customers can select during registration.

Each package supports:

* Name.
* Price.
* Description.

Each add-on supports:

* Name.
* Price.
* Description.

Prices are recalculated on the server when the booking is created. This prevents the browser from being the source of truth for package and add-on pricing.

=== Payment ===

The following payment plans can be enabled:

* Pay in Full.
* Deposit.
* Installment.

For deposits, configure the percentage due at registration.

For installments, configure the number of installments.

PayPal supports:

* Sandbox mode.
* Live mode.
* Client ID.
* Client Secret.

Stripe supports:

* Test mode.
* Live mode.
* Publishable Key.
* Secret Key.

=== Email ===

Configure:

* From name.
* From email.
* Admin notification recipients.
* Customer confirmation email.
* Payment confirmation email.
* Admin booking notification.

Email templates support placeholders including:

`{event_name}`
`{event_date}`
`{venue_name}`
`{venue_address}`
`{reg_deadline}`
`{refund_policy}`
`{site_name}`
`{site_url}`
`{reg_no}`
`{name}`
`{first_name}`
`{last_name}`
`{email}`
`{phone}`
`{branch}`
`{headcount}`
`{total}`
`{due}`
`{paid}`
`{balance}`
`{plan}`
`{plan_label}`
`{status}`
`{status_label}`
`{gateway}`
`{gateway_label}`
`{txn_id}`
`{payment_method}`
`{package_summary}`
`{addon_summary}`
`{attendees_list}`
`{admin_url}`

WordPress uses `wp_mail()` to send the messages. For production sites, an SMTP plugin or properly configured transactional email service is recommended.

=== Appearance ===

Configure:

* Primary color.
* Accent color.
* Background color.
* Button color.
* Font family.
* Early-bird banner visibility.
* Hotel button visibility and URL.
* Custom CSS.

== Using the Booking Form ==

Add the following shortcode to any page or post:

`[event_booking_pro]`

The form contains four steps:

1. Select Your Household
2. Add Attendees
3. Registration Packages & Add-Ons
4. Payment

The front end dynamically calculates the estimated total and then creates a booking through WordPress AJAX.

== Payment Flow ==

=== Stripe ===

Stripe uses Payment Intents and Stripe Elements.

The general flow is:

1. The visitor submits registration information.
2. The plugin creates the booking.
3. The server creates a Stripe PaymentIntent for the amount due.
4. Stripe Elements handles the card details.
5. The payment is confirmed.
6. The server retrieves the PaymentIntent.
7. The booking is marked as paid after Stripe reports a successful status.
8. Payment email notifications are sent.

No Stripe webhook is required by the current implementation.

=== PayPal ===

PayPal uses the Orders API.

The general flow is:

1. The visitor submits registration information.
2. The plugin creates the booking.
3. The server creates a PayPal order.
4. The visitor approves the PayPal payment.
5. The server captures the order.
6. The booking is marked as paid after a completed capture.
7. Payment email notifications are sent.

No PayPal webhook is required by the current implementation.

== Important Payment Notes ==

Deposit and installment plans currently calculate the first amount due and store the remaining balance in the booking.

They do not provide an automatic recurring payment schedule, automatic future payment collection, or automatic reminders for later installment payments.

Also, the current payment implementation should be treated as a synchronous checkout flow. Webhooks are not used to recover from delayed, asynchronous, disputed, refunded, or otherwise changed payment states.

For high-value production transactions, adding webhook-based payment verification is recommended.

== Managing Bookings ==

Open **Event Booking > Bookings** to manage registrations.

Each booking stores:

* Registration number.
* Primary registrant name.
* Email.
* Phone.
* Family branch.
* Mailing address.
* Attendees.
* Selected packages.
* Selected add-ons.
* Headcount.
* Payment plan.
* Total amount.
* Amount due.
* Remaining balance.
* Payment gateway.
* Transaction ID.
* Payment status.
* Booking status.
* Registration date.

Available booking statuses:

* Pending
* Paid
* Refunded
* Cancelled

Administrators can also resend confirmation and payment emails from the booking screen.

== Security ==

The plugin includes:

* WordPress AJAX nonces for public requests.
* WordPress capability checks for administrative AJAX actions.
* Input sanitization for submitted registration data.
* Server-side package and add-on price calculation.
* Escaped output in the front-end and admin interfaces.
* Secret payment credentials kept server-side.
* Automatic deletion of stored plugin data during uninstall.

For production use, HTTPS should be enabled and payment credentials should never be exposed in front-end code.

== Data and Uninstall ==

Bookings are stored as a custom post type named `ebp_booking`.

Plugin settings are stored in the WordPress option:

`ebp_settings`

When the plugin is deleted through WordPress, the uninstall routine removes:

* Event Booking Pro settings.
* Stored plugin version data.
* Plugin admin notice data.
* All `ebp_booking` booking posts and their metadata.

WARNING: Deleting the plugin permanently deletes all booking records created by the plugin. Back up the site before uninstalling.

Deactivating the plugin does not delete booking data.

== Troubleshooting ==

=== The form does not appear ===

Make sure the plugin is activated and the page contains:

`[event_booking_pro]`

You can also try:

`[event_booking]`

Clear any page cache and browser cache after changing plugin settings.

=== Payment buttons are not displayed ===

Check that the selected payment gateway is enabled and that all required credentials have been entered.

For Stripe, verify the publishable key and secret key belong to the same environment.

For PayPal, verify the Client ID and Client Secret belong to the selected Sandbox or Live environment.

=== Emails are not being delivered ===

The plugin uses WordPress `wp_mail()`.

If emails are not arriving, configure SMTP or a transactional email provider and verify the WordPress site's sender address and DNS/email authentication settings.

=== Currency errors occur during payment ===

Make sure the configured currency is supported by the selected payment gateway and that the currency code is a valid ISO 4217 three-letter code.

Examples:

`USD`
`EUR`
`GBP`

=== Registration totals look incorrect ===

The browser displays a running estimate, but the final booking amount is recalculated on the server using the package and add-on prices saved in WordPress settings.

Check the configured package and add-on prices under **Event Booking > Settings > Packages & Add-Ons**.

== Developer Notes ==

Plugin slug:

`event-booking-pro`

Custom post type:

`ebp_booking`

Primary shortcode:

`[event_booking_pro]`

Compatibility shortcode:

`[event_booking]`

Main plugin option:

`ebp_settings`

The plugin uses WordPress AJAX endpoints for registration and payment operations.

== Known Limitations ==

* Payment verification is synchronous and does not use PayPal or Stripe webhooks.
* Deposit and installment plans do not automatically collect future balances.
* There is no built-in automated recurring billing system.
* Refunds are represented as an admin booking status; the plugin does not implement gateway-side refund processing.
* Email delivery depends on the site's WordPress mail configuration.
* The current form is designed around a single event configuration rather than a database of multiple independent events.
* Attendee age groups and shirt sizes are currently predefined in the front-end configuration.
* The hotel button is a configurable external link rather than a built-in hotel booking system.

== Changelog ==

= 1.0.0 =
* Initial release.
* Added four-step event registration form.
* Added household and attendee registration.
* Added packages and add-ons.
* Added PayPal checkout.
* Added Stripe Payment Intents and Elements.
* Added full, deposit, and installment payment plans.
* Added booking management and status controls.
* Added customer and admin email notifications.
* Added customizable email templates.
* Added appearance customization.
* Added early-bird registration banner.
* Added shortcode support.
* Added uninstall cleanup.

== License ==

Event Booking Pro is licensed under the GPL-2.0-or-later license.
