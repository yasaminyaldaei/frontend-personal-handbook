## Table Of Contents:
- [How is responsive design different from adaptive design?](#How-is-responsive-design-different-from-adaptive-design?)
- [Have you ever worked with retina graphics? If so, when and what techniques did you use?](#have-you-ever-worked-with-retina-graphics-if-so-when-and-what-techniques-did-you-use)


### How is responsive design different from adaptive design?
- Both attempt to optimize UX across different devices
    - Adjusting different viewport sizes
    - resolutions
    - usage contexts
    - control mechanisms
- **Responsive design works on the principle of flexibility**
    - A single fluid website that flexes and changes based on the multitude of factors
    - By using:
        - media queries
        - flexible grids
        - responsive images
    - Setting the breakpoints could be quite challenging
        - Standardized values or customized based on our own layout?
- **Adaptive design is more of a progressive enhancement approach**
    - It detects the device and other features and then provides the appropriate feature and layout
        - Based on a predefined set of viewport sizes and other characteristics 
    - This approach requires unreliable methods
        - User-agent sniffing
        - DPI detection

### Have you ever worked with retina graphics? If so, when and what techniques did you use?
- Retina is just a high-resolution screen with a pixel ration bigger than 1
    - Using a pixel ratio means these displays are emulating a lower resolution screen to show the elements in the same size
        - Because of this all mobile devices are considered retina defacto displays
- Browsers by default render DOM elements according to the device resolutions, except for **images**
    - For retina displays, we need high res images
        - It may have performance issues
        - To overcome this we can use the *responsive images* feature of HTML5
        - Which is providing different resolution files of the same image and let the browser decide which one to use
        - We should use `srcset` for this issue
        - Browsers which don't support `srcset` will ignore it and use `src` instead 
            - Polyfill can be used for further support
- For icons, SVGs and icon fonts can be used for perfect result regardless of resolution




## Sources:
1. [h5b5/Front-end Developer Interview Questions](https://github.com/h5bp/Front-end-Developer-Interview-Questions)
2. [yangshun/front-end-interview-handbook](https://github.com/yangshun/front-end-interview-handbook)

