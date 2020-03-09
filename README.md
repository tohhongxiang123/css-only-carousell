# Carousel with only HTML and CSS

```html
<div class="carousel">
    <div class="carousel__slides">
        <div class="carousel__slide" id="slide-1">
            <a href="#slide-7" class="carousel__prev">7</a>
            <img src="https://http.cat/100" />
            <a href="#slide-2" class="carousel__next">2</a>
        </div>
        <div class="carousel__slide" id="slide-2">
            <a href="#slide-1" class="carousel__prev">1</a>
            <img src="https://http.cat/200" />
            <a href="#slide-3" class="carousel__next">3</a>
        </div>
        <div class="carousel__slide" id="slide-3">
            <a href="#slide-2" class="carousel__prev">2</a>
            <img src="https://http.cat/300" />
            <a href="#slide-4" class="carousel__next">4</a>
        </div>
        <div class="carousel__slide" id="slide-4">
            <a href="#slide-3" class="carousel__prev">3</a>
            <img src="https://http.cat/400" />
            <a href="#slide-5" class="carousel__next">5</a>
        </div>
        <div class="carousel__slide" id="slide-5">
            <a href="#slide-4" class="carousel__prev">4</a>
            <img src="https://http.cat/500" />
            <a href="#slide-6" class="carousel__next">6</a>
        </div>
        <div class="carousel__slide" id="slide-6">
            <a href="#slide-5" class="carousel__prev">5</a>
            <img src="https://http.cat/404" />
            <a href="#slide-7" class="carousel__next">7</a>
        </div>
        <div class="carousel__slide" id="slide-7">
            <a href="#slide-6" class="carousel__prev">6</a>
            <img src="https://http.cat/206" />
            <a href="#slide-1" class="carousel__next">1</a>
        </div>
    </div>
    <div class="carousel__links">
        <a href="#slide-1" class="carousel__link">1</a>
        <a href="#slide-2" class="carousel__link">2</a>
        <a href="#slide-3" class="carousel__link">3</a>
        <a href="#slide-4" class="carousel__link">4</a>
        <a href="#slide-5" class="carousel__link">5</a>
        <a href="#slide-6" class="carousel__link">6</a>
        <a href="#slide-7" class="carousel__link">7</a>
    </div>
</div>
```

We have a carousel with 7 slides. Each slide has a corresponding link on the bottom, and a NEXT/PREV button to go to the next/previous slide.
Each carousel link has a href which points to the ID of a slide. When clicked, the carousel will navigate to that slide. 

```css
* {
	box-sizing: border-box
}
img {
	max-width: 100%;
	max-height: 100%;
}
a {
	text-decoration: none;
	color: black;
}
```

This is some global css we will apply to the page, to make everything look a little bit nicer. `img` has a max-width and max-height to prevent overflow from its container.

Firstly, 

```css
.carousel {
	width: 1000px;
	padding: 20px;
	border: 10px solid black;
}
```

This just gives the overall carousel container a specified width, padding and a thick black border.

```css
.carousel__slides {
	display: flex;
	scroll-behavior: smooth;
	margin: 0 auto;
	overflow-x: auto;
	scroll-snap-type: x mandatory;
	width: 500px;
}
.carousel__slide {
	scroll-snap-align: center;
	width: 500px;
	flex-shrink: 0;
	border: 1px solid black;
	display: flex;
	justify-content: center;
	align-items: center;
}align-items: center;
}
```

`.carousel__slides` contains all the slides in the carousel. 
- `display: flex` makes sure the images stack up horizontally.
- `scroll-behavior: smooth` makes sure that when you click on  a navigation link, there is a smooth transition from one slide to another. 
- `margin: 0 auto` makes sure the carousel slides are in the center of the overall carousel container
- `overflow-x: auto` so that only 1 slide is visible at a time. 
- `scroll-snap-type: x mandatory` ensures that when scrolling through the carousel, it will snap to a single slide, instead of being stuck between 2 slides

`.carousel__slide` is 1 specific slide within the carousel
- If you do not define a width here, `.carousel__slides` will take the size of the largest image
- `scroll-snap-align: center` is affected by `scroll-snap-type` above. With these 2 combinations, everytime we scroll past a slide, the center of the slide will align with the center of the `.carousel__slides` container. Experiment with `scroll-snap-align: start` and `scroll-snap-align: end` to see the differences.
- `flex-shrink: 0` forbids each carousel slide from shrinking. We want this because we want to turn the carousel into a scrollable container, and if all the slides shrink to fit withink `.carousel__slides`, there's nothing to scroll, and there's no slideshow anymore
- Everything else is styling for individual slides to make sure the images are centered within the container, and a border for us to know how big each slide is. 

I have given equal widths to both `.carousel__slides` and `.carousel__slide` because I prefer the way it looks. Experiment with different widths and see how it goes.

```css
.carousel__links {
	display: flex;
	justify-content: center;
	align-items: center;
}
.carousel__link {
	width: 20px;
	height: 20px;
	color: black;
	border: 1px solid black;
	border-radius: 50%;
	text-align: center;
	text-decoration: none;
	margin: 5px;
}
```
- This just makes the carousel navigation links line up side by side, and styles them to look like a circle with the slide number within it.

Currently, the overall css looks like this
```css
* {
	box-sizing: border-box
}
img {
	max-width: 100%;
	max-height: 100%;
}
a {
	text-decoration: none;
	color: black;
}
.carousel {
	width: 1000px;
	padding: 20px;
	border: 10px solid black;
}

.carousel__slides {
	display: flex;
	scroll-behavior: smooth;
	margin: 0 auto;
	overflow-x: auto;
	scroll-snap-type: x mandatory;
}
.carousel__slide {
	scroll-snap-align: center;
	flex-shrink: 0;
	border: 1px solid black;
	display: flex;
	justify-content: center;
	align-items: center;
}
.carousel__links {
	display: flex;
	justify-content: center;
	align-items: center;
}
.carousel__link {
	width: 20px;
	height: 20px;
	color: black;
	border: 1px solid black;
	border-radius: 50%;
	text-align: center;
	text-decoration: none;
	margin: 5px;
}
```

And the carousel is already functional, except for the prev/next buttons. Now we are going to add functionality for the next/prev buttons

```css
.carousel__prev,
.carousel__next {
	display: block;
	z-index: 1;
	background: white;
	width: 30px;
	height: 30px;
	border-radius: 50%;
	font-size: 0;
	padding: 20px;
	display: flex;
	justify-content: center;
	align-items: center;
	text-align: center;
}

.carousel__prev {
	transform: translateX(100%);
}
.carousel__prev::before {
	content: '<';
	font-size: 20px;
}

.carousel__next {
	transform: translateX(-100%);
}
.carousel__next::before {
	content: '>';
	font-size: 20px;
}
```

- `.carousel__prev` and `.carousel__next` both have a font-size of 0 so that the words don't show. I made it a circle with diameter of 30px by setting a width and height of 30px and a  `border-radius: 50%`
- `display: flex; justify-content: center; align-items: center;` helps to centralise the content we will put in next (which is just a `<` and a `>` symbol)
- `carousel__prev` and `carousel__next` is transformed inwards, so that it is visible
- The `::before` content is filled with the appropriate arrows
