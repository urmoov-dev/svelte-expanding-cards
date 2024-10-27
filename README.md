# Svelte Expanding Card Library

A Svelte library that provides an animated card component that expands to fill a target container when clicked, revealing additional details with a smooth transition.

## Features

- Smooth animation from card to full-container view
- Maintains aspect ratio during transition
- Customizable border radius and easing functions
- Responsive to container size changes
- Automatic flex layout adjustment for card grid layouts
- Frosted glass overlay effect

## Installation

```bash
npm install svelte-expanding-card
```

## Basic Usage

```svelte
<script>
  import { Card, ExpandedCardTarget } from 'svelte-expanding-card';
</script>

<ExpandedCardTarget>
  <Card jsStyle={{ height: "300px", borderRadius: "1vh" }}>
    <div slot="card">
      <!-- Your card content here -->
    </div>
    <div slot="details">
      <!-- Your expanded details content here -->
    </div>
  </Card>
</ExpandedCardTarget>
```

## Components

### Card.svelte

The main card component that handles the expansion animation and content display.

#### Props

- `jsStyle` (object):
  - `height` (string, optional): Card height
  - `borderRadius` (string, default: "1vh"): Border radius of the card
  - `easing` (string, default: "quintOut"): Svelte easing function name

#### Slots

- `card`: Content displayed in the collapsed card view
- `details`: Content displayed in the expanded view

### ExpandedCardTarget.svelte

Container component that defines the expansion target area and handles layout management.

#### Props

- `classes` (string, default: ""): Additional CSS classes
- `style` (string, default: ""): Inline styles for the container

## Example

```svelte
<script>
  import { Card, ExpandedCardTarget } from 'svelte-expanding-card';
</script>

<ExpandedCardTarget style="display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 1rem;">
  <Card jsStyle={{ height: "300px" }}>
    <div slot="card">
      <h3>Card Title</h3>
      <p>Preview content</p>
    </div>
    <div slot="details">
      <h2>Detailed View</h2>
      <p>Extended content that appears when expanded</p>
    </div>
  </Card>
  
  <!-- Add more cards as needed -->
</ExpandedCardTarget>
```

## Advanced Features

### Auto Grid Layout

The `ExpandedCardTarget` component automatically handles flex layouts, ensuring:
- Even distribution of cards
- Proper alignment of the last row
- Dynamic adjustment to container size changes

### Animation Customization

The expansion animation can be customized through the `jsStyle` prop:

```svelte
<Card jsStyle={{
  height: "300px",
  borderRadius: "2vh",
  easing: "elasticOut"
}}>
```

Available easing functions include all standard Svelte easing functions (linear, quad, cubic, etc.).

### Responsive Design

The component automatically recalculates positions and sizes when:
- The container size changes
- The window is resized
- The layout updates

## Styling

The component provides several CSS classes for customization (they must be targeted with the :global keyword) in your `.svelte` file:

```svelte
<style>
    /* Card styles */
    :global(.expanding-card) {
        /* Your custom styles */
    }

    /* Expanded view styles */
    :global(.expanding-card .details) {
        /* Your custom styles */
    }

    /* Close button styles */
    :glboal(.expanding-card) {
        /* Your custom styles */
    }
</style>
```

## Browser Support

- Works in all modern browsers
- Requires CSS transform support
- Recommended to use in Svelte 3.x or later

## Notes

- Cards require a minimum height of 300px and width of 200px by default
- The expansion animation duration is set to 750ms by default
- Content overflow is handled automatically in both card and expanded states

## License

MIT