# Theme Syntax Definition

The basic wireframe of a theme file is this: 

    {
        "name": "Solarized",
        "style": "Light", // or "Dark"
        "author": [
            // List of authors
        ],
        "version": "1.0",

        "editor": {
            // Editor attributes
        },
        
        "savedSearchesSidebar": {
            // Saved Searches sidebar attributes (optional)
        },
        
        "resultsList": {
            // List of notes/search results (optional)
        },

        "styles": {
            // Highlighter attributes
        }
    }

**Note:** The `//` should indicate a comment. Please keep in mind that JSON does not in fact support comment syntax like this. So make sure you don't put these inside your theme files during theme development.

## Root-Level Keys

List of all root-level attributes:

- `name`: String
- `style`: String, either "Light" or "Dark"; used to filter themes for dark mode
- `author`: Array of author detail objects
- `version`: String
- `editor`: a settings Object for the text editor; see details below
- `savedSearchesSidebar`: a settings Object for the Saved Searches sidebar; see details below
- `resultsList`: a settings Object for the search results sidebar, showing the note titles; see details below
- `styles`: a settings Object for text and Markdown highlighting; see details below

When I say "object" or "settings object", I mean a `{...}` delimited collection of sub-attributes.

Example of the preamble part, consisting of `name`, `style`, `author`, and `version`:

    "name": "My Theme",
    "style": "Light", // or "Dark"
    "author": [
        {
            // Free-form object; consider "name", "url", "email"
            "name": "Your Name",
            "twitter": "@you",
            "url": "http://yourdomain.free-exposure.com/"
        },
        {
            "name": "Maybe another author"
        }
    ],
    "version": "1.2", 
    // ...

## `editor` Settings

This is a special set of styles that apply to the app text editor itself:

- `backgroundColor`: hexadecimal color string for the app's background, like `"#93a1a1"`
- `caretColor`: hexadecimal color string for the insertion point in text fields, like `"#93a1a1"`
- `highlight`: settings object for search highlights, if enabled by the user; accepts `color`, `backgroundColor`, and `unfocusedBackgroundColor`
- `selection`: settings for user-selected text; accepts `color`, `unfocusedColor`, `backgroundColor`, and `unfocusedBackgroundColor`

The difference between `backgroundColor` and `unfocusedBackgroundColor` is that the latter will be used when the user clicks into another component, for example the Omnibar, to indicate the text editor is not receiving key events. In other words: use this to help users figure out if typing on the keyboard would overwrite the highlighted part.

### Example

    // ...
    "editor": {
        "backgroundColor": "#ffffff",
        "caretColor": "#000000",
        "highlight": {
            "color": "#000000"
            "backgroundColor": "#cccccc",
            "unfocusedBackgroundColor": "#dddddd"
        },
        "selection": {
            "color": "#000000"
            "backgroundColor": "#cccccc",
            "unfocusedBackgroundColor": "#dddddd"
        },
    }, 
    // ...

## `savedSearchesSidebar` Settings

This is an optional set of colors to style the "Saved Searches" sidebar differently. By default, it inherits colors from the editor to blend in. To keep the color variety limited and not produce a rainbow of effectful color combinations, consider reusing editor colors for search highlights or similar here if in doubt.

The accepted settings to override the default colors:

- `color`: optional hexadecimal color string for the label text; defaults to `style.base.color`
- `backgroundColor`: optional hexadecimal color string for the sidebar background; defaults to `editor.backgroundColor`
- `highlightedBackgroundColor`: optional hexadecimal color string for item background when they are clicked on to provide visual feedback; defaults to `backgroundColor`

### Example 

    // ...
    "savedSearchesSidebar": {
        "color": "#333333",
        "backgroundColor": "#0850AA",
        "highlightedBackgroundColor: "#154B8F"
    },
    // ...

## `resultsList` Settings

This is also optional. The colors are used to style the list of search results, or note titles. By default, it inherits colors from the editor to blend in.

The accepted settings to override the default colors:

- `color`: optional hexadecimal color string for the search result title text; defaults to `style.base.color`
- `backgroundColor`: optional hexadecimal color string for the list item's background; defaults to `editor.backgroundColor`
- `alternateBackgroundColor`: optional hexadecimal color string for the list item's background when "Alternate Row Colors" is enabled by the user; defaults to `editor.backgroundColor`
- `border`: optional, use to configure the visible border between the list of results and the editor. You may want to deactivate a visible border when the background colors provide enough contrast, for example. Accepted values: `false`, to deactivate; `true`, to use the macOS system default color; or a hexadecimal color string. Defaults to `true`.
- `selection`: optional settings for user-selected notes in the list; similar to text selection in the `editor` block (see above). Accepts `color`, `unfocusedColor`, `backgroundColor`, and `unfocusedBackgroundColor`. The "unfocused" variants are used to help users figure out when hitting keys on their keyboard will affect the notes selection, for example. If you don't provide these variants, the default (focused) variants are used.

### Example

This is from the _Solarized (Light)_ theme:

    // ...
    "resultsList": {
        "color": "#657b83",
        "backgroundColor": "#FDF6E4",
        "alternateBackgroundColor": "#F2EBDA",
        "selection": {
            "color": "#FDF6E4",
            "backgroundColor": "#657b83",
            "unfocusedColor": "#586e75",
            "unfocusedBackgroundColor": "#E0DAC9"
        },
        "border": "#EAE4D2",
    },
    // ...

- The background is the same color as the editor; the alternate is a slightly darker variation;
- the text color is the default editor text color, too, so the sidebar would blend in with the editor seamlessly;
- a border is set to have a visual separator;
- the selection colors are similar to text selection colors.

Note that other themes, like _Solarized (Dark)_, omit borders and use a darker background for the whole result list for maximum contrast. That works just as well, visually.

## `styles` Settings

These are Markdown highlighter settings.

### Block Styles

Here's a list of all blocks you can configure:

- `base`
- `h1`
- `h2`
- `h3`
- `h4`
- `h5`
- `h6`
- `horizontalRule`
- `codeBlock`
- `inlineCode`
- `blockquote`
- `list`
- `comment`
- `referenceDefinition` (unused at the moment)
- `table` (unused at the moment)
- `header` (unused at the moment)

#### Supported attributes by all block styles

- `font`: override of the app's or parent block's base font setting; accepted attributes are `name` (String, required), `size` (points as a float), and `style` (one of "regular", "italic", "bold")
- `color`: hexadecimal color string, like "#93a1a1"
- `backgroundColor`: hexadecimal color string, like "#93a1a1"
- `syntax`: settings for the Markdown syntax element characters; accepts with `color` and `backgroundColor`
- `indent`: settings to indent the whole block visually; can be "none", "inherit", or `{"points": 12.34}` (measured in the base font size) or `{"characters": 2}`

##### Overriding fonts

You can override the app's font setting, which is not recommended, by the way, like this:

    "codeBlock": {
        "font": { 
            "name": "Monaco",
            "size": "12"
        }
    }, // ...

Assuming the app uses a non-monospace font for text, this would style inline code in monospace. Some people prefer a mix of fonts for legibility. But you have to provide `name` _and_ `size` to make it work. (We'll change this in the future to be a bit more convenient.)

### Inline styles

- `inlineCode`
- `emphasis`
- `strong`
- `image`
- `link`
- `comment`
- `footnote`

#### Supported attributes by all inline styles

- `font`: override of the app's or parent block's base font setting; accepted attributes are `name` (a string, if you want to replace the font), `size` (points as a float), or `style` (one of "regular", "italic", "bold")
- `color`: hexadecimal color string, like `"#93a1a1"`
- `backgroundColor`: hexadecimal color string, like `"#93a1a1"`
- `syntax`: settings for the Markdown syntax element characters; accepts with `color` and `backgroundColor`

#### Example

    "emphasis": {
        "color": "#93a1a1",
        "font": { "style": "italic" },
    }, // ...
