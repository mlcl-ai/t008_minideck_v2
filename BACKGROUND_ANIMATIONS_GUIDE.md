# How t004 Background Animations Work

## Key Mechanisms:

### 1. **Slide Properties** (Flag-based approach)
Each slide has custom properties that trigger different behaviors:

```javascript
{
  title: "...",
  subtitle: "...",
  background: "#667eea",  // or gradient
  textColor: "white",
  isIntro: true,          // Special intro styling
  hasNewsBackground: true  // Triggers NewsCardBackground component
}
```

### 2. **Conditional Rendering in SlideContent**

The `SlideContent` component checks slide properties and `currentSlide` index:

```javascript
const SlideContent = ({ slide, isFullscreen }) => {
  // Check if papers should show
  const shouldShowPapers = slide.hasNewsBackground || showSweepAnimation;
  const isSweepingAway = showSweepAnimation && currentSlide === 2;
  
  return (
    <div style={{background: slide.background}}>
      {/* Conditionally render background animation */}
      {shouldShowPapers && <NewsCardBackground ... />}
      
      {/* Conditionally render overlay bar */}
      {slide.hasNewsBackground && (
        <div style={{ position: 'absolute', ... }}>
          {/* Semi-transparent overlay bar */}
        </div>
      )}
      
      {/* Conditionally render intro decoration */}
      {slide.isIntro && (
        <div className="absolute top-1/4 right-1/4">
          <StrokePage width={200} height={260} />
        </div>
      )}
      
      {/* Main text content */}
      <h1>{slide.title}</h1>
      {slide.subtitle && <h2>{slide.subtitle}</h2>}
    </div>
  );
};
```

### 3. **Slide Index-Based Logic**

Navigation logic triggers animations based on slide transitions:

```javascript
const nextSlide = () => {
  if (currentSlide < deck.slides.length - 1) {
    // Trigger sweep animation when moving from slide 1 to slide 2
    if (currentSlide === 1) {
      setShowSweepAnimation(true);
      setTimeout(() => setShowSweepAnimation(false), 500);
    }
    setCurrentSlide(currentSlide + 1);
  }
};
```

### 4. **CSS Animations**

Keyframe animations defined in `<style>`:

```css
@keyframes popIn {
  0% { transform: scale(0) rotate(0deg); opacity: 0; }
  60% { transform: scale(1.15) rotate(var(--rotation)); opacity: 1; }
  100% { transform: scale(1) rotate(var(--rotation)); opacity: 1; }
}

.news-slide .stroke-page {
  animation: popIn 0.3s cubic-bezier(0.34, 1.56, 0.64, 1) forwards,
             float 3s ease-in-out infinite;
  animation-delay: var(--delay), calc(var(--delay) + 0.3s);
}
```

## How to Apply to Your Deck:

### Method 1: Add Custom Properties
```javascript
{
  title: "Demo: AI with eyes and hands",
  subtitle: "",
  background: "linear-gradient(...)",
  textColor: "white",
  hasImage: true,           // New flag
  imagePath: "images/handoff_zoomin.png",
  imagePosition: "center"
}
```

### Method 2: Check currentSlide Index
```javascript
const SlideContent = ({ slide, isFullscreen }) => {
  return (
    <div style={{background: slide.background}}>
      {/* Slide 7 - Demo slide with image */}
      {currentSlide === 7 && (
        <img 
          src="images/handoff_zoomin.png" 
          style={{
            position: 'absolute',
            width: '80%',
            height: '80%',
            objectFit: 'contain',
            zIndex: 10
          }}
        />
      )}
      
      {/* Slide 8 - World model diagram */}
      {currentSlide === 8 && (
        <img 
          src="images/world_model.png"
          style={{
            position: 'absolute',
            bottom: '10%',
            right: '10%',
            width: '40%',
            zIndex: 10
          }}
        />
      )}
      
      {/* Regular text content */}
      <h1>{slide.title}</h1>
    </div>
  );
};
```

### Method 3: Background Images via CSS
```javascript
{
  title: "Demo...",
  background: "url('images/handoff_zoomin.png') center/cover no-repeat",
  textColor: "white"
}
```

## Recommended Approach for Your Deck:

Use **Method 2** (index-based) for maximum flexibility:

```javascript
// In SlideContent component, add after line ~453
const SlideContent = ({ slide, isFullscreen }) => {
  return (
    <div className="w-full h-full relative" style={{background: slide.background}}>
      
      {/* Slide 7: Demo with hands image */}
      {currentSlide === 7 && (
        <img 
          src="images/handoff_zoomout.png"
          alt="Demo"
          className="absolute inset-0 w-full h-full object-cover"
          style={{ zIndex: 5 }}
        />
      )}
      
      {/* Slide 8: World model diagram */}
      {currentSlide === 8 && (
        <div className="absolute bottom-0 right-0 w-1/2 h-1/2" style={{ zIndex: 5 }}>
          <img 
            src="images/world_model.png"
            alt="World Model"
            className="w-full h-full object-contain"
          />
        </div>
      )}
      
      {/* Regular text content (zIndex: 30) */}
      <div className="relative h-full..." style={{ zIndex: 30 }}>
        <h1>{slide.title}</h1>
        {slide.subtitle && <h2>{slide.subtitle}</h2>}
      </div>
    </div>
  );
};
```

This keeps text on top (z-index: 30) while images sit below (z-index: 5-10).

