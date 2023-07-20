**React will only update state if a new object/array is passed to setState.**

1. ## Understand Why We Can't Mutate State

`State` handles datatypes in js, object are not to be change in the react state directly.

To change the object create a new one or make a copy of the existing one,the set state to use the new one.

`example to demo`

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount((count) => count + 1);
  }

  return <button onClick={handleClick}>{count}</button>;
}
```
3. React **re-renders** the `Counter` component based on changes to internal
   state


   ```jsx
function Counter() {
  const [count, setCount] = useState(0);

  function handleClick() {
    // setting state with the same value
    setCount(count);
  }

  return <button onClick={handleClick}>{count}</button>;
}
``

in step 3 above, one key detail is that React re-renders components based on
_changes_ to state. For example, if we updated the code of our component in such
a way that instead of incrementing the count when the button was clicked, we
simply called `setCount` _with the same value_, React wouldn't re-render our
component, since _we gave it the same value for state_ as it already has:


# working with an object instead

```jsx
function Counter() {
  const [count, setCount] = useState({ x: 0 });

  function handleClick() {
    // increment the count
    count.x++;
    // set state with the new count value
    setCount(count);
  }

  return <button onClick={handleClick}>{count.x}</button>;
}
```


This works;

```jsx
function Counter() {
  const [count, setCount] = useState({ x: 0 });

  function handleClick() {
    // set state with a new object
    setCount({ x: count.x + 1 });
  }

  return <button onClick={handleClick}>{count.x}</button>;
}
```

## Adding Elements To Arrays In State

add;

- Shows a button to generate a new spicy food
- When the button is clicked, adds the newly generated food to a list

**React will only update state if a new object is passed to `setState`**.

see the difference

```js
function handleAddFood() {
    const newFood = getNewRandomSpicyFood();
    console.log(newFood);
  }
  ```

  the correct way
  
   ```jsx
function handleAddFood() {
  const newFood = getNewRandomSpicyFood();
  const newFoodArray = [...foods, newFood];
  setFoods(newFoodArray);
}
```


```jsx
const newFoodArray = [...foods, newFood];
```
here we are using spread operator (...food) to make copy of foods.

 That's why we're using the spread operator here
to make a copy of the array, instead of `.push`, which will mutate the original
array.


### Removing Elements From Arrays In State

Next, in the `handleLiClick` function, we need to figure out a way to update our
array in state so it no longer includes the food.

```jsx
const foodList = foods.map((food) => (
  <li key={food.id} onClick={() => handleLiClick(food.id)}>
    {food.name} | Heat: {food.heatLevel} | Cuisine: {food.cuisine}
  </li>
));
```
`Solution`

Here we use the `filter method` not to include a specific element

```jsx
function handleLiClick(id) {
  const newFoodArray = foods.filter((food) => food.id !== id);
  setFoods(newFoodArray);
}
```
Calling `.filter` will return a _new array_ based on which elements match our
criteria in the callback function. So if we write our callback function in
`.filter` to look for all foods _except_ the number we're trying to remove,
we'll get back a new, shortened list of foods:

```jsx
[1, 2, 3].filter((number) => number !== 3);
// => [1,2]
```

### Updating Elements in Arrays in State

Here's an example of using `.map` to update _one element_ of an array:

```js
[1, 2, 3].map((number) => {
  if (number === 3) {
    // if the number is the one we're looking for, increment it
    return number + 100;
  } else {
    // otherwise, return the original number
    return number;
  }
});
// => [1,2,103]
```

```js
function handleLiClick(id) {
    const newFoodArray = foods.filter((food) => food.id !== id);
    setFoods(newFoodArray);
  }
```

here its how it should be 

```js
function handleLiClick(id) {
  const newFoodArray = foods.map((food) => {
    if (food.id === id) {
      return {
        ...food,
        heatLevel: food.heatLevel + 1,
      };
    } else {
      return food;
    }
  });
  setFoods(newFoodArray);
}
```

To break down the steps:

- `.map` will iterate through the array and return a new array
- Whatever value is returned by the callback function that we pass to `.map`
  will be added to this new array
- If the ID of the food we're iterating over matches the ID of the food we're
  updating, return a new food object with the heat level incremented by 1
- Otherwise, return the original food object


in array ill always use;


- **Add**: use the spread operator (`[...]`)
- **Remove**: use `.filter`
- **Update**: use `.map`


## Working With Multiple State Variables

```jsx
<select name="filter">
  <option value="All">All</option>
  <option value="American">American</option>
  <option value="Sichuan">Sichuan</option>
  <option value="Thai">Thai</option>
  <option value="Mexican">Mexican</option>
</select>
```


```jsx
const [filterBy, setFilterBy] = useState("All");
```

With this state variable in place, we can update the `<select>` element to set
the `filterBy` variable when its value is changed, like so:

```jsx
function handleFilterChange(event) {
  setFilterBy(event.target.value);
}

return (
  <select name="filter" onChange={handleFilterChange}>
    <option value="All">All</option>
    <option value="American">American</option>
    <option value="Sichuan">Sichuan</option>
    <option value="Thai">Thai</option>
    <option value="Mexican">Mexican</option>
  </select>
);
```




 