This page lists the stylistic changes made to the upstream branch. Ideally
it's kept to a minimum to make it easier to keep it in sync with upstream.

Because front-end code is so path-dependent, maintaining the fork through
branches and merges is kind of annoying, when the number of changes made is
relatively minial. Rather than constantly trying to resolve merge conflicts,
this page lists the changes, and when pulling from upstream gives me too
conflicts I "restart" the fork from this page.

## Typography


```
--- a/resources/variables.less
+++ b/resources/variables.less
-@content-line-height: 1.6;
-@content-margin-top: 0.8rem;
+@content-line-height: 1.4;
+@content-margin-top: 0.5em;

--- a/resources/skins.citizen.styles/common/content.less
+++ b/resources/skins.citizen.styles/common/content.less
@@ -94,10 +94,7 @@
        }

        h1,
-       h2 {
-               margin-top: @content-margin-top * 3;
-       }
-
+       h2,
        h3,
        h4,
        h5,

@@ -107,17 +107,16 @@
        h3 + h4,
        h4 + h5,
        h5 + h6,
-       p,
        table {
                margin-top: @content-margin-top;
        }
@@ -120,18 +119,16 @@
        ul {
-               margin: @content-margin-top 0 0 @content-margin-top * 2;
-
-               ul {
-                       margin: 0 0 0 @content-margin-top * 2;
-               }
+               margin-left: 24px;
        }

        ol {
-               margin: @content-margin-top 0 0 @content-margin-top * 3;
+               margin-left: 30px;
+       }

-               ol {
-                       margin: 0 0 0 @content-margin-top * 3;
+       ul, ol {
+               ul, ol {
+                       margin-top: 0;
                }
        }

--- a/resources/skins.citizen.styles/common/typography.less
+++ b/resources/skins.citizen.styles/common/typography.less
@@ -97,11 +97,12 @@
        & h1,
        &-content h1 {
                font-size: @content-h1-size;
+               font-weight: normal;
+               font-family: @fonts-serif;
        }

        &-content h1,
        &-content h2 {
-               font-weight: bold;
                line-height: 1.2;
        }

@@ -115,6 +116,13 @@

                h2 {
                        font-size: @content-h2-size;
+
+                       &:not(#mw-toc-heading) {
+                               font-weight: normal;
+                               font-family: @fonts-serif;
+                               padding-bottom: 0.2em;
+                               border-bottom: 3px double var(--color-base--subtle);
+                       }
                }

--- a/resources/skins.citizen.styles/common/wikitable.less
+++ b/resources/skins.citizen.styles/common/wikitable.less
@@ -8,30 +8,29 @@ table.wikitable {
        caption {
                margin-top: @content-margin-top;
                font-weight: 600;
+               font-size: 1.1em;
                text-align: left;
        }

        tr {
-               vertical-align: top;
+               vertical-align: middle;

                th {
                        background-color: transparent;
-                       color: var( --color-base--subtle );
-                       font-size: @content-caption-size;
-                       font-weight: 500;
-               }
-
-               td {
-                       font-size: @content-monospace-size;
+                       font-weight: bold;
                }

                th,
                td {
-                       padding: @margin-side / 2 @margin-side @margin-side / 2 0;
+                       padding: @margin-side / 4;
                        border: 0;
                        border-bottom: 1px solid var( --border-color-base );
                }

+               th.headerSort {
+                       padding-right: @margin-side;
+               }

--- a/resources/variables.less
+++ b/resources/variables.less
-@content-line-height: 1.6;
-@content-margin-top: 0.8rem;
+@content-line-height: 1.4;
+@content-margin-top: 0.5em;

--- a/skinStyles/extensions/Cite/ext.cite.styles.less
+++ b/skinStyles/extensions/Cite/ext.cite.styles.less
@@ -8,12 +8,15 @@
 .mw-references-wrap {
        margin-top: @content-margin-top;
        color: var( --color-base );
-       font-size: @content-small-text-size;
+}
+
+.mw-references-columns {
+       column-width: 25em;
 }

 .mw-body-content {
        .references {
-               margin: 0 @content-margin-top * 2;
+               margin: 0 0 0 @content-margin-top * 2;

                li {
                        margin-bottom: @content-margin-top / 4;
@@ -21,10 +24,6 @@
        }
 }

-span.reference {
-       font-size: 80%;
-}
```

## Colors

Check branch `feat/link-color`

```
||| /resources/variables.less
@@ somewhere
@dark-color-base--subtle: rgba( 255, 255, 255, 0.5 );
@dark-color-base: rgba( 255, 255, 255, 0.7 );

--- a/resources/skins.citizen.styles/Pagetools.less
+++ b/resources/skins.citizen.styles/Pagetools.less
@@ -156,28 +156,6 @@
        #ca-edit {
                order: 99; // Align to last
        }
-
-       #ca-edit,
-       #ca-ve-edit {
-               > a {
-                       background-color: var( --color-primary );
-                       color: #fff;
-
-                       &:after {
-                               filter: invert( 1 );
-                               // white icon
-                               opacity: 1;
-                       }
-
-                       &:hover {
-                               background-color: var( --color-primary--hover );
-                       }
-
-                       &:active {
-                               background-color: var( --color-primary--active );
-                       }
-               }
-       }
 }

```

## Image frame

```
--- a/resources/skins.citizen.styles/common/common.less
+++ b/resources/skins.citizen.styles/common/common.less
@@ -210,26 +210,13 @@ figcaption,
 .thumb {
        > .thumbinner {
                > a {
-                       transition: @transition-box-shadow-quick;
-
-                       &:hover:not( .lazy ):not( .new ) {
-                               .boxshadow(2);
-
-                               img {
-                                       transform: scale( 1.1 );
-                               }
-                       }
-
                        &:before {
                                content: none;
                        }

-                       &.new {
-                               display: block;
-                               padding: @margin-side / 2;
-                               background-color: var( --background-color-framed );
-                               transition: @transition-background-quick, @transition-color-quick;
-                       }
+                       display: block;
+                       margin: @margin-side / 2;
                }
        }
||| somewhere
img {
   filter: grayscale(30%);
}
```

## Fonts

```
||| /resources/variables.less
@@ /* Fonts */
/**
 * System sans-serif font stack
 *
 * `BlinkMacSystemFont` - Mac
 * `-apple-system` - Mac
 * `Segoe UI` - Windows
 * `Roboto` - Android
 */
@fonts: "BlinkMacSystemFont", -apple-system, "Segoe UI", Roboto, sans-serif;
/**
 * System serif font stack
 *
 * `Linux Libertine` - Linux
 * `Georgia` – Windows, Mac
 */
@fonts-serif: "Linux Libertine", Georgia, serif;
/**
 * System monospace font stack
 *
 * `SFMono-Regular` - macOS 10.12+
 * `Menlo` – macOS 10.6+
 * `Roboto Mono` - Android 4.0+
 * `Consolas` – Windows
 * `Liberation Mono` – Linux
 */
@fonts-monospace: 'SFMono-Regular', 'Menlo', 'Roboto Mono', 'Consolas', 'Liberation Mono', monospace;

--- a/skin.json
+++ b/skin.json
@@ -26,7 +26,6 @@
                                        "styles": [
                                                "skins.citizen.styles",
                                                "skins.citizen.styles.theme",
-                                               "skins.citizen.styles.fonts",
                                                "skins.citizen.styles.toc",
                                                "skins.citizen.icons",
                                                "skins.citizen.icons.ca",
@@ -103,15 +102,6 @@
                        "features": [],
                        "styles": [ "resources/skins.citizen.styles.theme/skins.citizen.styles.theme.less" ]
                },
-               "skins.citizen.styles.fonts": {
-                       "class": "ResourceLoaderSkinModule",
-                       "targets": [
-                               "desktop",
-                               "mobile"
-                       ],
-                       "features": [],
-                       "styles": [ "resources/skins.citizen.styles.fonts/skins.citizen.styles.fonts.less" ]
-               },
                "skins.citizen.styles.lazyload": {

```

## Header logo

```
--- a/resources/skins.citizen.styles/Logo.less
+++ b/resources/skins.citizen.styles/Logo.less
@@ -7,14 +7,3 @@
                color: var( --color-base--emphasized );
        }
 }
-
-.mw-logo-icon {
-       margin-bottom: 10px;
-}
-
-// Only show title when screen height is less than 800px
-@media ( max-height: 800px ) {
-       .mw-logo-icon {
-               display: none;
-       }
-}

--- a/resources/skins.citizen.styles/Siteinfo.less
+++ b/resources/skins.citizen.styles/Siteinfo.less
@@ -4,12 +4,11 @@
        overflow: hidden;
        flex-grow: 1;
        align-items: center;
-       margin-right: 10px;

-       a {
+       a:not(.mw-logo) {
                color: var( --color-base--emphasized );
                font-size: 16px;
-               text-overflow: ellipsis;
+               text-overflow: '-';
                transition: @transition-opacity-quick;
                white-space: nowrap;

@@ -23,38 +22,32 @@
        }
 }

+#header-logo {
+       .mw-logo-wordmark {
+               display: none;
+       }
+
+       .mw-logo {
+               margin-right: 12px;
+       }
+
+       .mw-logo-icon {
+               height: 42px;
+               width: 42px;
+       }
+}
+
 #header {
        &-sitetitle {
-               img.mw-logo-wordmark {
-                       width: auto;
-                       height: 14px;
-               }
+               display: none;
+               visibility: hidden;
        }

        &-pagetitle {
                overflow: hidden;
                color: var( --color-base--emphasized );
                font-weight: 500;
+               visibility: visible;
-               opacity: 0;
-               pointer-events: none;
-               visibility: hidden;
-       }
-}
-
-.skin-citizen--titlehidden {
-       #header {
-               &-sitetitle {
-                       position: absolute;
-                       opacity: 0;
-                       pointer-events: none;
-                       visibility: hidden;
-               }
-
-               &-pagetitle {
-                       opacity: 1;
-                       pointer-events: auto;
-                       visibility: visible;
-               }
        }
 }
--- a/templates/Header.mustache
+++ b/templates/Header.mustache
@@ -5,6 +5,7 @@
 <header class="mw-header">
        {{#data-drawer}}{{>Drawer}}{{/data-drawer}}
        <div id="mw-header-siteinfo">
+               <span id="header-logo">{{>Logo}}</span>
                <a id="header-sitetitle" class="mw-wiki-title" {{{html-mainpage-attributes}}}>{{>Wordmark}}</a>
                <a id="header-pagetitle" href="#top" title="{{msg-citizen-jumptotop}}">{{{html-title}}}</a>
        </div>
```

## Header shadow

```
--- a/resources/skins.citizen.styles/Header.less
+++ b/resources/skins.citizen.styles/Header.less
@@ -9,6 +9,11 @@
        display: flex;
        height: var( --height-header );
        justify-content: space-between;
+       pointer-events: none;
+
+       & > * {
+               pointer-events: auto;
+       }
@@ -28,7 +28,21 @@
                position: absolute;
                right: 0;
                left: 0;
-               box-shadow: 0 0 30px var( --height-header ) var( --background-color-dp-00 );
+               height: ~'calc( var( --height-header ) * 1.5 )';
+               /*
+                * ease(x) := 1 - pow(1 - x, 2);
+                */
+               background-image: linear-gradient(to bottom,
+                       ~"rgba(var(--background-color-dp-00--rgb),1)" 0%,
+                       ~"rgba(var(--background-color-dp-00--rgb),0.988)" 11.11%,
+                       ~"rgba(var(--background-color-dp-00--rgb),0.951)" 22.22%,
+                       ~"rgba(var(--background-color-dp-00--rgb),0.889)" 33.33%,
+                       ~"rgba(var(--background-color-dp-00--rgb),0.802)" 44.44%,
+                       ~"rgba(var(--background-color-dp-00--rgb),0.691)" 55.56%,
+                       ~"rgba(var(--background-color-dp-00--rgb),0.556)" 66.67%,
+                       ~"rgba(var(--background-color-dp-00--rgb),0.395)" 77.78%,
+                       ~"rgba(var(--background-color-dp-00--rgb),0.210)" 88.89%,
+                       ~"rgba(var(--background-color-dp-00--rgb),0)" 100%);
                content: '';
        }

--- a/resources/skins.citizen.styles/common/rootvariables.less
+++ b/resources/skins.citizen.styles/common/rootvariables.less
@@ -10,6 +10,7 @@
  */
 :root {
        /* Background Colors */
+       --background-color-dp-00--rgb: red(@background-color-dp-00), green(@background-color-dp-00), blue(@background-color-dp-00);
        --background-color-dp-00: @background-color-dp-00;
        --background-color-dp-01: @background-color-dp-01;
        --background-color-dp-02: @background-color-dp-02;
@@ -86,6 +87,7 @@ html {
  */
 :root.skin-citizen-dark {
        /* Background Colors */
+       --background-color-dp-00--rgb: red(@dark-background-color-dp-00), green(@dark-background-color-dp-00), blue(@dark-background-color-dp-00);
        --background-color-dp-00: @dark-background-color-dp-00;
```
