## 1.Explain what the simple List component does.
- List component renders an unordered list , whose content(text) have been passed as Props .
- On clicking a list item , the background color becomes green but otherwise has a default color of red.  

## 2.What problems / warnings are there with code?
- Syntax Error <br />
  Wrong Code :  (ropTypes.array(PropTypes.shapeOf should be ropTypes.arrayOf(PropTypes.shape)
 ````
  WrappedListComponent.propTypes = {
  items: PropTypes.array(PropTypes.shapeOf({
    text: PropTypes.string.isRequired,
  })),
}; 
````

Correct Code should be  : 
````
   WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(PropTypes.shape({
    text: PropTypes.string.isRequired,
  })),
  
};
````

- UseState Syntax is wrong ( index 0 is the state, 1 is the setter but opposite in the given case  )
  This results in a warning (missing dependency as setSelectedIndex is being considered a stated) <br/>
 Wrong Code :
  ````
   const [setSelectedIndex, selectedIndex] = useState();

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);
  ````
Correct Code should be : 
  ````
    const [selectedIndex, setSelectedIndex] = useState();

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);
  ````
 
## 3.Please fix, optimize, and/or modify the component as much as you think is necessary.
-  Fix (Logical Error) :
      The browed doesn't know which list item is selected by the current logic of isSelected = {selectedIndex} , instead it should be , isSelected = {selectedIndex==index} 
     
     ````
      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
        />
      ))}
      ````
      ````
       {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex===index}
        />
      ))}
      ````

### New Code (Modified , Fixed , Optimized )  :
````

import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const WrappedSingleListItem = ({
  index,
  isSelected,
  onClickHandler,
  text,
}) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={onClickHandler(index)}
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
  items,
}) => {
//useState syntax destructing was reversed so I have fixed it .
  const [selectedIndex, setSelectedIndex] = useState();

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = index => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex===index}
          //new logic to keep track of which list item is currently selected
        />
      ))}
    </ul>
  )
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(PropTypes.shape({
    text: PropTypes.string.isRequired,
  })),
};

WrappedListComponent.defaultProps = {
  items: null,
};

const List = memo(WrappedListComponent);

export default List;


````
