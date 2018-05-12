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

        "styles": {
            // Highlighter attributes
        }
    }

## Root-Level Keys

List of all root-level attributes:

- `name`: String
- `style`: String, either "Light" or "Dark"; used to filter themes for dark mode
- `author`: Array of author detail objects
- `version`: String
- `editor`: a settings Object, see below for details
- `styles`: a settings Object, see below for details

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
- `selection`: settings for user-selected text; accepts `color`, `backgroundColor`, and `unfocusedBackgroundColor`
- `savedSearchesSidebar`: settings object to override the saved searches sidebar's colors; colors will be inherited from the editor and base style to blend in. You can override `color` (for text), `backgroundColor`, and the optional `highlightedBackgroundColor` to give visual feedback for click events. To keep the color variety limited, consider using the selection color as background.

The difference between `backgroundColor` and `unfocusedBackgroundColor` is that the latter will be used when the user clicks into another component, for example the Omnibar, to indicate the text editor is not receiving key events. In other words: use this to help users figure out if typing on the keyboard would overwrite the highlighted part.

### Example

Omitting the rest of the theme, here's the relevant part to change the `editor` settings:

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
        "savedSearchesSidebar": {
            "color": "#333333",
            "backgroundColor": "#0850AA",
            "highlightedBackgroundColor: "#154B8F"
        }
    },

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
