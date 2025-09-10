
## VLOOKUP
- **Definition**: Searches for a value in the **first column** of a table and returns a value in the same row from another column.
- **Syntax**:  
  `=VLOOKUP(lookup_value, table_array, col_index_num, [range_lookup])`
- **Example**:  
  `=VLOOKUP(101, A2:D10, 3, FALSE)`  
  → Looks for `101` in column A (first column of the range) and returns the value from the 3rd column of the table.

---

## HLOOKUP
- **Definition**: Searches for a value in the **first row** of a table and returns a value in the same column from another row.
- **Syntax**:  
  `=HLOOKUP(lookup_value, table_array, row_index_num, [range_lookup])`
- **Example**:  
  `=HLOOKUP("Q2", A1:H5, 3, FALSE)`  
  → Looks for `Q2` in the first row and returns the value from the 3rd row of the table.

---

## INDEX
- **Definition**: Returns the value of a cell within a given table or range based on row and column numbers.
- **Syntax**:  
  `=INDEX(array, row_num, [column_num])`
- **Example**:  
  `=INDEX(A2:C10, 4, 2)`  
  → Returns the value at **row 4, column 2** within the range `A2:C10`.

---

## MATCH
- **Definition**: Returns the **position** of a lookup value within a range (not the value itself).
- **Syntax**:  
  `=MATCH(lookup_value, lookup_array, [match_type])`
- **Example**:  
  `=MATCH(200, B2:B10, 0)`  
  → Returns the position of `200` in the range `B2:B10`.

---

## INDEX + MATCH Combination
- **Why use it**: More flexible than VLOOKUP/HLOOKUP because:
  - It doesn’t require the lookup column/row to be the first.
  - Works with both vertical and horizontal lookups.
  - Can do two-way lookups (row + column).
- **Example**:  
  `=INDEX(C2:C10, MATCH(101, A2:A10, 0))`  
  → Finds `101` in column A, gets its row position with `MATCH`, then returns the corresponding value from column C.
