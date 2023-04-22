1.Explain what the simple List component does.

Ans:-

The Simple List Component in React is a reusable component that displays a list of items and allows the user to select one item. It comprises two sub-components, 
WrappedSingleListItem and WrappedListComponent. The WrappedSingleListItem renders individual list items and sets the style of the li element based on whether 
the item is selected or not. The WrappedListComponent renders the list by mapping over the array of items and passing each item to the SingleListItem component,
which is a memoized version of WrappedSingleListItem. When an item is clicked, its background color changes to green to indicate that it is the current selection.

2.What problems / warnings are there with code?

Ans:- 

Given code has few pronlems / warnings which is meantioned below

First issue :-

`
const [setSelectedIndex, selectedIndex] = useState(); `

This line has an issue because of order of selectedIndex and setSelectedIndex because in the code we are using 
setSelectedIndex as a function which set our defined useState selectedIndex and for setting the state second element is needed which is a function.

Second issue:- 

```
<li
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={onClickHandler(index)}
```

This line gives an error because onClickHandler should be defined as an arrow function.

Third issue:-

`
WrappedListComponent.propTypes = {
  items: PropTypes.array(
    PropTypes.shapeOf({
      text: PropTypes.string.isRequired,
    })
  ),
};
`

shapeOf should be written as shape because shapeOf is not a function and array should be written as arrayOf.

Fourth issue:-

`
{items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
        />
))}
`

Here we are itterating over items if items is null then it will give error so we should put a condition here to check wheather items is null or has some elements.

Fifth issue:-

`
<SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
/>
`

As in this code isSelected has prototype of boolean type and we are passing just a selectedIndex which is not boolean so here we should pass a boolean value


3.Please fix, optimize, and/or modify the component as much as you think is necessary.
Ans:- All optmization ans  modifications are done in the component and modified code is given below 

```
import React, { useState, useEffect, memo } from "react";
import PropTypes from "prop-types";

// Single List Item
const WrappedSingleListItem = ({ 
          index, 
          isSelected, 
          onClickHandler, 
          text 
}) => {
	return (
		<li
			style={{ backgroundColor: isSelected ? "green" : "red" }}
			onClick={()=> onClickHandler(index)}
		>
			{text}
		</li>
	);
};

WrappedSingleListItem.propTypes = {
	index: PropTypes.number,
	isSelected: PropTypes.bool,
	onClickHandler: PropTypes.func.isRequired,
	text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({ 
  items 
}) => {  const [selectedIndex, setSelectedIndex] = useState(" ");

	useEffect(() => {
		setSelectedIndex(null);
	}, [items]);

	const handleClick = (index) => {
		setSelectedIndex(index);
	};

	return (
		<ul style={{ textAlign: 'left' }}>
		{items ? (
        items.map((item, index) => (
        <SingleListItem
          key={index}
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex === index }
        />
      )) ): (
        <>error, because of items has null value</>
        )}
		</ul>
	);
};

WrappedListComponent.propTypes = {
	items: PropTypes.arrayOf(
		PropTypes.shape({
			text: PropTypes.string.isRequired,
		})
	),
};

WrappedListComponent.defaultProps = {
	items: [{text: "Item-1", index:1}, {text: "item-2", index: 2}, {text: "Item-3", index:3}, 
          {text: "Item-4", index:4}, {text: "Item-5", index:5}],
};

const List = memo(WrappedListComponent);

export default List;

```
