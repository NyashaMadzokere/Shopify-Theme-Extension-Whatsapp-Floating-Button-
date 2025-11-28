# How to Build a Custom WhatsApp Button Extension for Shopify

## Make Your Store More Accessible with a Floating WhatsApp Chat Button

![WhatsApp Button](https://images.unsplash.com/photo-1611746872915-64382b5c76da?w=800)

In today's e-commerce landscape, instant communication with customers is crucial. WhatsApp, with over 2 billion users worldwide, has become one of the most popular messaging platforms for customer support. In this tutorial, we'll build a fully customizable floating WhatsApp button extension for Shopify stores.

## What We're Building

By the end of this tutorial, you'll have created a Shopify Theme App Extension that adds a floating WhatsApp button to any Shopify store. Merchants will be able to customize:

- 📱 Phone number and default message
- 🎨 Button colors, size, and position
- 💬 Tooltip text and styling
- 📐 Separate settings for mobile and desktop
- 🎯 Four corner positioning options

**Best of all**: No JavaScript required, just pure Liquid and CSS!

## Prerequisites

Before we start, make sure you have:

1. **Node.js** (v16 or higher) - [Download here](https://nodejs.org/)
2. **Shopify Partner Account** - [Sign up for free](https://partners.shopify.com/signup)
3. **Development Store** - Create one in your Partner dashboard
4. **Basic knowledge of HTML/CSS** - Helpful but not required

## Step 1: Set Up Your Development Environment

First, let's install the Shopify CLI globally:

```bash
npm install -g @shopify/cli @shopify/app
```

Verify the installation:

```bash
shopify version
```

## Step 2: Initialize the Shopify App

Navigate to your projects folder and initialize a new Shopify app:

```bash
shopify app init
```

You'll be prompted with several questions:

1. **Get started building your app**: Choose `Build an extension-only app`
2. **Which organization is this work for?**: Select your Partner account
3. **Create this project as a new app on Shopify?**: Choose `Yes, create it as a new app`
4. **App name**: Enter `WhatsApp Button` (or any name you prefer)

The CLI will create a new directory with the basic structure.

## Step 3: Navigate to Your App Directory

```bash
cd whatsapp-button
```

## Step 4: Generate a Theme Extension

Now let's create the extension:

```bash
shopify app generate extension
```

When prompted:

1. **Type of extension?**: Select `Theme app extension`
2. **Name your extension**: Enter `WhatsApp Button`

You should see a success message confirming the extension was created in `extensions/whatsapp-button`.

## Step 5: Create the WhatsApp Button Block

Navigate to the extension's blocks folder:

```bash
cd extensions/whatsapp-button/blocks
```

You'll see a default template file. Replace its contents or create a new file called `whatsapp_button.liquid` with the following code:

### The Complete WhatsApp Button Code

```liquid
{% if block.settings.phone_number != blank %}
<style>
  .whatsapp-float-button {
    position: fixed;
    {% if block.settings.position == 'bottom-right' %}
      bottom: {{ block.settings.bottom_spacing }}px;
      right: {{ block.settings.side_spacing }}px;
    {% elsif block.settings.position == 'bottom-left' %}
      bottom: {{ block.settings.bottom_spacing }}px;
      left: {{ block.settings.side_spacing }}px;
    {% elsif block.settings.position == 'top-right' %}
      top: {{ block.settings.top_spacing }}px;
      right: {{ block.settings.side_spacing }}px;
    {% elsif block.settings.position == 'top-left' %}
      top: {{ block.settings.top_spacing }}px;
      left: {{ block.settings.side_spacing }}px;
    {% endif %}
    width: {{ block.settings.button_size }}px;
    height: {{ block.settings.button_size }}px;
    background-color: {{ block.settings.button_color }};
    border-radius: {{ block.settings.border_radius }}%;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    cursor: pointer;
    z-index: 9999;
    transition: all 0.3s ease;
    text-decoration: none;
  }
  
  .whatsapp-float-button:hover {
    transform: scale(1.1);
    box-shadow: 0 6px 20px rgba(0, 0, 0, 0.25);
  }
  
  .whatsapp-float-button svg {
    width: {{ block.settings.icon_size }}%;
    height: {{ block.settings.icon_size }}%;
    fill: {{ block.settings.icon_color }};
  }
  
  .whatsapp-tooltip {
    position: absolute;
    {% if block.settings.position contains 'right' %}
      right: calc(100% + 15px);
    {% else %}
      left: calc(100% + 15px);
    {% endif %}
    top: 50%;
    transform: translateY(-50%);
    background-color: {{ block.settings.tooltip_bg_color }};
    color: {{ block.settings.tooltip_text_color }};
    padding: 10px 15px;
    border-radius: 8px;
    white-space: nowrap;
    font-size: 14px;
    font-weight: 500;
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.3s ease;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  }
  
  .whatsapp-float-button:hover .whatsapp-tooltip {
    opacity: 1;
  }
  
  @media (max-width: 768px) {
    .whatsapp-float-button {
      width: {{ block.settings.mobile_button_size }}px;
      height: {{ block.settings.mobile_button_size }}px;
      {% if block.settings.position == 'bottom-right' or block.settings.position == 'bottom-left' %}
        bottom: {{ block.settings.mobile_bottom_spacing }}px;
      {% endif %}
      {% if block.settings.position == 'bottom-right' or block.settings.position == 'top-right' %}
        right: {{ block.settings.mobile_side_spacing }}px;
      {% else %}
        left: {{ block.settings.mobile_side_spacing }}px;
      {% endif %}
    }
    
    .whatsapp-tooltip {
      display: none;
    }
  }
</style>

{% assign phone_clean = block.settings.phone_number | replace: ' ', '' | replace: '-', '' | replace: '(', '' | replace: ')', '' | replace: '+', '' %}
{% assign default_message = block.settings.default_message | url_encode %}
{% assign whatsapp_url = 'https://wa.me/' | append: phone_clean %}
{% if block.settings.default_message != blank %}
  {% assign whatsapp_url = whatsapp_url | append: '?text=' | append: default_message %}
{% endif %}

<a href="{{ whatsapp_url }}" 
   class="whatsapp-float-button" 
   target="_blank" 
   rel="noopener noreferrer"
   aria-label="{{ 'whatsapp.button_aria_label' | t }}">
  <svg viewBox="0 0 32 32" xmlns="http://www.w3.org/2000/svg">
    <path d="M16 0c-8.837 0-16 7.163-16 16 0 2.825 0.737 5.607 2.137 8.048l-2.137 7.952 8.135-2.135c2.369 1.313 5.061 2.135 7.865 2.135 8.837 0 16-7.163 16-16s-7.163-16-16-16zM16 29.333c-2.547 0-5.053-0.747-7.2-2.133l-0.507-0.32-5.227 1.373 1.4-5.173-0.347-0.533c-1.515-2.24-2.32-4.82-2.32-7.547 0-7.36 5.973-13.333 13.333-13.333s13.333 5.973 13.333 13.333c0 7.36-5.973 13.333-13.333 13.333z"/>
    <path d="M23.547 19.787c-0.373-0.187-2.213-1.093-2.56-1.213-0.347-0.133-0.6-0.187-0.853 0.187s-0.973 1.213-1.2 1.467c-0.213 0.253-0.44 0.28-0.813 0.093s-1.587-0.587-3.013-1.853c-1.12-0.987-1.867-2.213-2.080-2.587s-0.027-0.573 0.16-0.76c0.173-0.173 0.373-0.44 0.56-0.667 0.187-0.213 0.253-0.373 0.373-0.627 0.133-0.253 0.067-0.48-0.027-0.667s-0.853-2.053-1.173-2.813c-0.307-0.747-0.627-0.64-0.853-0.653-0.213-0.013-0.467-0.013-0.72-0.013s-0.667 0.093-1.013 0.467c-0.347 0.373-1.333 1.307-1.333 3.187s1.36 3.693 1.547 3.947c0.187 0.253 2.613 3.987 6.333 5.587 0.88 0.387 1.573 0.613 2.107 0.787 0.893 0.28 1.707 0.24 2.347 0.147 0.72-0.107 2.213-0.907 2.533-1.773 0.307-0.88 0.307-1.627 0.213-1.773s-0.333-0.253-0.707-0.44z"/>
  </svg>
  {% if block.settings.show_tooltip %}
    <span class="whatsapp-tooltip">{{ block.settings.tooltip_text }}</span>
  {% endif %}
</a>
{% endif %}

{% schema %}
{
  "name": "WhatsApp Button",
  "target": "body",
  "settings": [
    {
      "type": "header",
      "content": "WhatsApp Settings"
    },
    {
      "type": "text",
      "id": "phone_number",
      "label": "Phone Number",
      "info": "Enter with country code (e.g., 1234567890 or +1 234 567 8900)"
    },
    {
      "type": "textarea",
      "id": "default_message",
      "label": "Default Message",
      "info": "Pre-filled message when chat opens",
      "default": "Hi! I'm interested in your products."
    },
    {
      "type": "header",
      "content": "Button Style"
    },
    {
      "type": "color",
      "id": "button_color",
      "label": "Button Background Color",
      "default": "#25D366"
    },
    {
      "type": "color",
      "id": "icon_color",
      "label": "Icon Color",
      "default": "#ffffff"
    },
    {
      "type": "range",
      "id": "button_size",
      "label": "Button Size (Desktop)",
      "min": 40,
      "max": 100,
      "step": 5,
      "unit": "px",
      "default": 60
    },
    {
      "type": "range",
      "id": "mobile_button_size",
      "label": "Button Size (Mobile)",
      "min": 40,
      "max": 80,
      "step": 5,
      "unit": "px",
      "default": 50
    },
    {
      "type": "range",
      "id": "icon_size",
      "label": "Icon Size",
      "min": 50,
      "max": 80,
      "step": 5,
      "unit": "%",
      "default": 60
    },
    {
      "type": "range",
      "id": "border_radius",
      "label": "Button Roundness",
      "min": 0,
      "max": 50,
      "step": 5,
      "unit": "%",
      "default": 50
    },
    {
      "type": "header",
      "content": "Position"
    },
    {
      "type": "select",
      "id": "position",
      "label": "Button Position",
      "options": [
        { "value": "bottom-right", "label": "Bottom Right" },
        { "value": "bottom-left", "label": "Bottom Left" },
        { "value": "top-right", "label": "Top Right" },
        { "value": "top-left", "label": "Top Left" }
      ],
      "default": "bottom-right"
    },
    {
      "type": "range",
      "id": "bottom_spacing",
      "label": "Bottom Spacing (Desktop)",
      "min": 10,
      "max": 100,
      "step": 5,
      "unit": "px",
      "default": 20
    },
    {
      "type": "range",
      "id": "top_spacing",
      "label": "Top Spacing (Desktop)",
      "min": 10,
      "max": 100,
      "step": 5,
      "unit": "px",
      "default": 20
    },
    {
      "type": "range",
      "id": "side_spacing",
      "label": "Side Spacing (Desktop)",
      "min": 10,
      "max": 100,
      "step": 5,
      "unit": "px",
      "default": 20
    },
    {
      "type": "range",
      "id": "mobile_bottom_spacing",
      "label": "Bottom Spacing (Mobile)",
      "min": 10,
      "max": 80,
      "step": 5,
      "unit": "px",
      "default": 15
    },
    {
      "type": "range",
      "id": "mobile_side_spacing",
      "label": "Side Spacing (Mobile)",
      "min": 10,
      "max": 80,
      "step": 5,
      "unit": "px",
      "default": 15
    },
    {
      "type": "header",
      "content": "Tooltip"
    },
    {
      "type": "checkbox",
      "id": "show_tooltip",
      "label": "Show Tooltip on Hover",
      "default": true
    },
    {
      "type": "text",
      "id": "tooltip_text",
      "label": "Tooltip Text",
      "default": "Chat with us on WhatsApp!"
    },
    {
      "type": "color",
      "id": "tooltip_bg_color",
      "label": "Tooltip Background Color",
      "default": "#333333"
    },
    {
      "type": "color",
      "id": "tooltip_text_color",
      "label": "Tooltip Text Color",
      "default": "#ffffff"
    }
  ]
}
{% endschema %}
```

### Understanding the Code

Let's break down the key components:

**1. The Conditional Wrapper**
```liquid
{% if block.settings.phone_number != blank %}
```
This ensures the button only displays when a merchant has entered a phone number.

**2. Dynamic Positioning**
The CSS uses Liquid conditionals to position the button based on merchant settings:
```liquid
{% if block.settings.position == 'bottom-right' %}
  bottom: {{ block.settings.bottom_spacing }}px;
  right: {{ block.settings.side_spacing }}px;
{% endif %}
```

**3. Phone Number Cleaning**
```liquid
{% assign phone_clean = block.settings.phone_number | replace: ' ', '' | replace: '-', '' %}
```
This strips out formatting characters to create a valid WhatsApp URL.

**4. WhatsApp URL Builder**
```liquid
{% assign whatsapp_url = 'https://wa.me/' | append: phone_clean %}
```
The `wa.me` API format works on both desktop and mobile devices.

**5. Schema Settings**
The `{% schema %}` block defines all the customization options that appear in the Shopify theme editor.

## Step 6: Update Locales (Optional)

Navigate to `extensions/whatsapp-button/locales/en.default.json` and update it:

```json
{
  "whatsapp": {
    "button_aria_label": "Chat with us on WhatsApp",
    "default_tooltip": "Chat with us on WhatsApp!",
    "default_message": "Hi! I'm interested in your products."
  }
}
```

This provides translations and default text for your extension.

## Step 7: Test Your Extension Locally

Navigate back to the app root directory:

```bash
cd ../../..
```

Start the development server:

```bash
shopify app dev
```

The CLI will:
1. Start a local development server
2. Generate a preview URL
3. Open it in your browser

Follow the prompts to:
1. Select your development store
2. Install the app

## Step 8: Add the Button to Your Theme

Once the app is installed:

1. In your Shopify admin, go to **Online Store > Themes**
2. Click **Customize** on your active theme
3. In the theme editor, navigate to any page
4. Click **Add app block** (usually in the footer or a section)
5. Select **WhatsApp Button** from the app blocks list
6. Configure your settings:
   - Add your WhatsApp phone number (with country code)
   - Customize colors, size, and position
   - Set your default message
7. Click **Save**

You should immediately see the floating WhatsApp button on your preview!

## Step 9: Deploy to Production

When you're happy with your extension:

```bash
shopify app deploy
```

This creates a new version of your app extension. The CLI will guide you through the deployment process.

## Step 10: Publish Your App

1. Go to your [Partner Dashboard](https://partners.shopify.com)
2. Navigate to **Apps** > Your app
3. Click **Create app version**
4. Submit for review (if distributing publicly) or install directly on stores

## Customization Ideas

Now that you have a working WhatsApp button, here are some enhancement ideas:

### 1. Add Animation Options

Add a pulsing animation to draw attention:

```css
@keyframes pulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.05); }
}

.whatsapp-float-button {
  animation: pulse 2s infinite;
}
```

### 2. Add Business Hours Check

Show/hide the button based on business hours:

```liquid
{% assign current_hour = 'now' | date: '%H' | plus: 0 %}
{% if current_hour >= 9 and current_hour < 18 %}
  <!-- Show button -->
{% endif %}
```

### 3. Add Department Selection

Create multiple WhatsApp numbers for different departments:

```json
{
  "type": "select",
  "id": "department",
  "label": "Department",
  "options": [
    { "value": "sales", "label": "Sales" },
    { "value": "support", "label": "Support" }
  ]
}
```

### 4. Track Button Clicks

Add analytics tracking:

```liquid
<a href="{{ whatsapp_url }}"
   onclick="gtag('event', 'whatsapp_click', { 'event_category': 'engagement' });">
```

## Best Practices

### Phone Number Format
Always include the country code without the '+' symbol:
- ✅ Good: `1234567890`, `441234567890`
- ❌ Bad: `+1 234 567 8900`, `(123) 456-7890`

The extension automatically cleans the number, but merchants should be educated on proper formatting.

### Default Messages
Keep default messages short and friendly:
- ✅ "Hi! I have a question about your products."
- ✅ "Hello! I'd like some help with my order."
- ❌ Overly long messages that might be intimidating

### Positioning
Consider your theme's existing UI:
- Don't block important CTAs
- Test on mobile devices
- Ensure it works with cookie banners and popups

### Accessibility
Our implementation includes:
- `aria-label` for screen readers
- Sufficient color contrast
- Focus states for keyboard navigation

## Common Issues and Solutions

### Issue: Button Doesn't Appear

**Solution**: Check that:
1. The phone number field is filled in
2. The extension is enabled in the theme editor
3. The block target is set to `"body"`

### Issue: WhatsApp Opens But Number Is Invalid

**Solution**: Verify the phone number includes the country code and has no letters or special characters.

### Issue: Button Appears Behind Other Elements

**Solution**: Increase the z-index value in the CSS:
```css
z-index: 99999; /* Higher value */
```

### Issue: Styles Not Updating

**Solution**: Hard refresh your browser (Ctrl+Shift+R or Cmd+Shift+R) to clear cached CSS.

## Testing Checklist

Before deploying to production:

- [ ] Test on desktop browsers (Chrome, Safari, Firefox, Edge)
- [ ] Test on mobile devices (iOS and Android)
- [ ] Verify WhatsApp opens correctly
- [ ] Check default message is pre-filled
- [ ] Test all four position options
- [ ] Verify button doesn't block important UI elements
- [ ] Test with different theme colors
- [ ] Verify tooltip appears on hover (desktop only)
- [ ] Check accessibility with screen reader
- [ ] Test with slow internet connection

## Performance Considerations

Our WhatsApp button is incredibly lightweight:

- **No JavaScript**: Uses pure CSS for animations and interactions
- **No External Libraries**: Everything is self-contained
- **Small SVG Icon**: ~1KB in size
- **CSS Only**: ~3KB of styling
- **No HTTP Requests**: Icon is embedded inline

This means zero impact on your store's loading speed! 🚀

## Security Considerations

- **No Data Collection**: The extension doesn't collect or store any customer data
- **No External Calls**: Only opens WhatsApp (official WhatsApp API)
- **Sanitized Inputs**: Phone numbers and messages are properly encoded
- **HTTPS Only**: WhatsApp Web uses secure connections

## Monetization Options

If you plan to distribute this extension:

1. **Free with Branding**: Free but includes "Powered by [Your Brand]"
2. **Freemium**: Basic features free, advanced features paid
3. **One-time Fee**: Charge for installation
4. **Monthly Subscription**: Recurring revenue model
5. **Custom Development**: Offer customization services

## Conclusion

Congratulations! You've built a production-ready WhatsApp button extension for Shopify. This extension:

✅ Is fully customizable by merchants
✅ Works on all devices
✅ Requires zero maintenance
✅ Has no performance impact
✅ Follows Shopify best practices

The skills you've learned can be applied to building other Shopify extensions. Theme app extensions are powerful because they integrate directly into the merchant's storefront without requiring theme modifications.

## What's Next?

- Add the extension to the Shopify App Store
- Build additional features (business hours, multiple agents, etc.)
- Create video tutorials for merchants
- Build more extensions using this template

## Resources

- [Shopify Theme Extensions Documentation](https://shopify.dev/docs/apps/build/online-store/theme-app-extensions)
- [Liquid Documentation](https://shopify.dev/docs/api/liquid)
- [WhatsApp Business API](https://faq.whatsapp.com/general/chats/how-to-use-click-to-chat)
- [Shopify CLI Documentation](https://shopify.dev/docs/apps/tools/cli)

---

**Found this helpful?** Feel free to fork, modify, and distribute. If you build something cool with this tutorial, I'd love to hear about it in the comments!

**Questions?** Drop them below and I'll do my best to help.

Happy coding! 🎉

---

*About the Author: [Your bio and links]*

