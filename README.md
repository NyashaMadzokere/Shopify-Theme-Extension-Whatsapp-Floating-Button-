# WhatsApp Button - Shopify Theme Extension

A floating WhatsApp button extension for Shopify stores that allows customers to instantly connect with merchants via WhatsApp.

## Features

- 🎨 **Fully Customizable** - Control colors, sizes, positions, and styles
- 📱 **Mobile Responsive** - Optimized display for all devices
- 💬 **Default Messages** - Pre-fill WhatsApp chats with custom messages
- 🎯 **Flexible Positioning** - Place button in any corner of the screen
- ✨ **Hover Tooltips** - Optional tooltips to guide customers
- ⚡ **Lightweight** - No JavaScript dependencies, pure CSS + Liquid

## Customization Options

Merchants can customize the following settings from the Shopify Theme Editor:

### WhatsApp Settings
- **Phone Number** - Enter the WhatsApp business number (with country code)
- **Default Message** - Pre-filled message when chat opens

### Button Style
- **Button Background Color** - Default: WhatsApp green (#25D366)
- **Icon Color** - Default: White
- **Button Size** - Adjustable for desktop (40-100px) and mobile (40-80px)
- **Icon Size** - Adjustable (50-80%)
- **Border Radius** - Control roundness (0-50%)

### Position
- **Button Position** - Choose from 4 corners (Bottom Right, Bottom Left, Top Right, Top Left)
- **Spacing** - Adjust distance from edges for desktop and mobile separately

### Tooltip
- **Show Tooltip** - Enable/disable hover tooltip
- **Tooltip Text** - Customize tooltip message
- **Tooltip Colors** - Background and text colors

## Installation & Setup

### Requirements

1. [Node.js](https://nodejs.org/en/download/) installed
2. [Shopify Partner Account](https://partners.shopify.com/signup)
3. A development store or Shopify Plus sandbox store for testing

### Development

1. Navigate to the project directory:
```shell
cd whatsapp-button
```

2. Start the development server:
```shell
npm run dev
# or
shopify app dev
```

3. Open the generated URL and grant permissions to the app

### Deploying the Extension

1. Deploy the extension:
```shell
shopify app deploy
```

2. Install the app on your Shopify store

### Merchant Setup

After installation, merchants can add the WhatsApp button to their store:

1. Go to **Online Store** > **Themes**
2. Click **Customize** on your active theme
3. In the theme editor, click **Add app block**
4. Select **WhatsApp Button** from the list
5. Configure the settings:
   - Enter your WhatsApp number (include country code)
   - Set a default message
   - Customize colors, size, and position
6. Click **Save**

The floating WhatsApp button will now appear on your storefront!

## How It Works

The extension creates a floating button that:
1. Opens WhatsApp Web/App when clicked
2. Pre-fills the chat with the merchant's phone number
3. Optionally includes a default message
4. Works on all devices (desktop and mobile)

## Example Phone Number Formats

The extension accepts various phone number formats:
- `1234567890`
- `+1 234 567 8900`
- `+44 20 1234 5678`
- `(555) 123-4567`

The extension automatically cleans the number for WhatsApp compatibility.

## Preview

The button appears as a floating circle with the WhatsApp icon. On hover (desktop), it displays a customizable tooltip. On mobile, it's slightly smaller and tooltips are hidden for better UX.

## Developer resources

- [Introduction to Shopify apps](https://shopify.dev/docs/apps/getting-started)
- [App extensions](https://shopify.dev/docs/apps/build/app-extensions)
- [Extension only apps](https://shopify.dev/docs/apps/build/app-extensions/build-extension-only-app)
- [Shopify CLI](https://shopify.dev/docs/apps/tools/cli)
