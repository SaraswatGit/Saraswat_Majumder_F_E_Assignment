## 1.Explain what the simple List component does.
- List component renders an unordered list , whose content(text) have been passed as Props .
- On clicking a list item , the background color becomes green but otherwise has a default color of red.  

## 2.What problems / warnings are there with code?
- Syntax Error <br />
  Wrong Code :
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
