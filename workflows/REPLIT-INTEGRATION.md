# How to Connect Your Replit Website to n8n Webhooks

Once the workflows are active on n8n cloud, each form on your Replit site just needs to POST to the right webhook URL.

## Your Webhook URLs (after activating workflows in n8n)

| Form | Webhook URL |
|------|------------|
| Homepage Newsletter | `https://cdlagency.app.n8n.cloud/webhook/newsletter-signup` |
| Dry Van OO Application | `https://cdlagency.app.n8n.cloud/webhook/dryvan-oo-submit` |
| Flatbed OO Application | `https://cdlagency.app.n8n.cloud/webhook/flatbed-oo-submit` |
| Direct Shipper Purchase | Stripe handles this (webhook from Stripe dashboard) |
| Website Inquiry / Contact | `https://cdlagency.app.n8n.cloud/webhook/website-inquiry` |

---

## Code Snippets for Each Form (paste into your Replit site)

### 1. Homepage Newsletter Form

```html
<form id="newsletter-form">
  <input type="text" name="name" placeholder="Your Name" required />
  <input type="email" name="email" placeholder="Your Email" required />
  <button type="submit">Subscribe</button>
</form>

<script>
document.getElementById('newsletter-form').addEventListener('submit', async (e) => {
  e.preventDefault();
  const form = e.target;
  const data = {
    name: form.name.value,
    email: form.email.value
  };
  try {
    await fetch('https://cdlagency.app.n8n.cloud/webhook/newsletter-signup', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    });
    alert('Thanks for subscribing!');
    form.reset();
  } catch (err) {
    alert('Something went wrong. Please try again.');
  }
});
</script>
```

### 2. Dry Van Owner Operator Application

```html
<form id="dryvan-form">
  <input type="text" name="name" placeholder="Full Name" required />
  <input type="tel" name="phone" placeholder="Phone Number" required />
  <input type="email" name="email" placeholder="Email" required />
  <input type="text" name="experience" placeholder="Years of Experience" required />
  <select name="sap_program" required>
    <option value="">SAP Program?</option>
    <option value="yes">Yes</option>
    <option value="no">No</option>
  </select>
  <input type="text" name="truck" placeholder="Truck Year, Make, Model" required />
  <button type="submit">Apply Now</button>
</form>

<script>
document.getElementById('dryvan-form').addEventListener('submit', async (e) => {
  e.preventDefault();
  const form = e.target;
  const data = {
    name: form.name.value,
    phone: form.phone.value,
    email: form.email.value,
    experience: form.experience.value,
    sap_program: form.sap_program.value,
    truck: form.truck.value
  };
  try {
    await fetch('https://cdlagency.app.n8n.cloud/webhook/dryvan-oo-submit', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    });
    alert('Application submitted! We will contact you soon.');
    form.reset();
  } catch (err) {
    alert('Something went wrong. Please try again.');
  }
});
</script>
```

### 3. Flatbed Owner Operator Application

```html
<form id="flatbed-form">
  <input type="text" name="name" placeholder="Full Name" required />
  <input type="tel" name="phone" placeholder="Phone Number" required />
  <input type="email" name="email" placeholder="Email" required />
  <input type="text" name="experience" placeholder="Years of Experience" required />
  <select name="sap_program" required>
    <option value="">SAP Program?</option>
    <option value="yes">Yes</option>
    <option value="no">No</option>
  </select>
  <input type="text" name="truck" placeholder="Truck Year, Make, Model" required />
  <button type="submit">Apply Now</button>
</form>

<script>
document.getElementById('flatbed-form').addEventListener('submit', async (e) => {
  e.preventDefault();
  const form = e.target;
  const data = {
    name: form.name.value,
    phone: form.phone.value,
    email: form.email.value,
    experience: form.experience.value,
    sap_program: form.sap_program.value,
    truck: form.truck.value
  };
  try {
    await fetch('https://cdlagency.app.n8n.cloud/webhook/flatbed-oo-submit', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    });
    alert('Application submitted! We will contact you soon.');
    form.reset();
  } catch (err) {
    alert('Something went wrong. Please try again.');
  }
});
</script>
```

### 4. Direct Shippers Purchase (Stripe)

No form code needed — this is handled by Stripe:
1. Go to Stripe Dashboard → Developers → Webhooks
2. Add endpoint: `https://cdlagency.app.n8n.cloud/webhook/stripe-webhook`
3. Select event: `checkout.session.completed`
4. Stripe will automatically POST to n8n when someone purchases

### 5. Website Inquiry / Contact Form

```html
<form id="inquiry-form">
  <input type="text" name="name" placeholder="Full Name" required />
  <input type="email" name="email" placeholder="Email" required />
  <input type="tel" name="phone" placeholder="Phone Number" />
  <input type="text" name="company" placeholder="Company Name" />
  <textarea name="message" placeholder="How can we help you?" required></textarea>
  <button type="submit">Send Inquiry</button>
</form>

<script>
document.getElementById('inquiry-form').addEventListener('submit', async (e) => {
  e.preventDefault();
  const form = e.target;
  const data = {
    name: form.name.value,
    email: form.email.value,
    phone: form.phone.value,
    company: form.company.value,
    message: form.message.value
  };
  try {
    await fetch('https://cdlagency.app.n8n.cloud/webhook/website-inquiry', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    });
    alert('Inquiry sent! We will get back to you shortly.');
    form.reset();
  } catch (err) {
    alert('Something went wrong. Please try again.');
  }
});
</script>
```

---

## If Your Replit Site Already Has Forms

If you already have forms built, you just need to add the `fetch()` call inside your existing form submit handler. The key is:

```javascript
// Add this inside your existing form submit function
await fetch('https://cdlagency.app.n8n.cloud/webhook/YOUR-WEBHOOK-PATH', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    name: '...',
    email: '...',
    phone: '...',
    // whatever fields your form has
  })
});
```

Just match the field names to what n8n expects (name, email, phone, experience, sap_program, truck, message, company).
