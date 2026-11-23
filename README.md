# Laboratory Exercise 3: React Props and Component Communication

## Guide Questions

1. Difference between props and state?
Answer:  Props are read-only inputs passed from a parent component to a child component, while state is internal data managed inside a component that can update over time and trigger re-renders.

2. Why a child component cannot modify props?
Answer:  Props belong to the parent/owner component and are kept immutable in the child so data flow stays predictable and changes do not unexpectedly affect other components.

3. How does `onAddToCart` pass data from child to parent?
Answer:  The parent passes a callback function down via the `onAddToCart` prop; when the child calls `onAddToCart(title)` on button click, it executes the parent's state updater with the product title.

4. Why is `key` required in `.map()`, and what are the effects of missing or duplicate keys?
Answer:  React uses `key` to track items across renders; missing keys trigger warnings and less reliable updates, while duplicate keys can cause rendering bugs or misplaced UI/component state.

5. What does `children` allow compared to normal props?
Answer:  `children` lets you pass nested JSX/elements directly between a component's opening and closing tags, making it easy to compose custom badges or content without defining separate named props for every layout variation.

---

# Laboratory Exercise 4: React `useState` and `useEffect` (Live Search and Filter)

## Guide Questions

1. Why do we use two separate states, `searchText` and `debouncedSearch`, instead of only one?
Answer:  `searchText` updates immediately on every keystroke so the controlled input box stays responsive to what is being typed, while `debouncedSearch` only updates after a 300ms pause so filtering does not run on every single keystroke while typing quickly.

2. What would happen if the debounce effect did not return a cleanup function?
Answer:  Without `return () => clearTimeout(timer)`, every keystroke would leave its own timer active and still call `setDebouncedSearch` later even during continuous typing, causing delayed updates and extra renders instead of waiting only for the final pause.

3. What is the purpose of the dependency array? What happens if it is empty (`[]`)? What happens if it is left out completely?
Answer:  The dependency array controls when `useEffect` re-runs based on changes to listed values. With dependencies (such as `[searchText]` or `[visibleProducts.length]`), it runs after the initial render and whenever any listed dependency changes; with an empty array (`[]`), it runs only once when the component mounts; and if left out completely, it runs after every render, which can cause extra work or infinite update loops if state is updated inside the effect.

4. Why should we avoid sorting the original `products` array directly?
Answer:  Sorting `products` in place mutates the original data array, changing the source order across renders and risking unpredictable behavior. Making a shallow copy first (such as `[...products]`) keeps the original array intact.

5. Is the filtered product list better stored in its own state, or calculated from existing state during rendering? Explain your choice.
Answer:  Calculating it from existing state (`debouncedSearch`, `category`, and `sortOrder`) during rendering is better because it is derived data. Storing it in separate state adds redundant state that has to be kept in sync and can easily go out of date.