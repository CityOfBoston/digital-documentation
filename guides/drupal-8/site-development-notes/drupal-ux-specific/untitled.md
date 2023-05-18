# Image Styles & UX

## Image folder structure

```
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
    ├── icons                          ICON LIBRARY ROOT FOLDER
    |   ├── circle                     - from designers
    |   ├── department                 - from designers
    |       └── red                    - from designers
    |   ├── drupal                     - from designers
    |   ├── experiential               - from designers
    |   └── map                        - from designers
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
    |   ├── icons(legacy)              * Holds svg icon files 
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

| Entity                         | Field                | min/max resolution & max filesize | View: Style                                                                                                                                                                                                           |
| ------------------------------ | -------------------- | --------------------------------: | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Images**                     |                      |                                   |                                                                                                                                                                                                                       |
| node:department\_profile       | field\_icon          |                  56x56/++ - 200KB | <p>default: (i) square_icon_56px<br>Article: (i) square_icon_56px<br>Card: (i) square_icon_56px<br>Article: not displayed<br>Published By: (i) square_icon_56px</p>                                                   |
| node:event                     | field\_intro\_image  |                  1440x396/++ 8 MB | <p>default: (b) intro_image_fields<br>featured_item: (i) Featured Item Thumbnail</p>                                                                                                                                  |
|                                | field\_thumbnail     |                   525x230/++ 8 MB | <p>default: (b) thumbnail_event<br>featured_item: (p) thumbnail_event</p>                                                                                                                                             |
| node:how\_to                   | field\_intro\_image  |                  1440x396/++ 8 MB | <p>default: (b) intro_image_fields<br>[all others (10)] not displayed</p>                                                                                                                                             |
| node:listing\_page             | field\_intro\_image  |                   1440x396/++ 8MB | <p>default: (b) intro_image_fields<br>[all others (12)]: not displayed</p>                                                                                                                                            |
| node:person\_profile           | field\_person\_photo |                    350x350/++ 5MB | <p>default: (p) person_photos<br>listing: (p) person_photos<br>embed: (p) person_photos</p>                                                                                                                           |
| node:place\_profile            | field\_intro\_image  |                   1440x396/++ 8MB | <p>default: (b) intro_image_fields<br>Listing: (p) card_images<br>Teaser: not displayed</p>                                                                                                                           |
| node:post                      | field\_intro\_image  |                   1440x396/++ 8MB | <p>default: (b) intro_image_fields<br>featured_item: not displayedListing: not displayed<br>Listing short: not displayed<br>Teaser: not displayed</p>                                                                 |
|                                | field\_thumbnail     |                    700x700/++ 5MB | <p>default: not displayed<br>featured_item: (p) featured_images<br>Listing: (i) News Item -thumbnail (725x725)<br>Listing short: (i) News Item -thumbnail (725x725)<br>Teaser: (i) News Item -thumbnail (725x725)</p> |
| node:program\_i\_p             | field\_intro\_image  |                   1440x396/++ 8MB | <p>default: (b) intro_image_fields <br>listing: (b) card_images</p>                                                                                                                                                   |
|                                | field\_program\_logo |                    800x800/++ 2MB | <p>default: (p) logo_images<br>Listing: not displayed</p>                                                                                                                                                             |
| node:site\_alert               | field\_icon          |                  56x56/++ - 200KB | <p>default: (s) n/a svg (square_icon_56px)<br>Embed: (i) square_icon_56px<br>Teaser: not displayed</p>                                                                                                                |
| node:status\_item              | field\_icon          |                  65x65/++ - 200KB | <p>default: (s) n/a svg (square_icon_65px)<br>listing: (s) n/a svg (square_icon_65px)<br>teaser: (s) n/a svg (square_icon_65px)</p>                                                                                   |
| node:tabbed\_content           | field\_intro\_image  |                   1440x396/++ 8MB | default: (b) intro\_image\_fields                                                                                                                                                                                     |
| node:topic\_page               | field\_intro\_image  |                   1440x396/++ 8MB | <p>default: (b) intro_image_fields<br>featured_topic not displayed<br>listing_long: (b) intro_image_fields<br>listing: (b) card_images</p>                                                                            |
|                                | field\_thumbnail     |                                   | <p>default: not displayed<br>featured_topic (p) featured_images: not displayed<br>listing: not displayed<br>listing_long: not displayed</p>                                                                           |
| para:card                      | field\_thumbnail     |                    670x235/++ 2MB | default: (b) card\_images                                                                                                                                                                                             |
| para:columns                   | field\_image         |                    200x200/++ 2MB | default: (i) Med Small Square (also Person photo a-mobile 1x (110x110))                                                                                                                                               |
| para:fyi                       | field\_icon          |                    56x56/++ 200KB | default: (s) n/a svg (square\_icon\_56px)                                                                                                                                                                             |
| para:hero\_image               | field\_image         |                  1440x800/++ 8 MB | <p>default: (b) Hero fixed image fields<br>Separated Title: not displayed</p>                                                                                                                                         |
| para:map                       | field\_image         |                  1440x800/++ 8 MB | default: (b) Photo Bleed Images                                                                                                                                                                                       |
| para:photo                     | field\_image         |                  1440x800/++ 8 MB | default: (b) Photo Bleed Images                                                                                                                                                                                       |
| para:quote                     | field\_person\_photo |                   350x350/++ 5 MB | default: (i) Person photo a-mobile 1x (110x110)                                                                                                                                                                       |
| para:signup\_emergency\_alerts | field\_icon          |                           n/a svg | default: (s) n/a svg (square\_icon\_65px)                                                                                                                                                                             |
| para:transactions              | field\_icon          |                  180x100/++ - 2MB | <p>default: (i) transaction_icon_180x100<br>group_of_links: (i) transaction_icon_180x100</p>                                                                                                                          |
| para:video                     | field\_image         |                  1440x800/++ 8 MB | default: (b) Photo Bleed Images                                                                                                                                                                                       |
| tax:features                   | field\_icon          |                               svg | <p>default: (s) n/a svg (square_icon_56px)<br>sidebar_right: (s) n/a svg (square_icon_56px)</p>                                                                                                                       |
| entity:user                    | user\_picture        |            100x100/1024/1024 1 MB | <p>default: (p) person_photos<br>compact: (i) Person photo a-mobile 1x (110x110)</p>                                                                                                                                  |
| entity:media.image             | image                |                +++/2400/2400 8 MB | <p>default: (i) original image <br>[all others]: (i) Media Fixed Height (100px)</p>                                                                                                                                   |
| **Files**                      |                      |                                   |                                                                                                                                                                                                                       |
| media.document                 | field\_document      |                                   |                                                                                                                                                                                                                       |
| node:procurement               | field\_document      |                                   |                                                                                                                                                                                                                       |
| para:document                  | field\_document      |                                   |                                                                                                                                                                                                                       |

**Key**\
\++ = not specified (unlimited)\
(b) = background, responsive.\
(p) = HTML5 Picture, responsive.\
(i) = Image, svg or picture, non-reponsive.

## (s) = svg.

## Site Breakpoints

The following breakpoint grups and breakpoints are defined in D8:

| Breakpoint        | Start width | end width | note                                           |
| ----------------- | ----------- | --------- | ---------------------------------------------- |
| **group: hero**   |             |           |                                                |
| mobile            | 0           | 419       |                                                |
| tablet            | 420         | 767       |                                                |
| desktop           | 768         | 1439      |                                                |
| large             | 1440        | 1919      | Introduced in D8                               |
| oversize          | 1920        | +++       | have a notional max-width of 2400px            |
| **group: card**   |             |           |                                                |
| mobile            | 0           | 419       |                                                |
| tablet            | 420         | 767       |                                                |
| desktop           | 768         | 839       |                                                |
| desktop           | 840         | 1439      |                                                |
| large             | 1440        | 1919      |                                                |
| oversize          | 1920        | +++       | have a notional max-width of 2400px            |
| **group: person** |             |           |                                                |
| mobile            | 0           | 839       |                                                |
| tablet            | 840         | 979       |                                                |
| desktop           | 980         | 1279      | There is also a breakpoint at 1300 in node:pip |
| desktop           | 1280        | +++       | have a notional max-width of 2400px            |

## Responsive Styles

| Breakpoint                                                                                                                                                                   | responsive Style                   | style                              | size     |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------- | ---------------------------------- | -------- |
| <p><strong>All Nodes: field_intro_image</strong><br>(excluding node:post)</p>                                                                                                |                                    |                                    |          |
| hero: mobile (<419px)                                                                                                                                                        | intro\_image\_fields               | Intro image a-mobile 1x            | 420x115  |
| hero: tablet (420-767px)                                                                                                                                                     | intro\_image\_fields               | Intro image b-tablet 1x            | 768x215  |
| hero: desktop (768-1439x)                                                                                                                                                    | intro\_image\_fields               | Intro image c-desktop 1x           | 1440x396 |
| hero: large (1440-1919px)                                                                                                                                                    | intro\_image\_fields               | Intro image d-large 1x             | 1920x528 |
| hero: oversize (>1920px)                                                                                                                                                     | intro\_image\_fields               | Intro image e-oversize 1x          | 2400x660 |
| **node:post field\_intro\_image**                                                                                                                                            |                                    |                                    |          |
| hero: mobile (<419px)                                                                                                                                                        | Hero fixed image fields            | Hero fixed a-mobile 1x             | 420x270  |
| hero: tablet (420-767px)                                                                                                                                                     | Hero fixed image fields            | Hero fixed b-tablet 1x             | 768x400  |
| hero: desktop (768-1439x)                                                                                                                                                    | Hero fixed image fields            | Hero fixed c-desktop 1x            | 1440x460 |
| hero: large (1440-1919px)                                                                                                                                                    | Hero fixed image fields            | Hero fixed d-large 1x              | 1920x460 |
| hero: oversize (>1920px)                                                                                                                                                     | Hero fixed image fields            | Hero fixed e-oversize 1x           | 2400x460 |
| <p><strong>para:photo field_image</strong><br><strong>para:video field_image</strong><br><strong>para:hero field_image</strong><br><strong>para:map field_image</strong></p> |                                    |                                    |          |
| hero: mobile (<419px)                                                                                                                                                        | Photo Bleed Images                 | Photo bleed a-mobile 1x            | 420x250  |
| hero: tablet (420-767px)                                                                                                                                                     | Photo Bleed Images                 | Photo bleed b-tablet 1x            | 768x420  |
| hero: desktop (768-1439x)                                                                                                                                                    | Photo Bleed Images                 | Photo bleed c-desktop 1x           | 1440x800 |
| hero: large (1440-1919px)                                                                                                                                                    | Photo Bleed Images                 | Photo bleed d-large 1x             | 1920x800 |
| hero: oversize (>1920px)                                                                                                                                                     | Photo Bleed Images                 | Photo bleed e-oversize 1x          | 2400x800 |
| **find**                                                                                                                                                                     |                                    |                                    |          |
| card: mobile (<419px)                                                                                                                                                        | Card Images 3w                     | Card grid vertical a-mobile 1x     | 335x117  |
| card: tablet (420-767px)                                                                                                                                                     | Card Images 3w                     | Card grid vertical b-tablet 1x     | 615x215  |
| card: desktop (768-839px)                                                                                                                                                    | Card Images 3w                     | Card grid vertical c-desktop 1x    | 670x235  |
| card: desktop (840-1439x)                                                                                                                                                    | Card Images 3w                     | Card grid horizontal c-desktop 1x  | 382x134  |
| card: large (1440-1919px)                                                                                                                                                    | Card Images 3w                     | Card grid horizontal d-large 1x    | 382x134  |
| card: oversize (>1920px)                                                                                                                                                     | Card Images 3w                     | Card grid horizontal e-oversize 1x | 382x134  |
| **para:column**                                                                                                                                                              | this should be a 200x200 circle ?? |                                    |          |
| card: mobile (<419px)                                                                                                                                                        | Card Images 3w                     | Photo bleed a-mobile 1x            | 335x117  |
| card: tablet (420-767px)                                                                                                                                                     | Card Images 3w                     | Photo bleed b-tablet 1x            | 615x215  |
| card: desktop (768-839px)                                                                                                                                                    | Card Images 3w                     | Photo bleed c-desktop 1x           | 670x235  |
| card: desktop (840-1439x)                                                                                                                                                    | Card Images 3w                     | Photo bleed c-desktop 1x           | 382x134  |
| card: large (1440-1919px)                                                                                                                                                    | Card Images 3w                     | Photo bleed d-large 1x             | 382x134  |
| card: oversize (>1920px)                                                                                                                                                     | Card Images 3w                     | Photo bleed e-oversize 1x          | 382x134  |
| **post:field\_thumbnail(feature)**                                                                                                                                           |                                    |                                    |          |
| card: mobile (<419px)                                                                                                                                                        | Featured Images                    | Featured image a-mobile 1x         | 335x350  |
| card: tablet (420-767px)                                                                                                                                                     | Featured Images                    | Featured image b-tablet 1x         | 614x350  |
| card: desktop (768-839px)                                                                                                                                                    | Featured Images                    | Featured image c-desktop 1x        | 671x388  |
| card: desktop (840-1439x)                                                                                                                                                    | Featured Images                    | Featured image d-full 1x           | 586x388  |
| card: large (1440-1919px)                                                                                                                                                    | Featured Images                    | Featured image d-full 1x           | 586x388  |
| card: oversize (>1920px)                                                                                                                                                     | Featured Images                    | Featured image d-full 1x           | 586x388  |
| <p><strong>node:person_profile:field_person_profile</strong><br><strong>user:user_picture</strong></p>                                                                       |                                    |                                    |          |
| person: mobile (<839px)                                                                                                                                                      | Person Photos                      | Person Photos a-mobile 1x          | 110x110  |
| person: tablet (840-979px)                                                                                                                                                   | Person Photos                      | Person Photos b-tablet 1x          | 120x120  |
| person: desktop (980-1279px)                                                                                                                                                 | Person Photos                      | Person Photos c-desktop 1x         | 148x148  |
| person: desktop (>1280x)                                                                                                                                                     | Person Photos                      | Person Photos d-full 1x            | 173x173  |
| **node:pip:field\_program\_logo**                                                                                                                                            |                                    |                                    |          |
| person: mobile (<839px)                                                                                                                                                      | Logo Images                        | logo square a-mobile 1x            | 672x672  |
| person: tablet (840-979px)                                                                                                                                                   | Logo Images                        | logo square b-tablet 1x            | 783x783  |
| person: desktop (980-1279px)                                                                                                                                                 | Logo Images                        | logo square c-desktop 1x           | 360x360  |
| person: desktop (>1280x)                                                                                                                                                     | Logo Images                        | logo square d-full 1x              | 360x360  |
