## Table Of Contents:
- [Explain event delegation](#explain-event-delegation)


### Explain event delegation
- A technique to add event listeners to a parent element instead of adding them to the children
- The listener will fire whenever the event is triggered on the decendant elements due to **event bubbling up the DOM**
- Benefits:
    - Memory footprint goes down because of single handler being needed instead of multiple ones
    - No need to unbind handlers from the removed childrend and bind the event to new ones

## Sources:
1. [h5b5/Front-end Developer Interview Questions](https://github.com/h5bp/Front-end-Developer-Interview-Questions)
2. [yangshun/front-end-interview-handbook](https://github.com/yangshun/front-end-interview-handbook)

