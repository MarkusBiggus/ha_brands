[![Deploys by netlify](https://www.netlify.com/img/global/badges/netlify-color-bg.svg)](https://www.netlify.com)

# Home Assistant Brands

This repository holds the icons and logos for all the brands Home Assistant
supports.

This repository is used to generate a static website, serving these images
for use in our Home Assistant projects. The goal is to have a centralized
repository of brand images.

## Inner workings

This repository provides two main folders to store images in:

- `core_integrations`: Contains images for integrations bundled with the
  Home Assistant Core.
- `custom_integrations`: Contains images for custom integrations
  (custom components).

Each of these two main folders contain domain folders. Each domain folder is
named to the integration `domain` and must match the domain set in the
integration `manifest.json` file.

A domain folder can contain four files:

- `icon.png`: A square avatar-like icon, representing the brand or product for that domain.
- `logo.png`: The logo of the brand or product for that domain.
- `icon@2x.png`: hDPI version of `icon.png`
- `logo@2x.png`: hDPI version of `logo.png`

Those images are served in the following format:

- `https://brands.home-assistant.io/[domain]/icon.png`
- `https://brands.home-assistant.io/[domain]/logo.png`
- `https://brands.home-assistant.io/[domain]/icon@2x.png`
- `https://brands.home-assistant.io/[domain]/logo@2x.png`
- `https://brands.home-assistant.io/_/[domain]/icon.png`
- `https://brands.home-assistant.io/_/[domain]/logo.png`
- `https://brands.home-assistant.io/_/[domain]/icon@2x.png`
- `https://brands.home-assistant.io/_/[domain]/logo@2x.png`

### Missing image handling

The website can service images with and without a fallback to a placeholder
image.

### Without placeholder fallback

This method uses the plain URLs, **WITHOUT** the `/_/` in the URL path.
A missing image will result in status 404 being served.

For example: <`https://brands.home-assistant.io/[domain]/icon.png`>

- If a domain is missing the `icon.png` file, status 404 will be served
- If a domain is missing the `logo.png` file, the `icon.png` is served instead (if available).
- If a domain is missing the `icon@2x.png` file, the `icon.png` is served instead (if available).
- If a domain is missing the `logo@2x.png` file:
  - the `logo.png` file is served (if available).
  - the `icon@2x.png` file is served if available when `logo.png` is missing
- If an image optimised for dark themes (image is prefixed with 'dark_') is missing, its non-prefixed match will be served instead (if available).

### With placeholder fallback

This method uses the plain URLs, **WITH** the `/_/` in the URL path.
A missing image will result in placeholder image being served telling the logo/icon is missing.
This also applies to domains, in case the integration domain is missing.

For example: <`https://brands.home-assistant.io/_/[domain]/icon.png`>

### Caching

All icons and logos are cached by browsers for 7 days, so additions and changes may take time to reach all users. This gives users the full benefits of local caching with minimal revalidation, and protects against missing content during an internet outage.

Images are simultaneously cached by Cloudflare for 24 hours. This allows changes to begin being distributed to users relatively quickly without losing the CDN benefits.  It also guarantees a simple refresh (F5) will bring content no more than 1 day old.

The Cloudflare cache is also fully flushed in each major version of Home Assistant Core.

## Image specification

All images must have the following requirements:

- The filetype of all images must be PNG.
- They should be properly compressed and optimized (lossless is preferred) for use on the web.
- Interlaced is preferred (also known as progressive).
- Images with transparency are preferred.
- If multiple images are available, those optimized for a white background are preferred.
- Images optimized for a dark background should be prefixed with `dark_`
- The image should be trimmed, so it contains the minimum amount of empty space on the edges.

  This includes things like solid color borders or transparent spacing around the actual subject in the image.
- Custom integrations must not use Home Assistant branded images, as this might confuse the end-user into thinking that the integration is an internal/official integration.

### Icon image requirements

Additional to the general image requirements listed above, for the icon image,
the following requirements are applied as well:

- Aspect ratio must be 1:1 (square).
- Icon size must be 256x256 pixels, for the hDPI this is 512x512 pixels.
- The maximum icon pixel size is preferred.

### Logo image requirements

Additional to the general image requirements listed, for the logo image,
the following requirements are applied as well:

- Landscape aspect ratio is preferred.
- Aspect ratio should respect the logo of the brand.
- The shortest side of the image must be at least 128 pixels, 256 pixels for the hDPI version.
- The shortest side of the image must be no bigger than 256 pixels, 512 pixels for the hDPI version.
- The maximum pixel size for the shortest side of the image is preferred.

## Using the same image for logo & icon

If the brand uses the same image for the logo and icon (e.g., if the logo has a square aspect ratio),
only add the icon images. The icon will be used as a fallback for the logo.

## Using the same logo & icon for different brands

To keep the size of this repository as efficient as possible,
symlinking domain folders for the same icon/logos is allowed for core integrations. The deployment
process at our hosting provider will unpack these symlinks to actual files
during the deployment process.

Please note, symlinks should only be created between integration domain
directories. The `_placeholder` & `_homeassistant` directories are special
cases and new directories with an underscore (`_`) should not be created.

Symlinks are currently not allowed in the custom integrations folder.

The names of directories must always match the integration domain. Additional
directories are not allowed.

## Domain name conflict between custom and core integrations

It is possible for a custom integration and a core integration to collide on
their `domain` name. In these cases, the core integration domain name takes precedence.

## Tips, Tools & Resources

When adding a new set of icons and logos, the following resources can help you
finding the needed images and getting them to match our specifications:

- [**RedKetchup Image Resizer**](https://redketchup.io/image-resizer):
  Resizes most images formats, including SVG, into any format using just your
  browser.
- [**Worldvectorlogo**](https://worldvectorlogo.com/):
  Thousands of SVG brand images, which are perfect to use as a base.
- [**Wikimedia Commons**](https://commons.wikimedia.org/):
  Has a lot of good quality images on file.

A lot of brands (especially the larger ones) often offer a press kit on
their (corporate) website, that contains high quality images.

## Trademark Legal Notices

All product names, trademarks and registered trademarks in the images in this
repository, are property of their respective owners. All images in this
repository are used by the Home Assistant project for identification purposes
only.

The use of these names, trademarks and brands appearing in these image files,
does not imply endorsement.
