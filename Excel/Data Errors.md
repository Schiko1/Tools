- Input errors
	- Misspelling
	- Unnecessary character
	- Unnecessary spaces
	- incorrectly placed entry
	- Abbr. and acronyms
	- Date and Time
	- Duplicate information
- Poor layout
	- Example: instead of full address in one column, spilt it into multiple

## Function to correct errors

- LEFT, RIGHT, MID
	- return a specific number of characters from either the left, the right, or the middle side of a cell entry
	- used in situations where you need to transfer parts of the cell content to a different column
	- The **LEFT** and **RIGHT** function formulas need two arguments. You must specify the cell which contains the original entry and the number of characters you would like extracted from it.
		- =LEFT(B2;6)
		- =RIGHT(B2;6)
	- A **MID** function formula needs three arguments:
		-  location of the text entry.
		- position in the entry where Excel must begin counting characters.
		- number of characters to the right of that position to extract
	- called **Text to columns** which performs this action more efficiently

- **TRIM**
	- used to remove any empty spaces from text strings, except for the spaces between words

- UPPER, LOWER and PROPER
	- **UPPER** function converts all lowercase characters into uppercase
	- LOWER function does the opposite.
	-  PROPER function will only capitalize the first character of each word in a piece of text

- CONCAT
	- combine entries from different cells in a spreadsheet into a single-cell entry
	- =CONCAT(A2;B2;C2)
	- =CONCAT(A2,” “,B2,” “,C2)