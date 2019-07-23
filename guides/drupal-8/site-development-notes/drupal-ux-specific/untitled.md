# Untitled

## Image folder structure

```text
sites/default/files
    ├── file                           DOCUMENT FILES ROOT
    |   ├── [dated]
    |   └── document_files
    |       ├── [year]
    |       └── unk
    ├── embed                          RICH-TEXT EDITOR ROOT
    |   ├── [file-firstchar]
    |   └── file
    |       └── [dated]
    ├── img                            IMAGE ROOT FOLDER
    |   ├── [dated]                     - migrated files from public:// (root)
    |   ├── columns                    * Paragraph columns 
    |   |   └── [dated]                 - migrated files
    |   ├── event                      * Node event
    |   |   ├── intro_images            - from field_intro_image
    |   |   └── thumbnails              - from field_thumbnail
    |   ├── hero_image                 * Paragraph hero_image 
    |   |   └── [dated]                 - migrated files
    |   ├── how_to                     * Node how_to 
    |   |   └── intro_images            - from field_intro_image
    |   ├── icons                      * Holds svg icon files 
    |   |   ├── department              
    |   |   |   └── [dated]             - migrated files
    |   |   ├── emergency                  
    |   |   |   └── [dated]             - migrated files
    |   |   ├── feature                  
    |   |   |   └── [dated]             - migrated files
    |   |   ├── fyi                      
    |   |   |   └── [dated]             - migrated files
    |   |   ├── site_alert                   
    |   |   |   └── [dated]             - migrated files
    |   |   ├── status                   
    |   |   |   └── [dated]             - migrated files
    |   |   └── transactions             
    |   |       └── [dated]             - migrated files
    |   ├── library                    * Holds photographic images 
    |   |   └── photos                   
    |   |       └── [dated]             - migrated files
    |   ├── listing_page               * Node listing_page 
    |   |   └── intro_images            - from field_intro_image
    |   ├── maps                       * Paragraph listing_page 
    |   |   └── [dated]                 - from field_intro_image
    |   ├── person_profile             * Node person_profile
    |   |   └── photos                  - from field_photos
    |   ├── place_profile              * Paragraph place_profile
    |   |   └── intro_images            - from field_intro_image
    |   ├── post                       * Node post
    |   |   ├── intro_images            - from field_intro_image
    |   |   └── thumbnails              - from field_thumbnail
    |   ├── program                    * Node program_initiative_profiles
    |   |   ├── intro_images            - from field_intro_image
    |   |   └── logo                    - from field_logo
    |   ├── quote_person               * Paragraph quote
    |   |   └── photos                  - from field_photos
    |   ├── tabbed                     * Node tabbed
    |   |   └── intro_images            - from field_intro_image
    |   ├── topic                      * Node topic_page (guide)
    |   |   ├── intro_images            - from field_intro_image
    |   |   └── thumbnails              - from field_thumbnail
    |   ├── unk                        * Unknown origin (to migration)
    |   |   ├── intro_images            
    |   |   └── thumbnails              
    |   ├── user                       * User object field.
    |   |   └── photos                  - from user_picture
    |   └── video                      * Paragraph video
    |       └── [dated]                 - migrated files
    ├── private                         PRIVATE FILES
    └── unk                             UNKNOWN ORIGIN (to migration) FILES
```

## Image styles used

| Entity | Field | min/max resolution & max filesize | View: Style |
| :--- | :--- | ---: | :--- |
| **Images** |  |  |  |
| node:department\_profile | field\_icon | 56x56/++ - 200KB | default: \(i\) square\_icon\_56px Article: \(i\) square\_icon\_56px Card: \(i\) square\_icon\_56px Article: not displayed Published By: \(i\) square\_icon\_56px |
| node:event | field\_intro\_image | 1440x396/++ 8 MB | default: \(b\) intro\_image\_fields featured\_item: \(i\) Featured Item Thumbnail |
|  | field\_thumbnail | 525x230/++ 8 MB | default: \(b\) thumbnail\_event featured\_item: \(p\) thumbnail\_event |
| node:how\_to | field\_intro\_image | 1440x396/++ 8 MB | default: \(b\) intro\_image\_fields \[all others \(10\)\] not displayed |
| node:listing\_page | field\_intro\_image | 1440x396/++ 8MB | default: \(b\) intro\_image\_fields \[all others \(12\)\]: not displayed |
| node:person\_profile | field\_person\_photo | 350x350/++ 5MB | default: \(p\) person\_photos listing: \(p\) person\_photos embed: \(p\) person\_photos |
| node:place\_profile | field\_intro\_image | 1440x396/++ 8MB | default: \(b\) intro\_image\_fields Listing: \(p\) card\_images Teaser: not displayed |
| node:post | field\_intro\_image | 1440x396/++ 8MB | default: \(b\) intro\_image\_fields featured\_item: not displayedListing: not displayed Listing short: not displayed Teaser: not displayed |
|  | field\_thumbnail | 700x700/++ 5MB | default: not displayed featured\_item: \(p\) featured\_images Listing: \(i\) News Item -thumbnail \(725x725\) Listing short: \(i\) News Item -thumbnail \(725x725\) Teaser: \(i\) News Item -thumbnail \(725x725\) |
| node:program\_i\_p | field\_intro\_image | 1440x396/++ 8MB | default: \(b\) intro\_image\_fields  listing: \(b\) card\_images |
|  | field\_program\_logo | 800x800/++ 2MB | default: \(p\) logo\_images Listing: not displayed |
| node:site\_alert | field\_icon | 56x56/++ - 200KB | default: \(s\) n/a svg \(square\_icon\_56px\) Embed: \(i\) square\_icon\_56px Teaser: not displayed |
| node:status\_item | field\_icon | 65x65/++ - 200KB | default: \(s\) n/a svg \(square\_icon\_65px\) listing: \(s\) n/a svg \(square\_icon\_65px\) teaser: \(s\) n/a svg \(square\_icon\_65px\) |
| node:tabbed\_content | field\_intro\_image | 1440x396/++ 8MB | default: \(b\) intro\_image\_fields |
| node:topic\_page | field\_intro\_image | 1440x396/++ 8MB | default: \(b\) intro\_image\_fields featured\_topic not displayed listing\_long: \(b\) intro\_image\_fields listing: \(b\) card\_images |
|  | field\_thumbnail |  | default: not displayed featured\_topic \(p\) featured\_images: not displayed listing: not displayed listing\_long: not displayed |
| para:card | field\_thumbnail | 670x235/++ 2MB | default: \(b\) card\_images |
| para:columns | field\_image | 200x200/++ 2MB | default: \(i\) Med Small Square \(also Person photo a-mobile 1x \(110x110\)\) |
| para:fyi | field\_icon | 56x56/++ 200KB | default: \(s\) n/a svg \(square\_icon\_56px\) |
| para:hero\_image | field\_image | 1440x800/++ 8 MB | default: \(b\) Hero fixed image fields Separated Title: not displayed |
| para:map | field\_image | 1440x800/++ 8 MB | default: \(b\) Photo Bleed Images |
| para:photo | field\_image | 1440x800/++ 8 MB | default: \(b\) Photo Bleed Images |
| para:quote | field\_person\_photo | 350x350/++ 5 MB | default: \(i\) Person photo a-mobile 1x \(110x110\) |
| para:signup\_emergency\_alerts | field\_icon | n/a svg | default: \(s\) n/a svg \(square\_icon\_65px\) |
| para:transactions | field\_icon | 180x100/++ - 2MB | default: \(i\) transaction\_icon\_180x100 group\_of\_links: \(i\) transaction\_icon\_180x100 |
| para:video | field\_image | 1440x800/++ 8 MB | default: \(b\) Photo Bleed Images |
| tax:features | field\_icon | svg | default: \(s\) n/a svg \(square\_icon\_56px\) sidebar\_right: \(s\) n/a svg \(square\_icon\_56px\) |
| entity:user | user\_picture | 100x100/1024/1024 1 MB | default: \(p\) person\_photos compact: \(i\) Person photo a-mobile 1x \(110x110\) |
| entity:media.image | image | +++/2400/2400 8 MB | default: \(i\) original image  \[all others\]: \(i\) Media Fixed Height \(100px\) |
| **Files** |  |  |  |
| media.document | field\_document |  |  |
| node:procurement | field\_document |  |  |
| para:document | field\_document |  |  |

**Key**  
++ = not specified \(unlimited\)  
\(b\) = background, responsive.  
\(p\) = HTML5 Picture, responsive.  
\(i\) = Image, svg or picture, non-reponsive.

## \(s\) = svg.

## Site Breakpoints

The following breakpoint grups and breakpoints are defined in D8:

| Breakpoint | Start width | end width | note |
| :--- | :--- | :--- | :--- |
| **group: hero** |  |  |  |
| mobile | 0 | 419 |  |
| tablet | 420 | 767 |  |
| desktop | 768 | 1439 |  |
| large | 1440 | 1919 | Introduced in D8 |
| oversize | 1920 | +++ | have a notional max-width of 2400px |
| **group: card** |  |  |  |
| mobile | 0 | 419 |  |
| tablet | 420 | 767 |  |
| desktop | 768 | 839 |  |
| desktop | 840 | 1439 |  |
| large | 1440 | 1919 |  |
| oversize | 1920 | +++ | have a notional max-width of 2400px |
| **group: person** |  |  |  |
| mobile | 0 | 839 |  |
| tablet | 840 | 979 |  |
| desktop | 980 | 1279 | There is also a breakpoint at 1300 in node:pip |
| desktop | 1280 | +++ | have a notional max-width of 2400px |

## Responsive Styles

| Breakpoint | responsive Style | style | size |
| :--- | :--- | :--- | :--- |
| **All Nodes: field\_intro\_image** \(excluding node:post\) |  |  |  |
| hero: mobile \(&lt;419px\) | intro\_image\_fields | Intro image a-mobile 1x | 420x115 |
| hero: tablet \(420-767px\) | intro\_image\_fields | Intro image b-tablet 1x | 768x215 |
| hero: desktop \(768-1439x\) | intro\_image\_fields | Intro image c-desktop 1x | 1440x396 |
| hero: large \(1440-1919px\) | intro\_image\_fields | Intro image d-large 1x | 1920x528 |
| hero: oversize \(&gt;1920px\) | intro\_image\_fields | Intro image e-oversize 1x | 2400x660 |
| **node:post field\_intro\_image** |  |  |  |
| hero: mobile \(&lt;419px\) | Hero fixed image fields | Hero fixed a-mobile 1x | 420x270 |
| hero: tablet \(420-767px\) | Hero fixed image fields | Hero fixed b-tablet 1x | 768x400 |
| hero: desktop \(768-1439x\) | Hero fixed image fields | Hero fixed c-desktop 1x | 1440x460 |
| hero: large \(1440-1919px\) | Hero fixed image fields | Hero fixed d-large 1x | 1920x460 |
| hero: oversize \(&gt;1920px\) | Hero fixed image fields | Hero fixed e-oversize 1x | 2400x460 |
| **para:photo field\_image** **para:video field\_image** **para:hero field\_image** **para:map field\_image** |  |  |  |
| hero: mobile \(&lt;419px\) | Photo Bleed Images | Photo bleed a-mobile 1x | 420x250 |
| hero: tablet \(420-767px\) | Photo Bleed Images | Photo bleed b-tablet 1x | 768x420 |
| hero: desktop \(768-1439x\) | Photo Bleed Images | Photo bleed c-desktop 1x | 1440x800 |
| hero: large \(1440-1919px\) | Photo Bleed Images | Photo bleed d-large 1x | 1920x800 |
| hero: oversize \(&gt;1920px\) | Photo Bleed Images | Photo bleed e-oversize 1x | 2400x800 |
| **find** |  |  |  |
| card: mobile \(&lt;419px\) | Card Images 3w | Card grid vertical a-mobile 1x | 335x117 |
| card: tablet \(420-767px\) | Card Images 3w | Card grid vertical b-tablet 1x | 615x215 |
| card: desktop \(768-839px\) | Card Images 3w | Card grid vertical c-desktop 1x | 670x235 |
| card: desktop \(840-1439x\) | Card Images 3w | Card grid horizontal c-desktop 1x | 382x134 |
| card: large \(1440-1919px\) | Card Images 3w | Card grid horizontal d-large 1x | 382x134 |
| card: oversize \(&gt;1920px\) | Card Images 3w | Card grid horizontal e-oversize 1x | 382x134 |
| **para:column** | this should be a 200x200 circle ?? |  |  |
| card: mobile \(&lt;419px\) | Card Images 3w | Photo bleed a-mobile 1x | 335x117 |
| card: tablet \(420-767px\) | Card Images 3w | Photo bleed b-tablet 1x | 615x215 |
| card: desktop \(768-839px\) | Card Images 3w | Photo bleed c-desktop 1x | 670x235 |
| card: desktop \(840-1439x\) | Card Images 3w | Photo bleed c-desktop 1x | 382x134 |
| card: large \(1440-1919px\) | Card Images 3w | Photo bleed d-large 1x | 382x134 |
| card: oversize \(&gt;1920px\) | Card Images 3w | Photo bleed e-oversize 1x | 382x134 |
| **post:field\_thumbnail\(feature\)** |  |  |  |
| card: mobile \(&lt;419px\) | Featured Images | Featured image a-mobile 1x | 335x350 |
| card: tablet \(420-767px\) | Featured Images | Featured image b-tablet 1x | 614x350 |
| card: desktop \(768-839px\) | Featured Images | Featured image c-desktop 1x | 671x388 |
| card: desktop \(840-1439x\) | Featured Images | Featured image d-full 1x | 586x388 |
| card: large \(1440-1919px\) | Featured Images | Featured image d-full 1x | 586x388 |
| card: oversize \(&gt;1920px\) | Featured Images | Featured image d-full 1x | 586x388 |
| **node:person\_profile:field\_person\_profile** **user:user\_picture** |  |  |  |
| person: mobile \(&lt;839px\) | Person Photos | Person Photos a-mobile 1x | 110x110 |
| person: tablet \(840-979px\) | Person Photos | Person Photos b-tablet 1x | 120x120 |
| person: desktop \(980-1279px\) | Person Photos | Person Photos c-desktop 1x | 148x148 |
| person: desktop \(&gt;1280x\) | Person Photos | Person Photos d-full 1x | 173x173 |
| **node:pip:field\_program\_logo** |  |  |  |
| person: mobile \(&lt;839px\) | Logo Images | logo square a-mobile 1x | 672x672 |
| person: tablet \(840-979px\) | Logo Images | logo square b-tablet 1x | 783x783 |
| person: desktop \(980-1279px\) | Logo Images | logo square c-desktop 1x | 360x360 |
| person: desktop \(&gt;1280x\) | Logo Images | logo square d-full 1x | 360x360 |

